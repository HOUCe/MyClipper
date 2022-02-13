---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在本模块中，我们将学习 Spark 如何处理图，也就是 Spark 的图挖掘套件 GraphX。虽然图这种数据结构在最近几年中，越来越多地出现在业务场景中，但平心而论，图的使用频率相比前面所学的内容还没有那么频繁。但是，一旦有这方面的需求，无论是工程师还是科学家，都可以用 Spark 提供的解决方案很好地完成任务，甚至可以说是“**屠龙技**”也不为过，经过本模块的学习之后，相信你也会有这样的感受。

本课时主要围绕图这种核心结构介绍，分为以下三个部分：

-   图结构
    
-   图存储
    
-   图相关计算场景和技术
    

### 图结构

**图是一种较线性表和树更为复杂的数据结构，线性表和树分别表现的是一对一和一对多的关系，而图则表达的是多对多的关系。**如下图所示，G1 是一个简单的图，其中 V1、V2、V3、V4 被称作**顶点（Vertex）**，任意两个顶点之间的通路被称为**边（Edge）**，它可以由（V1、V2）有序对来表示，这时称 G1 为有向图，意味着边是有方向的，若以无序对来表示图中一条边，则该图为无向图，如 G2。

![2.png](https://s0.lgstatic.com/i/image/M00/2D/E0/CgqCHl8EGx-AeYjiAADuOL7FRm8484.png)

在 G1 中，与顶点相关联的边的数量被称为顶点的**度（Degree）**。其中，以顶点为起点的边的数量被称为该顶点的**出度（OutDegree）**，以顶点为终点的边的数量被称为该顶点的**入度（InDegree）。**

以 G1 中的 V1 举例，V1 的度为 3，其中出度为 2，入度为 1。在无向图 G2 中，如果任意两个顶点之间是连通的，则称 G2 为**连通图（Connected Graph）**。在有向图中 G1 中，如果任意两个顶点 Vm、Vn 且 m ≠ n，从 Vm 到 Vn 以及从 Vn 到 Vm 之间都存在通路，则称 G1 为**强连通图（Strongly Connected Graph）**。任意两个顶点之间若存在通路，则称为**路径（Path）**，用一个顶点序列表示，若第一个顶点和最后一个顶点相同，则称为**回路或者环（Cycle）**。

### 图存储

由于图的结构比较复杂，所以无法存储在顺序映像的存储结构中，本课时中，我将为你介绍常用的几种图存储结构。

#### 邻接矩阵

**邻接矩阵是表示顶点之间相邻关系的矩阵**。一般用二维数组存储，若图有 n 个顶点，则矩阵大小为 n \* n，下面我用邻接矩阵来表示 G1、G2 ：

![image (8).png](https://s0.lgstatic.com/i/image/M00/2D/E0/CgqCHl8EG06ASTpoAAAdHb0EYLk424.png)

![image (9).png](https://s0.lgstatic.com/i/image/M00/2D/D4/Ciqc1F8EG1WAAZB7AAAup5N2anE784.png)

其中矩阵的行标为起点，列标为终点。**可以看到无向图必为对称矩阵**，这一点可以在存储时进行优化。

#### 邻接表

**邻接表表示图的一种链式存储结构**，在邻接表中，会对每个顶点建立一个单链表，每个单链表中保存了所有依附于该顶点的边，每个单链表中的结点由三个域组成：**邻接点域、链域和数据域**。其中，邻接点域指向与该顶点相邻的顶点，链域指向下一个与该顶点相关联的顶点，数据域保存了边相关的属性信息。用邻接表来表示 G1 ，则如下图所示：

![3.png](https://s0.lgstatic.com/i/image/M00/2D/E0/CgqCHl8EG2yAAM9XAACLIqI93yU586.png)

对于无向图，V1 链表的结点个数就是 V1 的度数。而对于有向图来说，则只是 V1 的出度，如果想统计入度，则比较麻烦，需要遍历整个图。这种情况，可以通过建立一个逆邻接表来解决。在逆邻接表中，链域保存的是指向该顶点的下一个顶点，G1 的逆邻接表如下图所示：

![图片](https://uploader.shimo.im/f/GnomtreeXb3gllfc.png!thumbnail)

如果是无向图 G2，邻接表则如下图所示：

![4.png](https://s0.lgstatic.com/i/image/M00/2D/E0/CgqCHl8EG3mAObZKAABli1AXfNM782.png)

#### 邻接多重表

虽然邻接表是一种很有效的存储结构，但对于无向图来说，当同一条边（Vm、Vn）存在于第 m 个链表与第 n 个链表中时，对于修改图来说并不是特别方便。因此，进行这一类操作的无向图一般采用邻接多重表。在邻接多重表中，分为两个表：顶点表和边表，边表结构如下所示：

![5.png](https://s0.lgstatic.com/i/image/M00/2D/D5/Ciqc1F8EG4qAWzB8AABAOblVRhI629.png)

其中，mark 为标志域，可以用来标记该条边是否被搜索过；ivex 和 jvex 为该条边的起点和终点，ilink 指向下一条顶点 ivex 的出边；jlink 指向下一条以顶点 jvex 的入边，info 保存和边相关的信息，顶点表结构如下所示。

![6.png](https://s0.lgstatic.com/i/image/M00/2D/E0/CgqCHl8EG5OACrlvAAAU9SsfBj4109.png)

其中，data 域保存了和该顶点相关的信息，firstedge 域指向该顶点的第一条出边，下图中是 G2 以邻接多重表的形式进行存储：

![7.png](https://s0.lgstatic.com/i/image/M00/2D/E0/CgqCHl8EG7aAMpbyAADsnIU2REQ696.png)

比较流行的 Neo4j 图数据库就是采取邻接多重表的结构保存数据的。

#### 十字链表

邻接表只体现了出度，虽然我们可以用逆邻接表来弥补入度信息，但是否存在一种将邻接表和逆邻接表结合起来的数据结构呢？这就是十字链表。十字链表分为顶点表和边表这两个表。边表结构如下所示：

![8.png](https://s0.lgstatic.com/i/image/M00/2D/E1/CgqCHl8EG-yAfpXkAAAnsxpK7Do338.png)

其中 tailvex 和 headvex 为边的起点和终点，hlink 指向下一条顶点 tailvex 的出边；tlink 指向下一条以headvex为顶点的入边，顶点表结构如下所示：

![9.png](https://s0.lgstatic.com/i/image/M00/2D/D5/Ciqc1F8EG_aANSEhAAAcTwANkWA141.png)

其中 data 域保存了和该顶点相关的信息，firstIn 和 firstOut 是两个指针域，分别指向该顶点的第一条入边和第一条出边，如下图所示，该结构代表 G2 以十字链表的形式存储：

![Lark20200707-152616.png](https://s0.lgstatic.com/i/image/M00/2D/DF/Ciqc1F8EI8KAJJ_oAADiV7bncEs483.png)

**十字链表与邻接多重表的不同之处在于顶点表多了表示第一条入边的指针域**。

#### 边集数组

**边集数组是一种利用一维数组存储图中所有边的图表示方法**。该数组中每个元素都用来存储一条边的起点、终点（对于无向图，可选定边的任一端点为起点或终点）和边属性。此外，边集数组通常包括一个边数组和一个顶点数组。这种方式非常适合将数据进行分区，所以 GraphX 以及很多图数据库都采取这种方式存储数据。

### 图相关计算场景和技术

图面对的计算场景与普通的数据处理场景类似，主要可以分为 3 类：

-   OLTP
    
-   OLAP
    
-   离线处理
    

OLTP 和 OLAP 对应事务和分析场景，这两类场景通常由图数据库负责，它的特点是对实时性要求很高，比如在图中新增若干顶点并新增与之相连的边，这属于事务。再比如，查询以某个顶点为中心的，三度以内的顶点，这属于查询。这两类操作目前主流的图数据库都能很好地完成。

#### Neo4j 与 Cypher

![Drawing 8.png](https://s0.lgstatic.com/i/image/M00/2D/D5/Ciqc1F8EHByALpVfAAAsX4lvgPc342.png)

Neo4j 是一个比较老牌的开源图数据库，目前在业界的使用也较为广泛，它提供了一种简单易学的查询语言 Cypher。Neo4j 采取类似于邻接多重表的数据结构存储数据，查询与插入速度较快，但由于没有分布式版本，图容量有限，而且一旦图变得非常大，如数十亿顶点，数百亿边，查询速度将变得缓慢。Neo4j 分为社区版和企业版，企业版有一些高级功能，但需要授权，非常昂贵，动辄数十万一年。

#### Apache TinkerPop与 Gremlin

![Drawing 9.png](https://s0.lgstatic.com/i/image/M00/2D/D6/Ciqc1F8EHCaAaa5tAADiikSqnUA979.png)

TinkerPop 是一个开源的、面向集成的图计算框架，它包含一系列的组件，上图中每一个卡通形象都代表一个组件，分别是：Blueprints、Pipes、Frames、Furnace、Rexster 和 Gremlin。

中间的小人代表 Gremlin，它是 TinkerPop 的图查询语言，所有支持 TinkerPop 的系统都可以互相集成，例如原生的 TinkerPop 是用内存作为图存储，这无疑达不到生产环境的标准。你可以使用 MySQL 作为存储，也可以使用 NoSQL 分布式数据库，如 Cassandra、HBase、BerkeleyDB 等。索引也可以采用 Elasticsearch 或者 Lucene。使用 NoSQL 数据库作为存储引擎，只是实现了存储分布式，解决了图容量问题，但是没有实现查询分布式，使用 NoSQL 数据库作为存储引擎的产品有 Titan、JanusGraph。

TinkerPop 的核心是集成，如下图所示，用户可以根据自己需要，选择不同的组件，博采众家之长，并实现 TinkerPop 要求的标准，从而构建出一个图数据库，从这个意义上来说，TinkerPop 确实不单是一个数据库，而是一个框架。

![Drawing 10.png](https://s0.lgstatic.com/i/image/M00/2D/E1/CgqCHl8EHC6ANFL-AAC8Pt_4UzU445.png)

值得一提的是，和关系型数据库一样，得益于社区和资本的助力，国产图数据库的发展也非常快，TigerGraph、Nebula 也非常值得你关注。

再来看看离线处理，这类场景通常代表一些比较复杂的分析和算法，如基于图的聚类，PageRank 算法等，这类计算任务对于图数据库来说就很难胜任了，主要由一些图挖掘技术来负责，这里我列出了两个比较常见的技术类型：

#### Pregel

Pregel 是 Google 于 2010 年在 SIGMOD 会议上发表的《Pregel: A System for Large-Scale Graph Processing》论文中提到的海量并行图挖掘的抽象框架，Pregel 与 Dremel 一样，是 Google 新三驾马车之一，它基于 BSP 模型（Bulk Synchronous Parallel，整体同步并行计算模型），将计算分为若干个超步（super step），在超步内，通过消息来传播顶点之间的状态。Pregel 可以看成是同步计算，即等所有顶点完成处理后再进行下一轮的超步，它的代表作就是 Spark 基于 Pregel 论文实现的海量并行图挖掘框架 GraphX，类似的还有 Yahoo 基于 MapReduce 实现的 Giraph 框架。

#### NetworkX

**NetworkX 是一个用 Python 语言开发的图论与复杂网络建模工具**，内置了常用的图与复杂网络分析算法，可以很方便地进行复杂网络数据分析、仿真建模等工作。用 NetworkX 可以很轻易地计算出基于图的一些指标和特征，这在传统方法中是非常困难的。NetworkX 与 GraphX 都是图计算工具。不同的是，GraphX 偏向于超大规模的图处理，例如千万顶点级别，而 NetworkX 处理的数据规模通常在百万顶点以内，十万顶点以内的图，算法效率最好。

GraphX 与 NetworkX 相比，最主要的区别在于处理数据的容量，GraphX 处理的数据量可以随着计算能力的增加而线性增长，十亿百亿顶点规模的图对于 GraphX 来说不在话下，但是由于 NetworkX 是单机计算包，所以图的容量最好不要超过百万顶点。此外，NetwrokX 对算法的封装程度很高，开箱即用，GraphX则更加底层。

### 小结

由于图结构的特殊性，没有接触过的同学未免会对这个概念有些陌生，本课时主要讲解了图存储和相关计算场景和对应的技术类型，主要是为后面的学习做铺垫，值得注意的是，图存储中介绍的 5 种数据结构非常底层，后面提到的技术对于图数据本身的存储无外乎这几种。

这里给你留一个思考题：

前面提到，Neo4j 采用的是邻接多重表作为自己的存储结构，那么 Tinkerpop 采用的是哪种存储结构呢？它们各自又有哪些优点呢？
