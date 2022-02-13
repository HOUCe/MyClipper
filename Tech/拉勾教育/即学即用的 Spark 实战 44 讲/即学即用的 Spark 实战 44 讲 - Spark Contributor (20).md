---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在上一课时中，我们学习了 Spark Streaming 的抽象、架构以及数据处理方式，但是流处理的输入是动态的数据源，假设在出错是常态的情况下，如何在动态的数据流中仍然兼顾恰好一次的消息送达保证（结果正确性），是生产环境中必须考虑的问题。

本课时的主要内容有：

-   输入和输出
    
-   容错与结果正确性
    

### 输入和输出

Spark Streaming 作为一个流处理系统，对接了很多输入源，除了一些实验性质的输入源，如ConstantInputDStream（每批次都为常数集合）、socketTextStream（监听套接字作为输入）、textFileStream（本地文件作为输入，常常用来监控文件夹），在生产环境中用得最多的还是 Kafka 这类消息队列。在本课时中，我们选用 Kafka 0.8 版本，介绍 Spark Streaming 与 Kafka 集成，这是在生产环境中最常见的一种情况。

在 Spark 中，为连接 Kafka 提供了两种方式，即基于 Receiver 的方式和 Kafka Direct API。这两种方式在使用上大同小异，但原理却截然不同，先来看看基于 Receiver 的方式：

```
...
val kafkaParams = Map[String, Object](
"bootstrap.servers" -> "localhost:9092,anotherhost:9092",
"key.deserializer" -> classOf[StringDeserializer],
"value.deserializer" -> classOf[StringDeserializer],
"group.id" -> "groupId",
"auto.offset.reset" -> "latest",
"enable.auto.commit" -> (true: java.lang.Boolean)
)
val topics = Array("topicA", "topicB")
val messages = KafkaUtils.createDirectStream[String, String](
  ssc,
PreferConsistent,
Subscribe[String, String](topics, kafkaParams)
)
messages.map(record => (record.key, record.value))
```

用这种方式来与 Kafka 集成，配置中设置了 enable.auto.commit 为 true，表明自己不需要维护 offset，而是由 Kafka 自己来维护（在 Kafka 0.10 后，默认的 offset 存储位置改为了 Kafka，实际上就是 Kafka 的一个 topic），Kafka 消费者会周期性地（默认为 5s）去修改偏移量。这种方式接收的数据都保存在 Receiver 中，一旦出现意外，数据就有可能丢失，要想避免丢失的情况，就必须采用 WAL（Write Ahead Log，预写日志）机制，在数据写入内存前先进行持久化。

现在我们来试想一种情况，数据从 Kafka 取出后，进行了 WAL，在这个时候，Driver 与 Executor 因为某种原因宕机，这时最新偏移量还没来得及提交，那么在 Driver 恢复后，会从记录的偏移量继续消费数据并处理 WAL 的数据，这样一来，被 WAL 持久化的数据就会被重复计算一次。因此，开启了 WAL 后，这样的容错机制最多只能实现“至少一次”的消息送达语义。而且开启 WAL 后，增加了 I/O 开销，降低了 Spark Streaming 的吞吐量，还会产生冗余存储。这个过程如下图所示。

![11.png](https://s0.lgstatic.com/i/image/M00/22/47/Ciqc1F7sH4OAQpACAAQQ-fsjH8c019.png)

如果业务场景对“恰好一次”的消息送达语义有着强烈的需求，那么基于 Receiver 的方式是无法满足的，基于 Spark Streaming 提供的 Direct API 形式，克服了这一缺点。Direct API 是 Spark 1.3 后增加的新特性，相比基于 Receiver 方法的“间接”，这种方式更加“直接”。

在这种方式中，消费者会定期轮询 Kafka，得到在每个 topic 中每个分区的最新偏移量，根据这个偏移量来确定每个批次的范围，这个信息会记录在 Checkpoint 中，当作业启动时，会根据这个范围用消费者 API 直接获取数据。这样的话，就相当于把 Kafka 变成了一个文件系统，而 offset 的范围就是文件地址，Direct API 用这种方式将流式数据源变成了静态数据源，再利用 Spark 本身的 DAG 容错机制，使所有计算失败的数据均可溯源，从而实现了“恰好一次”的消息送达语义。\*\*请注意，Direct API 不需要采用WAL预写日志机制，因为所有数据都相当于在 Kafka 中被持久化了，作业恢复后直接从 Kafka 读取即可，\*\*如下图所示：

![12.png](https://s0.lgstatic.com/i/image/M00/22/53/CgqCHl7sH5WAX7rOAAMNV673x94978.png)

这种方式带来的优点显而易见，不仅克服了 WAL 带来的效率缺陷，还简化了并行性，使用 Direct API，Spark Streaming 会为每个 Kafka 的分区创建对应的 RDD 分区，这样就不需要使用 ssc.union() 方法来进行合并了，这也便于理解和调优。另外，这样的架构还保证了**输入-处理阶段**的“恰好一次”的消息送达语义，这就类似于消息的“回放”，虽然目前 Kafka 本身不支持消息回放，但用这种方式间接地实现了消息回放的功能。下面我们来看一个使用 Direct API 的完整例子：

```
import org.apache.spark.SparkConf
import org.apache.spark.streaming.StreamingContext
import org.apache.spark.streaming.Seconds
import org.apache.spark.streaming.kafka010.LocationStrategies
import org.apache.spark.streaming.kafka010.KafkaUtils
import org.apache.spark.streaming.kafka010.ConsumerStrategies
import org.apache.spark.streaming.kafka010.HasOffsetRanges
import org.apache.spark.streaming.kafka010.CanCommitOffsets
import org.apache.spark.sql.SparkSession
object SparkStreamingKafkaDirexct {
def main(args: Array[String]) {
val spark = SparkSession
    .builder
    .master("local[2]")
    .appName("SparkStreamingKafkaDirexct")
    .getOrCreate()
val sc = spark.sparkContext
val ssc = new StreamingContext(sc, batchDuration = Seconds(2))
val topics = args(2)
val topicsSet: Set[String] = topics.split(",").toSet
val kafkaParams: Map[String, Object] = Map[String, String](
"metadata.broker.list" -> "kafka01:9092,kafka02:9092,kafka03:9092",
"group.id" -> "apple_sample",
"serializer.class" -> "kafka.serializer.StringEncoder", 
"auto.offset.reset" -> "latest"
    ) 
val messages = KafkaUtils.createDirectStream[String, String](
        ssc,
LocationStrategies.PreferConsistent,
ConsumerStrategies.Subscribe[String, String](topicsSet, kafkaParams)
     )
    messages.map(...).foreachRDD(mess => { 
val offsetsList = mess.asInstanceOf[HasOffsetRanges].offsetRanges
         asInstanceOf[CanCommitOffsets].commitAsync(offsetsList)
       }
    )
    ssc.start()
    ssc.awaitTermination()
  }
}
```

从上面这段代码中我们可以发现，首先关闭了自动提交偏移量，改由手动维护。然后再从最新的偏移量开始生成 RDD，经过各种转换算子处理后输出结果，最后用 commitAsync 异步向 Kafka 提交最新的偏移量。一旦使用了 Direct API，用户需要追踪到结果数据输出完成后，再提交偏移量的改动，否则会造成不确定的影响。使用这种方式，无法在事务层面保证**处理-输出这个阶段**做到“恰好一次”，因此只能采用输出幂等的方式来达到同样的效果。

如果想要在事务的层面，让**处理-输出这个阶段**做到“恰好一次”，那么可以将 Kafka 的偏移量与最终结果存储在同一个数据库实例上，这就需要修改代码，一开始，需要从外部数据库上获取最新的偏移量：

```
...
val fromOffsets: Map[TopicPartition, Long] = setFromOffsets(offsetList)
...
val messages = KafkaUtils.createDirectStream[String, String](ssc,
LocationStrategies.PreferConsistent,
ConsumerStrategies.Subscribe[String, String](topicsSet, kafkaParams, fromOffsets))
```

在最后输出的操作里，由于偏移量与最终数据处理结果要保存到同一个数据库，因此可以利用外部数据库的事务特性，完成最后的工作：

```
messages.map(…).foreachRDD(mess => {
val offsetsList = mess.asInstanceOf[HasOffsetRanges].offsetRanges
}
)
```

这样一来，Spark Streaming 才算是真正实现了端到端的消息送达保证。

**在实际开发中，将偏移量和输出结果存储到同一个外部数据库的方式用得并不多，因为这会使业务数据与消息数据耦合在一起，结构不够优雅，反而幂等输出更加流行。**

最后，来看看 Spark Streaming 的输出操作。Spark Streaming 也是懒加载模式，同样需要类似于 RDD 的行动算子才能真正开始运行，在 Spark Streaming 中，我们称其为输出算子，一共有下面这几种。

-   print()：打印 DStream 中每个批次的前十个元素。
    
-   saveAsTextFiles(prefix, \[suffix\])：将 DStream 中的内容保存为文本文件。
    
-   saveAsObjectFiles(prefix, \[suffix\])：将 DStream 中的内容保存为 Java 序列化对象的 SequenceFile。
    
-   saveAsHadoopFiles(prefix, \[suffix\])：将 DStream 中的内容保存为 Hadoop 序列化格式（Writable）的文件，可以指定 K、V 类型。
    
-   foreachRDD(func)：该算子是 Spark Streaming 独有的，与 transform 算子类似，都是直接可以操作 RDD，我们可以利用该算子来做一些处理工作，例如生成 Parquet 文件写入 HDFS、将数据插入到外部数据库中，如 HBase、Elasticsearch。
    

### 容错与结果正确性

介绍了 Spark Streaming 的架构、用法之后，在本课时中，将会讨论 Spark Streaming 的容错机制，以及结果的正确性保证。要想 Spark Streaming 应用能够全天候无间断地运行，需要利用 Spark 自带的 Checkpoint 容错机制。Checkpoint 会在 Spark Streaming 运行过程中，周期性地保存一些作业相关信息，这样才能让 Spark Streaming 作业从故障（例如系统故障、JVM 崩溃等）中恢复。**值得注意的是，作为 Checkpoint 的存储系统，是必须保证高可用的，常见的如 HDFS 就很可靠，更优的选择则是 Alluxio。**

Checkpoint 主要保存了以下两类信息，其中元数据检查点主要用来恢复 Driver 程序，数据检查点主要用来恢复 Executor 程序。下面我们来分别介绍一下这两类信息：

1.  元数据检查点。元数据主要包括。
    

-   配置：创建该 Spark Streaming 应用的配置。
    
-   DStream 算子：Spark Streaming 作业中定义的算子。
    
-   未完成的批次：那些还在作业队列里未完成的批次。
    

Checkpoint 会周期性地将这些信息保存至外部可靠存储（如 HDFS、Alluxio）。

2.  数据检查点。
    

将中间生成的 RDD 保存到可靠的外部存储中。我们在上一节中讨论过，如果要使用状态管理的算子，如 updateStateByKey、mapWithState 等，就必须开启 Checkpoint 机制，因为这类算子必须保存中间结果，以供下次计算使用。另外，我们知道 Spark 本身的容错机制是依靠 RDD DAG 的依赖关系通过计算恢复的，但是这也会造成依赖链过长、恢复时间过长的问题，因此我们必须周期性地存储中间结果（状态）至可靠的外部存储来缩短依赖链。我们也可以手动调用 DStream 的 checkpoint 算子进行缓存。如下：

```
val ds: DStream[Int] = ...
val cds: DStream[Int] = ds.checkpoint(Seconds(5))
```

Checkpoint 机制会按照设置的间隔对 DStream 进行持久化。如果需要启用 Checkpoint 机制，需要对代码做如下改动：

```
val checkpointDirectory = "/your_cp_path" 
def functionToCreateContext(): StreamingContext = {
val conf = new SparkConf().setMaster("local[*]").setAppName("Checkpoint")
val ssc = new StreamingContext(conf,Seconds(1))
val lines = ssc.socketTextStream("localhost",9999) 
      ssc.checkpoint(checkpointDirectory)
      ssc
}
val context = StreamingContext.getOrCreate(checkpointDirectory, 
 functionToCreateContext _)
```

StreamingContext 需要以 getOrCreate 的方式初始化。这样就能保证，如果从故障中恢复，会获取到上一个检查点的信息。

如果数据源是文件，那么上面的方法可以保证完全不丢数据，因为所有的状态都可以根据持久化的数据源复现出来，但如果是流式数据源，想要保证不丢数据是很困难的。因为当 Driver 出故障的时候，有可能接收的数据会丢失，并且不能找回。为了解决这个问题，Spark 1.2 之后引入了预写日志（WAL），Spark Streaming WAL 指的是接收到数据后，在数据处理之前，先对数据进行持久化，完成这个工作的是 BlockManagerBasedBlockHandler 类的实现类WriteAheadLog BasedBlockHandler 。如果开启了 WAL，那么数据会先进行持久化再写到 Executor 的内存中。这样即使内存数据丢失了，在 Driver 恢复后，丢失的数据还是会被处理，这就实现了“至少一次”的消息送达语义。打开 WAL 的方式为：设置spark.streaming.receiver.writeAheadLog.enable 为 true。这种方法其实是对 Receiver 的容错。

那么 Checkpoint 就以这种形式完成了 Driver、Executor 和 Receiver 的容错。下面我们来讨论一下 Spark Streaming 计算结果的正确性。

先来看看消息送达保证，Spark Streaming 框架本身实现“恰好一次”的消息送达语义比较容易，因为 Spark Streaming 本质上还是进行的批处理，所以它只需在批的层面通过 BatchId 追踪数据处理情况，这和 Spark 是完全一致的，因此它完全能够保证一个批只被处理一次，当一个批没有被成功处理时，肯定就是发生了故障，这时 Checkpoint 机制能够保证从最近持久化的中间结果与待执行的计算任务（DStreamGraph）开始重新计算，保证数据只被处理一次，从而得到正确的结果，该阶段可以认为是**处理阶段的消息送达保证**。

**在流处理场景下，容错问题与结果正确性问题不能孤立地来看待，而是需要考虑在出现故障的情况下如何能够保证结果的正确性。**

### 小结

在本课时中，将流式处理流程抽象为输入-处理-输出，而基于这个流程，又将流程拆分为三个部分：

-   输入-处理
    
-   处理
    
-   处理-输出
    

**而这每个部分，都需要假设错误会经常发生的情况下，还要保证“恰好一次”的消息送达保证，才是真正的端到端的消息送达保证，这也是生产环境中必须考虑的问题。** 本课时从三个部分出发，给出了 Spark Streaming 的答案，其中处理-输出这个过程，通常会以幂等的方式解决，这也是在生产环境中非常常用的做法。

最后给你留一个思考题：

用偏移量与最终数据处理结果保存到同一个数据库，这么做的缺点是什么？
