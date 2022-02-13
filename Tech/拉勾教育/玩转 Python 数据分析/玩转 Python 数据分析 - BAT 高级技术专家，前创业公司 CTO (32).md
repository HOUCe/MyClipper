---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


绝大多数互联网公司都面临一个非常重要的问题：用户流失问题。随着互联网和移动互联网的充分发展，发展新用户（也就是一般所说的拉新）的成本越来越高，往往要几块或者几十块的成本才能发展出一个新用户。

但如果用户在使用服务的时候觉得不开心就不用了，那就算流失用户。流失用户对公司会带来非常直接的损失，所以最大可能地减少用户流失就成为互联网公司的重要命题。常规的提升产品的使用体验，提供更多用户喜欢的功能是一个方向。

另一方向就是识别出潜在的流失用户，发放一定的权益或者红包让他们留下来。目前，潜在流失用户预测逐渐成为互联网公司数据分析的工作热点，今天我们就一起来做一个这方面的实战。

### 准备数据

今天我们使用的是 IBM 公布的一份电信公司用户流失的数据集。

> 下载地址是：链接:[https://pan.baidu.com/s/1BIhV7iSPUaeDK3S4HGa6aw](https://pan.baidu.com/s/1BIhV7iSPUaeDK3S4HGa6aw)提取码: xwsv

数据集的字段释义如下：

![1.png](https://s0.lgstatic.com/i/image6/M01/4C/C3/Cgp9HWDr_SiAerHQAAZZBJoMImY223.png)

在课程目录新建 chapter29 文件夹，然后将下载的文件放到该文件夹中，这次的项目不再有 train 和 test 两个分开的数据集，只有一个总的数据集。

之后，我们用 VS code 打开上述文件夹，并新建 notebook，同时保存为 chapter29.ipynb。

接下来，导入必要的工具包（直接从上一讲中拷贝过来即可）：

```
import pandas as pd 
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns 
import numpy as np
import random
from sklearn.preprocessing import LabelEncoder 
from sklearn.metrics import recall_score
from sklearn.metrics import accuracy_score
from sklearn.metrics import precision_score
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from plotly import tools
import plotly.express as px
from plotly.offline import init_notebook_mode, iplot, plot 
import plotly.figure_factory as ff
import plotly.graph_objs as go 
import ast
```

然后我们将数据集导入到 notebook 中。

```
df = pd.read_csv("WA_Fn-UseC_-Telco-Customer-Churn.csv")
df
```

输出之后发现列太多，无法完整显示，所以我们还是用上一讲学习的方法，来打印第一行的转置来查看所有的列。

输出如下：

![2.png](https://s0.lgstatic.com/i/image6/M00/4C/CC/CioPOWDr_T-AUQroAADF-WgJ9d8635.png)

### 任务目标

今天我们的任务和之前有点不太一样。之前我们面对的主要都是回归问题，简单来说就是通过一系列特征来预测出一个具体的数值，比如上一讲的电影票房，上上讲的确诊病例数。而我们今天要使用模型计算的则是一个用户是否会流失，Yes or No。

所以今天我们要处理的实际上是一个分类的问题。还有一些不一样的是，这次我们数据集中的特征有很多是布尔类型的值。

任务目标已经很清楚了：通过选择与构造合适的特征，建立分类模型，来预测用户是否会流失。

### 数据清洗

第一步，仍然是数据清洗环节。

#### 缺失值处理

我们首先查看是否存在缺失值：

输出如下：

```
customerID          0
gender              0
SeniorCitizen       0
Partner             0
Dependents          0
tenure              0
PhoneService        0
MultipleLines       0
InternetService     0
OnlineSecurity      0
OnlineBackup        0
DeviceProtection    0
TechSupport         0
StreamingTV         0
StreamingMovies     0
Contract            0
PaperlessBilling    0
PaymentMethod       0
MonthlyCharges      0
TotalCharges        0
Churn               0
dtype: int64
```

从统计结果上来看，该数据集不存在缺失值，所以暂时不需要进一步处理。

#### 数据类型处理

接下来，我们进一步看下数据集的数据类型。

输出如下：

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 7043 entries, 0 to 7042
Data columns (total 21 columns):
 #   Column            Non-Null Count  Dtype
---  ------            --------------  -----
 0   customerID        7043 non-null   object 
 1   gender            7043 non-null   object 
 2   SeniorCitizen     7043 non-null   int64
 3   Partner           7043 non-null   object 
 4   Dependents        7043 non-null   object 
 5   tenure            7043 non-null   int64
 6   PhoneService      7043 non-null   object 
 7   MultipleLines     7043 non-null   object 
 8   InternetService   7043 non-null   object 
 9   OnlineSecurity    7043 non-null   object 
 10  OnlineBackup      7043 non-null   object 
 11  DeviceProtection  7043 non-null   object 
 12  TechSupport       7043 non-null   object 
 13  StreamingTV       7043 non-null   object 
 14  StreamingMovies   7043 non-null   object 
 15  Contract          7043 non-null   object 
 16  PaperlessBilling  7043 non-null   object 
 17  PaymentMethod     7043 non-null   object 
 18  MonthlyCharges    7043 non-null   float64
 19  TotalCharges      7043 non-null   object 
 20  Churn             7043 non-null   object 
dtypes: float64(1), int64(2), object(18)
memory usage: 1.1+ MB
```

从上述结果中，可以看到存在一处问题， MonthlyCharges 是 float类型，但是 TotalCharges 却是 object 类型。考虑到后续我们需要计算 TotalCharges 和是否流失的相关性，所以这里我们将其转换为 float。

```
df["TotalCharges"] = pd.to_numeric(df["TotalCharges"])
```

执行上述代码，发现报错，错误提示为 “无法被转换为 float“。 说明 TotalCharges 字段虽然没有缺失值，但有部分值是空格，这导致我们无法直接将其转换为 float 类型。

为了解决上述问题，我们首先将 TotalCharges 字段中的空格，统一替换为数字 0，然后再进行转换：

```
df["TotalCharges"] = df["TotalCharges"].replace(' ', 0)
df["TotalCharges"] = pd.to_numeric(df["TotalCharges"])
df.info()
```

输出如下：

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 7043 entries, 0 to 7042
Data columns (total 21 columns):
 #   Column            Non-Null Count  Dtype
---  ------            --------------  -----
 0   customerID        7043 non-null   object 
 1   gender            7043 non-null   object 
 2   SeniorCitizen     7043 non-null   int64
 3   Partner           7043 non-null   object 
 4   Dependents        7043 non-null   object 
 5   tenure            7043 non-null   int64
 6   PhoneService      7043 non-null   object 
 7   MultipleLines     7043 non-null   object 
 8   InternetService   7043 non-null   object 
 9   OnlineSecurity    7043 non-null   object 
 10  OnlineBackup      7043 non-null   object 
 11  DeviceProtection  7043 non-null   object 
 12  TechSupport       7043 non-null   object 
 13  StreamingTV       7043 non-null   object 
 14  StreamingMovies   7043 non-null   object 
 15  Contract          7043 non-null   object 
 16  PaperlessBilling  7043 non-null   object 
 17  PaymentMethod     7043 non-null   object 
 18  MonthlyCharges    7043 non-null   float64
 19  TotalCharges      7043 non-null   float64
 20  Churn             7043 non-null   object 
dtypes: float64(2), int64(2), object(17)
memory usage: 1.1+ MB
```

可以看到，现在 TotalCharges 已经变为 float 类型了。

### 可视化分析

数据基本的清洗完之后，我们通过可视化的手段，来分析数据集的各种特征和最终的用户流失与否的关系。

#### 流失比例分析

首先，我们来看一下总的用户里面流失用户和没流失用户占比。

```
sns.catplot(data=df, y="Churn", kind="count")
```

输出如下：

![3.png](https://s0.lgstatic.com/i/image6/M00/4C/C3/Cgp9HWDr_XWAJyvYAAGLIsxKTkI239.png)  
可以看到，最终整体的用户还是未流失的更多，大概是流失用户的 3 倍。这说明业务状态大概还算正常。但从另一个方面，将近 1/4 的用户流失了，也应该引起警觉。

#### 数值类型特征分析

这次的数据集只包含三个数值类型的特征：tenure、MonthlyCharges、TotalCharges。

因为今天我们分析的是一个分类问题（是否流失），所以这里我们分析的重点就应该围绕流失用户和非流失用户，在这三个特征上是否展示出一定的差异。

首先从 tenure 开始，因为数据量偏少，我们使用核密度图进行分析。

```
fig1 = plt.figure(figsize=(10,5))
plt.rcParams["font.sans-serif"] = "SimHei"
sns.kdeplot(df[df["Churn"] == "Yes"]["tenure"], label = "流失用户")
sns.kdeplot(df[df["Churn"] == "No"]["tenure"], label = "未流失用户")
plt.legend()
```

输出如下：

![4.png](https://s0.lgstatic.com/i/image6/M00/4C/C3/Cgp9HWDr_YqADayMAAPrjqwwkUc167.png)

可以看到，对于流失用户而言，绝大多数用户的 tenure 值都集中在 0 的附近。因为 tenure 代表的是用户使用服务的月份数，所以我们可以看出流失用户绝大多数都是新用户，对于使用了一段时间的用户反倒流失的比较少。

因为对于 MonthlyCharges 和 TotalCharges 都需要核密度图来分析，为了节省代码，我们首先将刚才画核密度图曲线的代码封装成一个函数, 然后把特征的名称作为参数。

```
def show_kde_for_feature(feat):
    fig1 = plt.figure(figsize=(10,5))
    plt.rcParams["font.sans-serif"] = "SimHei"
    sns.kdeplot(df[df["Churn"] == "Yes"][feat], label = "流失用户")
    sns.kdeplot(df[df["Churn"] == "No"][feat], label = "未流失用户")
    plt.legend()
```

接下来，我们分析 MonthlyCharges 字段：

```
show_kde_for_feature("MonthlyCharges")
```

输出如下：

![5.png](https://s0.lgstatic.com/i/image6/M00/4C/C3/Cgp9HWDr_ZyAUm7tAAS59MVB1lg366.png)  
从上图中可以看出，流失用户的月缴费普遍比较高，而非流失用户普遍集中在月费 20 元这个区间，可以看出来有很多用户可能是因为月费太高而直接流失了。说明 MonthlyCharges 应该是我们来判断用户是否会流失的主要指标之一。

接下来分析 TotalCharges 。

```
show_kde_for_feature("TotalCharges")
```

输出如下：

![6.png](https://s0.lgstatic.com/i/image6/M01/4C/CC/CioPOWDr_ayAP56oAAO_XihL68g035.png)

从 TotalCharges 上来看，流失用户和非流失用户的分布并不是很大，所以这个字段的信息量应该比较小。

#### 用户维度的类别特征

接下来我们分析一下用户维度的类别特征，主要是 gender、SeniorCitizen、Partner 以及 Dependents。为了分析方便，我们直接将它们以子图的形式画在同一张大图里。代码如下：

```
fig = plt.figure(figsize=(10, 10))
fig.add_subplot(2,2,1)
sns.countplot(data=df, x="gender", hue="Churn")
fig.add_subplot(2,2,2)
sns.countplot(data=df, x="SeniorCitizen", hue="Churn")
fig.add_subplot(2,2,3)
sns.countplot(data=df, x="Partner", hue="Churn")
fig.add_subplot(2,2,4)
sns.countplot(data=df, x="Dependents", hue="Churn")
```

输出如下：

![7.png](https://s0.lgstatic.com/i/image6/M01/4C/CC/CioPOWDr_byAEmJlAAQNeY0-rE0957.png)

从图中可以看出，对于性别而言，流失用户和非流失用户的特征基本一致，说明用户是否流失和用户的性别基本无关，我们在建立模型的时候基本可以排除 gender 这个特征。

对于老年人来说，流失的比例也比非老年人要高，说明老年人可能对这类服务的依赖并不强。比较有趣的是 partner，没有配偶的流失用户的比例远高于有配偶的，这个情况也和第四个图匹配，有家人的流失比例远低于没有家人的。说明有家人和配偶，对于该电影公司的服务更依赖。而对于单身汉，可能偏向于其他公司提供的服务。

除了用户维度的类别特征，其他都是用户是否使用了某项服务的类别值。这里不进行进一步的分析，有兴趣的话你可以参考上面的方法做一些可视化的分析。

### 特征工程

通过可视化分析，我们判断除了 gender 之外的字段对于结果或多或少有一些影响。所以我们将有可能使用到的特征都单独提出来。另外， CustomerID 也是不需要的。

```
df_feature =  df[["SeniorCitizen", "Partner", "Dependents", "tenure", "PhoneService","MultipleLines","InternetService", "OnlineSecurity","OnlineBackup","DeviceProtection","TechSupport","StreamingTV","StreamingMovies","PaperlessBilling","PaymentMethod","Churn"]]
df_feature
```

输出如下：

![8.png](https://s0.lgstatic.com/i/image6/M01/4C/CC/CioPOWDr_c6AQD8kAAL3YanZLak495.png)

可以看到，我们一共选出了 16 列作为我们后续建立模型的特征备选值。 其中除了数据特征外，还有非常多的 Yes/No 或者字符串的列。这些列我们以前也处理过了，可以用 LabelEncoder 将其转换为 0 和 1 的值。所以第一步，我们要将所有需要编码的列找出来。代码如下：

```
object_columns = df_feature.select_dtypes(['object'])
```

下一步就是遍历这个 object\_columns 的列名，来将其转化为编码后的值。

```
encoder = LabelEncoder()
for item in object_columns.columns:
    df_feature[item] = encoder.fit_transform(df_feature[item])
```

之后，我们查看 df\_features。

输出如下：

![9.png](https://s0.lgstatic.com/i/image6/M01/4C/CC/CioPOWDr_d2Aag4UAAIBUhNvV2E788.png)

可以看到，现在 DataFrame 的值都已经被转换成对应的数字了。

### 建立模型

现在我们来建立模型，这次我们要进行的是分类任务，所以以往的回归模型是不太实用的。分类任务常见的模型有逻辑回归、随机森林等。具体理论部分就不在本课程展开，我们直接学习使用方法即可。这些模型的使用方法和之前我们使用的 XGBoost 和 LinearRegression 等都没特别的区别。

在训练模型之前，第一步首先就是将数据集拆分为训练集和测试集，直接使用我们之前使用的 train\_test\_split 函数即可。代码如下：

```
classifier = RandomForestClassifier(n_estimators=30 , oob_score = True, n_jobs = -1,random_state =50, max_features = "auto", min_samples_leaf = 50)
classifier.fit(df_train.drop(columns="Churn"), df_train["Churn"])
score = classifier.score(df_train.drop(columns="Churn"), df_train["Churn"])
score
```

输出如下：

输出的分数代表模型拟合效果，0.8 算一个还不错的结果。

#### 获取预测结果

在模型训练完成之后，我们就可以对之前拆分出来的测试集进行预测了。

```
prediction = classifier.predict(df_train_validator.drop(columns="Churn"))
prediction
```

输出如下：

```
array([0, 0, 0, ..., 0, 0, 0])
```

prediction 存储了我们模型针对测试集合特征预测的结果，是一个NumPy 的数组。我们可以将其转化成 pandas 的 Series ，来看下在我们预测的结果中，流失与非流失的比例。

```
pd.Series(prediction).value_counts()
```

输出如下：

```
0    1494
1     267
dtype: int64
```

可以看到，我们预测出来的流失与非流失的比例，和我们之前数据集中的情况基本是差不多的。

#### 衡量分类模型的结果

我们之前在回归模型的评估上，往往使用预测结果和真实结果的均方误差来衡量，但对于分类问题并不合适。对于分类问题，最常用来衡量模型效果的指标有三个：准确率、召回率和查准率。

对于分类问题，我们针对某条数据的预测的结果会有以下四种情况：

-   TruePosition(TP), 本来是真，我们的预测结果也为真；
    
-   TrueNegtive(TN)，本来是假，我们的预测结果也是假；
    
-   FalsePosition(FP), 本来是假，我们预测的结果为真；
    
-   FalseNegtive(FN), 本来是真，我们预测的结果为假。
    

不难看出，如果我们的结果是 TP 和 TN 都代表是对的，FP 和 FN 都代表是错的。但是不同的场景关注的终点也不同。

（1）准确率

准确率衡量的就是我们预测结果总的正确个数处于总预测的数量，公式为:

![10.png](https://s0.lgstatic.com/i/image6/M01/4C/C4/Cgp9HWDr_feAcfUWAACfre9SUQw566.png)

（2）召回率

召回率衡量的是我们找到了多少正样本。换句话说，对于本来是真的样本，我们找到了多少个。比如本来有 100 个用户会流失，而我们预测为真的结果中有 40 个人在这 100 个人里面，那我们的召回率就是 40%。 公式为:

![11.png](https://s0.lgstatic.com/i/image6/M01/4C/C4/Cgp9HWDr_gaAWfySAACTG7iafhg354.png)

（3）查准率

查准率衡量的是我们预测为真的所有样本中，真的为真的比例是多少。比如我们认为有 100 个用户会流失，然后有 80 个用户真的流失了，那说明我们的查准率为 80%。 公式为：

![13.png](https://s0.lgstatic.com/i/image6/M01/4C/CC/CioPOWDr_hKAM-XxAACdoMUBkqg732.png)

在不同的业务场景，会使用不同的指标来衡量我们分类模型的好坏。sklearn 提供了现成的函数可以直接计算这三个数值，代码如下：

```
accuracy = accuracy_score(df_train_validator.Churn.values, prediction)
recall = recall_score(df_train_validator.Churn.values, prediction)
precision = precision_score(df_train_validator.Churn.values, prediction)
print("准确率：", accuracy)
print("召回率：", recall)
print("查准率：", precision)
```

输出如下：

```
准确率： 0.7978421351504826
召回率： 0.401330376940133
查准率： 0.6779026217228464
```

可以看到，我们的准确率和查准率还是相当不错的。但召回率不算高，但这也算一个不错的平衡。一般来说召回率都是比较难提升的，毕竟如果盲目提升召回率，则可能会导致查准率下降，这样可能也会导致一个不太置信预测结果。

### 小结

至此，网络服务流失用户预测的实战案例就结束了。伴随着这几次的案例，相信你对于数据分析的几大核心步骤也有了基本的认识，了解如何进行数据清洗和特征工程以及根据任务的类型来选择合适的模型。

今天我们第一次接触分类问题，首先分类问题使用的模型和回归问题一般是不一样的，其次评价模型好坏的指标也不太一样，对于分类问题的三个指标应用也是非常广泛的。

回顾一下今天学习的内容，新的知识主要有：

-   使用 Series 的replace方法来批量替换 Series 中的字符串的值；
    
-   使用 pd\_tonumeric 来将 object 类型的字段转换为数值型；
    
-   使用 sns.catlot 来查看布尔值的分布比例；
    
-   使用 sns.kdeplot 配合 dataframe 的条件选择，来查看流失与不流失用户在其他字段上的分布特征；
    
-   使用 df.select\_dtypes 来选择出某个数据类型的字段；
    
-   使用 RandomForestClassifier 来建立分类模型。
