---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在流处理这个模块中，我们已经学习了 Spark 的两种流处理解决方案，在本课时中，我们将进行一个略微复杂的实践，也是我们本模块的实践环节。

在本模块中，我们将对实时股票价格数据进行处理。我们将在本课时中计算一个在股票分析中的比较常见的指标：CCI。

实时股票价格数据蕴含着巨大的价值，如何能在交易过程中敏锐地捕捉到机会非常重要，所以这就是一个非常典型的流处理应用场景。下面介绍一个股票交易实时分析应用：计算分钟级 CCI。

**CCI（Commodity Channel Index），也被称为顺势指标**。它最早用于期货市场的判断，后期才运用于股票市场的研判，并被广泛使用。与大多数单一利用股票的收盘价、开盘价、最高价或最低价而发明出的各种技术分析指标不同，**CCI 指标是根据统计学原理，引进价格与固定期间的股价平均区间的偏离程度的概念，强调股价平均绝对偏差在股市技术分析中的重要性**，是一种比较独特的技术指标。CCI 有事件时间区间的概念，很适合用 Structured Streaming 来完成。

CCI 有日 CCI、周 CCI、年 CCI 以及分钟 CCI 等很多种类型。本例主要实现的是 30 分钟 CCI（一个周期为 30 分钟），其计算公式为：

![image (3).png](https://s0.lgstatic.com/i/image/M00/2B/37/CgqCHl79v5uAX9KwAAAkw-PcxfE329.png)

其中

![image (4).png](https://s0.lgstatic.com/i/image/M00/2B/37/CgqCHl79v6OAVF7lAAAOQzHAHtY193.png)

这个公式为给定 30 分钟内的最高价、最低价和收盘价的平均值，SMA 是 N 个周期的 pt 的移动平均值，MD 是 pt 的平均离差，本例中 N = 3。

再来看看数据，目前股票的实时数据来源渠道有很多，如 Wind、大智慧等，都提供了自己的接口。假定数据已经被实时拉取并灌入到消息队列中，在这个过程中，有可能会出现数据晚到和乱序的现象。为了结果的准确，在处理时需要考虑这些情况，来看一条数据样例：

> 000002.SZ, 1502126681, 22.71, 21.54, 22.32, 22.17

其中每个字段分别是股票代码、事件时间戳、现价、买入价、卖出价、成交均价。

下面我们来看看 CCI 的计算方式，SMA、MD 都需要 pt 序列计算得到，而从公式可以看到计算 pt 需要先得到 30 分钟内的最高价、最低价和收盘价。当 pt 序列按照时间被保存到数据库后，那么计算 CCI 就非常容易了，一个应用定期进行查询并计算即可，**所以计算 CCI 的核心是计算 pt。**

下面的代码采用 Structured Streaming 对数据流进行处理，得到最高价、最低价和收盘价并求其均值，从而得到 pt 序列。其中，我们需要开发一个求收盘价的 UDAF，然后还要开发一个输出到 HBase 的 Sink，正好可以把我们前面学到的知识用上。

```
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.functions._
import org.apache.spark.sql.types.TimestampType
import org.apache.spark.sql.streaming.Trigger
import java.sql.Timestamp
object StockCCICompute {
def main(args: Array[String]): Unit = {
val spark = SparkSession
      .builder
      .appName("StockCCICompute")
      .getOrCreate()
val windowDuration = "30 minutes"
val waterThreshold = "5 minutes"
val triggerTime = "1 minutes"
import spark.implicits._
    spark.readStream
    .format("kafka")
    .option("kafka.bootstrap.servers", "broker1:port1,broker2:port2")
    .option("subscribe", "stock")
    .load()
    .selectExpr("CAST(key AS STRING)", "CAST(value AS STRING)")
    .as[(String, String)]
    .map(f => {
val companyNo = f._1
val infos = f._2.split(",")
        (f._1,infos(0),infos(1),infos(2),infos(3),infos(4))
    })
    .toDF("companyno","timestamp","price","bidprice","sellpirce","avgprice")
    .selectExpr(
"CAST(companyno AS STRING)",
"CAST(timestamp AS TIMESTAMP[DF1] )",
"CAST(price AS DOUBLE)",
"CAST(bidprice AS DOUBLE)",
"CAST(sellpirce AS DOUBLE)",
"CAST(avgprice AS DOUBLE)")
    .as[(String,Timestamp,Double,Double,Double,Double)]
    .withWatermark("timestamp", waterThreshold)
    .groupBy(
          window($"timestamp", 
              windowDuration), 
              $"companyno")
    .agg(
          max(col("price")).as("max_price"),
          min(col("price")).as("min_price"),
ClosePriceUDAF(col("price").as("latest_price")))
    .writeStream
    .outputMode("append")
    .trigger(Trigger.ProcessingTime(triggerTime))
    .foreach(HBaseWriter)
    .start()
    .awaitTermination()
  }
}
```

代码中选取了 append 模式，所以分析应用不用处理结果发生变化的情况。另外代码风格特意采用了 Dataflow 的数据管道式，本例中的数据处理的逻辑是完全可以应用于批处理的。开发的 UDAF 目的是求出收盘价，也就是窗口内时间戳最大的那一条，代码如下：

```
import org.apache.spark.sql.expressions._
import org.apache.spark.sql.types._
import org.apache.spark.sql.Row
import org.apache.spark.sql.functions._
import java.sql.Timestamp
object ClosePriceUDAF extends UserDefinedAggregateFunction {
override def inputSchema: StructType 
    = StructType(Array(StructField[DF1] ("price", DoubleType, true)))
override def bufferSchema: StructType 
    = StructType(
Array(StructField("latestprice", DoubleType, true),
StructField("timestamp", TimestampType, true)))
override def dataType: DataType = DoubleType
override def deterministic: Boolean = true
override def initialize(buffer: MutableAggregationBuffer): Unit 
    = {
      buffer(0) = 0D
      buffer(1) = 0L
    }
override def update(buffer: MutableAggregationBuffer, input: Row): Unit = {
val priceNow = input.getAs[Double]("price")
val timestampNow = input.getAs[Timestamp]("timestamp")
val timestampBuf = buffer.getAs[Timestamp]("timestamp")
if(timestampNow.after(timestampBuf)){
      buffer(0) = priceNow
      buffer(1) = timestampNow
    }
  }
override def merge(buffer1: MutableAggregationBuffer, buffer2: Row): Unit = {
val buffer1Timestamp = buffer1.getAs[Timestamp]("timestamp")
val buffer2Timestamp = buffer2.getAs[Timestamp]("timestamp")
if(buffer2Timestamp.after(buffer1Timestamp)){
       buffer1(0) = buffer2.getAs[Double]("price")
       buffer1(1) = buffer2Timestamp
     }
  }
override def evaluate(buffer: Row): Any = buffer.getAs[Double]("price")
}
```

最后为了保证结果的正确性，需要实现自定义 Writer。这也是选取 HBase 的原因，因为插入到 HBase 的操作天然就具有幂等性（重复 Put 会覆盖之前的值），所以可以实现端到端的恰好一次的消息送达的效果，代码如下：

```
import org.apache.spark.sql.ForeachWriter
import org.apache.spark.sql.Row
import org.apache.hadoop.hbase.HBaseConfiguration
import org.apache.hadoop.hbase.client.ConnectionFactory
import org.apache.hadoop.hbase.client.Connection
import org.apache.hadoop.hbase.TableName
import org.apache.hadoop.hbase.client[DF1] .Put
import org.apache.hadoop.hbase.util.Bytes
object HBaseWriter extends ForeachWriter[Row] {
var conn: Connection  = null
def open(partitionId: Long, version: Long): Boolean = {
val conf = HBaseConfiguration.create()
     conn = ConnectionFactory.createConnection(conf)
true
  }
def process(row: Row): Unit = {
val window = row.getAs[String]("window")
val maxPrice = row.getAs[Double]("max_price")
val minPrice = row.getAs[Double]("min_price")
val latestPrice = row.getAs[Double]("latest_price")
val table = conn.getTable(TableName.valueOf("CCI"))
val put = new Put(Bytes.toBytes("window"))
      put.addColumn(Bytes.toBytes("cf"), Bytes.toBytes("max_price"), Bytes.toBytes(maxPrice))
      put.addColumn(Bytes.toBytes("cf"), Bytes.toBytes("min_price"), Bytes.toBytes(minPrice))
      put.addColumn(Bytes.toBytes("cf"), Bytes.toBytes("latest_price"), Bytes.toBytes(latestPrice))
      table.put(put)
      table.close()
  }  
def close(errorOrNull: Throwable): Unit = {
      conn.close()
  }
}
```

Structured Streaming 用窗口起点--窗口终点作为 window 字段的值，也就是窗口唯一标识。在入库时，该值作为行键方便应用查询。在数据入库后，分析应用可以用窗口标识进行查询，例如用 12:00-12:30、12:30-13:00、13:00-13:30 这三个值分别发起三次查询，从而得到这些窗口的最高价、最低价和收盘价，再分别计算出 pt，最后就能得到当前周期的 CCI。

如果你看到这里，就会发现这个应用的开发过程以及它所需要的组件还是比较复杂的，除了上面的开发过程，我们还需要开发一个后端查询应用才能计算出 CCI，这也是流处理通常是属于数据工程领域而非数据科学领域。

**最后值得注意的是，从代码 spark.readStream 开始到最后处理过程的完成，其实是一行代码，这一行代码稍加改动也可直接用于批处理，这也是 Spark 统一编程接口的一种体现。**

本课时的内容就到这里，下个课时我们将进入下一个模块的学习，我将为你讲解什么是图：图模式，图相关技术与使用场景。
