---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在开始今天的课程前，我们先来看看上个课时的习题：推荐引擎如何冷启动？冷启动是一个比较复杂也比较常见的问题。这里简单介绍下，可以选用的方法有用户登录自选标签，利用用户的注册信息分类并进行相应的推荐等。

而当我们 完成训练之后我们还需要对模型进行评估，根据评估的结果，有可能需要对模型超参进行优化，以达到理想的效果。本课时的主要内容有：

-   模型评估
    
-   交叉验证与超参调优
    

如果你是用 MLlib 来进行模型构建，那么本节内容的使用频率会大大高于本模块的其他内容。

### 模型评估

评估模型的质量，通常可以用一些评价方法说明，如精确率、召回率、AUC 、ROC 、PRC 等。我们先从混淆矩阵讲起，如果模型是一个二分类模型，当预测的概率值大于某个阈值，我们就将其归为类 1，那么我们可以从测试集的结果中得到如下表所示的矩阵。

![1.png](https://s0.lgstatic.com/i/image/M00/43/72/CgqCHl87ncyAUK8bAABpsM2lFeI981.png)

上表中 True Positive 是本身为类 1，预测为类 1 的数目，称为真阳性；False Positive 是本身为类 0，预测为类 1 的数目，称为伪阳性；False Negative 是本身为类 1，预测为类 0 的数目，称为伪阴性；True Negative 是本身为类 0，预测为类 0 的数目，称为真阴性。根据这 4 个值，可以计算出精确率、召回率和 F 值。

-   精确率 P= TP / (TP + FP)。精确率表示在预测为类 1 的样本中，实际为类 1 的样本比例。有一些模型对精确率要求很高，例如垃圾邮件监测，宁愿放过垃圾邮件也不愿误判一封正常邮件。
    
-   召回率 R= TP / (TP + FN)。召回率表示在实际为类 1 的样本中，预测为类 1 的样本比例。有一些模型对召回率的要求很高，例如肿瘤监测，宁愿误判也不愿错过。
    
-   准确率 = (TP + TN) / ALL。准确率表示正确预测的样本比例。
    
-   F=2/(1/P+1/R)=2PR/(P+R)。F 值是精确率与召回率的几何平均值，是对模型的一种综合评估，一个值比一条曲线来得直观。
    
-   ROC 与 PRC。ROC 曲线的横轴 FPR（False Positive Rate，伪阳性率）= FP/(FP + TN)；纵轴 TPR（True Positive Rate，真阳性率）= TP/(TP + FN)，曲线上的每个点代表不同的阈值所带来不同分类结果，阈值变化越细，ROC 曲线越平滑。如果存在一个理想的阈值，高于该阈值的都为类 1，反之为类 2，那么该模型就能完美区分两个类，此时横轴值为 0，纵轴值为 1，所以该 ROC 曲线经过 (0, 1) 点，曲线下面积为 1。通常我们用 ROC 曲线下面积（Area Under Curve，AUC）来评估模型质量，最大值为 1，越大说明模型效果越好，如下图所示中 3 条曲线分别代表了 3 个模型表现。
    

![Drawing 1.png](https://s0.lgstatic.com/i/image/M00/43/73/CgqCHl87npmASyfKAAET6Rbivy8093.png)

还有一种曲线叫精确召回率曲线（Precision Recall Curve，PRC），横轴是召回率，纵轴是精确率，与 ROC 越左凸效果越好不同，PRC 是越右凸效果越好，同样是曲线下面积越大越好。如下图所示，最上面的一条（AUC = 0.79）比最下面的一条（AUC = 0.39）效果好。PRC 可以在给定精确率或者召回率的情况下，去比较召回率或者精确率。

![Drawing 2.png](https://s0.lgstatic.com/i/image/M00/43/68/Ciqc1F87nqCAQwL9AAGtxOauXzc349.png)

Spark MLlib 提供了模型评估的 API，我们在得到了测试结果后，可以使用如下代码对模型进行评估：

```
val result = model.transform(test) 
val evaluator = new org.apache.spark.ml.evaluation.RegressionEvaluator 
var evaluatorParamMap = ParamMap(evaluator.metricName-> "areaUnderROC") 
var aucTraining = evaluator.evaluate(result, evaluatorParamMap)
```

### 交叉验证与超参调优

在机器学习中，我们都会将数据分为两份，一份作为训练集，另一份作为测试集，这就是交叉验证的思想。但在样本量不足的情况下，我们经常采用 _k_ 折交叉验证法来充分测试，以期得到更好的效果。

_k_ 折交叉验证法将全量数据分为 _k_ 份，每次选一份作为测试集，剩下的作为训练集，如下图所示是一个10 折交叉验证。

![Drawing 3.png](https://s0.lgstatic.com/i/image/M00/43/73/CgqCHl87nqqAVvieAADgeC3PtEM025.png)

_k_ 折交叉验证通常和超参调优一起使用，超参是在开始训练过程之前设置值的参数，而不是通过训练得到的参数。例如，随机森林模型的超参数有：

-   树的个数；
    
-   树的最大深度；
    
-   特征子集选取策略；
    
-   每个特征分裂时，最大划分数量；
    
-   纯度。
    

调整这些参数对模型性能有着巨大影响。通常，没有特别好的方法来指定这些参数，经验通常会很有用。我们只能通过不停地实验各种参数组合来测试模型性能，这样做的前提是，一定要保证组合非常全面。Spark MLlib 强大的运算能力能让我们快速测试大量的组合。

还是以随机森林模型为例，下面的代码可以让我们进行参数组合的测试：

```
val paramGrid = new ParamGridBuilder() 
.addGrid(rf.numTrees, 3 :: 5 :: 10 :: Nil) 
.addGrid(rf.featureSubsetStrategy, "auto" :: "all" :: Nil) 
.addGrid(rf.impurity, "gini" :: "entropy" :: Nil) 
.addGrid(rf.maxBins, 2 :: 5 :: Nil) 
.addGrid(rf.maxDepth, 3 :: 5 :: Nil) 
.build()
```

这里实际上我们定义了一个超参空间，空间的维度与超参的个数一样，所以 5 维空间中的每一个点代表了一个参数组合。下面我们创建一个交叉验证器，并定义 _k_ = 5：

```
val crossValidator = new CrossValidator() 
.setEstimator(new Pipeline().setStages(Array(labelIndexer, featureIndexer, rf))) 
.setEstimatorParamMaps(paramGrid) 
.setNumFolds(5) 
.setEvaluator(evaluator)
```

并通过训练测试所有的参数组合从而得到最好的一组参数组合：

```
val crossValidatorModel = crossValidator.fit(data) 
val bestModel = crossValidatorModel.bestModel 
val bestPipelineModel =crossValidatorModel.bestModel.asInstanceOf[PipelineModel] 
val stages = bestPipelineModel.stages 
val rfStage = stages(stages.length-1).asInstanceOf[RandomForestClassificationModel] 
val numTrees = rfStage.getNumTrees 
val featureSubsetStrategy = rfStage.getFeatureSubsetStrategy 
val impurity = rfStage.getImpurity 
val maxBins = rfStage.getMaxBins 
val maxDepth = rfStage.getMaxDepth
```

CrossValidator 继承了 Estimator 并实现了 fit() 方法。在 fit() 方法调用后，整个 Pipeline 会执行多次，包含从数据预处理到训练模型。在这个例子中，超参一共有 5 个，并进行了 5 折实验，需要执行 3 × 2 × 2 × 2 × 2 × 5 = 240 次 Pipeline，因此每多一个超参数，计算量通常都会大大增长，得益于 Spark 的并行计算能力，计算时间可以大大缩短。

### 总结

MLlib 不光为数据预处理与训练模型提供了解决方案，还为模型评估与调优提供了接口。如开头所述，不要看本课时的篇幅较短，但实际上使用率会很高。

本课时学完，Spark 所有组件就已经学完。其实大家不难发现，从模块三开始，使用场景是越来越窄的，批处理 -> 流处理 -> 图挖掘 -> 机器学习，所以本模块对于数据科学家来说在某些情况下是很有用的，但是对于数据工程师来说，使用场景并没有那么多。

最后给大家留一个**思考题**：树的最大深度对随机森林模型效果有什么影响呢？
