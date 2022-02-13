---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


今天我将为你讲解计算框架的分布式实现：剖析 Spark Shuffle 原理。我们在前面几个课时，或多或少地提到了 Shuffle， Shuffle 一般被翻译为数据混洗，是类 MapReduce 分布式计算框架独有的机制，也是这类分布式计算框架最重要的执行机制。本课时主要从两个层面讲解 Shuffle，主要分为：

-   逻辑层面；
    
-   物理层面。
    

逻辑层面主要从 RDD 的血统机制出发，从 DAG 的角度来讲解 Shuffle，另外也会讲解 Spark 容错机制，而物理层面是从执行角度来剖析 Shuffle 是如何发生的。

### RDD 血统与 Spark 容错

在 DAG 中，最初的 RDD 被称为基础 RDD，后续生成的 RDD 都是由算子以及依赖关系生成的，也就是说，无论哪个 RDD 出现问题，都可以由这种依赖关系重新计算而成。这种依赖关系被称为 RDD 血统（lineage）。血统的表现形式主要分为宽依赖（wide dependency）与窄依赖（narrow dependency），如下图所示：

![图片1.png](https://s0.lgstatic.com/i/image/M00/0D/45/CgqCHl7DnZCATfLcAAEWt4pokjU626.png)

窄依赖的准确定义是：子 RDD 中的分区与父 RDD 中的分区只存在一对一的映射关系，而宽依赖则是子 RDD 中的分区与父 RDD 中的分区存在一对多的映射关系，那么从这个角度来说，map、 filter、 union 等就是窄依赖，而 groupByKey、 coGroup 就是典型的宽依赖，如下图所示：

![图片2.png](https://s0.lgstatic.com/i/image/M00/0D/45/CgqCHl7DnZqADD-RAALWDw37CfQ883.png)

![图片3.png](https://s0.lgstatic.com/i/image/M00/0D/39/Ciqc1F7DnaSAAoOWAAJZYzj7SwE562.png)

**宽依赖还有个名字，叫 Shuffle 依赖，也就是说宽依赖必然会发生 Shuffle 操作，在前面也提到过 Shuffle 也是划分 Stage 的依据**。而窄依赖由于不需要发生 Shuffle，所有计算都是在分区所在节点完成，它类似于 MapReduce 中的 ChainMapper。所以说，**在你自己的 DAG 中，如果你选取的算子形成了宽依赖，那么就一定会触发 Shuffle**。

当 RDD 中的某个分区出现故障，那么只需要按照这种依赖关系重新计算即可，窄依赖最简单，只涉及某个节点内的计算，而宽依赖，则会按照依赖关系由父分区计算而得到，如下图所示：

![图片4.png](https://s0.lgstatic.com/i/image/M00/0D/39/Ciqc1F7DnbCAbGWPAACD6RGoD3A707.png)

如果 P1\_0 分区发生故障，那么按照依赖关系，则需要 P0\_0 与 P0\_1 的分区重算，如果 P0\_0与 P0\_1 没有持久化，就会不断回溯，直到找到存在的父分区为止。当计算逻辑复杂时，就会引起依赖链过长，这样重算的代价会极其高昂，所以用户可以在计算过程中，适时调用 RDD 的 checkpoint 方法，保存当前算好的中间结果，这样依赖链就会大大缩短。RDD 的血统机制就是 RDD 的容错机制。

Spark 的容错主要分为资源管理平台的容错和 Spark 应用的容错， Spark 应用是基于资源管理平台运行，所以资源管理平台的容错也是 Spark 容错的一部分，如 YARN 的 ResourceManager HA 机制。在 Spark 应用执行的过程中，可能会遇到以下几种失败的情况：

-   Driver 报错；
    
-   Executor 报错；
    
-   Task 执行失败。
    

Driver 执行失败是 Spark 应用最严重的一种情况，标志整个作业彻底执行失败，需要开发人员手动重启 Driver；Executor 报错通常是因为 Executor 所在的机器故障导致，这时 Driver 会将执行失败的 Task 调度到另一个 Executor 继续执行，重新执行的 Task 会根据 RDD 的依赖关系继续计算，并将报错的 Executor 从可用 Executor 的列表中去掉；Spark 会对执行失败的 Task 进行重试，重试 3 次后若仍然失败会导致整个作业失败。在这个过程中，Task 的数据恢复和重新执行都用到了 RDD 的血统机制。

### Spark Shuffle

很多算子都会引起 RDD 中的数据进行重分区，新的分区被创建，旧的分区被合并或者被打碎，**在重分区的过程中，如果数据发生了跨节点移动，就被称为 Shuffle**，在 Spark 中， Shuffle 负责将 Map 端（这里的 Map 端可以理解为宽依赖的左侧）的处理的中间结果传输到 Reduce 端供 Reduce 端聚合（这里的 Reduce 端可以理解为宽依赖的右侧），它是 MapReduce 类型计算框架中最重要的概念，同时也是很消耗性能的步骤。**Shuffle 体现了从函数式编程接口到分布式计算框架的实现**。与 MapReduce 的 Sort-based Shuffle 不同，Spark 对 Shuffle 的实现方式有两种：**Hash Shuffle 与 Sort-based Shuffle**，这其实是一个优化的过程。在较老的版本中，Spark Shuffle 的方式可以通过 spark.shuffle.manager 配置项进行配置，而在最新的 Spark 版本中，已经去掉了该配置，统一称为 Sort-based Shuffle。

#### Hash Shuffle

在 Spark 1.6.3 之前， Hash Shuffle 都是 Spark Shuffle 的解决方案之一。 Shuffle 的过程一般分为两个部分：Shuffle Write 和 Shuffle Fetch，前者是 Map 任务划分分区、输出中间结果，而后者则是 Reduce 任务获取到的这些中间结果。Hash Shuffle 的过程如下图所示：

![图片5.png](https://s0.lgstatic.com/i/image/M00/0D/3A/Ciqc1F7DnbmAAR5LAAS9bfIkvrg055.png)

在图中，Shuffle Write 发生在一个节点上，该节点用来执行 Shuffle 任务的 CPU 核数为 2，每个核可以同时执行两个任务，每个任务输出的分区数与 Reducer（这里的 Reducer 指的是 Reduce 端的 Executor）数相同，即为 3，每个分区都有一个缓冲区（bucket）用来接收结果，每个缓冲区的大小由配置 spark.shuffle.file.buffer.kb 决定。这样每个缓冲区写满后，就会输出到一个文件段（filesegment），而 Reducer 就会去相应的节点拉取文件。这样的实现很简单，但是问题也很明显。主要有两个：

1.  生成的中间结果文件数太大。理论上，每个 Shuffle 任务输出会产生 R 个文件（ R为Reducer 的个数），而 Shuffle 任务的个数往往由 Map 任务个数 M 决定，所以总共会生成 M \* R 个中间结果文件，而往往在一个作业中 M 和 R 都是很大的数字，在大型作业中，经常会出现文件句柄数突破操作系统限制。
    
2.  缓冲区占用内存空间过大。单节点在执行 Shuffle 任务时缓存区大小消耗为 m \* R \* spark.shuffle.file.buffer.kb，m 为该节点运行的 Shuffle 任务数，如果一个核可以执行一个任务，m 就与 CPU 核数相等。这对于动辄有 32、64 物理核的服务器来说，是笔不小的内存开销。
    

为了解决第一个问题， Spark 推出过 File Consolidation 机制，旨在通过共用输出文件以降低文件数，如下图所示：

![图片6.png](https://s0.lgstatic.com/i/image/M00/0D/3A/Ciqc1F7DncKABtxxAAVwHoozu4I409.png)

每当 Shuffle 任务输出时，同一个 CPU 核心处理的 Map 任务的中间结果会输出到同分区的一个文件中，然后 Reducer 只需一次性将整个文件拿到即可。这样，Shuffle 产生的文件数为 C（CPU 核数）\* R。 Spark 的 FileConsolidation 机制默认开启，可以通过 spark.shuffle.consolidateFiles 配置项进行配置。

#### Sort-based Shuffle

在 Spark 先后引入了 Hash Shuffle 与 FileConsolidation 后，还是无法根本解决中间文件数太大的问题，所以 Spark 在 1.2 之后又推出了与 MapReduce 一样（你可以参照《Hadoop 海量数据处理》（第 2 版）的 Shuffle 相关章节）的 Shuffle 机制： Sort-based Shuffle，才真正解决了 Shuffle 的问题，再加上 Tungsten 计划的优化， Spark 的 Sort-based Shuffle 比 MapReduce 的 Sort-based Shuffle 青出于蓝。如下图所示：

![图片7.png](https://s0.lgstatic.com/i/image/M00/0D/45/CgqCHl7Dnc2AV9OgAAL1Z7bbIkE547.png)

每个 Map 任务会最后只会输出两个文件（其中一个是索引文件），其中间过程采用的是与 MapReduce 一样的归并排序，但是会用索引文件记录每个分区的偏移量，输出完成后，Reducer 会根据索引文件得到属于自己的分区，在这种情况下，Shuffle 产生的中间结果文件数为 2 \* M（M 为 Map 任务数）。

在基于排序的 Shuffle 中， Spark 还提供了一种折中方案——Bypass Sort-based Shuffle，当 Reduce 任务小于 spark.shuffle.sort.bypassMergeThreshold 配置（默认 200）时，Spark Shuffle 开始按照 Hash Shuffle 的方式处理数据，而不用进行归并排序，只是在 Shuffle Write 步骤的最后，将其合并为 1 个文件，并生成索引文件。这样实际上还是会生成大量的中间文件，只是最后合并为 1 个文件并省去排序所带来的开销，该方案的准确说法是 Hash Shuffle 的Shuffle Fetch 优化版。

Spark 在1.5 版本时开始了 Tungsten 计划，也在 1.5.0、 1.5.1、 1.5.2 的时候推出了一种 tungsten-sort 的选项，这是一种成果应用，类似于一种实验，该类型 Shuffle 本质上还是给予排序的 Shuffle，只是用 UnsafeShuffleWriter 进行 Map 任务输出，并采用了要在后面介绍的 BytesToBytesMap 相似的数据结构，把对数据的排序转化为对指针数组的排序，能够基于二进制数据进行操作，对 GC 有了很大提升。但是该方案对数据量有一些限制，随着 Tungsten 计划的逐渐成熟，该方案在 1.6 就消失不见了。

从上面整个过程的变化来看， Spark Shuffle 也是经过了一段时间才趋于成熟和稳定，这也正像学习的过程，不用一蹴而就，贵在坚持。

### 习题讲解

最后我们在这里公布 08 课时的第 2 道练习题答案，从这道题你可以看出 Shuffle 的方式，对性能影响非常大。

首先来回顾下这道题：  
练习题 2：用 Spark 算子实现对 1TB 的数据进行排序？

关于这道题的解法，你可能很自然地想到了归并排序的原理，首先每个分区对自己分区进行排序，最后汇总到一个分区内进行全排序，如下图所示：

![图片8.png](https://s0.lgstatic.com/i/image/M00/0D/3A/Ciqc1F7DndeAOQzcAABqfnwq9Xk083.png)

可想而知，最后 1TB 的数据都会汇总到 1 个 Executor，就算这个 Executor 分配到的资源再充足，面对这种情况，无疑也是以失败告终。所以这道题的解法应该是另一种方案，首先数据会按照键的区间进行分发，也就是 Shuffle，如 \[0，100000\]、 \[100000，200000）和 \[200000，300000\]，每个分区没有交集。照此规则分发后，分区内再进行排序，就可以在满足性能要求的前提下完成全排序，如下图：

![图片9.png](https://s0.lgstatic.com/i/image/M00/0D/46/CgqCHl7DnhuAR4YEAACiYfUpajA523.png)

这种方式的全排序无疑实现了计算的并行化，很多测试性能的场景也用这种方式对 1TB 的数据进行排序，目前世界纪录是腾讯在 2016 年达到的 98.8 秒。对于这种排序方式，Spark 也将其封装为 sortByKey 算子，它采用的分区器则是 RangePartitioner。

### 小结

Spark Shuffle 是 Spark 最重要的机制，作为大数据工程师的你，有必要深入了解，Shuffle机制也是面试喜欢问到的一个问题，前面三张关于Shuffle的图大家一定要吃透。另外， Spark 作业的性能问题往往出现在 Shuffle 上，在上个课时中，我们也是通过广播变量而避免了 Shuffle，从而得到性能的提升，所以掌握了 Spark Shuffle 能帮助你有针对性地进行调优。

最后给你出一个思考题：还是在 08 课时第 2 道思考题的基础上，如果数据并没有均匀分布，那么很可能某个分区的数据会异常多，同样会导致作业失败，对于这种数据倾斜的情况，你认为有没有办法避免呢？
