---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在开始之前，先来看看上个课时的思考题。在配置分类器时，我们需要设置的参数主要有：

-   树的个数；
    
-   树的最大深度；
    
-   特征子集选取策略；
    
-   纯度。
    

在特征子集的选取策略中可以配置信息增益、信息增益率以及基尼系数。

然后我们开始今天的课程。上一课时中介绍了随机森林和决策树分类器，它们都属于监督学习，本节将介绍聚类算法，它属于机器学习的另一种应用——**无监督学习**。

### 物以类聚

通俗地说，人们习惯将相似的东西归为一类，这就是“物以类聚”。先来看几个例子，用苹果举例，如下图所示。

![Drawing 0.png](https://s0.lgstatic.com/i/image/M00/40/52/CgqCHl8yU0CALbDUAABrSRl7RCg892.png)

一共有 6 个苹果，如果将这些苹果分类，我们可以很自然地将其分为两类，因为前 4 个的大小明显和后面不同。再来看看另外 6 个苹果，如下图所示。

![Drawing 1.png](https://s0.lgstatic.com/i/image/M00/40/47/Ciqc1F8yU0eAUStOAAChV1NLmds129.png)

对这 6 个苹果进行分类，同样很容易分为两类，前 4 个苹果的颜色和后面的明显不同。在上面的两次分类过程中，已经完成了两次聚类，聚类的依据第一次是大小，第二次是颜色。接下来看看最后 6 个苹果，如下图所示。

![Drawing 2.png](https://s0.lgstatic.com/i/image/M00/40/47/Ciqc1F8yU0yAHHBqAAC5gQcgG5E448.png)

现在还能一口气将这 6 个苹果进行分类吗？这 6 个苹果有大有小，有红有绿，确实令人困惑，如果再加上第 2 个和第 5 个苹果很甜、其余苹果很酸等特征，问题就更棘手了。人对于单一维度的数据很敏感，维度一多就有些力不从心，而计算机恰恰擅长处理这些数据，聚类算法就是“物以类聚”这一朴素思想的数学体现。

**聚类**是把相似的对象通过静态分类的方法分成不同的组别或者更多的子集，使得同一个子集中的成员对象都有相似的一些属性，常见的包括在坐标系中更加短的空间距离。一般把聚类归纳为一种非监督式学习，它与分类算法最大的不同在于训练集没有标签。

### _K_ 均值聚类算法

到目前为止，我们对聚类的理解可以用下图来表示。聚类的目标就是将相似的物体进行分组，并将其标注，图中的相似性体现在点与点之间的距离。

![Drawing 3.png](https://s0.lgstatic.com/i/image/M00/40/47/Ciqc1F8yU1iAa_T7AADWSLFVU3A993.png)

_K_ 均值算法是一种被广泛使用的直接聚类算法，位列数据挖掘十大算法之二，可见其影响力。 _K_ 均值算法是一种**迭代型聚类算法**，它将一个给定的数据集分为用户指定的 _K_ 个聚簇，速度较快，易于修改。从上面这句话可以得出 _K_ 均值算法的输入对象为数据集 _D_ 和一个非常关键的 _K_ 值， _K_ 值也就是用户认为数据集 _D_ 应该被分为几类，数据集 _D_ 可以被认为是 _d_ 维向量空间中的一些点（ _D_ ={ _xi_ | _i_ = 1,…, _N_ }，其中 _xi∈Rd_ 表示数据集 _D_ 中第 _i_ 个对象）。

在 _K_ 均值算法中，每个聚簇都用一个点来代表，这些聚簇用集合 _C_ ={ _cj_ | _j_ = 1,…, _k_ } 来表示，这 _K_ 个代表聚簇有时也被称为聚簇均值或聚簇中心。聚类算法通常用相似度的概念对点集进行分组，具体到 _K_ 均值算法，默认的相似度标准为欧氏距离。 _K_ 均值的实质是要最小化一个如下的非负代价函数：

![Drawing 4.png](https://s0.lgstatic.com/i/image/M00/40/53/CgqCHl8yU2qAB5lTAABeO6TQn-Y438.png)

换言之， _K_ 均值的最小化目标是每个点 _xi_ 和离它最近的聚簇中心 _cj_ 之间的欧氏距离的平方和，这也是 _K_ 均值的目标函数。

**下面是** K **均值算法的伪代码**：

输入：数据集 D，聚簇数 k

输出：聚簇代表集合 C，聚簇成员向量 m

/_初始化聚簇代表 C_/

从数据集 D 中随机挑选 k 个数据点3

使用这 k 个数据点构成初始聚簇代表集合 C

repeat

/_再分数据_/

将 D 中的每个数据点重新分配至与之最近的聚簇均值

更新 m（mi 表示D中第 i 个点的聚簇标识）

/_重定均值_/

更新 C（cj 表示第 j 个聚簇均值）

until 目标函数  
![Drawing 5.png](https://s0.lgstatic.com/i/image/M00/40/48/Ciqc1F8yU5SAc4YdAABABs5NrzY650.png)  
收敛

从上面的伪代码可以看出，算法主要包括两个交替执行的步骤，即再分数据和重定均值，并且是通过随机选取 _K_ 个点来启动算法。

**再分数据**指的是：将每个数据点分配到当前与之最近的那个聚簇中心，同时打破了上次迭代确定的归属关系。这一步会对全部数据进行一个新的划分。

**重定均值**指的是：重新确定每一个聚簇中心，即计算所有分配给该聚簇的数据点的中心（如算术平均值），这也是 _K_ 均值的名称由来。

当满足收敛条件时，算法停止。

### 鸢尾花数据集进行聚类

本节将介绍用 _K_ 均值算法对鸢尾花数据集（也就是预处理课时中用到的 iris 数据集）进行聚类的过程，**在聚类之前，需要先对特征用 PCA 进行降维，用 Z 分数进行归一化**。由于鸢尾花数据本身带有标签列（Species，一共有 3 种类型，即 setosa 、 versicolor 和 virginica ），因此最后可以用该列来评估聚类效果，代码如下：

```
import org.apache.spark.ml.feature.{PCA, VectorAssembler} 
import org.apache.spark.sql.{Dataset, Row, SparkSession} 
import org.apache.spark.ml.feature.StandardScaler 
import org.apache.spark.sql.types.{DoubleType, StringType, StructField, StructType} 
import org.apache.spark.ml.Pipeline 
import org.apache.spark.ml.feature.MinMaxScaler 
import org.apache.spark.ml.clustering.KMeans 
import org.apache.spark.ml.evaluation.ClusteringEvaluator 
object IRISKmeans { 
def main(args: Array[String]): Unit = { 
val spark = SparkSession 
    .builder() 
    .master("local[2]") 
    .appName("IRISKmeans") 
    .getOrCreate() 
val fields = Array("id","SepalLength","SepalWidth","PetalLength","PetalWidth","Species") 
val fieldsType = fields.map( 
      r => if (r == "id"||r == "Species") 
             {StructField(r, StringType)} 
else
             {StructField(r, DoubleType)} 
    ) 
val schema = StructType(fieldsType) 
val featureCols = Array("SepalLength","SepalWidth","PetalLength","PetalWidth") 
val data = spark.read.schema(schema) 
    .option("header", false) 
    .csv("data/iris/") 
val vectorAssembler = new VectorAssembler() 
    .setInputCols(featureCols) 
    .setOutputCol("features") 
val pca = new PCA() 
    .setInputCol("features") 
    .setOutputCol("pcaFeatures") 
    .setK(2) 
val scaler = new StandardScaler() 
    .setInputCol("features") 
    .setOutputCol("scaledFeatures") 
    .setWithStd(true) 
    .setWithMean(false) 
val kmeans = new KMeans() 
    .setK(3) 
    .setSeed(System.currentTimeMillis()) 
    .setMaxIter(10) 
val pipeline = new Pipeline() 
    .setStages(Array(vectorAssembler,pca,scaler)) 
val model = pipeline.fit(data) 
val predictions = model.transform(data) 
    predictions.show() 
val evaluator = new ClusteringEvaluator() 
val silhouette = evaluator.evaluate(predictions) 
    println(s"Silhouette with squared euclidean distance = $silhouette") 
  } 
}
```

下面是部分的聚类结果：

![Drawing 6.png](https://s0.lgstatic.com/i/image/M00/40/53/CgqCHl8yU6WAUgR_AAJJxj-Boic557.png)  
……

![Drawing 7.png](https://s0.lgstatic.com/i/image/M00/40/48/Ciqc1F8yU6yAKNibAAIV7Wmjm8U735.png)  
……

![Drawing 8.png](https://s0.lgstatic.com/i/image/M00/40/53/CgqCHl8yU7OAcHxQAAIqHMXe320159.png)

可以看到，聚类结果为 setosa 类型的样本全部被归到类 1，无一例外；versicolor 类型的样本大部分被归到类 2，极少部分被归到类 0；virginica 类型的样本大部分被归到类 0，少部分被归到类 2，结果基本正确。

### 评估聚类结果

聚类是一种典型的无监督学习，与监督学习很容易评估模型性能不同，无监督学习通常没有一个标准。那么如何才能判断聚类的结果是优质的，这同样是一个值得讨论的问题。我们先来看一份数据，假设一份数据在数据空间中均匀分布，如下图所示：

![Drawing 9.png](https://s0.lgstatic.com/i/image/M00/40/48/Ciqc1F8yU7qAbdSGAAAxoq-vQ68447.png)

那么可以想象这份数据的聚类结果质量一定不高，数据的均匀分布导致这些聚簇没有任何实际意义。我们还可以通过霍普金斯统计量来检测变量的空间随机性，这里就不展开讲解了。

一般来说，我们可以通过簇间距离和簇内距离来评估聚类质量，簇间距离指的是两个簇中心之间的距离，而簇内距离是指簇内部成员间的距离。 我们可以这样判断一次优质的聚类结果，如下图所示，有较大的簇间距离和较小的簇内距离，也就是说，优质的聚类是簇中的点尽量紧密，而簇和簇之间尽量远离。

![Drawing 10.png](https://s0.lgstatic.com/i/image/M00/40/53/CgqCHl8yU8OAAiEJAAAyGf6HRaI127.png)

![Drawing 11.png](https://s0.lgstatic.com/i/image/M00/40/53/CgqCHl8yU8mAfAFoAAAphYBFt9I725.png)

### 小结

本课时学习了另一类有代表性的无监督学习算法：聚类算法。在编写 Pipeline 时，我们也用到了前面学习的 Transformer 来进行降维和归一化处理，有了前面的基础，相信同学们对预处理会有更深的理解。

最后给大家留个**思考题**：如果不进行归一化处理，那么会对聚类结果有什么影响呢？
