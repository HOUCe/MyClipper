---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


今天我将为你讲解：如何用共享变量在数据管道中使用中间结果。共享变量是 Spark 中进阶特性之一，一共有两种：

-   广播变量；
-   累加器。

这两种变量可以认为是在用算子定义的数据管道外的两个全局变量，供所有计算任务使用。在 Spark 作业中，用户编写的高阶函数会在集群中的 Executor 里执行，这些 Executor 可能会用到相同的变量，这些变量被复制到每个 Executor 中，而 Executor 对变量的更新不会传回 Driver。

在计算任务中支持通用的可读写变量一般是低效的，即便如此，Spark 还是提供了两类共享变量：广播变量（broadcast variable）与累加器（accumulator）。当然，对于分布式变量，如果不加限制会出现一致性的问题，所以共享变量是两种非常特殊的变量。

-   广播变量：只读；
-   累加器：只能增加。

### 广播变量

广播变量类似于 MapReduce 中的 DistributeFile，通常来说是一份不大的数据集，一旦广播变量在 Driver 中被创建，整个数据集就会在集群中进行广播，能让所有正在运行的计算任务以只读方式访问。广播变量支持一些简单的数据类型，如整型、集合类型等，也支持很多复杂数据类型，如一些自定义的数据类型。

广播变量为了保证数据被广播到所有节点，使用了很多办法。这其实是一个很重要的问题，我们不能期望 100 个或者 1000 个 Executor 去连接 Driver，并拉取数据，这会让 Driver 不堪重负。Executor 采用的是通过 HTTP 连接去拉取数据，类似于 BitTorrent 点对点传输。这样的方式更具扩展性，避免了所有 Executor 都去向 Driver 请求数据而造成 Driver 故障。

Spark 广播机制运作方式是这样的：Driver 将已序列化的数据切分成小块，然后将其存储在自己的块管理器 BlockManager 中，当 Executor 开始运行时，每个 Executor 首先从自己的内部块管理器中试图获取广播变量，如果以前广播过，那么直接使用；如果没有，Executor 就会从 Driver 或者其他可用的 Executor 去拉取数据块。一旦拿到数据块，就会放到自己的块管理器中。供自己和其他需要拉取的 Executor 使用。这就很好地防止了 Driver 单点的性能瓶颈，如下图所示。

![图片1.png](https://s0.lgstatic.com/i/image/M00/09/F1/CgqCHl69BwmAP2B7AAGvkqEmVkk327.png)

下面来看看如何在 Spark 作业中创建、使用广播变量。代码如下：

```
scala> val rdd_one = sc.parallelize(Seq(1,2,3))
rdd_one: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[101] at
parallelize at <console>:25
    scala> val i = 5
i: Int = 5
scala> val bi = sc.broadcast(i)
bi: org.apache.spark.broadcast.Broadcast[Int] = Broadcast(147)
scala> bi.value
res166: Int = 5
scala> rdd_one.take(5)
res164: Array[Int] = Array(1, 2, 3)
scala> rdd_one.map(j => j + bi.value).take(5)
res165: Array[Int] = Array(6, 7, 8)
```

在用户定义的高阶函数中，可以直接使用广播变量的引用。下面看一个集合类型的广播变量：

```
scala> val rdd_one = sc.parallelize(Seq(1,2,3))
    rdd_one: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[109] at
parallelize at <console>:25
scala> val m = scala.collection.mutable.HashMap(1 -> 2, 2 -> 3, 3 -> 4)
    m: scala.collection.mutable.HashMap[Int,Int] = Map(2 -> 3, 1 -> 2, 3 -> 4)
scala> val bm = sc.broadcast(m)
bm:
org.apache.spark.broadcast.Broadcast[scala.collection.mutable.HashMap[Int,I
nt]] = Broadcast(178)
scala> rdd_one.map(j => j * bm.value(j)).take(5)
res191: Array[Int] = Array(2, 6, 12)
```

该例中，元素乘以元素对应值得到最后结果。广播变量会持续占用内存，当我们不需要的时候，可以用 unpersist 算子将其移除，这时，如果计算任务又用到广播变量，那么就会重新拉取数据，如下：

```
    ...
scala> val rdd_one = sc.parallelize(Seq(1,2,3))
rdd_one: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[101] at
parallelize at <console>:25
scala> val k = 5
k: Int = 5
scala> val bk = sc.broadcast(k)
bk: org.apache.spark.broadcast.Broadcast[Int] = Broadcast(163)
scala> rdd_one.map(j => j + bk.value).take(5)
res184: Array[Int] = Array(6, 7, 8)
scala> bk.unpersist
scala> rdd_one.map(j => j + bk.value).take(5)
res186: Array[Int] = Array(6, 7, 8)
```

你还可以使用 destroy 方法彻底销毁广播变量，调用该方法后，如果计算任务中又用到广播变量，则会抛出异常：

```
scala> val rdd_one = sc.parallelize(Seq(1,2,3))
rdd_one: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[101] at
parallelize at <console>:25
scala> val k = 5
k: Int = 5
scala> val bk = sc.broadcast(k)
bk: org.apache.spark.broadcast.Broadcast[Int] = Broadcast(163)
scala> rdd_one.map(j => j + bk.value).take(5)
res184: Array[Int] = Array(6, 7, 8)
scala> bk.destroy
scala> rdd_one.map(j => j + bk.value).take(5)
17/05/27 14:07:28 ERROR Utils: Exception encountered
org.apache.spark.SparkException: Attempted to use Broadcast(163) after it
was destroyed (destroy at <console>:30)
at org.apache.spark.broadcast.Broadcast.assertValid(Broadcast.scala:144)
at
org.apache.spark.broadcast.TorrentBroadcast$$anonfun$writeObject$1.apply$mc
V$sp(TorrentBroadcast.scala:202)
at org.apache.spark.broadcast.TorrentBroadcast$$anonfun$wri
```

**广播变量在一定数据量范围内可以有效地使作业避免 Shuffle，使计算尽可能本地运行，Spark 的 Map 端连接操作就是用广播变量实现的。**

为了让你更好地理解上面那句话的意思，我再举一个比较典型的场景，我们希望对海量的日志进行校验，日志可以简单认为是如下的格式：  
表 A：校验码，内容

也就是说，我们需要根据校验码的不同，对内容采取不同规则的校验，而检验码与校验规则的映射则存储在另外一个数据库：  
表 B：校验码，规则

这样，情况就比较清楚了，如果不考虑广播变量，我们有这么两种做法：

1.  直接使用 map 算子，在 map 算子中的自定义函数中去查询数据库，那么有多少行，就要查询多少次数据库，这样性能非常差。
2.  先将表 B 查出来转化为 RDD，使用 join 算子进行连接操作后，再使用 map 算子进行处理，这样做性能会比前一种方式好很多，但是会引起大量的 Shuffle 操作，对资源消耗不小。

当考虑广播变量后，我们有了这样一种做法（Python 风格伪代码）：

```
tableA = spark.sparkcontext.textFrom('/path')
validateTable = spark.sparkcontext.broadcast(queryTable())
def validate(validateNo,validateTable ):
......
validateResult = tableA.map(validate).reduceByKey((lambda x , y: x + y))
....
```

这样，相当于先将小表进行广播，广播到每个 Executor 的内存中，供 map 函数使用，这就避免了 Shuffle，虽然语义上还是 join（小表放内存），但无论是资源消耗还是执行时间，都要远优于前面两种方式。

### 累加器

与广播变量只读不同，累加器是一种只能进行增加操作的共享变量。如果你想知道记录中有多少错误数据，一种方法是针对这种错误数据编写额外逻辑，另一种方式是使用累加器。用法如下：

```
    ...
scala> val acc1 = sc.longAccumulator("acc1")
acc1: org.apache.spark.util.LongAccumulator = LongAccumulator(id: 10355,
name: Some(acc1), value: 0)
scala> val someRDD = tableRDD.map(x => {acc1.add(1); x})
someRDD: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[99] at map at
<console>:29
scala> acc1.value
res156: Long = 0 not get incremented*/
scala> someRDD.count
res157: Long = 351
scala> acc1.value
res158: Long = 351
scala> acc1
res145: org.apache.spark.util.LongAccumulator = LongAccumulator(id: 10355,
name: Some(acc1), value: 351)
```

上面这个例子用 SparkContext 初始化了一个长整型的累加器。LongAccumulator 方法会将累加器变量置为 0。行动算子 count 触发计算后，累加器在 map 函数中被调用，其值会一直增加，最后定格为 351。Spark 内置的累加器有如下几种。

-   LongAccumulator：长整型累加器，用于求和、计数、求均值的 64 位整数。
-   DoubleAccumulator：双精度型累加器，用于求和、计数、求均值的双精度浮点数。
-   CollectionAccumulator\[T\]：集合型累加器，可以用来收集所需信息的集合。

所有这些累加器都是继承自 AccumulatorV2，如果这些累加器还是不能满足用户的需求，Spark 允许自定义累加器。如果需要某两列进行汇总，无疑自定义累加器比直接编写逻辑要方便很多，例如：

![图片2.png](https://s0.lgstatic.com/i/image/M00/09/F1/CgqCHl69BxaAR5emAAAg3H3pKGc444.png)

这个表只有两列，需要统计 A 列与 B 列的汇总值。下面来看看根据上面的逻辑如何实现一个自定义累加器。代码如下：

```
import org.apache.spark.util.AccumulatorV2
import org.apache.spark.SparkConf
import org.apache.spark.SparkContext
import org.apache.spark.SparkConf
case class SumAandB(A: Long, B: Long)
class FieldAccumulator extends AccumulatorV2[SumAandB,SumAandB] {
private var A:Long = 0L
private var B:Long = 0L
    override def isZero: Boolean = A == 0 && B == 0L
override def copy(): FieldAccumulator = {
        val newAcc = new FieldAccumulator
        newAcc.A = this.A
        newAcc.B = this.B
        newAcc
    }
override def reset(): Unit = { A = 0 ; B = 0L }
override def add(v: SumAandB): Unit = {
        A += v.A
        B += v.B
    }
override def merge(other: AccumulatorV2[SumAandB, SumAandB]): Unit = {
        other match {
case o: FieldAccumulator => {
            A += o.A
            B += o.B}
case _ =>
        }
    }
    override def value: SumAandB = SumAandB(A,B)
}
```

凡是有关键字 override 的方法，均是重载实现自己逻辑的方法。累加器调用方式如下：

```
package com.spark.examples.rdd
import org.apache.spark.SparkConf
import org.apache.spark.SparkContext
class Driver extends App{
  val conf = new SparkConf
  val sc = new SparkContext(conf)
  val filedAcc = new FieldAccumulator
  sc.register(filedAcc, " filedAcc ")
  val tableRDD = sc.textFile("table.csv").filter(_.split(",")(0) != "A")
  tableRDD.map(x => {
     val fields = x.split(",")
     val a = fields(1).toInt
     val b = fields(2).toLong
     filedAcc.add(SumAandB (a, b))
     x
  }).count
}
```

最后计数器的结果为（3100, 31）。

### 小结

本课时主要介绍了 Spark 的两种共享变量，注意体会广播变量最后介绍的 map 端 join 的场景，这在实际使用中非常普遍。另外广播变量的大小，按照我的经验，要根据 Executor 和 Worker 资源来确定，几十兆、一个 G 的广播变量在大多数情况不会有什么问题，如果资源充足，那么1G~10G 以内问题也不大。

最后我要给你留一个思考题，请你对数据集进行空行统计。你可以先用普通算子完成后，再用累加器的方式完成，并比较两者的执行效率 ，如果有条件的，可以在生产环境中用真实数据集比较下两者之间的差异，差异会更明显。
