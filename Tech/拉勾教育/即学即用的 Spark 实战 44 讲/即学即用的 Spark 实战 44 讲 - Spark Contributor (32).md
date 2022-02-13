---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在开始今天的课程前，我们先来讲解一下上节课的课后思考题。深度学习与机器学习的不同之处在于：

-   数据量大小。深度学习通常需要更多的样本才能达到更好的效果，所以通常深度学习的训练时间更长。
    
-   硬件区别。深度学习算法通常涉及大量浮点运算，样本量也巨大，而 GPU 天然的海量流处理器架构非常适合并行计算，所以一般复杂的深度学习应用通常需要 GPU 的硬件架构。
    
-   特征选择。一般机器学习解决问题时，都需要专家指定或者先验知识来确定特征，如信用模型，这些特征在很大程度上影响了模型的准确性。
    
-   解决问题的方法。当使用传统机器学习方法解决问题时，经常采取化整为零、分别解决、再合并结果求解的策略。而深度学习是端到端的模型，输入训练数据，再直接输出最终结果，让深度神经网络自己学习如何提取关键特征，比如对一张有着多个目标的照片进行目标检测，需要识别出目标的类别，并指出图中所在位置。典型机器学习方法会将这个问题分为两步：目标检测与目标识别。首先，使用边框检测技术，扫描全图找到所有可能的对象，对这些对象使用目标识别算法，如支持向量机，识别出相关物体。深度学习方法则按照端到端的方式处理这个问题，比如通过卷积神经网络就能够实现目标的定位与识别，也就是将原始图像输入到卷积神经网络模型中，再直接输出图像中目标的位置和类别。
    
-   可解释性。同神经网络算法一样，深度学习模型很难进行解释，这也使深度学习算法无法应用于很多要求模型可解释的场景，如金融等。
    

接下来我们开始讲解今天的内容，标准化机器学习流程：ML Pipeline。Spark MLlib 是 Spark 机器学习套件， 它的目的是成为大数据机器学习的最佳实践。为了简化机器学习过程并使其可扩展，Spark ML API 引入了 Pipelines API（管道），这类似于 Python 机器学习库 Scikit-Learn 中的 Pipeline，它采用了一系列 API 定义并标准化了上一课时中我们学习的机器学习工作流，包含了数据收集、预处理、特征抽取、特征选择、模型拟合、模型验证、模型评估等一系列阶段。例如，对文档进行分类时，也许会包含分词、特征抽取、训练分类模型，以及调优等过程。大多数机器学习库不是为分布式计算而设计的，也不提供 Pipeline 的创建与调优，而这就是 Spark ML PipeLines 要做的。

Spark ML Pipelines 就是对分布式机器学习过程进行模块化地抽象，这样可以使多个算法合并成一个 Pipeline 或者使工作流变得更加容易，下面是 Pipelines API 的关键概念：

-   DataFrame：DataFrame 与 Spark SQL 中用到的 DataFrame 一样，是 Spark 的基础数据结构，贯穿了整个 Pipeline。它可以存储文本、特征向量、训练集以及测试集。除了常见的类型，DataFrame 还支持 Spark MLlib 特有的 Vector 类型。
    
-   Transformer：Transformer 对应了数据转换的过程，它接收一个 DataFrame，在它的作用下，会生成一个新的 DataFrame。在机器学习中，在涉及特征转换的过程中经常会用到它。Transformer也可以用于使训练完成后的模型将特征数据集（测试集）转换为带有预测结果的数据集的场景。Transformer 必须实现 transform() 方法。
    
-   Estimator：从上面 Transformer 的定义中可以得知， **训练完成好的模型也是一个 Transformer** ，所以 Estimator 包含了一个可以让数据集拟合出一个 Transformer 的算法。 Estimator 必须实现 fit() 方法。
    
-   Pipeline：一个 Pipeline 可以将多个 Transformer 和 Estimator 组装成一个特定的机器学习工作流。
    
-   Parameter：所有的 Estimator 和 Transformer 共用一套通用的 API 来指定参数。
    

文档分类是一个在自然语言处理中非常常见的应用，如垃圾邮件监测、情感分析等。下面，我们将通过一个文档分类的例子来让读者对 Spark 的 Pipeline 有一个感性的理解。简单来说，任何文档分类应用都需要以下 4 步：

1.  将文档分词。
    
2.  将分词的结果转换为词向量。
    
3.  学习模型。
    
4.  预测（是否为垃圾邮件或者正负情感）。
    

比如在垃圾邮件监测中，我们需要通过邮件正文甄别出哪些是垃圾邮件。垃圾邮件的正文一般会是一段文字，如：“代开各种发票，手续费极低，请联系我。”但这样一段文字是无法直接应用于 Estimator 的，需要将其转换为特征向量。一般做法是用一个词典构建一个向量空间，其中每一个维度都是一个词，出现过的为 1 ，未出现的为 0 ，再根据文档中出现的词语的频数，用 TF-IDF 算法为词维度赋予权重。这样的话，每个文档就能被转换为一个等长的特征向量，如下：

(0, 0, …, 0.27, 0, 0, …, 0.1, 0) ，接着就可以用它来拟合模型并输出测试结果。

我们用一个流程图来表示整个过程，如下图所示： Tokenizer 和 HashingTF 为 Transformer，作用分别是分词和计算权重，训练出的模型也是 Transformer，用来生成测试结果；Estimator 采用的是逻辑回归算法（LR）；DS0-DS3 都是不同阶段输出的数据，这就是一个完整意义上的 Pipeline。

![1.png](https://s0.lgstatic.com/i/image/M00/3A/CE/Ciqc1F8igOiAGSXYAAEv2-wuZAs564.png)

下面用代码实现整个 Pipeline，如下：

```
import org.apache.spark.ml.{Pipeline, PipelineModel} 
import org.apache.spark.ml.classification.LogisticRegression 
import org.apache.spark.ml.feature.{HashingTF, Tokenizer} 
import org.apache.spark.ml.linalg.Vector 
import org.apache.spark.sql.Row 
import org.apache.spark.sql.SparkSession 
object PipelineExample{ 
def main(args: Array[String ]): Unit = { 
val spark = SparkSession 
    .builder 
    .master("local[2]") 
    .appName("PipelineExample") 
    .getOrCreate() 
import spark.implicits._ 
val training = spark.createDataFrame(Seq( 
      (0L, "a b c d e spark", 1.0), 
      (1L, "b d", 0.0), 
      (2L, "spark f g h", 1.0), 
      (3L, "hadoop mapreduce", 0.0) 
    )).toDF("id", "text", "label") 
val tokenizer = new Tokenizer() 
    .setInputCol("text") 
    .setOutputCol("words") 
val hashingTF = new HashingTF() 
    .setNumFeatures(1000) 
    .setInputCol(tokenizer.getOutputCol) 
    .setOutputCol("features") 
val lr = new LogisticRegression() 
    .setMaxIter(10) 
    .setRegParam(0.001) 
val pipeline = new Pipeline() 
    .setStages(Array(tokenizer, hashingTF, lr)) 
val model = pipeline.fit(training) 
    model.write.overwrite().save("/tmp/spark-logistic-regression-model") 
    pipeline.write.overwrite().save("/tmp/unfit-lr-model") 
val sameModel = PipelineModel.load("/tmp/spark-logistic-regression-model") 
val test = spark.createDataFrame(Seq( 
      (4L, "spark i j k"), 
      (5L, "l m n"), 
      (6L, "spark hadoop spark"), 
      (7L, "apache hadoop") 
    )).toDF("id", "text") 
    sameModel.transform(test) 
    .select("id", "text", "probability", "prediction") 
    .collect() 
    .foreach { case Row(id: Long, text: String, prob: Vector, prediction: Double) => 
      println(s"($id, $text) --> prob=$prob, prediction=$prediction") 
    } 
  } 
}
```

这样就用 Spark 完整实现了一个机器学习的流程。从上面的代码中可以看出，这样的结构非常有利于复用 Transformer 与 Estimator 组件。

Spark MLlib（ML API）的算法包主要分为以下几个部分：

-   特征抽取、转换与选择；
    
-   分类和回归；
    
-   聚类；
    
-   协同过滤；
    
-   频繁项集挖掘。
    

其中每一类都有若干种算法的实现，用户可以利用 Pipeline 按需进行切换，下面我们将根据这几个类别，分别实现一些真实数据的案例，让读者可以直接上手应用。

此外，在上面的代码中，我们用 Pipeline API 将模型序列化成文件，这样的好处在于可以将模型看成一个黑盒，非常方便模型上线，而不用在上线应用时再去对模型进行硬编码，这类似于 Python 的 Pickle 库的用法。

### 小结

在 Spark 的早期版本中，并没有 ML pipeline API，这导致代码难以维护、可读性较差、也一定程度上影响了 MLlib 的流行。而 Pipeline API 的引入抽象了机器学习流程，让整个代码变得简洁优美，另外这种抽象也利于与第三方库进行结合，如 Tensorflow、XgBoost 等等。

如何理解本课时内容中的这句话：最后给大家留一个思考题，

训练完成好的模型也是一个 Transformer。
