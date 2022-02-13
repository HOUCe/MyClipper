---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


从 Pipeline 的角度来说，上一个课时我们主要学习的是 Transformer，从本课时开始将进入 Estimator 的学习，也就是机器学习算法的学习。从本课时开始，我们将从 MLlib 实现的算法中按照算法类型：分类（有监督）、聚类（无监督）、推荐算法，各选取一个算法进行学习，并且**每个课时后还会有基于真实数据的实践训练**，这是本模块与其他模块不同的地方。

上节课的思考题可以看成是本课时的预习，在下面的内容中，我们会通过案例解答这个问题。

分类器是机器学习最常见的应用，MLlib 中也内置了许多分类模型，而其中支持分布式计算最好的也是最常用的方法就是随机森林算法，它的基础是决策树算法。本课时，我将介绍决策树算法和随机森林算法，以及用 Spark MLlib 的随机森林分类器实现根据身体监控数据判断人体状态。

### 决策树

决策树是一种机器学习的方法，它通过一种树形结构对样本进行分类，每个非叶子结点代表一次判断，每个叶子结点代表的是分类结果。它是一种**典型的监督学习**，需要一定量的样本，常见的决策树构造树算法有 C4.5 与 ID3。

下面先来看一个例子。下表中的内容是信贷审批常见的场景：根据信息判断客户是否会逾期。它的样本一共有 4 个特征，最后得出“是否逾期”的结果，这是一个二分类场景。

![2.png](https://s0.lgstatic.com/i/image/M00/3E/30/CgqCHl8rs1yAeh-3AADxNFeYbKs151.png)

根据这些样本，我们可以构造出一棵这样的树，如下图所示。

![1.png](https://s0.lgstatic.com/i/image/M00/3E/25/Ciqc1F8rs22AZdTQAAFhkujn7lI795.png)

构造一棵决策树需要从样本中学习结点分裂的时机，并判断阈值，也就是上图中的 A、B、C、D、E，这个过程被称为特征选择。通过这样的方法，我们可以达到对样本分类的效果。

下面我们先来简要介绍一下决策树构造算法的整个过程。决策树构造算法通常是先选择一个最优特征，将样本分为若干个子集，如果子集已经被正确分类，那么就构造叶子结点，将样本分到叶子结点中；如果某个子集没有被正确分类，那么就对这个子集继续进行选择特征。决策树构造是一个递归过程，递归停止条件是所有样本被基本正确分类，这样就构造出了一棵决策树。

从这个过程中可以看出，决策树通常对训练数据有良好的表现，但对新样本却未必如此，容易出现过拟合，也就是说模型能将训练数据很好地正确分类，而对测试数据和真实数据来说，分类结果却不尽如人意 。因此我们需要对已经生成的树进行剪枝，从而提升模型的泛化能力。如果特征过多，在一开始构造的时候，我们也会对特征进行选择，只留下足够有区分度的特征。决策树的生成对应模型的局部选择，而剪枝对应模型的全局选择。

从上述过程可以看出，构造决策树主要包含**特征选择**、**决策树生成**与**决策树剪枝**。下面我们将来详细介绍。

#### 特征选择

本节主要介绍两种特征选择的方式：**信息增益**与**信息增益率**。在介绍这两种方式前，先来看看熵的概念：熵是表示随机变量的不确定性的度量。

**定义**：假设随机变量 _X_ 的可能取值有 _x1 , x2 , …, xn_ ，对于每一个可能的取值 _xi_ ，其概率 _P（X_ = _xi）_ = _pi （_ _i_ = 1, 2, …, _n）_ ，则随机变量 _X_ 的熵为：

![Drawing 2.png](https://s0.lgstatic.com/i/image/M00/3E/36/CgqCHl8ruuSATGy3AAAmmKUzEis127.png)

对于样本集合 _D_ 来说，随机变量 _X_ 是样本的类别，即假设样本有 _k_ 个类别，每个类别的概率是  
![Drawing 3.png](https://s0.lgstatic.com/i/image/M00/3E/36/CgqCHl8ruwWASQ_iAAANoL6crhU032.png)  
，其中 | _Ck_ | 表示类别 _k_ 的样本个数，| _D_ | 表示样本总数，对于样本集合 _D_ 来说，熵（经验熵）为：

![Drawing 4.png](https://s0.lgstatic.com/i/image/M00/3E/37/CgqCHl8ruxOAHrq8AABK5ZnXoS8805.png)

而条件熵的概念为：设有随机变量（ _X_ , _Y)_，其联合概率分布为：

![Drawing 5.png](https://s0.lgstatic.com/i/image/M00/3E/37/CgqCHl8ruxqAOg4aAABq0voqPeg077.png)

条件熵 _H_（_Y_ | \*X）\*表示在已知随机变量 _X_ 的条件下，随机变量 _Y_ 的不确定性。在随机变量 _X_ 给定的条件下，随机变量 _Y_ 的条件熵 _H（Y_ | _X）_，定义为 _X_ 给定条件下 _Y_ 的条件概率分布的熵对 _X_ 的数学期望：

![Drawing 6.png](https://s0.lgstatic.com/i/image/M00/3E/37/CgqCHl8ruyKAcbBLAABJvNn0hIA847.png)

当熵和条件熵中的概率由数据估计得到时，所对应的熵与条件熵分别称为经验熵与经验条件熵。从经验熵与经验条件熵可以得到**信息增益的定义**：

特征 _A_ 对训练数据集 _D_ 的信息增益 _g_ （_D_ , _A）_ 的定义，是集合 _D_ 的经验熵 _H（D)_ 与特征 _A_ 给定条件下 _D_ 的经验熵与条件熵之差：

![Drawing 7.png](https://s0.lgstatic.com/i/image/M00/3E/37/CgqCHl8ruyqAQoMqAABDo99VV2A633.png)

信息增益通常用来选择特征，经验熵 _H（D）_ 表示的是对数据集 _D_ 进行分类的不确定性。而经验条件熵 _H（D_ | _A）_ 表示在特征 _A_ 给定的条件下，对数据集 _D_ 进行分类的不确定性，那么信息增益就表示由于特征 _A_ 而使得对数据集 _D_ 的分类的不确定性的减少程度。显然，对于数据集 _D_ 而言，信息增益依赖于特征，不同特征往往具有不同的信息增益。信息增益大的特征具有更强的分类能力。根据信息增益准则，选择特征的方法是：对训练数据集（或子集） _D_，计算其每个特征的信息增益，并选择信息增益最大的特征。

信息增益率是对信息增益的改进，特征 _A_ 对训练数据集 _D_ 的信息增益率 _gR（D_ , _A）_ 的定义，是其信息增益 _g（D_ , _A）_ 与训练数据集 _D_ 的经验熵 _H_ （_D）_ 之比，如下图所示：

![Drawing 8.png](https://s0.lgstatic.com/i/image/M00/3E/2B/Ciqc1F8ruzGAPP1YAABNXeHlZfw189.png)

除了信息增益与信息增益率之外，还可以用基尼系数来完成特征选择。

#### 决策树生成

决策树生成算法与特征选择方法相对应，选用信息增益进行特征选择的是 ID3 算法，选择信息增益比进行特征选择的是 C4.5 算法，选择基尼系数来完成特征选择的是分类回归树（CART）算法。本节将介绍 C4.5 与 ID3 算法。

**ID3 算法如下：**

简单来说，ID3 算法会在决策树各个结点上应用信息增益准则选择特征，递归地构建决策树。

给定训练数据集 _D_，特征集 _S_，阈值 _ϵ_ ：

1.  若 _D_ 中所有实例属于同一类 _Ck_，则 _T_ 为单结点树，并将类 _Ck_ 作为该结点的类标记，返回 T；
    
2.  若 _S_ = Æ，则 _T_ 为单结点树，并将 _D_ 中实例数最大的类 _Ck_ 作为该结点的类标记，返回 _T_；
    
3.  否则，计算 _S_ 中各特征对 _D_ 的信息增益，选择信息增益最大的特征 _Sg_；
    
4.  如果 _Sg_ 的信息增益小于阈值 _ϵ_，则置 _T_ 为单结点树，并将 _D_ 中实例数最大的类 _Ck_ 作为该结点的类标记，返回 _T_；
    
5.  否则，对 _Sg_ 的每一个可能值 _ai_，将 _D_ 分割为若干个非空子集 _Di_，将 _Di_ 中实例数最大的类作为标记，构建子结点，由结点及其子结点构成树 _T_，返回 _T_；
    
6.  对第 _i_ 个子结点，以 _Di_ 为训练集，以 _S -Sg_ 为特征集，递归调用第 1～5 步，得到子树 _Ti_，返回 _Ti_。
    

**C4.5 算法**与 ID3 算法非常类似，只是把用到信息增益的地方换成了信息增益比。

#### 剪枝

通常决策树在训练数据上表现很好，但是在测试数据上就不尽如人意，这就是模型过拟合。决策树剪枝主要分为预剪枝和后剪枝。**预剪枝**是在构造决策树的同时进行剪枝，通常作为停止条件，即设定一个熵的阈值，就算可以继续降低熵，也停止创建分支。而**通常我们说的剪枝是指后剪枝**，后剪枝通常有以下两种做法：

-   **应用交叉验证的思想**，若局部剪枝能够使得模型在测试集上的错误率降低，则进行局部剪枝。
    
-   **应用正则化的思想**，综合考虑不确定性和模型复杂度来定出一个新的损失，用该损失作为一个结点是否应该局部剪枝的标准。这种做法的核心是定义新的代价函数，通常会采用树的结构复杂度与模型预测误差之和作为代价衡量。
    

在 ID3、C4.5 中我们会应用前者做法，而在分类回归树中，我们会采取后者做法。

### 随机森林

在决策树的基础上了解随机森林的原理相对容易。随机森林就是通过集成学习的思想将多棵树集成的一种算法，它的基本单元是决策树，属于机器学习的一大分支——集成学习（Ensemble Learning）方法。集成学习是通过构建多个弱分类器，并按一定规则组合起来的分类系统，常常比单一分类器具有显著优越的泛化性能，常见集成学习算法有随机森林、AdaBoost、XgBoost、梯度提升树等，在风险建模、疾病预测等领域应用相当广泛。

随机森林将 N 棵决策树集成，每一棵决策树都是一个分类器，相当于每个分类器都会对结果进行投票，随机森林会综合所有的分类结果，并将票数最高的分类结果作为最终结果输出。可以想到，在随机森林中，每一棵决策树的生成是算法的关键。**每棵树的生成规则如下**：

1.  如果训练集大小为 N，对每棵树而言，随机且有放回地从训练集中抽取 N 个训练样本（这种采样方式称为 Bootstrap Sample 方法），作为该树的训练集。
    
2.  如果每个样本的特征维度为 M，指定一个常数 m<<M，随机地从 M 个特征中选取 m 个特征子集，每次树进行分裂时，从这 m 个特征中选择最优的。
    
3.  每棵树都尽最大限度地生长，并且没有剪枝过程。
    

随机森林中所谓“随机”的含义，就是模型在这里引入了随机性（随机抽取训练集、随机抽取特征），两个随机性的引入对随机森林的分类性能至关重要。由于它们的引入，使得随机森林不容易陷入过拟合，并且具有很好的抗噪能力。随机森林的特点有：

-   在当前所有算法中，具有极佳的准确率，在国内外最近几年的数据挖掘大赛中，随机森林取得了令人瞩目的成绩。
    
-   能够高效地运行在大数据集上，很容易可以看出，随机森林是非常容易分布式的。
    
-   能够处理具有高维特征的输入样本。
    
-   能够评估各个特征在分类问题上的重要性。
    
-   在生成过程中，能够获取到内部生成误差的一种无偏估计。
    
-   对于缺省值问题也能够获得很好的结果。
    

### 人体状态监测器

下面，我们将用真实的数据（数据集下载链接： [https://pan.baidu.com/s/1rUxHKl119qGDlV6KeTtkLA](https://pan.baidu.com/s/1rUxHKl119qGDlV6KeTtkLA)提取码：ev1d）拟合出一个随机森林分类器。案例的内容是通过身体监测数据来判断人体状态，比如监测走路、骑行、跑步、看电视。数据集包括时间戳、心跳、活动标签和 3 个传感器，传感器分别佩戴在手上、胸部、踝关节处，每个传感器有 17 个检测指标 （温度、3D 加速度、陀螺仪和磁强计数据、方位数据） 。数据集共计 54 个属性，3850505 条样本，包含了 18 种人体活动。代码如下：

```
import org.apache.spark.ml.Pipeline 
import org.apache.spark.ml.classification.{RandomForestClassificationModel, RandomForestClassifier} 
import org.apache.spark.ml.evaluation.MulticlassClassificationEvaluator 
import org.apache.spark.sql.{Dataset, Row, SparkSession} 
import org.apache.spark.rdd.RDD 
import org.apache.spark.mllib.regression.LabeledPoint 
import org.apache.spark.mllib.linalg.Vectors 
import org.apache.spark.ml.feature.{IndexToString, StringIndexer, VectorAssembler} 
import org.apache.spark.sql.types._ 
import scala.collection.mutable 
object RandomForestBodyDetection { 
def main(args: Array[String]): Unit = { 
val spark = SparkSession 
    .builder() 
    .appName("RandomForestBodyDetection") 
    .master("local[2]") 
    .enableHiveSupport() 
    .getOrCreate() 
import spark.implicits._ 
val dataFiles = spark.read.textFile("data/bodydetect") 
val rawData = dataFiles.map(r=>r.toString().split(" ")).rdd.map(row => { 
val list = mutable.ArrayBuffer[Any]()
for (i <- row.toSeq) { 
        list.append(i) 
      } 
Row.fromSeq(list.map(v=>if (v.toString.toUpperCase == "NAN") Double.NaN else v.toString.toDouble)) 
    }) 
val schema = StructType(Array( 
StructField("timestamp", DoubleType), StructField("activityId", DoubleType), StructField("hr", DoubleType),
StructField("hand_temp", DoubleType), StructField("hand_accel1X", DoubleType), StructField("hand_accel1Y", DoubleType),
StructField("hand_accel1Z", DoubleType), StructField("hand_accel2X", DoubleType), StructField("hand_accel2Y", DoubleType),
StructField("hand_accel2Z", DoubleType), StructField("hand_gyroX", DoubleType), StructField("hand_gyroY", DoubleType),
StructField("hand_gyroZ", DoubleType), StructField("hand_magnetX", DoubleType), StructField("hand_magnetY", DoubleType),
StructField("hand_magnetZ", DoubleType), StructField("hand_orientX", DoubleType), StructField("hand_orientY", DoubleType),
StructField("hand_orientZ", DoubleType), StructField("hand_orientD", DoubleType), StructField("chest_temp", DoubleType),
StructField("chest_accel1X", DoubleType), StructField("chest_accel1Y", DoubleType), StructField("chest_accel1Z", DoubleType),
StructField("chest_accel2X", DoubleType), StructField("chest_accel2Y", DoubleType), StructField("chest_accel2Z", DoubleType),
StructField("chest_gyroX", DoubleType), StructField("chest_gyroY", DoubleType), StructField("chest_gyroZ", DoubleType),
StructField("chest_magnetX", DoubleType), StructField("chest_magnetY", DoubleType), StructField("chest_magnetZ", DoubleType),
StructField("chest_orientX", DoubleType), StructField("chest_orientY", DoubleType), StructField("chest_orientZ", DoubleType),
StructField("chest_orientD", DoubleType), StructField("ankle_temp", DoubleType), StructField("ankle_accel1X", DoubleType),
StructField("ankle_accel1Y", DoubleType), StructField("ankle_accel1Z", DoubleType), StructField("ankle_accel2X", DoubleType),
StructField("ankle_accel2Y", DoubleType), StructField("ankle_accel2Z", DoubleType), StructField("ankle_gyroX", DoubleType),
StructField("ankle_gyroY", DoubleType), StructField("ankle_gyroZ", DoubleType), StructField("ankle_magnetX", DoubleType),
StructField("ankle_magnetY", DoubleType), StructField("ankle_magnetZ", DoubleType), StructField("ankle_orientX", DoubleType),
StructField("ankle_orientY", DoubleType), StructField("ankle_orientZ", DoubleType), StructField("ankle_orientD", DoubleType))) 
val df = spark.createDataFrame(rawData,schema) 
val allColumnNames = Array( 
"timestamp", "activityId", "hr") ++ Array( 
"hand", "chest", "ankle").flatMap(sensor => 
Array( 
"temp", 
"accel1X", "accel1Y", "accel1Z", 
"accel2X", "accel2Y", "accel2Z", 
"gyroX", "gyroY", "gyroZ", 
"magnetX", "magnetY", "magnetZ", 
"orientX", "orientY", "orientZ", "orientD").map(name => s"${sensor}_${name}") 
    ) 
val ignoredColumns = Array(0, 16, 17, 18, 19, 33, 34, 35, 36, 50, 51, 52, 53) 
val inputColNames = ignoredColumns.map(l => allColumnNames(l)) 
val columnNames = allColumnNames. 
    filter { !inputColNames.contains(_) } 
val typeTransformer = new FillMissingValueTranformer().setInputCols(inputColNames) 
val labelIndexer = new StringIndexer() 
    .setInputCol("activityId") 
    .setOutputCol("indexedLabel") 
    .fit(df) 
val vectorAssembler = new VectorAssembler() 
    .setInputCols(columnNames) 
    .setOutputCol("featureVector") 
val rfClassifier = new RandomForestClassifier() 
    .setLabelCol("indexedLabel") 
    .setFeaturesCol("featureVector") 
    .setFeatureSubsetStrategy("auto") 
    .setNumTrees(350) 
    .setMaxBins(30) 
    .setMaxDepth(30) 
    .setImpurity("entropy") 
    .setCacheNodeIds(true) 
val labelConverter = new IndexToString() 
    .setInputCol("prediction") 
    .setOutputCol("predictedLabel") 
    .setLabels(labelIndexer.labels) 
val Array(trainingData, testData) = df.randomSplit(Array(0.8, 0.2)) 
val pipeline = new Pipeline().setStages( 
Array(typeTransformer, 
            labelIndexer,
            vectorAssembler,
            rfClassifier,
            labelConverter)) 
val model = pipeline.fit(trainingData) 
val predictionResultDF = model.transform(testData) 
    predictionResultDF.select( 
"hr", "hand_temp", "hand_accel1X", "hand_accel1Y", "hand_accel1Z", "hand_accel2X", "hand_accel2Y", "hand_accel2Z", "hand_gyroX", "hand_gyroY", "hand_gyroZ", "hand_magnetX", "hand_magnetY", "hand_magnetZ", "chest_temp", "chest_accel1X", "chest_accel1Y", "chest_accel1Z", "chest_accel2X", "chest_accel2Y", "chest_accel2Z", "chest_gyroX", "chest_gyroY", "chest_gyroZ","chest_magnetX", "chest_magnetY", "chest_magnetZ", "ankle_temp", "ankle_accel1X", "ankle_accel1Y", "ankle_accel1Z", "ankle_accel2X","ankle_accel2Y", "ankle_accel2Z", "ankle_gyroX", "ankle_gyroY", "ankle_gyroZ", "ankle_magnetX", "ankle_magnetY", "ankle_magnetZ", "indexedLabel", "predictedLabel") 
    .show(20) 
val evaluator = new MulticlassClassificationEvaluator() 
    .setLabelCol("indexedLabel") 
    .setPredictionCol("prediction") 
    .setMetricName("accuracy") 
val predictionAccuracy = evaluator.evaluate(predictionResultDF) 
    println("Testing Error = " + (1.0 - predictionAccuracy)) 
val randomForestModel = model.stages(2).asInstanceOf[RandomForestClassificationModel] 
    println("Trained Random Forest Model is:\n" + randomForestModel.toDebugString) 
  } 
}
```

在处理流程中，用自定义的 Transformer 过滤掉了不需要的数据，并通过填充缺失值的方式对数据进行预处理。自定义的 Transformer 代码如下：

```
import org.apache.spark.ml.Transformer 
import org.apache.spark.ml.param.{Param, ParamMap} 
import org.apache.spark.ml.util.{DefaultParamsWritable, Identifiable} 
import org.apache.spark.sql.types.{BooleanType, NumericType, StructType} 
import org.apache.spark.sql.{DataFrame, Dataset, Row} 
class FillMissingValueTranformer extends Transformer 
{ 
val uid: String = Identifiable.randomUID("MissingValueTransformer") 
final val inputCols = new Param[Array[String]](this, "inputCol", "The input column") 
override def transformSchema(schema: StructType): StructType = {
val inputColNames = $(inputCols) 
val incorrectColumns = inputColNames.flatMap { name => 
        schema(name).dataType match { 
case _: NumericType | BooleanType => None 
case other => Some(s"Data type $other of column $name is not supported.") 
        } 
      } 
if (incorrectColumns.nonEmpty) { 
throw new IllegalArgumentException(incorrectColumns.mkString("\n")) 
      } 
StructType(schema.fields) 
   } 
def setInputCols(value: Array[String]): this.type = set(inputCols, value) 
override def transform(dataset: Dataset[_]): DataFrame = { 
val inputColNames = $(inputCols) 
var rawdata=dataset 
for (i<-inputColNames) {rawdata=rawdata.drop(i)} 
val allColumnNames = dataset.columns 
      // 过滤掉不需要的列名 
val columnNames = allColumnNames.filter { !inputColNames.contains(_) } 
      // 心率的空值填充为60，其他属性的空值填充为0 
var imputedValues:Map[String,Double]=Map() 
for (colname<-columnNames){ 
if(colname=="hr"){imputedValues += (colname->60.0)} 
else {imputedValues += (colname->0.0)} 
      } 
val processdata=rawdata.na.drop(26,columnNames).na.fill(imputedValues) 
      processdata.toDF() 
    } 
override def copy(extra: ParamMap): Transformer = defaultCopy(extra) 
}
```

### 小结

本课时，我们学习了决策树算法与随机森林算法，随机森林算法也曾经是各种数据科学竞赛中的明星算法，属于集成学习的一种。此外，可以发现随机森林算法对于分布式计算来说是很好的方法，这也是 MLlib 将其实现的原因。在本课时中，我们还实现了一个自定义 Transformer，也是对上一节课的复习。

最后给大家留一个思考题： 在配置分类器时，我们一共设置了多少个参数，每个参数有什么用？
