---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


你好，我是范东来，本课时我们要讲解的内容是：用户自定义函数。在上个课时，你了解了 DataFrame、Dataset 和 Spark SQL，如果说 Spark 隐藏了分布式计算的复杂性，那么可以认为 DataFrame、Dataset 和 Spark SQL 比它要更近一步，用统一而简洁的接口隐藏了数据分析的复杂性。

在本课时，我们会主要介绍在上个课时中没有详细讲解的函数与自定义函数。在实际使用中，函数和自定义函数的使用频率非常高，可以说，对于复杂的需求，如果用好了函数，那么事情会简单许多，反之，则会事倍功半。

本课时的主要内容有：

-   窗口函数
-   函数
-   用户自定义函数

### 窗口函数

首先，我们来看下窗口函数，窗口函数可以使用户针对某个范围的数据进行聚合操作，如：

-   累积和
-   差值
-   加权移动平均

可以想象一个窗口在全量数据集上进行滑动，用户可以自定义在窗口中的操作，如下图所示。

![1.png](https://s0.lgstatic.com/i/image/M00/12/13/Ciqc1F7M72uAIFTtAABmsVbo0mQ737.png)

使用窗口函数，首先需要定义窗口，DataFrame 提供了 API 定义窗口，以及窗口中的计算逻辑，还是以学生成绩为例，现在需要得出每个学生单科最佳成绩以及成绩所在的年份，这个需求就要用到窗口中的 row\_number 函数，row\_number 函数可以根据窗口中的数据生成行号，首先来定义窗口：

```
import org.apache.spark.sql.expressions.Window
import org.apache.spark.sql.functions._
val window = Window
.partitionBy("name","subject")
.orderBy(desc("grade"))
```

上面的代码定义了窗口的范围：按照每个人的姓名与科目的组合进行开窗，并控制了数据在窗口中的顺序：按照 grade 降序进行排序，row\_number 函数就可以作用在这个窗口上，对每个人每个科目成绩赋予行号，代码如下：

```
dfSG.select(
col("name"),
col("subject"),
col("year"),
col("grade"),
row_number().over(window)
.show()
```

结果如下：

![image (3).png](https://s0.lgstatic.com/i/image/M00/12/13/Ciqc1F7M75KAeDK1AADO_U0qaqQ227.png)

最后只需要从这张表中过滤出 row\_number 等于 1 的数据即可。

此外，DataFrame 还提供了 rowsBetween 和 rangeBetween 来进一步定义窗口范围，其中 rowsBetween 是通过物理行号进行控制，rangeBetween 是通过逻辑条件来对窗口进行控制，来看一个简单的例子，一份两个字段的样例数据：

```
{"key":"1", "num":2}
{"key":"1", "num":2}
{"key":"1", "num":3}
{"key":"1", "num":4}
{"key":"1", "num":5}
{"key":"1", "num":6}
{"key":"2", "num":2}
{"key":"2", "num":2}
{"key":"2", "num":3}
{"key":"2", "num":4}
{"key":"2", "num":5}
{"key":"2", "num":6}
```

现在通过窗口函数对相同 key 的 num 字段做累加计算。代码如下：

```
val windowSlide = Window
.partitionBy("key")
.orderBy("num")
.rangeBetween(Window.currentRow + 2,Window.currentRow + 20)
val dfWin = spark.read.json("json/window.json")
dfWin
.select(col("key"),sum("num").over(windowSlide))
.sort("key")
.show()
```

在 rangeBetween 中，定义的窗口是当前行的 num 值 +2 到当前行的 num 值 +20 这个区间中的数据，如下所示：

```
{"key":"1", "num":2}       窗口为[4,22]           累加和为4 + 5 + 6 = 15
{"key":"1", "num":2}       窗口为[4,22]           累加和为4 + 5 + 6 = 15
{"key":"1", "num":3}       窗口为[5,23]           累加和为5 + 6 = 11
{"key":"1", "num":4}       窗口为[6,24]           累加和为6
{"key":"1", "num":5}       窗口为[8,25]           累加和为null
{"key":"1", "num":6}       窗口为[8,26]           累加和为null
{"key":"2", "num":1}       窗口为[3,21]           累加和为12
{"key":"2", "num":2}       窗口为[4,22]           累加和为12
{"key":"2", "num":5}       窗口为[7,25]           累加和为7
{"key":"2", "num":7}       窗口为[9,27]           累加和为null
```

rangeBetween 通过字段的值定义了参与计算的逻辑窗口大小，也可以使用 rowsBetween 通过行号来指定参与计算的物理窗口，如下所示：

```
val windowSlide = Window
.partitionBy("key")
.orderBy("num")
.rowsBetween(Window.currentRow - 1,Window.currentRow + 1)
dfWin
.select(col("key"),sum("num").over(windowSlide))
.sort("key")
.show()
```

代码中定义的窗口由当前行、当前行的前一行、当前行的后一行组成，也就是说窗口大小为 3，计算结果如下：

```
{"key":"1", "num":2}              累加和为2 + 2 = 4
{"key":"1", "num":2}              累加和为2 + 2 + 3 = 7
{"key":"1", "num":3}              累加和为2 + 3 + 4 = 9
{"key":"1", "num":4}              累加和为3 + 4 + 5 = 12
{"key":"1", "num":5}              累加和为4 + 5 + 6 = 15
{"key":"1", "num":6}              累加和为5 + 6 = 11
{"key":"2", "num":1}              累加和为1 + 2 = 3
{"key":"2", "num":2}              累加和为1 + 2 + 5 = 8
{"key":"2", "num":5}              累加和为2 + 5 + 7 = 14
{"key":"2", "num":7}              累加和为5 + 7 = 12
```

### 函数

在需要对数据进行分析的时候，我们经常会使用到函数，Spark SQL 提供了丰富的函数供用户选择，基本涵盖了大部分的日常使用。下面介绍一些常用函数：

#### 1\. 转换函数

cast(value AS type) → type

它显式转换一个值的类型。可以将字符串类型的值转为数字类型，反过来转换也可以，在转换失败的时候，会返回 null。这个函数非常常用。

#### 2\. 数学函数

log(double base, [Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) a)

求与以 base 为底的 a 的对数。

factorial([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e)

返回 e 的阶乘。

#### 3\. 字符串函数

split([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) str,String pattern)

根据正则表达式 pattern 匹配结果作为依据来切分字符串 str。

substring([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) str,int pos,int len)

返回字符串 str 中，起始位置为 pos，长度为 len 的字符串。

concat([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html)... exprs)

连接多个字符串列，形成一个单独的字符串。

translate([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) src,String matchingString,String replaceString)

在字符串 src 中，用 replaceString 替换 mathchingString。

字符串函数也是非常常用的函数类型。

#### 4\. 二进制函数

bin(Column e)

返回输入内容 e 的二进制值。

base64([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e)

计算二进制列e的 base64 编码，并以字符串返回。

#### 5\. 日期时间函数

current\_date()

获取当前日期

current\_timestamp()

获取当前时间戳

date\_format([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) dateExpr,String format)

将日期/时间戳/字符串形式的时间列，按 format 指定的格式表示，并以字符串返回。

#### 6\. 正则表达式函数

regexp\_extract([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e,String exp,int groupIdx)

首先在 e 中匹配正则表达式 exp，按照 groupIdx 的值返回结果，groupIdx 默认值为 1，返回第 1 个匹配成功的内容，0 表示返回全部匹配成功的内容。

regexp\_replace([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e,String pattern,String replacement)

用 replacement 替换在 e 中根据 pattern 匹配成功的字符串。

#### 7\. JSON 函数

get\_json\_object([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e,String path)

解析 JSON 字符串 e，返回 path 指定的值。

#### 8\. URL 函数

parse\_url(string urlString, string partToExtract \[, stringkeyToExtract\])

该函数专门用来解析 URL，提取其中的信息，partToExtract 的选项包含 HOST、PATH、QUERY、REF、PROTOCOL、AUTHORITY、USEINFO，函数会根据选项提取出相应的信息。

#### 9\. 聚合函数

countDistinct([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) expr,[Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html)... exprs)

返回一列数据或一组数据中不重复项的个数。expr 为返回 column 的表达式。

avg([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e)

返回 e 列的平均数。

count([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e)

返回 e 列的行数。

max([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e)

返回 e 中的最大值

sum([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e)

返回 e 中所有数据之和

skewness([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e)

返回 e 列的偏度。

stddev\_samp([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e)

stddev([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e)

返回 e 的样本标准差。

var\_samp([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e)

variance([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e)

返回 e 的样本方差。

var\_pop([Column](http://spark.apache.org/docs/2.0.2/api/java/org/apache/spark/sql/Column.html) e)

返回 e 的总体方差。

这类函数顾名思义，作用于很多行，所以往往与统计分析相关。

#### 10\. 窗口函数

row\_number()

对窗口中的数据依次赋予行号。

rank()

与 row\_number 函数类似，也是对窗口中的数据依次赋予行号，但是 rank 函数考虑到了 over 子句中排序字段值相同的情况，如下表所示。

![2.png](https://s0.lgstatic.com/i/image/M00/12/20/CgqCHl7M8PuAZAR6AABqraAQ7HE674.png)  
dense\_rank()

与 row\_number 函数类似，也是对窗口中的数据依次赋予行号，但是 dense\_rank 函数考虑到over 子句中排序字段值相同的情况，并保证了序号连续。

ntile(n)

将每一个窗口中的数据放入 n 个桶中，用 1-n 的数字加以区分。

**在实际开发过程中，大量的需求都可以直接通过函数以及函数的组合完成，一般来说，函数的丰富程度往往超乎你的想象，所以在面临新需求时，建议首先查阅文档，看看有没有函数可以利用，如果实在不行，我们才会使用用户自定义函数（User Defined Function）。**

**Spark SQL 的函数文档目前我没有发现特别全面的，所以我通常就会直接阅读源码，源码列出了所有的函数，如下：**

[https://github.com/apache/spark/blob/6646b3e13e46b220a33b5798ef266d8a14f3c85b/sql/core/src/main/scala/org/apache/spark/sql/functions.scala](https://github.com/apache/spark/blob/6646b3e13e46b220a33b5798ef266d8a14f3c85b/sql/core/src/main/scala/org/apache/spark/sql/functions.scala)

### 用户自定义函数

DataFrame API 支持用户自定义函数，自定义函数有两种：UDF 和 UDAF，前者是类似于 map操作的行处理，一行输入一行输出，后者是聚合处理，多行输入，一行输出，先来看看 UDF，下面的代码会开发一个根据得分显示分数等级的函数 level：

```
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.functions._
import scala.reflect.api.materializeTypeTag
object MyUDF {
def main(args: Array[String]): Unit = {
val spark = SparkSession
    .builder
    .master("local[2]")
    .appName("Test")
    .getOrCreate()
import spark.implicits._
val dfSG = spark.read.json("examples/target/scala-2.11/classes/student_grade.json")
def level(grade: Int): String = {
if(grade >= 85)
"A"
else if(grade < 85 & grade >= 75)
"B"
else if(grade < 75 & grade >= 60)
"C"
else if(grade < 60)
"D"  
else
"ERROR"
    }
val myUDF = udf(level _)
    dfSG.select(col("name"),myUDF(col("grade"))).show()
  }
}
```

接下来看看 UDAF，UDAF 是用户自定义聚合函数，分为两种：un-type UDAF 和 safe-type UDAF，前者是与 DataFrame 配合使用，后者只能用于 Dataset，UDAF 需要实现 UserDefinedAggregateFunction 抽象类，本例实现了一个求某列最大值的 UDAF，代码如下：

```
import org.apache.spark.sql.expressions._
import org.apache.spark.sql.types._
import org.apache.spark.sql.Row
import org.apache.spark.sql.functions._
import org.apache.spark.sql.SparkSession
object MyMaxUDAF extends UserDefinedAggregateFunction {
override def inputSchema: StructType 
    = StructType(Array(StructField("input", IntegerType, true)))
override def bufferSchema: StructType 
    = StructType(Array(StructField("max", IntegerType, true)))
override def dataType: DataType = IntegerType
override def deterministic: Boolean = true
override def initialize(buffer: MutableAggregationBuffer): Unit 
    = {buffer(0) = 0}
override def update(buffer: MutableAggregationBuffer, input: Row): Unit = {
val temp = input.getAs[Int](0)
val current = buffer.getAs[Int](0)
if(temp > current)
       buffer(0) = temp
  }
override def merge(buffer1: MutableAggregationBuffer, buffer2: Row): Unit = {
if(buffer1.getAs[Int](0) < buffer2.getAs[Int](0))
        buffer1(0) = buffer2.getAs[Int](0)     
  }
override def evaluate(buffer: Row): Any = buffer.getAs[Int](0)
}
object MyMaxUDAFDriver extends App{
val spark = SparkSession
    .builder
    .master("local[2]")
    .appName("Test")
    .getOrCreate()
import spark.implicits._
val dfSG = spark.read.json("examples/target/scala-2.11/classes/student_grade.json")
  dfSG.select(MyMaxUDAF(col("grade"))).show()
}
```

可以从代码看到 UDAF 的逻辑，还是类似于 MapReduce 的思想，先通过 update 函数处理每个分区，最后再通过 merge 函数汇总结果。

Dataset 的 UDAF 对应的是 safe-type UDAF，这种类型的 UDAF 只有 Dataset 能够使用，因为 Dataset 是类型安全的。使用方式和 un-type UDAF 类似，也是先要结合自己聚合的逻辑实现 Aggregator 抽象类，最后再通过 Dataset API 调用，此处实现一个求学生成绩平均值的 UDAF，代码如下：

```
import org.apache.spark.sql.Encoders
import org.apache.spark.sql.Encoder
import org.apache.spark.sql.expressions._
import org.apache.spark.sql.types._
import org.apache.spark.sql.functions._
import scala.reflect.api.materializeTypeTag
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.Dataset
case class StudentGrade(name: String, subject: String, grade: Long)
case class Average(var sum: Long, var count: Long)
object MyAvgUDAF extends Aggregator[StudentGrade,Average,Double]{
def zero: Average = Average(0L,0L)
def reduce(buffer: Average, sg: StudentGrade): Average = {
      buffer.sum += sg.grade
      buffer.count += 1
      buffer
    }
def merge(b1: Average, b2: Average): Average = {
      b1.sum += b2.sum
      b1.count += b2.count
      b1
    }
def finish(reduction: Average): Double = reduction.sum / reduction.count
def bufferEncoder: Encoder[Average] = Encoders.product
def outputEncoder: Encoder[Double] = Encoders.scalaDouble
}
通过Dataset API调用：
object MyAvgUDAFDriver extends App{
val spark = SparkSession
    .builder
    .master("local[2]")
    .appName("Test")
    .getOrCreate()
import spark.implicits._
val dfSG = spark.read.json("examples/target/scala-2.11/classes/student_grade.json")
val dsSG: Dataset[StudentGrade] = dfSG.map(a => StudentGrade(a.getAs[String](0),a.getAs[String](1),a.getAs[Long](2)))
val MyAvg = MyAvgUDAF.toColumn.name("MyAvg")
    dsSG.select(MyAvg).show()
}
```

自定义函数注册以后，同样可以在 Spark SQL 中使用。

### 小结

最后来做一个小结，这不光针对本课时，而是一个阶段性小结，现在我们学习了 RDD API、DataFrame API 和 Dataset API，对于数据处理来说，它们都能胜任，那么在实际使用中应该如何选择呢。

一般来说，在任何情况下，都不推荐使用 RDD 算子，原因如下：

-   在某种抽象层面上来说，使用 RDD 算子编程相当于直接使用汇编语言或者机器代码进行编程；
-   RDD算子与 SQL、DataFrame API 和 Dataset API 相比，更偏向于如何做，而非做什么，这样优化的空间很少；
-   RDD 语言不如 SQL 语法友好。

此外，在其他情况，应优先考虑 Dataset，因为静态类型的特点会使计算更加迅速，但用户必须使用静态语言才行，如 Java 与 Scala，像 Python 这种动态语言是没有 Dataset API 的。

下图是用户用不同语言基于 RDD API 和 DataFrame API 开发的应用性能对比，可以看到 Python + RDD API 的组合是远远落后其他组合的，此外，RDD API 开发应用的性能整体要明显落后于 DataFrame API 开发的应用性能。从开发速度和性能上来说，DataFrame + SQL 无疑是最好选择。

![3.png](https://s0.lgstatic.com/i/image/M00/12/21/CgqCHl7M8VSAOjrdAACafd9hNk4280.png)

最后，还是留一个思考题，最开始我们举了一个窗口函数的例子，后面在介绍 Spark SQL 中也学习了窗口函数，你能用 SQL 的窗口函数实现同样的逻辑吗？
