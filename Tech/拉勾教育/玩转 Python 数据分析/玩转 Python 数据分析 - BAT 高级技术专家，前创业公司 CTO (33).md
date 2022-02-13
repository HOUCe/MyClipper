---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


时间过得很快，转眼间就来到了课程的最后一讲。我可以很负责地说，坚持学习到了这里。你已经踏入了数据分析师的大门，先为自己鼓个掌。

今天依然是实战的环节，与之前稍微不同的是，今天我们尝试分析的并不是围绕专门用来做分析的数据集。因为用来做分析的数据集多多少少都包含了一些设定好的潜在规律等着我们去探索，缺少了一丝真实的味道。

今天我们尝试分析我们之前通过爬虫技术收集的国产电视剧的评分数据集，难度比之前会变大，但也更贴近真实世界收集到的数据的特点：信息量有限，但需要尽量挖掘出有用的信息。

### 准备数据

首先在工作目录创建 chapter30 文件夹，并将 chapter21 中使用过的 tv\_rating.csv 拷贝到 chapter30 文件夹中。

用 VS code 打开 chapter30 文件夹。并新建 notebook 保存为 chapter30.ipynb。导入必要的工具包：

```
import pandas as pd 
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns 
import numpy as np
import random
from sklearn.preprocessing import LabelEncoder 
from sklearn.linear_model import LinearRegression
from sklearn.metrics import recall_score
from sklearn.metrics import accuracy_score
from sklearn.metrics import mean_squared_error
from sklearn.metrics import mean_absolute_error
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from plotly import tools
import plotly.express as px
from plotly.offline import init_notebook_mode, iplot, plot 
import plotly.figure_factory as ff
import plotly.graph_objs as go 
import ast
```

之后，我们加载并打印我们抓取到的电视剧的数据集。

```
df = pd.read_csv("tv_rating.csv")
df
```

输出如下：  
![1.png](https://s0.lgstatic.com/i/image6/M00/4D/BA/Cgp9HWDwEmWALRFcAAYUVULjxV4663.png)  
相信大家对这个数据集应该不陌生了。一共包含 3600 条记录，并且有三列：分别是 电视剧的标题、评分和演员表。

### 任务目标

尝试基于目前手上已经有的数据，建立模型，来预测一个电视剧可能的评分。简单的说，需要我们基于一个电视剧标题和演员表的信息来预估它的评分。

由于我们只有一个数据集，所以我们从原始数据集中拆 80% 出来训练模型，然后用剩下的 20% 的数据来测试我们模型的效果。

### 问题分析

第一印象来说，从标题和演员表来预测电视剧的评分，多少是有点不够的。信息比较有限。但如果我们充分发挥我们想象力的话，标题和演员表也不是完全没用，比如是否存在以下可能。

-   标题长的电视剧是否会评分更高？
    
-   电影标题中包含数字的是否会评分更高？ 比如 2、第二部。 毕竟烂片一般就不会拍第二部了。
    
-   演员表中包含流量明星是否会评分更高？ 比如鹿晗、关晓彤。
    
-   另一方面，如果一个演员演的电视剧评分都很低，如果他也是待预测电视剧的演员，是否可以认为可能这部电视剧的评分也不高？
    

虽然乍看之下棘手。但我们仔细分析过后，还是可以从标题和演员表中挖掘出一些和评分有关的信息，这就是数据分析师最关键的工作，从一堆看似关联性很弱的数据中挖掘出有用的信息。理论上只要特征变量和目标变量存在相关性，那就肯定存在一个可用的模型，能比随机猜有更好的表现。今天我们的任务就是找到这个模型。

### 数据清洗

#### 缺失值处理

接下来进行数据清洗，我们依然是从缺失值检测开始。

输出如下：

```
title     0
rating    0
stars     0
dtype: int64
```

可以看出，我们的数据表是不存在缺失值。

#### 处理评分数据

下一步我们要处理的是评分数据，评分数据目前还是字符串，因为数据中还包含了分数的“分”字。为了方便后续的处理，我们首先将其转换为数字。转换为数字有两个步骤：

1.  移除“分”字；
    
2.  把剩下的数字字符串转换为数字。
    

代码如下：

```
df.rating = df.rating.apply(lambda x: x.replace("分", ""))
df.rating = pd.to_numeric(df.rating)
df
```

输出如下：

![图片2.png](https://s0.lgstatic.com/i/image6/M01/4D/BB/Cgp9HWDwEp2AZE5RAAYAosJ82Cc422.png)  
可以看到，分数已经被我们转换为数字了。

#### 处理演员表

接下来就是演员表的处理，从我们之前粗略的分析中，演员表是我们今天分析的重点。所以我们首先将其处理成列表。

由于现在是用逗号链接的字符串，所以我们可以用 split 函数将其拆为列表。另外，通过观察数据我们可以发现演员表开头都有“主演”这两个字，同样需要去除。代码如下：

```
def handleStars(stars):
    stars = stars.replace("主演", "")
    result_arr = []
    base_arr = stars.split(",")
return base_arr
df.stars = df.stars.apply(handleStars)
df
```

输出如下：  
![图片3.png](https://s0.lgstatic.com/i/image6/M00/4D/C3/CioPOWDwEuOAU1xOAAYIT8BaOPQ250.png)  
可以看到，我们的演员表字段已经被处理成列表了。细心的你可能已经发现，有记录的演员表字段没有被分割成功。比如【实习女捕快】 这个电视剧，演员之间用空格隔开。所以当我们尝试用逗号分割时，这个列表还是被当成整个字符串处理了。本质就是很多演员表的格式可能是不标准的。

为了找出没有被成功分割的记录，我们可以创建一个新的列：star\_count 来记录每一行记录 stars 字段的长度。

```
df["star_count"] = df.stars.apply(len)
df
```

之后，我们筛选长度为 1 的记录查看：

```
df[df.star_count==1].stars
```

输出如下：  
![图片4.png](https://s0.lgstatic.com/i/image6/M01/4D/C3/CioPOWDwEvmAOTZCAARTE3pBHzo558.png)

一共有 348 条长度为 1 的列表，当然这个列表页不全是异常值，比如有的电视剧确实就只有一个演员。由于 pandas 输出太多数据时会被截断，我们可以用 \[\] 索引器来输出某一段的记录，比如第 100 条到第 150 条。

```
df[df.star_count==1][100:150]
```

输出如下（未完整截取）：  
![图片5.png](https://s0.lgstatic.com/i/image6/M01/4D/BB/Cgp9HWDwEwmABM9kAAg40L2AGqE611.png)  
通过分析，我们可以发现未能被成功分割的主要有以下几种情况：

-   使用空格（ ）分割；
    
-   使用/斜杠（/）分割；
    
-   使用中文逗号（，）分割；
    
-   使用顿号（、）分割。
    

基于此，我们可以编写代码，对目前已经是列表类型 stars 列进行二次处理，将那些没有被成功分割的字段分割开。代码如下：

```
def handleStarStep2(stars):
    spliters = [' ', '/','，','、']
    result_arr = []
for item in stars:
for spliter in spliters:
            temp_arr = item.split(spliter)
for sub_item in temp_arr:
                temp_item = sub_item.strip()
if temp_item not in result_arr:
                    result_arr.append(temp_item)
return result_arr
df.stars = df.stars.apply(handleStarStep2)
df
```

输出如下：  
![图片6.png](https://s0.lgstatic.com/i/image6/M01/4D/BB/Cgp9HWDwExaAXVAcAAV8_dyYxMo021.png)  
之后，我们再次刷新 star\_count 字段的值并查看。

```
df["star_count"] = df.stars.apply(len)
df[df.star_count ==1]
```

输出如下：  
![图片7.png](https://s0.lgstatic.com/i/image6/M00/4D/C3/CioPOWDwE52AcczfAAW6lkBzQCs642.png)

可以看到，演员个数为 1 的记录从 348 减少为 81。从结果中看，绝大多数都确实只有一个演员的电视剧，而也存在着极少数仍未被分开的记录，因为中间完全没有分隔符。由于个数较少，我们不做进一步的处理。

### 可视化分析

基本的数据处理完毕后，我们尝试可视化分析，目前我们的数据表中仅有一个数值字段：star\_count， 以及按照我们之前说的，标题的长度也应该是一个值得参考的值。

#### 数据分布分析

首先我们看一下目标变量的分布：

输出如下：  
![图片8.png](https://s0.lgstatic.com/i/image6/M00/4D/C3/CioPOWDwE66AWf7LAAKfJ-Z-Pu0025.png)

数据集中的评分分布还是比较平均的。

接下来看一下 title\_len 字段。

输出如下：  
![图片9.png](https://s0.lgstatic.com/i/image6/M00/4D/C3/CioPOWDwE7-AFfQTAAJ6Dsw2YSU428.png)

可以看到，绝大多数电影的标题都是 4~5 个字。由于数据太过集中，所以 title\_len 可能并不是一个特别好的特征。

最后，我们来看一下 star\_count。

输出如下：  
![图片10.png](https://s0.lgstatic.com/i/image6/M01/4D/BB/Cgp9HWDwE8qAHt8vAAIAGy6vtBg303.png)

可以看到类似 title\_len、star\_count 的数量都集中到 0~5 的范围内。

#### 相关性分析

接下来我们分析一下这两个字段与 rating 字段的相关性，首先是标题长度。

```
df["title_len"] = df.title.apply(len)
px.scatter(df, x="title_len", y = "rating")
```

输出如下：  
![图片11.png](https://s0.lgstatic.com/i/image6/M01/4D/BB/Cgp9HWDwE-KAKMovAAMH_aPFgDs239.png)  
从上图中可以看到，我们这次的数据量比较小。这也导致我们做分析可能很难得出一个比较完美的结果，另一方面，title\_len 和 rating 从图中反映的相关性也比较弱。

我们再来看一下 star\_count 。

```
px.scatter(df, x="star_count", y="rating")
```

输出如下：  
![图片12.png](https://s0.lgstatic.com/i/image6/M00/4D/C3/CioPOWDwE_mAdwVvAASXCQEqo1E113.png)  
从上图中可以看到，数据尤其是高分的数据都集中在 star\_count 3 到 10 这个区间。虽然也反映不出 star\_count 与 rating 存在明显的关系，但至少从概率来看，不在这个区间的评分不高的概率就偏大了，可以说存在一定的弱相关性。

### 特征工程与模型

因为这次我们的任务并没有特别明显的特征，所以我们将特征功能和模型放到一起来讲，因为需要一边测试模型的表现一边筛选特征。

#### 基准结果

在本讲的开头我们就描述了这次任务的性质：

-   数据量少，稀疏；
    
-   收集自现实世界，数据中不一定隐含有意义的结论。
    

对于这类不确定性高的数据分析任务，最重要就需要设定一个评价是否有用的基准。比如我们建立的模型比随便猜的错误率还高，那就没有任何意义了。结合本案例来说，我们 80% 用于训练，20% 的数据集用于测试，对于 20% 的测试集合，我们用随机的方式计算电视剧的评分，随机评分和测试集中的真实评分做均方误差。

这个均方误差就是我们的基准结果，换句话来说，我们的模型对于测试集中电视剧评分的预测需要超过这个基准结果，模型才能算是有用的。

首先第一步，拆分训练和测试集。

```
df_train,df_test = train_test_split(df, test_size=0.2)
df_test
```

输出如下：  
![图片13.png](https://s0.lgstatic.com/i/image6/M01/4D/BB/Cgp9HWDwFCSAUNCCAAUNs28cC_c412.png)  
现在测试集中包含 720 行记录。接下来我们随机出 720 个 \[0,5\] 的评分，计算随机结果与测试集评分的 MSE。

```
random_result = np.random.uniform(0.0,5.0, 720)
mean_squared_error(random_result, df_test["rating"])
```

输出如下：

#### 初版模型

接下来进入模型的训练，目前我们有两个特征 title\_len 和 star\_count，相关性都不是很强。但我们仍然可以先试试用它们训练一版模型。代码如下：

```
from xgboost import XGBRegressor
features =  ["title_len","star_count"]
target = "rating"
xgb = XGBRegressor(n_estimators = 2000 , random_state = 0 , max_depth = 27)
df_train_features = df_train[features]
df_train_target = df_train[target]
xgb.fit(df_train_features, df_train_target)
df_test_feat = df_test[features]
df_test_result = xgb.predict(df_test_feat)
mean_squared_error(df_test_result, df_test[target])
```

输出如下：

通过 title\_len 和 star\_count 训练的 XGBoost 模型，在测试集上获得了 MSE 2.26 的成绩，已经大幅超过了我们之前准备的基准结果。这说明虽然这两个特征并不是非常的强，但也足够做出比乱猜靠谱得多的预测。

#### 模型优化

虽然我们第一版的模型就能够得出不错的结果，但仍然有优化的空间。比如我们都知道，优秀的演员对于评分的帮助显然比标题长度，或者演员个数来的直接。我们可以定义两个维度。

-   优秀演员：参演过 4 分及以上电视剧的演员；
    
-   资深演员：参与过 3 部以上电视剧的演员（数据集中为准）。
    

基于此，我们可以制定出三个新的特征。

1.  电视剧包含优秀演员的个数：top\_stars\_count；
    
2.  电视剧包含资深演员的个人：sr\_stars\_count；
    
3.  电视剧包含的优秀演员共演过几部4分以上电视剧：top\_stars\_movie\_count。
    

要实现上述三个特征，我们首先需要遍历数据表，筛选出普通演员、优秀演员和电视剧个数的对应关系。代码如下，我们用 all\_stars 存储普通演员，all\_top\_stars 存储优秀演员。

```
all_stars={}
all_top_stars = {}
i = 0 
for item in df.stars.values:
    rating = df.loc[i, "rating"]
for sub_item in item:
if sub_item in all_stars:
            all_stars[sub_item] = all_stars[sub_item] + 1
else:
            all_stars[sub_item] = 1
if rating >=4:
if sub_item in all_top_stars:
            all_top_stars[sub_item] = all_top_stars[sub_item] + 1
else:
            all_top_stars[sub_item] = 1
    i = i+1
print("普通演员个数：", len(all_stars))
print("优秀演员个数：",len(all_top_stars))
```

执行之后输出如下：

```
普通演员个数： 8808
优秀演员个数： 1237
```

可以看到，数据集中一共有 8800 多个演员，其中有 1200+ 个优秀演员。  
之后，我们基于这两个字典，来生成我们三个新的特征，代码如下：

```
def get_sr_stars_count(c,l):
    result = 0
    for item in l:
        if all_stars[item] > c:
            result = result+1
    return result
def get_top_stars_count(l):
    result = 0 
    for item in l:
        if item in all_top_stars:
            result = result + 1
    return result
def get_top_stars_movie_count(l):
    result = 0 
    for item in l:
        if item in all_top_stars:
            result = result + all_top_stars[item]
    return result
df["sr_stars_count"] = df.stars.apply(lambda x:get_sr_stars_count(3,x))
df["top_stars_count"] = df.stars.apply(get_top_stars_count)
df["top_stars_movie_count"] = df.stars.apply(get_top_stars_movie_count)
df
```

输出如下：  
![图片14.png](https://s0.lgstatic.com/i/image6/M01/4D/BB/Cgp9HWDwFDuADoy-AAROfzvEh9Y742.png)  
一切准备妥当，现在用我们三个新特征来训练模型。代码如下：

```
from xgboost import XGBRegressor
features =  ["top_stars_count", "top_stars_movie_count","sr_stars_count"]
target = "rating"
df_train,df_test = train_test_split(df, test_size=0.2)
xgb = XGBRegressor(n_estimators = 2000 , random_state = 0 , max_depth = 27)
df_train_features = df_train[features]
df_train_target = df_train[target]
xgb.fit(df_train_features, df_train_target)
df_test_feat = df_test[features]
df_test_result = xgb.predict(df_test_feat)
mean_squared_error(df_test_result, df_test[target])
```

输出如下：

可以看到，在我们使用三个新的特征后，模型的准确率相比初版模型提升了 17%，相比基准结果提升了 55.9%。

> 为什么第二次训练我们没有包含第一次的 title\_len、star\_count 这两个特征？  
> 原因是数据集本身比较少，特征太多很容易导致模型无法收敛，进而出现更差的结果。

### 小结

至此，今天的数据分析案例实战就讲完了。回顾一下今天所涉及的内容：

-   现实世界的数据往往不存在明显的特征，需要我们基于现有特征不断挖掘出潜在的相关性，找出隐藏在数据背后的特征；
    
-   使用 replace 函数替换字符串中的任意字符，使用 pd.to\_numeric 将某一列转换为数字；
    
-   使用 split 函数将字符串以某个分隔符为准拆成一个字符串数组；
    
-   结合经验与现有的数据，用不同的特征尝试模型的训练，最终找到最好的特征集合。
    

本讲也是课程的最后一讲，学到这里你已经完全证明了你的能力与觉悟，是时候在数据分析的道路上扬帆起航了。
