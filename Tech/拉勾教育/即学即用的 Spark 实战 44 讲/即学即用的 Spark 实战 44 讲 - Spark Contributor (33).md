---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在开始今天的课程前，我们先来看看上一讲的思考题，如何理解训练好的模型也是一个 Transformer 呢？这说明 Transformer 只是描述了一种映射关系，而拟合数据，也就是训练模型的过程，就是为了得到这种映射关系。

今天的内容是 如何对数据进行预处理。 在机器学习实践中，数据科学家拿到的数据通常是不尽人意的，例如出现存在大量的缺失值、特征的值是不同的量纲、有一些无关的特征、特征的值需要再次处理等情况，这样的数据无法直接训练，因此我们需要对这些数据进行预处理。预处理在机器学习中是非常重要的步骤，如果没有按照正确的方法对数据进行预处理，往往会得到错误的训练结果。下面我先介绍几种常见的预处理方法。

### 数据标准化

通常，我们直接获得的数据包含了量纲，也就是单位，例如身高 180 cm、体重 75 kg。对于某些算法来说，如果特征的单位不统一，就无法直接进行计算，因此在很多情况下的预处理过程中，数据标准化是必不可少的过程。数据标准化的方法一般有 Z 分数法、最大最小法等。

#### Z 分数法

这种方法根据原始数据（特征）的均值（Mean）和标准差（Standard Deviation）进行数据标准化，从而将原始数据变换为 Z 分数，转化函数如下：

![Drawing 2.png](https://s0.lgstatic.com/i/image/M00/3D/19/Ciqc1F8pHlyAczDfAAAGj85eEFw493.png)

其中 _μ_ 为所有样本数据的均值， _σ_ 为所有样本数据的标准差。Spark MLlib 内置了 Z 分数标准化转换器 StandardScaler，下面的代码演示了通过 StandardScaler 对 Libsvm 数据集的特征进行标准化的过程：

```
import org.apache.spark.ml.feature.StandardScaler 
val dataFrame = spark.read.format("libsvm").load("data/mllib/sample_libsvm_data.txt") 
val scaler = new StandardScaler() 
.setInputCol("features") 
.setOutputCol("scaledFeatures") 
.setWithStd(true) 
.setWithMean(false) 
val scalerModel = scaler.fit(dataFrame) 
val scaledData = scalerModel.transform(dataFrame) 
scaledData.show()
```

#### 最大最小法

这种方法也称为离差标准化，是对原始数据的线性变换，使结果值映射到 \[0 - 1\] 之间。转换函数如下：

![Drawing 3.png](https://s0.lgstatic.com/i/image/M00/3D/24/CgqCHl8pHnCAUHvxAAAisWhfTiI536.png)

其中 max 为样本数据的最大值，min 为样本数据的最小值。这种方法的缺陷是当有新数据加入时，可能导致 max 和 min 出现变化，需要重新定义，但这种情况在训练过程中很少见。对于方差特别小的特征，这种方法可以增强其稳定性。Spark MLlib 内置了最大最小转换器 MinMaxScaler。下面的代码演示了通过 MinMaxScaler 对测试数据集的特征进行标准化的过程：

```
import org.apache.spark.ml.feature.MinMaxScaler 
import org.apache.spark.ml.linalg.Vectors 
val dataFrame = spark.createDataFrame(Seq( 
  (0, Vectors.dense(1.0, 0.1, -1.0)), 
  (1, Vectors.dense(2.0, 1.1, 1.0)), 
  (2, Vectors.dense(3.0, 10.1, 3.0)) 
)).toDF("id", "features") 
val scaler = new MinMaxScaler() 
.setInputCol("features") 
.setOutputCol("scaledFeatures") 
val scalerModel = scaler.fit(dataFrame) 
val scaledData = scalerModel.transform(dataFrame) 
println(s"Features scaled to range: [${scaler.getMin}, ${scaler.getMax}]") 
scaledData.select("features", "scaledFeatures").show()
```

#### _p_ 范数法

_p_ 范数法指的是计算样本的 _p_ 范数，用该样本除以该样本的 _p_ 范数，得到的值就是标准化的结果。 _p_ 范数的计算公式如下：

![Drawing 4.png](https://s0.lgstatic.com/i/image/M00/3D/19/Ciqc1F8pHoKAVk6gAAAMcm1E_XI378.png)

当 _p_ =1 时， _p_ 范数也叫 L1 范数，此时 L1 等于样本的所有特征值的绝对值相加。当 _p_ =2 时， _p_ 范数也叫 L2 范数，此时 L2 等于样本 _x_ 距离向量空间的原点的欧氏距离：

![Drawing 5.png](https://s0.lgstatic.com/i/image/M00/3D/19/Ciqc1F8pHoyAH6qCAAAKPd-gKDk980.png)

归一化的结果为：

![Drawing 6.png](https://s0.lgstatic.com/i/image/M00/3D/19/Ciqc1F8pHpSAOctmAAAIfKPjFb4327.png)

Spark MLlib 内置了 _p_ 范数标准化转化器 Normalizer，下面的代码演示了通过 Normalizer 对测试数据集的特征进行标准化的过程：

```
import org.apache.spark.ml.feature.Normalizer 
import org.apache.spark.ml.linalg.Vectors 
val dataFrame = spark.createDataFrame(Seq( 
(0, Vectors.dense(1.0, 0.5, -1.0)), 
(1, Vectors.dense(2.0, 1.0, 1.0)), 
(2, Vectors.dense(4.0, 10.0, 2.0)) 
)).toDF("id", "features") 
val normalizer = new Normalizer() 
.setInputCol("features") 
.setOutputCol("normFeatures") 
.setP(1.0) 
val l1NormData = normalizer.transform(dataFrame) 
l1NormData.show() 
val lInfNormData = normalizer.transform(dataFrame, normalizer.p -> Double.PositiveInfinity) 
println("Normalized using L^inf norm") 
lInfNormData.show()
```

### 缺失值处理

缺失值的处理需要根据数据的具体情况来定：如果特征值是连续型，通常用中位数来填充；如果特征值是标签型，通常用众数来补齐；某些情况下，还可以用一个显著区别于已有样本中该特征的值来补齐。Spark 并没有提供预置的缺失值处理的 Transformer，这通常需要自己实现，在后面的课时中，我们会实现一个对缺失值处理的自定义 Transformer。

### 特征抽取

特征抽取（Feature Extraction）指的是按照某种映射关系生成原有特征的一个特征子集，而前面提到的特征选择（Feature Selection）指的是根据某种规则对原有特征筛选出一个特征子集。特征选择和特征抽取有相同之处，它们都试图减少数据集中的特征数目，但具体方法不同，特征抽取主要是通过特征间的关系来操作，如组合不同特征得到新的特征，这样就改变了原来的特征空间；而特征选择是从原始特征数据集中选择出子集，两者是一种包含关系，没有更改原始的特征空间，如下图所示。

![Drawing 7.png](https://s0.lgstatic.com/i/image/M00/3D/24/CgqCHl8pHqCATQtNAAAsMz3U2wU664.png)

Spark 也提供了很多种特征抽取的方法，常见的如主成分分析、广泛应用于文本的 Word2Vector 等。

#### 主成分分析

如果在样本中无关的特征太多，就会影响模式的发现，我们需要用降维技术从样本中生成用来代表原有特征的一个特征子集。

在了解主成分分析（PCA）之前，需要先了解协方差的概念， _X_ 特征与 _Y_ 特征之间的协方差为：

![Drawing 8.png](https://s0.lgstatic.com/i/image/M00/3D/24/CgqCHl8pHqmAXpqaAAALYIFlgZY287.png)

如果协方差为正，说明 _X_ 和 _Y_ 是正相关关系；如果协方差为负，则说明是负相关关系；当协方差为 0 时， _X_ 和 _Y_ 相互独立。如果样本集 _D_ 有 _n_ 维特征，那么两两之间的协方差就可以组成一个 _n_ × _n_ 的矩阵，如下是 _n_ =3 的情况：

![Drawing 9.png](https://s0.lgstatic.com/i/image/M00/3D/19/Ciqc1F8pHreAQBtCAAATNVzupVs430.png)

然后需要对这个矩阵进行特征值分解，得到特征值和特征向量，再取出最大的 _m_ （ _m_ < _n_ ） 个特征值对应的特征向量（w1、w2、…、 wm），从而组成特征向量矩阵 **W**，对每个样本 _xi_ 执行如下操作，得到降维后的样本 _zi_，如下：

![Drawing 10.png](https://s0.lgstatic.com/i/image/M00/3D/24/CgqCHl8pHsWAFsvxAAAEt-j1_VQ537.png)

降维后的数据集为：

![Drawing 11.png](https://s0.lgstatic.com/i/image/M00/3D/24/CgqCHl8pHs6ARdiqAAAHmI3k1_k058.png)

下面以数据挖掘领域著名的鸢尾花数据集（IRIS： https://pan.baidu.com/s/1CPB8NQEN5crGC3MQ\_eVutA 密码： yey1）为例，来展示用 PCA 实现降维操作的过程。

鸢尾花数据集是常用的分类数据集，包含 150 个样本。样本总共分为 3 种类别、各 50 条，每个样本有 4 个维度，分别是花萼长度、花萼宽度、花瓣长度和花瓣宽度。操作过程的代码如下：

```
import org.apache.spark.ml.feature.{PCA, VectorAssembler} 
import org.apache.spark.sql.{Dataset, Row, SparkSession} 
import org.apache.spark.ml.feature.StandardScaler 
import org.apache.spark.sql.types.{DoubleType, StringType, StructField, StructType} 
import org.apache.spark.ml.Pipeline 
object IRISPCA { 
def main(args: Array[String]): Unit = { 
val spark = SparkSession 
    .builder() 
    .master("local[2]") 
    .appName("IRISPCA") 
    .getOrCreate() 
val fields = Array("id","Species","SepalLength","SepalWidth","PetalLength","PetalWidth") 
val fieldsType = fields.map( 
        r => if (r == "id"||r == "Species") 
               {StructField(r, StringType)} 
else
                {StructField(r, DoubleType)} 
    ) 
val schema = StructType(fieldsType) 
val featureCols = Array("SepalLength","SepalWidth","PetalLength","PetalWidth") 
val data=spark.read.schema(schema).csv("data/iris") 
val vectorAssembler = new VectorAssembler() 
    .setInputCols(featureCols) 
    .setOutputCol("features") 
val vectorData=vectorAssembler.transform(data) 
val standardScaler = new StandardScaler() 
    .setInputCol("features") 
    .setOutputCol("scaledFeatures") 
    .setWithMean(true) 
    .setWithStd(false) 
    .fit(vectorData) 
val pca = new PCA() 
    .setInputCol("scaledFeatures") 
    .setOutputCol("pcaFeatures") 
    .setK(2) 
val pipeline = new Pipeline() 
    .setStages(Array(vectorAssembler,standardScaler,pca))
val model = pipeline.fit(data) 
    model.transform(data).select("Species", "pcaFeatures").show(100, false) 
  } 
}
```

通过操作，可以发现降维后的数据只有两个维度，如下，这样就实现了我们的降维过程：

![Drawing 12.png](https://s0.lgstatic.com/i/image/M00/3D/25/CgqCHl8pHtmAA0eIAADQjIqfWM0637.png)

#### Word2Vector

在自然语言处理领域，训练集通常为纯文本，这样的数据是无法直接训练的，前面提到的 TF-IDF 就是一种生成词向量的方式。但是 TF-IDF 的缺点在于单纯以“词频”衡量一个词的重要性，不够全面，忽略了上下文信息，例如“阿里巴巴成立达摩院”与“人工智能应用有望加速落地”字面无任何相似之处，但它们之间有很强的关联，这用 TF-IDF 却无法体现。

而 Word2Vec 可以解决这个问题。 Word2Vec 最先出现在谷歌公司在 2013 年发表的论文“Efficient Estimation of Word Representation in Vector Space”中，作者是 Mikolov。Word2Vec 的基本思想是采用一个 3 层的神经网络将每个词映射成 _n_ 维的实数向量，为接下来的聚类或者比较相似性等操作做准备。这个 3 层神经网络实际是在对语言模型进行建模，在建模的同时也获得了单词在向量空间上的表示，即词向量，也就是说这个词向量是建模过程的中间产物，而这个中间产物才是 Word2Vec 的真正目标。

Word2Vec 采用了：CBOW 与 Skip-gram，前者可以根据上下文预测下一个词，后者可以根据当前词预测上下文。以 CBOW 为例，如下图所示。

![Drawing 13.png](https://s0.lgstatic.com/i/image/M00/3D/19/Ciqc1F8pHuSAFsjJAAB5_1HQCOo265.png)

我们选择一个固定的窗口作为语境（上下文）：t - 2 — t + 2，输入层是 4 个 _n_ 维的词向量（初始为随机值）；隐藏层做的操作是累计求和操作，隐藏层包含 _n_ 个结点；输出层是一棵巨大的二叉树，构建这棵二叉树的算法就是霍夫曼树，它的叶子结点代表了语料中的 M 个词语，语料中有多少个词，就有多少个叶子结点。

假设左子树为 1，右子树为 0，这样每个叶子结点都有唯一的编码。最后输出的时候，CBOW 采用了层次 Softmax 算法，隐藏层的每个结点都与树的每个结点相连，即霍夫曼树上的每个结点都会有 M 条边，每条边都有权重，我们需要达到的是在输入的上下文一定的情况下，预测词 W 的概率最大。以 010110 为例，由霍夫曼树的定义可知树有 5 层，我们希望在根结点，词向量与根结点相连，也就是在第一层，希望经过回归运算后第一位等于 0 的概率尽量等于 1 。依次类推，在第二层，希望第二位的值等于 1 的概率尽可能等于 1。这样一直下去，路径上所有的权重乘积就是预测词在当前上下文的概率 _P_ ( _wt_ )，而在语料中我们可以得到当前上下文的残差 1- _P_ ( _wt_ )，这样的话，就可以使用梯度下降来学习参数了。

由于不需要标注，Word2Vec 本质上是一种无监督学习，对自然语言处理有兴趣的读者不妨仔细阅读谷歌公司的那篇论文。

Spark MLlib 内置了 Word2Vec 转换器 Word2Vec，在下面的例子中对文档使用了 Word2Vec 转换器：

```
import org.apache.spark.ml.feature.Word2Vec 
import org.apache.spark.ml.linalg.Vector 
import org.apache.spark.sql.Row 
val documentDF = spark.createDataFrame(Seq( 
"Hi I heard about Spark".split(" "), 
"I wish Java could use case classes".split(" "), 
"Logistic regression models are neat".split(" ") 
).map(Tuple1.apply)).toDF("text") 
val word2Vec = new Word2Vec() 
.setInputCol("text") 
.setOutputCol("result") 
.setVectorSize(3) 
.setMinCount(0) 
val model = word2Vec.fit(documentDF) 
val result = model.transform(documentDF) 
result.collect().foreach { case Row(text: Seq[_], features: Vector) => 
println(s"Text: [${text.mkString(", ")}] => \nVector: $features\n") }
```

### 特征选择

特征选择的目标通常是提高预测准确性、提升训练性能，以便能够更好地解释模型。特征选择的一种很重要的思想就是对每一维的特征打分，这样就能选出最重要的特征了。基于这种思想的方法有：卡方检验、信息增益以及相关系数。相关系数主要量化的是任意两个特征是否存在线性相关，信息增益会在下一节介绍，本小节主要介绍卡方检验。

卡方检验是以卡方分布为基础的一种常用假设检验方法，它的原假设是观察频数与期望频数没有差别。该检验的基本思想是：首先假设原假设成立，基于此前提计算出 x2 值，它表示观察值与理论值之间的偏离程度。根据卡方分布及自由度可以确定在原假设成立的条件下，获得当前统计量及更极端情况的概率 P 。如果 P 值很小，说明观察值与理论值偏离程度太大，应当拒绝无效假设，表示比较资料之间有显著差异；否则就不能拒绝无效假设，尚不能认为样本所代表的实际情况和理论假设有差别。

假设样本的某一个特征，它的取值为 A 和 B 两个组，而样本的类别有 0 和 1 两类。对样本进行统计，可以得到如下表所示的统计表。

组别

0

1

合计

_A_

19

24

43

_B_

34

10

44

合计

53

34

87

从上表中，我们可看出 _A_ 和 _B_ 组对分类结果有很大的影响，但这不排除抽样的影响。首先假设该特征有结果是独立无关的，随机取一个样本，属于类 0 的概率为 (19 + 34)/(19 + 34 + 24 + 10)= 60.9%。接下来，我们需要根据上表得到一个理论值表，如下表所示。

组别

0

1

合计

_A_

43 × 0.609 = 26.2

43 × 0.391 = 16.8

43

_B_

44 × 0.609 = 26.8

44 × 0.391 = 17.2

44

如果两个变量是独立无关的，那么上表中的理论值与实际值的差别会非常小。

卡方值的计算公式为：

![Drawing 15.png](https://s0.lgstatic.com/i/image/M00/3D/25/CgqCHl8pHyqAAHYvAAAI87-YsrM071.png)

其中 _A_ 为实际值，也就是第一个表中的 4 个数据， _T_ 为理论值，也就是上表中给出的 4 个数据，计算后得到卡方值为 10.01。得到该值后，需要在给定的置信水平下查得卡方分布临界值，如下表所示。

_P_

_n_ ¢

0.995

0.99

0.975

0.95

0.9

0.75

0.5

0.25

0.1

0.05

0.025

0.01

0.005

1

…

…

…

…

0.02

0.1

0.45

1.32

2.71

3.94

5.02

5.63

7.88

显然 10.01 > 7.88，也就是说该特征与分类结果无关的概率小于 0.5%，换言之，我们应该保留这个特征。

Spark MLlib 内置了卡方检验组件 ChiSqSelector，下面的例子中对某个测试数据集采用了卡方检验：

```
import org.apache.spark.ml.feature.ChiSqSelector 
import org.apache.spark.ml.linalg.Vectors 
val data = Seq( 
  (7, Vectors.dense(0.0, 0.0, 18.0, 1.0), 1.0), 
  (8, Vectors.dense(0.0, 1.0, 12.0, 0.0), 0.0), 
  (9, Vectors.dense(1.0, 0.0, 15.0, 0.1), 0.0) 
) 
val df = spark.createDataset(data).toDF("id", "features", "clicked") 
val selector = new ChiSqSelector() 
.setNumTopFeatures(3) 
.setFeaturesCol("features") 
.setLabelCol("clicked") 
.setOutputCol("selectedFeatures") 
val result = selector.fit(df).transform(df) 
println(s"ChiSqSelector output with top ${selector.getNumTopFeatures} features selected") 
result.show()
```

### 小结

本课时主要介绍了数据预处理的方法，结合 Pipeline API 可以发现，编程已经完全不是预处理的重点，如何根据手上的数据集和目标选择预处理方法才是最重要的。老实说，MLlib 并没有提供非常多的预处理组件，也就是前面课时学习的 Transformer，好在它提供了自定义Transformer 的接口。

最后给大家留一个思考题：实现一个任意逻辑的自定义 Transformer。
