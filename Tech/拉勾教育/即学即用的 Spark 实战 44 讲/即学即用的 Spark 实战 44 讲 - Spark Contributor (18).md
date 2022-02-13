---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


本课时是流处理模块的第一课时，通过前面的模块实战，相信你对 Spark 数据处理能力已经有了一个感性的认识。在本模块，将着重介绍另一类处理场景：流处理与相应的解决办法。

本课时的主要内容有：

-   什么是流处理；
-   消息送达保证。

### 什么是流处理

在前面说到，Spark 为大数据处理提供了一整套解决方案，当然流处理也在其中。大数据的 4V 特征之一就是“Velocity（速度）”，它说明数据产生和流动的速度与以往不可同日而语，但数据所蕴含的价值却会随着时间的流逝而迅速降低，如监测预警、实时反欺诈、实时风险管理、网络攻击、计算广告等业务场景，如何快速而精确地捕获数据中所蕴含的价值是流处理面临的挑战。

流处理的概念从一诞生起，就得到了业界的热捧。从最开始雅虎 S4，到 Apache Storm、Spark Streaming，再到目前的 Structured Streaming、Flink、Apex，开源社区起到了决定性作用。纵观整个流处理的发展历程，似乎并没有一种技术像 Spark、MapReduce 那样在各自的时期独霸业界，更多的是用户根据自己的需求采用不同的技术，这或多或少都对功能和性能进行了一定取舍，这也是流处理技术非常有意思的一点，详细的内容我们将在后面进行介绍。

在开始介绍流处理相关概念以及具体技术前，先用一个例子让你对流处理有一个感性认识。还是以单词计数为例，输入的数据不再是 HDFS 上的文本文件，而来自网络套接字的数据流。

代码如下：

```
import org.apache.spark.SparkConf
import org.apache.spark.streaming.StreamingContext
import org.apache.spark.streaming.Seconds
object SparkStreamingWordCount {
def main(args: Array[String]): Unit = {
val conf = new SparkConf()
    .setMaster("local[2]")
    .setAppName("SparkStreamingWordCount")
val ssc = new StreamingContext(conf, Seconds(1))
val lines = ssc.socketTextStream("localhost", 9999)
val words = lines.flatMap(_.split(" "))
val pairs = words.map(word => (word, 1))
val wordCounts = pairs.reduceByKey(_ + _)
    wordCounts.print()
    ssc.start()
    ssc.awaitTermination()
  }
}
```

如上所示，我们以 local 方式启动一个 Spark Streaming 作业，监听 9999 端口的输入，并按单词进行计数。运行此作业，会发现 Spark Streaming 已经在等待 9999 端口的输入了，如下图所示：

![image.png](https://s0.lgstatic.com/i/image/M00/1D/51/Ciqc1F7h3JGAPpjxAADLVf8z9LM512.png)

接下来还需要对 9999 端口进行输入，我们选用 netcat 命令（nc，在 Windows 环境需要额外安装），打开命令行执行下面的命令：

```
Windows: nc -l -p 9999
Linux:   nc -lk 9999
```

然后我们就可以在命令行界面下输入 hello world，这时 Spark Streaming 会实时输出：

![image (1).png](https://s0.lgstatic.com/i/image/M00/1D/5D/CgqCHl7h3LyAcfXsAAC6eYQv7kM984.png)

这样，一个简单的流处理作业就完成了。注意，在以 local 方式提交时必须用 local\[n\] 的形式，且 n 需要大于 2。当 n = 1 时，Spark Streaming 的 Receiver 会消耗掉所有的计算资源，从而无法开始真正的业务处理。

下面是同样逻辑的 Python 版代码：

```
from pyspark import SparkContext
from pyspark.streaming import StreamingContext
sc = SparkContext("local[2]", "SparkStreamingWordCountPython")
ssc = StreamingContext(sc, 1)
lines = ssc.socketTextStream("localhost", 9999)
words = lines.flatMap(lambda line: line.split(" "))
pairs = words.map(lambda word: (word, 1))
wordCounts = pairs.reduceByKey(lambda x, y: x + y)
wordCounts.pprint()
ssc.start()
```

### 消息送达保证

在上一个例子中，数据（消息）在计算节点随着处理过程在不停流动，计算节点从它的上游收到数据，经过处理又发向下游计算节点，在这个过程中，如何保证当前节点一定能收到上游计算节点发送的处理结果是一个非常重要的问题，因为它直接影响了流处理结果的正确性。我们也称其为消息送达保证（delivery guarantee）问题，对于消息送达保证，业界一般有以下 3 种语义。

-   至少送达一次（at least once），下游节点一定会收到一次上游节点发过来的消息，但也可能会接收到重复的消息。
-   至多送达一次（at most once），下游节点不一定会收到上游节点发来的消息。这意味着，有可能上游节点发送的消息，下游节点丢失了，但上游节点不会重发。
-   恰好送达一次（exact once），下游节点一定会且只会收到一次上游节点发来的消息。这当然是所有用户最希望的，但对于某些用户来说可能不是必需的。

对于这 3 种解决方案，当然流处理技术应该以恰好送达一次为目标进行设计，因为如果没有这种消息送达保证，那么对于支付场景、计算广告场景（点击计费）等来说就无法保证结果的正确性，这对于业务方来说是不可接受的。但也有很多时候，业务场景可能只需要至少送达一次的保证，那么这个时候就需要进行取舍。

在谈论消息送达保证这个话题时，其实不能孤立地看待它，我们可以将其看成两个问题：即发送消息的可靠性保障和消费消息的可靠性保障。前者指的其实是可靠发送，而后者指的是可靠接收并处理。想要达到“恰好送达一次”的效果，需要这两者同时满足。很多系统声称自己提供“恰好一次”的解决方案，但当我们仔细研究其原理时，会发现并不准确，因为它们没有解释消费者（下游计算节点，消息接收者）或者生产者（上游计算节点，消息发送者）在发送或接收失败时，还如何能保证消息“恰好一次”地传递。

在大数据架构中，经常会使用到消息队列，从某种意义上说，消息队列是一种最简单的流处理系统，先以 Kafka 为例，来理解下消息送达保证的实现。Kafka 的数据流如下图所示。

![18.png](https://s0.lgstatic.com/i/image/M00/1D/61/Ciqc1F7h58iAKk8sAADwtVHGxLI687.png)

如前所述，我们将其分为两个阶段：生产者生产消息，消费者消费消息。只有这两个阶段都满足了“恰好一次”的语义，整个过程才能满足“恰好一次”的语义。我们先从生产者的角度来看，目前的 Kafka 采用异步方式发送消息，当消息被提交后，Kafka 会异步将其发送给 Broker，发送成功后会回调发送 ACK，如果没有则重试，直到收到 ACK 为止，消息会有一个主键，在Broker 端会做幂等处理，不会导致数据出现重复，这样就算在提交过程中，出现了网络故障，导致消息不能及时发送，最终还是能保证发送消息“恰好一次”的语义效果。我们现在再从消费者的角度来看，所有副本都保存有相同的日志以及偏移量（offset），假设由消费者控制偏移量在日志中的位置。当消费者读取了几条数据时，有下面两种情况：

1.  读取消息，然后在日志中保存偏移量位置，最后处理消息。但有可能消费者保存了偏移量位置之后，在处理消息输出之前崩溃了。在这种情况下，接管处理的进程会在已保存的位置开始，即使该位置之前有几个消息尚未处理。这就达到了“至多一次”的效果，在消费者处理失败消息的情况下，不进行处理。
2.  读取消息，处理消息，最后保存消息的位置。在这种情况下，可能消费进程处理消息之后，在保存偏移位置之前崩溃了。当新的进程重新接管时，将接收已经被处理的前几个消息。这就达到了“至少一次”的效果。

如果想实现“恰好一次”的语义，那么需要做的其实是将输出处理消费数据的结果和修改偏移量这两个操作放在一个事务里，就可以保证“恰好一次”语义，最完美的解决方案是采取经典的两段式提交，但很多消费者不支持两段式提交，并且两段式提交会影响性能表现。有一个变通的办法就是将消费者的输出与消费者的偏移量存储在一个地方，这就避免了分布式事务，当然这会引起一些不必要的麻烦，更常见的会使用幂等输出，来实现“恰好一次”的效果。

这里假设 Kafka Broker 永远是可用的，不会有丢数据的风险，考虑到日志的副本机制，这个假设是合理的。

上图其实展示了一个最简单的流处理实例，也说明了其实如果要很完美地做到“恰好一次”，需要从两个方面努力：发送消息可靠性和接收处理消息可靠性。而接收消息可靠性要做到“恰好一次”，往往又需要对消息的状态进行持久化（此处指的是偏移量）。

对于上图的数据流程，站在消费者的角度，其实是一个输入-处理-输出的过程，这里其实涉及 3 个部分：数据源（数据输入）、计算框架（处理）和数据存储（数据输出），而当我们说到 Storm、Spark Streaming 实现“恰好一次”消息送达语义时，都只是停留在计算框架这个层面，但是一旦涉及整个流程，那么问题就必须重新审视了，这也提出了一个新的问题：端到端（end-to-end）的消息送达保证，而只有解决了端到端的消息送达保证，才是真正解决了“恰好一次的消息送达语义”。

### 小结

在企业和组织里，流处理的场景越来越普遍。本课时通过一个小例子介绍了流处理，最后引出了一个流处理中非常重要的问题：消息一致性，端到端的一致性通常来说涉及输入-处理-输出的全过程。在后面的课时，我们可以看到 Spark 是如何解决这个问题的。

最后给你留一个思考题：

如果希望统计一段时间内（比如说 5s）每个单词的数量，上面的程序应该怎么写？
