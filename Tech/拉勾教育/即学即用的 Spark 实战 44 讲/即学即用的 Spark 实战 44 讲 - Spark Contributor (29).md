---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在开始之前，我们先对上个课时的习题进行讲解，连接的优化其实和我们前面讲到过的一种连接方式非常接近，那就是 mapjoin。你答对了吗？如果有问题，欢迎和我在留言中讨论。

在前面的课程中，我们已经完整学习了 GraphX 的所有内容，并且对 Pregel API 也有了深入了解。本课时将会用 GraphX 实现 PageRank 算法，实践前面学到的内容。

PageRank 是谷歌公司创始人拉里佩奇和谢尔盖布林提出的链接分析算法，也被评为数据挖掘十大算法之一，它**是用于计算网页重要性的基础算法**。PageRank 算法会赋予每个网页一个分值，我们称之为\*\* PR 分\*\*，**是用来衡量网页重要性的依据，值越大表示网页越重要，即越受欢迎**。

像谷歌这样的搜索引擎公司会泛爬全网所有公开网页，爬下来的网页会将自己的 URL 作为唯一标识进行存储，而网页中又有可能链接到其他网页，这样的数据天然就是图结构，网页就是图中的顶点，而边则是网页与网页之间的链接关系，如下图所示：

![x1.png](https://s0.lgstatic.com/i/image/M00/36/09/Ciqc1F8WpJGARXVJAAJi6KATlWk577.png)

PageRank 本质上是计算图中顶点重要性的算法，这其实属于复杂网络中的中心性课题，在 PageRank 之前，其实也有人提出过用顶点的度来衡量，**也就是简单地认为顶点的度越大，节点越重要，这其实就是度中心性**（Degree Centrality），而 PageRank 计算的结果，我们也可以将其称为 **PageRank 中心性**。与度中心性不同的是，**PageRank 除了考虑网页的链接数，还会考虑网页本身的质量，并将两者进行有机地结合**。

PageRank 会用算法输出一个概率分布，用来表示随机点击链接的人将会到达任意特定页面的可能性。在计算过程开始时，假定图中所有网页的初始 PR 值相同。PageRank 需要多次迭代完成计算，通过迭代来调整近似的 PR 值，以更贴近反映理论的真实值。

概率为介于 0 和 1 之间的数值，通常，0.5 的概率表示为发生事件的概率为 50％。因此，0.5 的 PR 值意味着有 50％ 的概率点击一个随机链接将被引导到该网页。

PageRank 若一开始假设每个网页的初始 PR 值为 0.25，在一次迭代中，给定网页的 PR 值会沿着出边将 PR 值进行传递，传输的值会根据出边的个数进行平均分配。如下图所示：

![xx.png](https://s0.lgstatic.com/i/image/M00/36/18/CgqCHl8Wp_OAS2BCAABwwQjnG9o781.png)

这个简单的图由网页 A、B、C、D 组成，图中链接除了 B、C、D 链向 A 以外，没有其他链接，那么在下一次，迭代 A 的 PR 值为 0.75，它的计算公式如下：

![Drawing 2.png](https://s0.lgstatic.com/i/image/M00/36/09/Ciqc1F8WpP-AB2M6AAANLUsq2DA734.png)

假设有另一种情况：B 链 向 C 和 A，C 链向 A，D 链向 A、B、C，如下图所示 ：

![x2.png](https://s0.lgstatic.com/i/image/M00/36/09/Ciqc1F8WpUGAVK2tAAB0hHbs0uM582.png)

在下一次迭代时，B 只会将自己 PR 值的一半（也就是 0.125）分别传送给 \*A \*和 _C_，而 \*C \*会将自己所有的 PR 值（也就是0.25）传输给自己链向的唯一页面 A。D 链向其他 3 个网页，所以传输给 3 个网页的值大概是 0.083。这样在下一次迭代完成后，A 的 PR 值大概为 0.458，计算公式如下：

![Drawing 4.png](https://s0.lgstatic.com/i/image/M00/36/0A/Ciqc1F8WpXKADwh4AAAUq2FbeXo285.png)

换句话说，**该网页赋予其他网页的值等于自己的 PR 值除以该网页的链出数 L**，这样一来 PR(A) 可以表示为：

![Drawing 5.png](https://s0.lgstatic.com/i/image/M00/36/0A/Ciqc1F8WpXuAYk0YAAAd_2ditZA295.png)

推而广之，任意网页 u 的 PR 值为：

![Drawing 6.png](https://s0.lgstatic.com/i/image/M00/36/15/CgqCHl8WpYyAA9UkAAAXnCjJPxE013.png)

即网页 u 的 PR 值等于，包含在集合 Bu（包含链入网页 u 的所有网页的集合）中每个网页 v 的 PR 值，除以相应网页 v 的链出数量 L(v) 的和。

PageRank 理论认为**用户在随机点击网页的过程中，最终还是会停止点击的**（如将网页加到收藏夹并退出，或者没有链出网页可点）。那么在每一步迭代中，用户还能继续点击的概率为 d，我们也称其为**阻尼因子**（damping factor），很多研究表明，该值大约在 0.85 左右。有了阻尼系数的 PR 值公式为：

![Drawing 7.png](https://s0.lgstatic.com/i/image/M00/36/0A/Ciqc1F8WpayAYkVRAAAgKPJCIKU605.png)

这个公式采用的是一个随机网上冲浪者模型，用户在几次点击后感到无聊，并且切换到了一个随机页面，PageRank 的值在前面也提到过，反映了随机点击链接的人将会到达任何特定页面的可能性。**这可以理解为马尔科夫链，状态就是下图中的页面，而转移概率就是页面之间的链接，是等可能的**。换言之，用户点击页面的过程，是一个随机游走过程。

![x2.png](https://s0.lgstatic.com/i/image/M00/36/15/CgqCHl8WpbyAbSA2AAB0hHbs0uM973.png)

了解了 PageRank 算法后，我们发现这种算法很适合用 Pregel 编程模型来实现，其过程非常自然。事实上，谷歌公司也在论文中明确表示 PageRank 是由 Pregel 完成。作为图挖掘算法中最经典的算法之一，GraphX 也将其实现，源码文件可以在[https://github.com/apache/spark/blob/master/graphx/src/main/scala/org/apache/spark/graphx/lib/PageRank.scala](https://github.com/apache/spark/blob/master/graphx/src/main/scala/org/apache/spark/graphx/lib/PageRank.scala)浏览并下载。

整个算法是由 runUntilConvergenceWithOptions 方法触发，在该方法中，首先会对图数据进行预处理：

```
...
val personalized = srcId.isDefined
val src: VertexId = srcId.getOrElse(-1L)
val pagerankGraph: Graph[(Double, Double), Double] = graph
.outerJoinVertices(graph.outDegrees) {
   (vid, vdata, deg) => deg.getOrElse(0)
}
.mapTriplets( e => 1.0 / e.srcAttr )
.mapVertices { (id, attr) =>
if (id == src) (0.0, Double.NegativeInfinity) else (0.0, 0.0)
}
.cache()
```

其中 delta 用于记录两次迭代之间该顶点 PR 值的变化。下面就进入实现 PageRank 的 3 个核心函数，它们和pregel中的三个核心函数相对应：

```
def vertexProgram(id: VertexId, attr: (Double, Double), msgSum: Double): (Double, Double) = {
val (oldPR, lastDelta) = attr
val newPR = oldPR + (1.0 - resetProb) * msgSum
   (newPR, newPR - oldPR)
}
def sendMessage(edge: EdgeTriplet[(Double, Double), Double]) = {
if (edge.srcAttr._2 > tol) {
Iterator((edge.dstId, edge.srcAttr._2 * edge.attr))
    } else {
Iterator.empty
    }
}
def messageCombiner(a: Double, b: Double): Double = a + b
```

这 3 个函数理解起来都比较容易，与前面介绍的 PageRank 并无二致。vertexProgram 函数计算新的 PR 值，并记录两次迭代产生的 PR 值之间的差异；sendMessage 函数根据链出边的权重生成发送对应顶点的消息（值）。**注意，如果两次迭代的 PR 值足够小，将停止发送消息；messageCombiner 函数将会求出下面这个式子的值**：

![Drawing 9.png](https://s0.lgstatic.com/i/image/M00/36/0A/Ciqc1F8WpeSAMs8UAAAQbslQjsE143.png)

下面的代码将开始运行 PageRank 算法：

```
val initialMessage = if (personalized) 0.0 else resetProb / (1.0 - resetProb)
val vp = if (personalized) {
   (id: VertexId, attr: (Double, Double), msgSum: Double) =>
   personalizedVertexProgram(id, attr, msgSum)
} else {
    (id: VertexId, attr: (Double, Double), msgSum: Double) =>
    vertexProgram(id, attr, msgSum)
}
val rankGraph = Pregel(pagerankGraph, initialMessage, activeDirection = EdgeDirection.Out)(
vp, sendMessage, messageCombiner
)
.mapVertices((vid, attr) => attr._1)
normalizeRankSum(rankGraph, personalized)
```

这样我们就学习了 PageRank 算法，并用GraphX将其实现，本课时为实践环节，难度并不是很大。理解 PageRank 算法并不难，理解它的 Pregel API 实现也不是很难，难的是习惯用这种思维来表达算法，在遇到图的场景时，不妨多“像顶点一样思考”。

这里给你留一个课后作业：Personalized PageRank 和 PageRank 有什么不同？
