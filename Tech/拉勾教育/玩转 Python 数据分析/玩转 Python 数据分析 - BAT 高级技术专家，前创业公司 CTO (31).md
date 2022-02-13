---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


近十年，电影行业在世界范围内都取得了蓬勃的发展，越来越多的资金和人都源源不断地流入到这个行业，但对于电影投资人来说，风险和收益也是并存的。投入很大最后收益很小的案例也屡见不鲜。

所以在电影未上映之前进行票房的预测就变得非常重要，这不仅对电影投资人来说是重要的避险手段，对于院线同样很有意义。试想如果院线对一个电影大规模拍片，结果票房不好，院线最终也是损失惨重的。

电影票房虽然受非常多因素的影响，但整体也算有迹可循，比如电影的投资额、演员、电影的类型等，都可能会最终影响电影的票房。所以理论上来说我们应该也可以从电影制作期间的一些基本信息来建立模型，预测出电影的票房。为院线和资方等提供参考。

今天的案例，我们就尝试建立一些电影票房的预测模型。

### 准备数据

今天的实战，我们使用 kaggle 公开的 tmdb 电影票房数据集。

> 你可以在这里下载：[https://pan.baidu.com/s/1JjX0XJY-Lmz34dfezJS4SA](https://pan.baidu.com/s/1JjX0XJY-Lmz34dfezJS4SA)提取码: cgsm 。

在课程目录新建文件夹 chapter28，并将下载的文件放到该文件夹中，解压之后可以看到一个 train.csv 和一个 test.csv.

该数据集包含了 1971到 2015 年的部分电影数据。主要字段的含义如下：

字段名

含义

belongs\_to\_collection

属于哪个系列

budget

预算

genres

种类

homepage

主页

imdb\_id

imdb 的 id

original\_language

初识语言

original\_title

初识标题

overview

电影简介

popularity

流行指数

poster\_path

封面图地址

production\_companies

出品公司

production\_countries

出品国家

release\_date

上映日期

runtime

电影时长

spoken\_languages

包含语言

status

状态

tagline

标语

title

标题

keywords

关键词

cast

演员表

crew

职员表

revenue

利润

test 数据集的字段和 train 一致，只是不包含 revenue 字段。

### 任务目标

今天我们的任务目标非常清晰，就是基于 train.csv 中的数据，建立电影的利润预测模型，这样对于 test.csv 中的电影，我们可以基于现有的数据去预测其他的 revenue。

### 数据清洗

#### 数据初探

首先进入数据清洗的环节，用 VScode 打开 chapter28 目录，并新建 notebook ，将其保存为 chapter28.ipynb。

首先第一步，还是导入必要的依赖工具：

```
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns
import numpy as np
import random
from plotly import tools
import plotly.express as px
from plotly.offline import init_notebook_mode, iplot, plot
import plotly.figure_factory as ff
import plotly.graph_objs as go
import ast
```

然后导入数据：

```
df_train = pd.read_csv("train.csv")
df_test = pd.read_csv("test.csv")
```

如果你直接输出 df\_train 概述的话，你会发现其中部分字段太长，会导致整个 DataFrame 概述一个屏幕都装不下。所以我们曲线救国，采取其他方式来看一下 DataFrame 的情况。

首先看一下 DataFrame 的 shape：

```
print(df_train.shape)
print(df_test.shape)
```

输出如下：

可以看到，记录数并不多，但是每条记录有很多个列。而且另一个有意思的现象就是 test 的记录数居然比 train 的还要多。

接下来，我们看一下单条数据的数据情况。由于单条数据很长，所以即便只输出一条仍然无法全部显示。这个时候我们可以考虑将结果转置为竖版的，即可完整显示。

```
df_train.head(1).transpose()
```

输出如下：  
![Drawing 0.png](https://s0.lgstatic.com/i/image6/M01/4B/85/Cgp9HWDlan6AWaAtAAFoLauny58574.png)

从以上的单条记录，可以发现这几个现象：

-   很多字段的内容都是 JSON 格式，需要对其进行展开。比如 genres, belongs\_to\_collection, production\_companies 等。
    
-   纯数字的字段较少，只有 budget、popularity、runtime 和 revenue，所以要建立回归模型，还需要处理其他的字段。
    
-   部分字段是存在缺失值的，比如 homepage。
    

> 什么是 JSON？  
> JSON 可以理解为是字典的字符串表示。我们可以把字典转成一个 json 字符串，也可以反向把一个 json 字符串转换为字典。在 json 字符串中， \[\]表示列表，{} 表示字典。

#### 处理表格中的 JSON 数据

首先，我们可以使用以下代码来逐个观察 json 数据的特征，比如 genres 字段：

```
print(df_train.head(1).Keywords.values)
```

输出如下：

```
["[{'id': 4379, 'name': 'time travel'}, {'id': 9663, 'name': 'sequel'}, {'id': 11830, 'name': 'hot tub'}, {'id': 179431, 'name': 'duringcreditsstinger'}]"]
```

通过逐个观察 json 字段，我们可以得出以下结论：

-   json 字段可能包含多个字典，比如 Keywords 字段，多个字典代表有多个关键词；
    
-   每个字典都有 name 这个 key，代表字典的名称。
    

基于上面的分析，我们首先提取字典中的 name 属性，并将其转换为 python 的列表。另外，从上一讲中你已经发现，往往最后我们需要对 test 数据集，做和 train 数据集一样的数据清洗和特征工程的操作。所以这一讲我们所有对 train 数据集的操作都对 test 同步进行操作。

```
def expand_json(json_str):
    d = ast.literal_eval(json_str)
return [item["name"] for item in d]
json_features = ["belongs_to_collection", "genres", "production_companies", "production_countries",\
"Keywords", "spoken_languages", "cast", "crew"]
for feature in json_features:
    df_train.loc[df_train[feature].notnull(), feature] = df_train.loc[df_train[feature].notnull(), feature].apply(expand_json)
    df_test.loc[df_test[feature].notnull(), feature] = df_test.loc[df_test[feature].notnull(), feature].apply(expand_json)
```

执行上述代码处理后，我们可以看到相关的字段都被转换为名字列表的形式，比如我们看一下 Keywords 字段。

```
print(df_train.head(1).Keywords.values)
```

输出如下：

```
[list(['time travel', 'sequel', 'hot tub', 'duringcreditsstinger'])]
```

可以看到，转换后的字段已经是字符串列表了，这样之后整个处理的难度都会比之前的 json 数据要低很多。

#### 处理缺失值

接下来，我们来看一下数据集中缺失值的情况。

```
id                          0
belongs_to_collection    2396
budget                      0
genres                      7
homepage                 2054
imdb_id                     0
original_language           0
original_title              0
overview                    8
popularity                  0
poster_path                 1
production_companies      156
production_countries       55
release_date                0
runtime                     2
spoken_languages           20
status                      0
tagline                   597
title                       0
Keywords                  276
cast                       13
crew                       16
revenue                     0
dtype: int64
```

其中，belongs\_collection、tagline 和 homepage 这三个字段的缺失值是最多的。所以这三个字段的价值可能会比较小。但另一方面，当一个电影有主页以及属于某个系列的话，会不会票房可能就更好一些呢？

我们都知道漫威系列的子电影一般票房都不会太差，又比如说有标语的电影可能更吸引更多的受众。也就是说，这三个字段的内容可能价值有限，但如果有，可能对票房还是有影响的。基于此，我们将其转换为三个新的字段： has\_homepage，has\_tagline 和 is\_belongs\_to\_collection ，用 0 和 1 来表示之前的三个字段是否有值。

代码如下：

```
df_train["has_homepage"] = df_train["homepage"].isna().apply(lambda x:0 if x else 1)
df_train["is_belongs_to_collection"] = df_train["belongs_to_collection"].isna().apply(lambda x:0 if x else 1)
df_train["has_tagline"] = df_train["tagline"].isna().apply(lambda x:0 if x else 1)
df_test["has_homepage"] = df_test["homepage"].isna().apply(lambda x:0 if x else 1)
df_test["is_belongs_to_collection"] = df_test["belongs_to_collection"].isna().apply(lambda x:0 if x else 1)
df_test["has_tagline"] = df_test["tagline"].isna().apply(lambda x:0 if x else 1)
```

执行之后，我们可以抽样两条数据看看我们的操作是否符合预期。

```
df_train[["has_homepage", "is_belongs_to_collection", "homepage", "belongs_to_collection"]].head(5)
```

输出如下：  
![Drawing 1.png](https://s0.lgstatic.com/i/image6/M00/4B/8D/CioPOWDlao-ANmbTAACBiaqKPUg222.png)

可以看到，输出是符合预期的。

在添加了新的字段之后，我们可以将原先的几个字段直接删除。

```
df_train = df_train.drop(columns=["belongs_to_collection", "tagline", "homepage"])
df_test = df_test.drop(columns=["belongs_to_collection", "tagline", "homepage"])
```

剩余的缺失值分为两种，一种是我们之前从 json 格式转换为 list 的字段，也就是 json\_features 中的字段。对于这种字段，我们直接填充为空的 list，和字段其他值保持一致。

```
json_features.remove("belongs_to_collection")
for item in json_features:
    df_train[item] = df_train[item].apply(lambda x: x if isinstance(x,list) else [])
    df_test[item] = df_test[item].apply(lambda x: x if isinstance(x,list) else [])
```

第二种情况就是本身是字符串的值，比如 overview 字段。这种我们直接填充空字符串，然后剩下的零星空值则直接 drop 掉即可。

```
df_train["overview"] = df_train["overview"].fillna("")
df_train = df_train.dropna()
df_test["overview"] = df_test["overview"].fillna("")
df_test = df_test.dropna()
```

执行之后，我们再次看一下 df\_train 中的空值情况。

输出如下：

```
id                          0
budget                      0
genres                      0
imdb_id                     0
original_language           0
original_title              0
overview                    0
popularity                  0
poster_path                 0
production_companies        0
production_countries        0
release_date                0
runtime                     0
spoken_languages            0
status                      0
title                       0
Keywords                    0
cast                        0
crew                        0
revenue                     0
has_homepage                0
is_belongs_to_collection    0
has_tagline                 0
dtype: int64
```

可以看到，目前已经没有缺失值了。

### 可视化分析

目前初步的数据分析已经完毕，我们进入可视化分析的环节。

#### 预算与收入的关系

```
px.scatter(df_train,x = "budget", y = "revenue")
```

输出如下：  
![Drawing 2.png](https://s0.lgstatic.com/i/image6/M01/4B/85/Cgp9HWDlapqAD1oIAAGzx04Vz7I066.png)

可以看到，大部分电影还是小成本、小票房的电影。随着预算的拉高，相对应的收入也会提高。预算和收入存在一定的正相关性。所以预算应该是我们的核心特征之一。

#### 时长和收入的关系

```
px.scatter(df_train,x = "runtime", y = "revenue")
```

输出如下：  
![Drawing 3.png](https://s0.lgstatic.com/i/image6/M00/4B/8D/CioPOWDlaqCAJqATAAGC7viTrsk423.png)

可以发现两个信息：

-   绝大多数电影的时长都在 90 ~ 150 分钟左右；
    
-   电影太短或者太长，收入都不会太高。
    

这只能说时长和收入存在一定的相关性。

#### 流行度与收入的关系

```
px.scatter(df_train,x = "popularity", y = "revenue")
```

输出如下：  
![Drawing 4.png](https://s0.lgstatic.com/i/image6/M01/4B/85/Cgp9HWDlaqaAFnlfAAEpT7-E8Wk967.png)

可以看到，流行度要高还是很难的，大多数电影的流行度非常一般。不过流行度高的电影虽然收入不一定会好，但基本也都不差。 流行度应该也可以作为我们的特征之一。

#### 列表类值与收入的关系

我们之前从 json 中抽取了它们的 name 属性，并转换成了 list，那我们如何查看这些列表和收入的关系呢？ 毕竟我们的可视化都基于数值。

有一种简单的处理方法就是，我们不去尝试理解列表中的 name 到底是什么值， 而是只看列表的元素个数，看是否和收入能产生联系。

我们以类别的值为例。首先我们创建一个新的列，来代表类别字段的列表长度, 实现方法也很简单，就是对该列 apply len 函数即可。

```
df_train["genres_list_length"] = df_train["genres"].apply(len)
```

之后就能够画出类别数与收入的关系：

```
px.scatter(df_train,x="genres_list_length", y = 'revenue')
```

输出如下：  
![Drawing 5.png](https://s0.lgstatic.com/i/image6/M00/4B/8D/CioPOWDlaq2AXcZUAAEEV_5DEDw643.png)

可以看到，类别的数量和收入还真有关系，类别为 3～4 个的电影收入上限都会高一些。当然不排除主流的电影类别都是 3～4 个所以才显得高。但是另一个方法，一个电影算不算主流本身也是可以预测收入的原因之一，所以可以认为类别数和收入之间是存在关系的。

所以我们对于所有列表类型的特征，都是用其个数作为特征。

#### 时间与收入的关系

现在来看一下我们不同年份的收入数据，我们都知道近几年来电影收入是逐渐增多的。所以理论上收入和年份应该是正相关的关系。要绘制年份的图，首先我们需要将 release\_date 字段转换为 datetime，然后提取年份作为新的字段，之后对年份字段做 groupby 即可。

```
df_train = df_train[df_train["year"] <= 2015]
df_train["year"] = pd.to_datetime(df_train["release_date"]).dt.year
df_year_group = df_train[["revenue","year"]].groupby("year").sum()
px.line(df_year_group) 
```

输出如下：  
![Drawing 6.png](https://s0.lgstatic.com/i/image6/M01/4B/85/Cgp9HWDlarOAX_VdAADI1pDAaA4918.png)

可以看到，数据上确实和我们估计的一致，电影收入和上映的年份呈强烈的正相关关系，说明电影行业的收入近几年有了蓬勃的发展。

到这里，我们的可视化分析就基本结束了。通过对这几类值进行可视化分析，我们对这些字段对目标值的影响有了一定的认识。

### 特征工程

现在进入我们的特征工程环节。毕竟最终我们在训练模型的时候，模型接收的只能是数字。

#### 处理列表类特征

基于刚才的分析，列表类数据我们统一使用它们的元素个数来作为特征。我们所有的列表类的列名字都存在 json\_features 数组中，我们只需要遍历它来批量添加新字段即可。

新字段的名称就为原字段名+count，代码如下：

```
for item in json_features:
    df_train[item + "_count"] = df_train[item].apply(len)
    df_test[item + "_count"] = df_test[item].apply(len)
df_train.columns
```

输出如下：

```
Index(['id', 'budget', 'genres', 'imdb_id', 'original_language',
'original_title', 'overview', 'popularity', 'poster_path',
'production_companies', 'production_countries', 'release_date',
'runtime', 'spoken_languages', 'status', 'title', 'Keywords', 'cast',
'crew', 'revenue', 'has_homepage', 'is_belongs_to_collection',
'has_tagline', 'genres_count', 'production_companies_count',
'production_countries_count', 'Keywords_count',
'spoken_languages_count', 'cast_count', 'crew_count'],
      dtype='object')
```

可以看到，我们生成的新字段都被添加到 DataFrame了。

#### 处理字符串的特征

我们还有几个特征是字符串的类型，比如 original\_title， overview 和 title。对它们，我们则采用它们字符串的长度来作为特征值。

```
string_features = ["original_title", "overview", "title"]
for item in string_features:
    df_train[item+"_len"] = df_train[item].apply(len)
    df_test[item+"_len"] = df_test[item].apply(len)
df_train.columns
```

输出如下：

```
Index(['id', 'budget', 'genres', 'imdb_id', 'original_language',
'original_title', 'overview', 'popularity', 'poster_path',
'production_companies', 'production_countries', 'release_date',
'runtime', 'spoken_languages', 'status', 'title', 'Keywords', 'cast',
'crew', 'revenue', 'has_homepage', 'is_belongs_to_collection',
'has_tagline', 'genres_count', 'production_companies_count',
'production_countries_count', 'Keywords_count',
'spoken_languages_count', 'cast_count', 'crew_count',
'original_title_len', 'overview_len', 'title_len'],
      dtype='object')
```

#### 处理日期特征

从刚才的分析，我们都知道年份对于收入很关键，事实上时间一直都和收入是正相关的。比如月份来看，7～9 月的收入是最高的，因为是暑期。所以这里我们把时间维度都加成新列，作为特征。

```
date_obj = pd.to_datetime(df_train.release_date)
df_train["year"] = date_obj.dt.year
df_train["month"] = date_obj.dt.month
df_train["day"] = date_obj.dt.day
df_train["dayofweek"] = date_obj.dt.dayofweek
date_obj = pd.to_datetime(df_test.release_date)
df_test["year"] = date_obj.dt.year
df_test["month"] = date_obj.dt.month
df_test["day"] = date_obj.dt.day
df_test["dayofweek"] = date_obj.dt.dayofweek
```

这里，我们也新增了年、月、日和星期几作为新的特征列。

#### 抽取特征

特征准备完毕，我们还需要把所有我们需要的特征抽取出来，抛弃掉那些已经不需要的。

```
feature_list = ["budget", "popularity", "runtime", "has_homepage", "is_belongs_to_collection", "has_tagline",  'genres_count', 'production_companies_count',
'production_countries_count', 'Keywords_count',
'spoken_languages_count', 'cast_count', 'crew_count',
'original_title_len', 'overview_len', 'title_len', "year","month","day", "dayofweek"]
df_train_features = df_train[feature_list]
df_train_features.columns
df_test_features = df_test[feature_list]
```

然后把模型拟合的目标列也抽取出来：

```
df_train_target = df_train["revenue"]
df_train_target
```

目前万事俱备，我们开始训练模型。

### 建立模型

这次的特征非常复杂，并且包含非常多和收入存在非线性关系的列，所以我们还是使用上一讲时候的 XGBoost 来建立模型。

```
from xgboost import XGBRegressor
xgb = XGBRegressor(n_estimators = 2000 , random_state = 0 , max_depth = 27)
xgb.fit(df_train_features, df_train_target)
```

执行一段时间后输出：

```
XGBRegressor(base_score=0.5, booster='gbtree', colsample_bylevel=1,
             colsample_bynode=1, colsample_bytree=1, gamma=0, gpu_id=-1,
             importance_type='gain', interaction_constraints='',
             learning_rate=0.300000012, max_delta_step=0, max_depth=27,
             min_child_weight=1, missing=nan, monotone_constraints='()',
             n_estimators=2000, n_jobs=8, num_parallel_tree=1, random_state=0,
             reg_alpha=0, reg_lambda=1, scale_pos_weight=1, subsample=1,
             tree_method='exact', validate_parameters=1, verbosity=None)
```

### 获取结果

我们用训练出来的模型去预测一次我们的训练集，并且和我们训练集的 revenue 进行比较，来看一下模型的均方误差。

```
from sklearn.metrics import mean_squared_error
pred_target = xgb.predict(df_train_features)
print("模型均方误差:", mean_squared_error(pred_target, df_train_target))
```

输出如下：

```
模型均方误差: 352.18567634130454
```

电影的 revenue 绝大多数都是千万量级，所以这个误差表示模型还是相对比较准确的。

最后一步，就是使用模型来预测 test 数据集中的电影收入。

```
df_test["pred_revenue"] = xgb.predict(df_test_features)
```

查看一下我们预测的结果：

```
df_test[["title", "pred_revenue"]].head(5)
```

输出如下：

![Drawing 7.png](https://s0.lgstatic.com/i/image6/M00/4B/8D/CioPOWDlasCAdPWNAABeMLDV3PU400.png)

### 小结

至此，我们电影预测的实战项目就基本完成了，今天接触数据集比以往的都要复杂一些。回顾一下，今天主要涉及了以下新的知识点：

-   对于列很多的数据，可以使用 df.head(1).transpose 来完成查看一条记录的内容；
    
-   JSON 数据，一种 python 字典的字符串表示，也可以表示字典列表；
    
-   使用 ast.literal\_eval 方法可以将 json 字符串转换为 Python 的字典或者列表；
    
-   可以使用 df.isna().apply(lambda x: 0 if x else 1) 来将有缺失值的列改为 0 或者 1 取值的数据列，用于模型使用；
    
-   对于内容很复杂的字段，可以考虑使用简单的数值属性来建立模型，比如我们今天的列表字段，我们就使用了它的元素个数作为特征来建立模型。
    

今天的例子比之前的都会更复杂一些，不过，恭喜你都成功完成了！
