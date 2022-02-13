---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在上一课时中，我们学习了 GraphX 的精髓：Pregel API 及其背后所蕴含的思想。你可能会发现，Pregel 和 MapReduce 模型差异很大，那么基于 MapReduce 的 Spark 和基于 Pregel 的 Graph 是否真有如此大的差别，它们是否存在某种共性？本课时将通过讲解 aggregateMessages 这个算子来回答这个问题。

aggregateMessages 是一个比较特殊的算子，它很少被直接使用，但却是 GraphX 的核心算子，是大规模并行图处理的基础，也是理解 Pregel 的关键所在。这个算子最大的特殊之处在于，**从顶点的角度出发，用户可以通过该算子自定义 sendMsg 函数来使所有顶点沿着边，向边终点的顶点发送消息，再使用 mergeMsg 函数在目标顶点对收到的消息进行合并，最后返回一个新的顶点表**，其中顶点属性的类型与消息类型相同，如下：

```
def aggregateMessages[Msg: ClassTag](
sendMsg: EdgeContext[VD, ED, Msg] => Unit,
mergeMsg: (Msg, Msg) => Msg,
tripletFields: TripletFields = TripletFields.All)
: VertexRDD[Msg]
```

学习了上个课时的内容后，相信你已经能够理解消息传递的概念。sendMsg 接收 EdgeContext 类型的对象作为参数，该对象可以看成是一个边点三元组，即 triplet。通过该对象，用户可以得到起点顶点、终点顶点以及各自的属性（包括边），如下图中的表 1。

还可以利用 sendToSrc 与 sendToDst 向源顶点与目标顶点发送消息。用户可以通过 mergeMsg 函数来对消息进行合并。虽然 sendMsg 与 mergeMsg 的说法看起来比较新颖，有了拟物化的感觉，**但其实可以将 sendMsg 看作是 MapReduce 模型中的 map 函数，而 mergeMsg 函数则可以看成是 reduce 函数**，如下图所示：

![28.png](https://s0.lgstatic.com/i/image/M00/33/61/CgqCHl8P-iWADBACAAB4wq1UMMY193.png)

上图表 1 中的每一行都会在 map 函数（sendMsg函数）的作用下，根据 srcAttr、desAttr、edgeAttr 生成待发送的消息 msg 和发送的目标顶点 Id（sendToDesId），中间结果（表 2）会根据目标顶点 Id（sendToDesId）进行分发，经过 reduce 函数（mergeMsg）的化简操作，得到最后的结果：具有全新属性值（newSrcAttr、newDesAttr、newEdgeAttr）的三元组表（表 3）。

这是一个简化的描述过程，但仍然可以看出 aggregateMessages 与 MapReduce 是非常相似的。事实上，aggregateMessages 上一个版本的名字更能说明问题——mapReduceTriplets，可以看到，这个算子的名字里面没有 Message 的字样，而是 MapReduce，说明这个算子和 MapReduce 模型有着很强的联系，而 mapReduceTriplets 的参数也印证了上图的过程：

```
def mapReduceTriplets[Msg](
map: EdgeTriplet[VD, ED] => Iterator[(VertexId, Msg)],
reduce: (Msg, Msg) => Msg)
: VertexRDD[Msg]
```

像上面讲到的那样，GraphX 通过顶点表与边表来生成图，这也是 Graph 的底层数据，**因此，要想得到上图中的表 1，也就是边点三元组表，需要将原始顶点表与边表进行连接操作**。

在上面的例子中，其实并不需要 desAttr（目标顶点属性）这个字段，所以在生成上图中的表 1 时，可以进行优化。aggregateMessages 的最后一个参数，可以指定用户需要哪一边顶点的属性值：源顶点、目标顶点或者二者都需要。默认为 TripletFields.All，那么这里用户的选择如下：

```
TripletFields.Src、
TripletFields.Dst、
TripletFields.All、
TripletFields.None
```

因此该字段的主要作用是对连接策略进行优化，它直接影响了连接的条件。如果图中有孤立的顶点，那么最后的结果中是不会包含这些顶点的。这也比较好理解，因为它们根本就没有出现在表 1 中。

**综上所述，整个 aggregateMessages 算子可以拆分为 3 步，即连接（join）、转换（map）和化简（reduce）**，本课时一开始对于 aggregateMessages 算子的描述（顶点沿着边发送消息）看起来不好理解，但拆分成 3 步后，其实是再简单不过的操作的组合，只是换了个说法。

**从这个层面上来说，Pregel 是基于 MapReduce 的计算模型，只不过声明的方式（编程接口）有所不同。**

连接（join）、转换（map）和化简（reduce）这 3 步也刚好是 aggregateMessages 3 个参数的作用。从这 3 步可以看出，其中性能消耗比较大的是第 1 步连接操作，**那么 aggregateMessages 的优化问题，其实又变成了连接问题的优化**。

利用该算子，用户可以很容易地求得图中每个顶点的度数（入度和出度），以及每个顶点的邻居顶点。这些 GraphX 已经实现，用户直接调用即可。

aggregateMessages 算子的第一步是连接操作，也是性能消耗最大的一步。如果按照最原始的办法使用顶点表与边表直接进行散列连接，无疑会有很大性能问题。GraphX 对此有自己的优化方案。

首先，像前面介绍的一样，在对数据的分区上，就针对图进行了优化，每个分区内都会有一定数量的边。接下来 GraphX 会生成顶点与分区的路由表，其中路由表结构大致可看成List\[(PartitionId,VertexIdList)\]。

通过该路由表，可以得到每个分区中需要哪些顶点属性信息，这样就可以通过 Graph 内部维护的索引快速找到对应的顶点属性值，完成连接操作。这种操作类似于 Map 端连接，相当于每个图分区和一个顶点表的子集进行连接操作。但这样保证了这个顶点表正好包含了该图分区的所有顶点，因此不会影响结果的正确性。这也是 aggregateMessages 对于连接优化问题给出的答案，如下图所示：

![image (1).png](https://s0.lgstatic.com/i/image/M00/33/5D/CgqCHl8P9oWATWlsAALo44vfJHM578.png)

![image (2).png](https://s0.lgstatic.com/i/image/M00/33/52/Ciqc1F8P9o6AEfXWAAGj0pJcwqc881.png)

### 小结

经过本课时的学习，你会发现 Pregel API 与 aggregateMessages 算子很像，**aggregateMessages 也有sendMsg、mergeMsg 等参数，只是没有迭代的概念**。事实上它们之间的关系是，Prege API 底层依赖的是 mapReduceTriplets，而 aggregateMessages 则是 mapReduceTriplets 的升级版，其中原理大致一样。

目前 Pregel API 实现仍然没有采用 aggregateMessages，虽然 aggregateMessages 相对 mapReduceTriplets 来说有较大性能提升。**这也侧面说明 GraphX 其实是 Spark 中发展比较缓慢的模块，但是这并不影响 GraphX 在图计算场景中发挥的重要作用。**

这里给你留一个思考题：连接的优化其实和我们前面讲到过的一种连接方式非常接近，是哪一种呢？
