---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


Spark Streaming 是 Spark 0.7 推出的流处理库，代表 Spark 正式进入流处理领域，距今已有快 6 年的时间。在这段时间中，随着 Spark 不断完善，Spark Streaming 在业界已得到广泛应用，应该算是目前最主要的流处理解决方案之一。随着 Spark 2.2 的 Structured Streaming 正式推出，Spark 下一代流处理技术已经呼之欲出。但是由于目前的客观情况，Structured Streaming 的成熟度还不太高，大量的流处理应用不可能也没有必要马上迁移至 Structured Streaming，所以 Spark Streaming 在今后一段时间还将继续活跃。另外，它的架构和它的抽象也值得我们学习和深思。在本节我们将会学习 Spark Streaming，主要内容有：

-   Spark Streaming 关键抽象与架构；
    
-   转换算子。
    

### 关键抽象与架构

要想深入理解 Spark Streaming，首先还是要了解 Spark Streaming 的关键抽象 DStream，DStream 意指 Discretized Stream（离散化流），**它大体上来说是一个 RDD 流（序列），其元素（RDD）可以理解为从输入流生成的批**。在流处理的过程中，用户其实就是在对 DStream 进行各种变换，最后再输出。如下图所示，可以看出 Spark Streaming 的输入是连续的，经过 Spark Streaming 接收后，会变成一个 RDD 序列，之后的处理逻辑是基于 DStream 来操作的。

![image](https://s0.lgstatic.com/i/image/M00/20/79/Ciqc1F7ohKSAGECBAADMWzmUmoQ981.png)

DStream 的生成依据是按照时间间隔切分，该时间间隔的数据会生成一个微批（mini-batch），即一个 RDD，所以该间隔也被称为批次间隔，DStream 里面持有对所有产生的 RDD 的引用，虽然 RDD 和 DStream 非常像，在种类上基本都是一一对应的，如 UnionDStream 与 UnionRDD，但是 DStream 还是和 RDD 有本质不同，如下图所示。

![image](https://s0.lgstatic.com/i/image/M00/20/84/CgqCHl7ohLWAZQ9hAACfMLRTSwM614.png)

具体表现为：

```
RDD = DStream @ batch T
DStream = RDD range (tn-1,tn)
```

RDD 是 DStream 中某个批次的数据，而 DStream 代表了一段时间所产生的 RDD。所以通过这种方式，Spark Streaming 把对连续流的处理，变成了对批序列 DStream 的处理。我们会在 StreamingContext 设置批次间隔大小，一般大于 200ms。

如果我们仔细思考的话，会发现，这种抽象将连续的流看成了某一时刻静止的批，所以在提到 Spark Streaming 中的 RDD 时，一定要注意它还有个时间维度，最准确的说法是某段时间内的 RDD。

**最后，可以看到 Spark Streaming 对流的抽象本质上还是流，只是处理是基于批来处理的。这与 Structured Streaming 来说是不同的，我们在后面会讲到。**

Spark Streaming 在架构上与 Spark 离线计算架构非常相似，主要分为 Driver 与 Executor，同样可以运行在 Yarn、Mesos 上，也能够以 standalone 和 local 模式运行。它们之间的关系仍旧是 Driver 负责调度，Executor 执行任务。如下图所示。

![image](https://s0.lgstatic.com/i/image/M00/20/84/CgqCHl7ohNmAEpWIAAHuZ2LI2To588.png)

在 Driver 中，有几个关键模块，SparkStreamingContext 、DStreamGraph、JobScheduler、Checkpoint、ReceiverTracker，下面我将就这几个模块分别介绍。

-   SparkStreamingContext：SparkStreamingContext是一开始在用户代码中初始化完成的。它主要的工作是对作业进行一些配置，例如 DStream 切分的批次间隔（Duration），以及与其他模块进行交互，如 DStreamGraph 和 JobScheduler 等。
    
-   DStreamGraph：既然 Spark Streaming 最后还是对批的处理，那么批处理中根据计算逻辑生成的 RDD DAG 也是存在的，它由 DStreamGraph 生成。DStreamGraph 维护了输入 DStream 与输出 DStream 的实例，还会通过 generateJobs 方法生成一个作业集合（RDD DAG），它会由 JobScheduler 调度启动执行任务。
    
-   JobScheduler：JobScheduler 顾名思义是 Spark Streaming 的作业调度器，在创建 SparkStreamingContext 的同时，JobScheduler 也会作为它的一部分被创建，所有的任务都是最后由它调度 Executor 来执行。
    
-   Checkpoint：在 Spark 中，任何一个 RDD 丢失，都可以通过依赖关系重新计算得到， Checkpoint 是 Spark Streaming 容错机制的核心，会定时对已算好的中间结果以及其他中间状态进行存储，避免了依赖链过长的问题。这样就算某个 DStream 丢失了，也不用从头开始计算，只需从最近的依赖关系开始计算即可。
    
-   ReceiverTracker：ReceiverTracker 通过 Executor 上的 ReceiverSupvisor 来管理所有的 Receiver。主要功能是把需要计算的数据发送给 Executor。当 Executor 接收完毕后，也会将数据块的元数据上报给 ReceiverTracker。
    

从上面这几个组件的关系上来说，SparkStreamingContext 负责与其他组件交互，DStreamGraph 与 JobScheduler 负责调度，Checkpoint 负责容错，ReceiverTracker 负责与 Executor 进行数据交互。

Executor 是具体的任务执行者，其中重要的组件有 Receiver、ReceiverSupvisor、ReceiveredBlockHandler，ReceiverTracker 会和 Executor 通信，启动 ReceiverSupvisor 实例，ReceiverSupvisor 会马上启动 Receiver 开始接收数据。Receiver 接收到数据后，用 ReceiverdBlockHandler 以块的方式写到 Executor 的磁盘或者内存，对应的实现是 BlockManagerBasedBlockHandler 和 WriteAheadLogBasedBlockHandler，前者是根据 Executor 的 StorageLevel 写到相应的存储层，后者会先进行预写日志（Write Ahead Log），其中，后者能对流式数据源提供更好的容错性。数据接收完毕后，会根据调度开始计算任务。

Spark Streaming 的作业初始化与提交和 Spark SQL 作业有些不同，我们还是通过初始化 SparkSession 的方式得到 StreamingContext 的引用，再对其设置一个关键参数：批次间隔后，就可以进行数据接收和数据处理的动作。

### 无状态的转换算子

基于上面的抽象，对流进行处理与批处理就没什么不同了，我们只着眼于此刻正在处理的这个时间范围内的 RDD，所以数据处理方式与批处理并没有什么不同，算子也与批处理没多大区别，算子作用与数据流中的每个 RDD，这类算子我们称之为无状态算子，如下图所示：

![image](https://s0.lgstatic.com/i/image/M00/20/79/Ciqc1F7ohOaAN6pwAADlQFH86M8061.png)

**我们在使用无状态算子时，仍然要注意，每次处理的结果都隐含着“这是...时间范围内数据的处理结果”的含义。**

这类算子与前面介绍的转换算子没什么不同，如 map、mapPartitions、reduceByKey、reduce、flatmap、glom、filter、repartition、union 等等，这里就不重复描述了。

### 有状态的转换算子

在实际工作场景中，默认的时间间隔很难满足流处理的业务需要，比如想对 DStream 中的某几个 RDD 进行操作，或者是想保存一些中间结果做增量计算，就需要运用到另一类转换算子：有状态的转换算子。

有状态的转换算子主要分为两种，一种是基于时间窗口，另一种是基于整个时间跨度。本课时将对其进行介绍。

基于时间窗口的概念其实早就深植于 Spark Streaming 中，我们在设置批次间隔时间（如 1 s）时，本质上就是设置了一个时间窗口，在用户代码中的计算逻辑其实是作用在每一个在该批次间隔中形成的 RDD 上，上一个 RDD 和这一个 RDD 的计算结果不会互相影响。当我们需要对若干批次的数据处理结果进行聚合的时候，就需要设置一个更大的时间窗口，如下图所示。

![image](https://s0.lgstatic.com/i/image/M00/20/84/CgqCHl7ohPCAS2oAAADX2hrAMOk915.png)

时间窗口是由批次间隔组成的有限时间跨度，基于窗口的操作对窗口中所有数据进行处理。此外，窗口是一个逻辑的概念，它可以进行滑动，图中所示的窗口跨度为 3，滑动步长为 2，滑动意味着每隔多少时间，窗口会被触发一次，每批次数据与窗口的对应关系为一对多，意味着某个批次的数据可以存在于多个窗口中。这里注意窗口间隔与滑动步长都必须是 DStream 批次间隔的整数倍。

基于窗口的转换算子主要有 slice、window、countByWindow、reduceByWindow、reduceByKeyAndWindow、countByValueAndWindow 等。

#### slice 算子

-   def slice(interval: Interval): Seq\[RDD\[T\]\] 和 def slice(fromTime: Time, toTime: Time): Seq\[RDD\[T\]\]：slice 算子返回该时间跨度内的 RDD 集合，批次间隔可以用 Interval 进行定义，也可以用起始时间与结束时间来定义，注意，开始时间与结束时间需要是批次间隔的倍数，否则系统会自动进行取整。该算子相当于在整个 DStream 流中截取了一段。
    

#### window 算子

-   window(windowDuration: Duration): DStream\[T\] 和 window(windowDuration: Duration, slideDuration: Duration): DStream\[T\]：window 算子定义了窗口的属性，如跨度（windowDuration）和滑动步长（slideDuration），并返回一个新的 DStream，默认的滑动步长为批次间隔。当我们通过 window 算子定义了滑动窗口以后，可以用使用 **join 算子**进行连接操作，例：
    

```
import org.apache.spark.streaming.StreamingContext
import org.apache.spark.streaming.Seconds
import org.apache.spark.streaming.dstream.ConstantInputDStream
import org.apache.spark.sql.SparkSession
import org.apache.spark.streaming.dstream.DStream.toPairDStreamFunctions
object SparkStreamingJoin {
def main(args: Array[String]): Unit = {
val spark = SparkSession
     .builder
     .master("local[2]")
     .appName("SparkStreamingJoin")
     .getOrCreate()
val sc = spark.sparkContext
val ssc = new StreamingContext(sc, batchDuration = Seconds(2))
val leftData = sc.parallelize(0 to 3)
val leftStream = new ConstantInputDStream(ssc, leftData)
val rightData = sc.parallelize(0 to 2)
val rightStream = new ConstantInputDStream(ssc, rightData)
val rightWindow = rightStream.map(f => (f,f)).window(Seconds(2),Seconds(4))
val leftWindow = leftStream.map(f => (f,f)).window(Seconds(6),Seconds(4))
     leftWindow.join(rightWindow).print()
     ssc.start()
     ssc.awaitTermination()
 }
}
```

这里的 join 要求两个窗口的滑动步长必须一致。

**window 算子很重要的用法是与无状态算子配合使用，使其结果满足需要的时间跨度限制。**

#### reduceByWindow 算子

●     def reduceByWindow(reduceFunc: (T, T) => T,windowDuration: Duration,slideDuration: Duration): DStream\[T\]和def reduceByWindow(reduceFunc: (T, T) => T,invReduceFunc: (T, T) => T,windowDuration: Duration,slideDuration: Duration): DStream\[T\]：按照 reduceFunc 的逻辑对滑动窗口中的数据进行聚合。

后一个 reduceByWindow 是前一个的重载版本，不同之处在于增加了反函数（invReduceFunc）作为参数。反函数存在的作用在于优化那些增量计算的逻辑，如下图所示，假设 reduceFunc 的作用是对窗口内数据进行累计求和，那么在没有 invReduceFunc 的情况下，计算逻辑是 DStream 每个批次的 RDD 先按照 reduceFunc 的逻辑做一次 reduce，然后在达到窗口触发条件时再做一次同样逻辑的 reduce 操作，但是我们可以发现两个窗口互相重叠的时间区间的数据（此处为 RDD@time3）在之前的窗口已经聚合过了，是没有必要再重新计算的，而反函数版本的 reduceByWindow 则针对此处做了优化，反函数的根本作用在于求重复计算部分的值（此处为 RDD@time3）。

![image](https://s0.lgstatic.com/i/image/M00/20/85/CgqCHl7ohVWARE9kAADX2hrAMOk450.png)

反函数的参数 (a, b) 所代表的含义为：a 为之前的时间窗口（此处为 window@time3）聚合的结果，b 为之前的时间窗口与当前时间窗口没有重叠部分的聚合结果（此处为 RDD@time1 与 RDD@time2），那么按照反函数的作用，正确反函数应该为 (a,b) => a - b，Spark Streaming 最后再将反函数的计算结果（RDD@time3）与当前时间窗口剩余的数据（此处为 RDD@time4 与 RDD@time5）进行聚合，得到当前窗口的聚合结果（此处为 window@time5）。不难发现，在时间窗口本身跨度很大，且两个时间窗口互相重叠的部分也很大时，反函数版本的 reduceByWindow在计算时性能会大大优于普通版本的 reduceByWindow。

#### reduceByKeyAndWindow算子

-   def reduceByKeyAndWindow(reduceFunc: (V, V) => V, windowDuration: Duration): DStream\[(K, V)\]、def reduceByKeyAndWindow(reduceFunc: (V, V) => V, windowDuration: Duration,slideDuration: Duration,partitioner: Partitioner): DStream\[(K, V)\]和def reduceByKeyAndWindow(reduceFunc: (V, V) => V, invReduceFunc: (V, V) => V,windowDuration: Duration,slideDuration: Duration = self.slideDuration,numPartitions: Int = ssc.sc.defaultParallelism,filterFunc: ((K, V)) => Boolean = null): DStream\[(K, V)\]：通过 reduceFunc 的化简逻辑，reduceByKey 的算子会根据 K 对窗口的数据进行分组聚合，返回化简结果。同时，还可以指定 reduce 任务的个数与 Shuffle 的逻辑。另外 reduceByBeyAndWindow 也有反函数的版本。
    

#### countByWindow 算子

-   def countByWindow(windowDuration: Duration, slideDuration: Duration): DStream\[Long\]：countByWindow 算子计算滑动窗口中数据的数量。
    

下面我们来看看该算子的实现：

```
def countByWindow(windowDuration: Duration,slideDuration: Duration): DStream[Long] = ssc.withScope {
this.map(_ => 1L).reduceByWindow(_ + _, _ - _, windowDuration, slideDuration)
}
```

可以看到 countByWindow 先将每行数据转换成 1，最后再用 reduceByWindow 进行累计求和，它默认就采取了反函数的版本，这也是官方推荐的。

#### countByValueAndWindow 算子

-   def countByValueAndWindow(windowDuration: Duration, slideDuration: Duration, numPartitions: Int = ssc.sc.defaultParallelism)(implicit ord: Ordering\[T\] = null): DStream\[(T, Long)\]：对每个滑动窗口的数据执行 countByValue 的操作。底层实现也是调用了 reduceByKeyAndWindow 的反函数版本。
    

介绍了基于窗口的转换算子，我们发现基于窗口的转换操作还是有其局限性，当我们想要对某个键的状态进行整个时间段追踪时，基于窗口就不是那么方便了。

所以我们还需要另外一种有状态的转换操作：mapWithState 与 updateStateByKey，mapWithState 是 Spark 1.6 以后的新特性，官方宣称性能是 updateStateByKey 的十倍，可以认为是 updateStateByKey 的升级版。这两种算子类似于定义一个全局累加器，每个批次的数据处理结果都会将其更新，这样就能得到整个时间段下该 key 的状态值（中间结果）。以 wordcount 为例：

```
import org.apache.spark.SparkConf
import org.apache.spark.streaming._
import org.apache.spark.streaming.dstream.DStream.toPairDStreamFunctions
import org.apache.spark.sql.SparkSession
object StatefulNetworkWordCount {
def main(args: Array[String]) {
val spark = SparkSession
     .builder
     .master("local[2]")
     .appName("StatefulNetworkWordCount")
     .getOrCreate()
val sc = spark.sparkContext
val ssc = new StreamingContext(sc, Seconds(1))
    ssc.checkpoint(".")
val initialRDD = ssc.sparkContext.parallelize(List(("hello", 1), ("world", 1)))
val lines = ssc.socketTextStream(args(0), args(1).toInt)
val words = lines.flatMap(_.split(" "))
val wordDstream = words.map(x => (x, 1))
val mappingFunc = (word: String, one: Option[Int], state: State[Int]) => {
val sum = one.getOrElse(0) + state.getOption.getOrElse(0)
val output = (word, sum)
      state.update(sum)
      output
    }
val stateDstream = wordDstream.mapWithState(
StateSpec.function(mappingFunc).initialState(initialRDD))
    stateDstream.print()
    ssc.start()
    ssc.awaitTermination()
  }
}
```

例子中的核心就是 mappingFunc 的函数，定义了状态更新的逻辑。updateStateByKey 的用法大同小异，也是通过定义状态更新函数来体现状态的变化。用法如下：

```
……
val updateFunc = (values: Seq[Int], state: Option[Int]) => {
val currentCount = values.sum
val previousCount = state.getOrElse(0)
Some(currentCount + previousCount)
}
val newUpdateFunc = (iterator: Iterator[(String, Seq[Int], Option[Int])]) => {
iterator.flatMap(t => updateFunc(t._2, t._3).map(s => (t._1, s)))
}
val stateDstream = wordDstream.updateStateByKey[Int](newUpdateFunc,  new HashPartitioner(ssc.sparkContext.defaultParallelism), true, initialRDD)
```

这两种实现同样功能的算子在性能上差异巨大的原因在于，updateStateByKey 的原理是将上次计算结果与新批次数据采用 cogroup 操作再进行聚合，而 mapWithState 则是通过维护一个中间状态表，存储上一次计算的结果与当前批次的计算结果，所以直接进行聚合处理即可。

### 小结

本课时介绍了 Spark Streaming 的关键抽象与架构，Spark Streaming 沿用了 RDD 原有的抽象，将流处理变成了连续的微批处理。如果把原有的数据处理看成是一维的，那么流处理无疑是二维的：它加入了一个很重要的时间维度，并且处理的需求往往与时间维度紧密相关，这就使得虽然 Spark Streaming 仍然使用了 RDD 的抽象，但转换算子分为了有状态和无状态之分。

最后给你留一个思考题：

如何每 2 分钟统计一次最近 5 分钟出现过的每个单词的数量？
