---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在开始本课时的内容之前，我们先来回顾一下上个课时的习题：Personalized PageRank 和 PageRank 有什么不同？

个性化 PageRank(Personalized PageRank) 算法继承了经典 PageRank 算法的思想，利用数据模型(图)链接结构来递归地计算各结点的权重，即模拟用户通过点击链接随机访问图中结点的行为 (随机行走模型)计算稳定状态下各结点得到的随机访问概率。个性化 PageRank 与 PageRank 的最大区别在于随机行走中的跳转行为。

接下来，我们就进入本课时的学习。GraphX 内置了 collectNeighborIds 函数，可以得到每个顶点的邻居顶点，也就是 1 度邻居顶点。如果需要得到每个顶点的 2 度或者 _n_ 度邻居顶点，应该如何利用 GraphX 完成这个任务呢？本课时将解答这个问题。

求 _n_ 度邻居顶点要比 1 度邻居顶点更复杂一些，**核心是要模拟出一个带有生命值的消息**，每传播一次，生命值就会相应减 1，那么在生命值为 0 的时候到达的顶点就是我们所求的 _n_ 度邻居顶点。

本课时将用 GraphGenerators 造出一个度分布为正态分布的图，然后实现 vertexProgress、sendMsg 和 mergeMsg 这 3 个关键函数，代码如下：

```
import org.apache.spark.graphx.EdgeTriplet
import org.apache.spark.graphx.EdgeDirection
import org.apache.spark.SparkConf
import org.apache.spark.SparkContext
import org.apache.spark.graphx.util.GraphGenerators
import org.apache.spark.graphx.VertexId
import org.apache.spark.graphx.PartitionStrategy
import org.apache.spark.graphx.Graph.graphToGraphOps
import scala.Iterator
object NDegreeNeighbor {
def main(args: Array[String]): Unit = {
val vextexNum = args(0).toInt
val u = args(1).toDouble
val sigma = args(2).toDouble
val eParts = args(3).toInt
val outputPath = args(4)
val numParts = args(5).toInt
val partStra = args(6).toInt
val n = args(7).toInt
val conf = new SparkConf()
    conf.setAppName("NDegreeNeighbor")
val sc = new SparkContext(conf)
val g = GraphGenerators
    .logNormalGraph(sc, vextexNum, eParts, u, sigma, System.currentTimeMillis())
    .mapVertices[(List[Long],List[(Long,Int)],Int)]((x,y) => (List(),List(),0))
    .partitionBy(
if(partStra == 0) 
PartitionStrategy.EdgePartition1D 
else if (partStra == 1) 
PartitionStrategy.EdgePartition2D 
else if (partStra == 2) 
PartitionStrategy.CanonicalRandomVertexCut 
else 
PartitionStrategy.RandomVertexCut,numParts)
     g.pregel[List[(Long,Int)]](List((-1L,-1)), n, EdgeDirection.Out)(vertexProgress, sendMsg, mergeMsg)
     .vertices
     .filter(x => if(x._2._1 == List()) false else true)
     .saveAsTextFile(outputPath)
def vertexProgress(
         id:VertexId,
         attr:(List[Long],List[(Long,Int)],Int),
         msgSum:List[(Long,Int)])
     :(List[Long],List[(Long,Int)],Int) = {
val iteCount = attr._3
val neighbor = attr._1
val msgStore = attr._2
val newIteCount = iteCount + 1
if(iteCount == 0){
          (List(),List(),newIteCount)
        }else if(newIteCount == n + 1){
val newNeighbor = msgSum.par.filter(x => if(x._2 == 1) true else false).map(_._1).toList
          (newNeighbor,List(),newIteCount)
        }else{
val newMsgStore:List[(Long,Int)] = msgSum.map(x => (x._1,x._2 - 1)).++(msgStore)
          .map(x => (x._1,x._2 - 1))
          (List(),newMsgStore,newIteCount)
        }
    }
def sendMsg(edgeTriplet: EdgeTriplet[(List[Long],List[(Long,Int)],Int), Int]):Iterator[(Long,List[(Long,Int)])] = {
val oldMsg = edgeTriplet.srcAttr._2
val iteCount = edgeTriplet.srcAttr._3
if(iteCount == 1){
Iterator((edgeTriplet.dstId,List((edgeTriplet.srcId,n))))
      }else{
Iterator((edgeTriplet.dstId,oldMsg.par.filter(x => if(x._2 + iteCount == n + 1) true else false).toList))
      }
    }
def mergeMsg(a:List[(Long,Int)],b:List[(Long,Int)]):List[(Long,Int)] = {
      a.++(b)
    }    
  }
}
```

其中，u、sigma 参数用来声明生成图的顶点数的期望与标准差（对数正态分布）。另外，核心的数据结构是消息的数据结构与顶点属性的数据结构，它们分别是 List\[(Long,Int)\] 和 (List\[Long\],List\[(Long,Int)\],Int)，前者的消息是顶点 Id 以及消息生命值组成的元组。后者元组中的第一个元素是保存当前顶点的 _n_ 度邻居顶点 Id 的集合，用于结果输出；第二个元素用于存储发送过来的消息，第三个元素是当前迭代次数。

在本例中，没有考虑成环的情况，你可以试着实现一下这种情况，这也是本课时留给你的思考题。

其实求顶点的 n 度邻居这个需求并不常见，之所以举这个例子，是因为它很好地体现了 Pregel API 的核心用法：消息传递。你可以仔细理解这个带有生命值的消息的抽象，如果有更好的方法，欢迎与我讨论。
