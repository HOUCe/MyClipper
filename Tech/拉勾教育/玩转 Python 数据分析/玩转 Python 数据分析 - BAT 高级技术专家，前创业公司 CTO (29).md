---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


在之前的课程里我们学习了各种各样的画图技术，从 matplotlib 到 seaborn，最后也了解了一些 plotly 的内容。学习下来你会发现，其实绘图都是相通的。无非就是按照绘图函数的语法传入数据源和配置。但另一方面，绘图又是难的。毕竟我们绘制的图表好和不好，绘图是一方面，更重要的是我们要想清楚，我们希望通过这个图表表示什么信息，这就需要你在数据分析的实战中不断积累经验。

今天我们就通过一个实战的分析案例，来进一步巩固我们学习的绘图技巧，以及学习在实际项目中，如何通过可视化的分析来更清晰地呈现数据。

### 任务目标

近期阿普尔星球的阿普闪购公司开始启动星际业务，把电商业务扩展到地球。第一步就是收购了一家欧洲的电商公司。

收购完之后，公司第一步需要对这家公司的销售情况进行摸底，这个重任自然就落在了在数据分析部门任职的你身上。你需要从这家电商公司的原始订单数据中分析出公司的业务是在变好还是变差，公司哪些产品最受欢迎，以及主要的销售区域和有哪些优质的用户等信息，以提供给公司的战投部门做进一步的经营策略制定。

### 准备数据

首先我们准备数据，其实这份数据我们在 chapter22 已初次见过，只是我们当时并没有进行深入的分析。

首先我们新建本讲的工作目录，在课程目录中新建 chapter26，并从 chapter22 中将 data.csv 拷贝至 chapter26 文件夹中。

#### 数据集描述

现在我们来正式介绍一下该数据集，这是某欧洲电商公司将近一年的交易数据，字段如下：

![Drawing 0.png](https://s0.lgstatic.com/i/image6/M00/49/BD/CioPOWDcKlmAdkT3AACOyljPMt4119.png)

#### 加载数据

下一步，我们先将数据集加载到 notebook 中。用 VS code 打开，并新建 notebook ，保存为 chapter26.ipynb。

首先导入必要的工具包：

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
import plotly.graph_objs as go 
```

加载数据，并查看源数据的摘要：

```
df_goods = pd.read_csv("data.csv")
df_goods
```

输出如下：  
![Drawing 1.png](https://s0.lgstatic.com/i/image6/M00/49/BD/CioPOWDcKmCAALlxAAGlhvxePOM731.png)

如上所示，整个数据集一共包含 54w 条数据，每行数据包含八列，字段的含义可以参考上方的说明表。

### 初步分析

#### 数据概览

DataFrame 有一个非常有用的函数：describe，提供了数据概览的功能。这个函数会将 DataFrame 中包含的数值特征单独拎出来，并给出一些基本的统计特征。这对于初始的分析非常有用。

代码如下:

输出如下所示：

![Drawing 3.png](https://s0.lgstatic.com/i/image6/M00/49/B5/Cgp9HWDcKmmAXR_RAAKV_9R9-7Q775.png)

从上面的输出可以看出，表格中一共有 Quantity、UnitPrice 和 CustomerID 这三个列被认定是数值列，虽然订单 ID：InvoiceNo 看起来也是数值列，但是因为有取消订单（以字母 C 开头就不算数值了），所以没有被列进来。

先解释一下上面输出表格的每一行含义：

-   count，代表 DataFrame 中该列有值的个数；
    
-   mean，平均值；
    
-   std，标准差；
    
-   min，最小值；
    
-   25%，数据从小到大排列，第 25% 大的值；
    
-   50%，同上，代表第 50% 的值， 比如 50%的值是 3 ，代表整个数据集小于3 的数量占比 50%；
    
-   75%，同上，第 75% 大的值；
    
-   max，最大值。
    

从统计信息中，我们大概可以看出以下信息：

-   Quantity，均值是 9.5 ，75% 是 10 ，代表小于 10 的值占比为 75%。 最小值和最大值明显存在异常数据；
    
-   UnitPrice，道理同上，1~5 之间占比绝大多数，最大值和最小值异常，可能存在异常；
    
-   CustomerID，分布基本正常，但似乎空值比较多，只有 40w 的有效值。
    

#### 缺失值处理

CustomerID 是我们这次分析的目标，我们只感兴趣有 CustomerID 字段的记录，所以我们需要处理以下缺失值，首先看一下目前缺失值的情况。

输出如下：

```
InvoiceNo           0
StockCode           0
Description      1454
Quantity            0
InvoiceDate         0
UnitPrice           0
CustomerID     135080
Country             0
dtype: int64
```

CustomerID 存在 13w 条缺失值，Description 存在 1400 条，基本和我们刚才的判断一致，由于 CustomerID 是本次的核心字段，所以我们简单删除缺失值即可。

```
df_goods = df_goods.dropna()
df_goods.isnull().sum()
```

输出如下：

```
InvoiceNo      0
StockCode      0
Description    0
Quantity       0
InvoiceDate    0
UnitPrice      0
CustomerID     0
Country        0
dtype: int64
```

这样，缺失值就处理完毕了。

#### 处理取消的订单

相信细心的你已经发现了，数据集中其实是存在被取消的订单，现在我们来看一下量级有多少。

因为表中的数据是以商品为维度，也就是说一个订单可能在 DataFrame 中有多条记录。我们先来看一下一共有多少个订单。

```
df_goods["InvoiceNo"].nunique()
```

输出如下：

接下来，我们看一下过滤出被取消的记录，保存在 df\_cancelled 中。

```
df_cancelled = df_goods[df_goods["InvoiceNo"].str.startswith("C")]
df_cancelled
```

输出如下：  
![Drawing 4.png](https://s0.lgstatic.com/i/image6/M01/49/BD/CioPOWDcKnWAcMzQAAGl8gILAfc991.png)

最新的 df\_goods 有大概 40w 条记录，其中 8905 条是取消的，整体来看还好，并不是很多。不过从上面的数据中我们还能看到，似乎被取消的订单数量都是负值，这似乎可以解释为什么 Quantity 字段会有很多 0 以下的取值。

我们再来看一下被取消的订单总数：

```
df_cancelled.InvoiceNo.nunique()
```

输出如下：

可以看到，虽然商品数占比较少，但是订单总数的占比还是不低的。为了避免对我们的分析产生影响，我们将这些订单从我们的 DataFrame 中清除。

```
df_goods = df_goods[~df_goods["InvoiceNo"].str.startswith("C")]
df_goods
```

输出如下：  
![Drawing 5.png](https://s0.lgstatic.com/i/image6/M01/49/BD/CioPOWDcKn2AIAmkAAGnRr8F-Ys326.png)

#### 处理异常值

我们再查看下统计数据：

输出如下：  
![Drawing 6.png](https://s0.lgstatic.com/i/image6/M01/49/B5/Cgp9HWDcKoKAAvkYAACGvLX_B4c263.png)

可以看到，随着我们刚才的清洗，UnitPrice 和 Quantity 已经没有负值了。但是最大值似乎仍然存在异常（远远超过均值和 75% 值）。

在之前的课程中，我们通过散点图来筛查异常值的范围，但整个过程还是比较费劲的。比如能看到存在异常大的值，但因为这些值把图像的尺度弄得很大，导致我们无法很好地判断出超过多少就属于异常值。

之前我们绘制的 Quantity 散点图如下所示。0 附近的数据非常密集，但是从这个坐标轴看很难看出最佳的剔除范围。

![Drawing 8.png](https://s0.lgstatic.com/i/image6/M01/49/BD/CioPOWDcKouAPrMkAACos2iTicU832.png)

上一讲中，我们学习了 plotly，我们知道 plotly 的图表是可以放大的，看起来正好可以满足我们分析的需要，而且通过直方图还能够更直观地看出正常值的分布。代码如下：

```
fig = px.histogram(df_goods.Quantity)
fig.show()
```

输出如下：  
![Drawing 10.png](https://s0.lgstatic.com/i/image6/M01/49/BD/CioPOWDcKpOAOMZkAACb8pUl2IQ348.png)

上图中，横轴是 Quantity 的取值，纵轴的取值对应的个数。可以看到，绝大多数值都集中在 0 附近，现在我们来尝试看看到底主流分布的范围是多少。首先把鼠标放在横轴上，按住往左拖动曲线，拖动完毕后如下所示：

![Drawing 12.png](https://s0.lgstatic.com/i/image6/M01/49/B5/Cgp9HWDcKrCActm0AACBjTAq0l8903.png)

然后我们框选我们感兴趣的区域放大，如下所示：

![Drawing 14.png](https://s0.lgstatic.com/i/image6/M01/49/BD/CioPOWDcKraATJjiAACGmDZK9BA055.png)

多次放大后，可以看到 0 附近的详细分布，如下图所示：

![Drawing 16.png](https://s0.lgstatic.com/i/image6/M01/49/B5/Cgp9HWDcKryAMb5aAADHrwxwk6A013.png)

从上图中不难看出，0~24 的占了绝大多数，所以基本可以认为 Quantity >= 25 则是异常数据。

同样的方法分析 UnitPrice。

```
fig1 = px.histogram(df_goods.UnitPrice)
fig1.show()
```

将输出的图像进行放大后，可以看到输出如下所示。

![Drawing 18.png](https://s0.lgstatic.com/i/image6/M01/49/BE/CioPOWDcKtmAXPyjAADaSYdpq2E479.png)

从上图中，基本可以认为 >=5 的 UnitPrice 即为异常的数据。

所以，我们认为合理的 Quantity 最大值为 25， UnitPrice 为 5。 回想之前通过 describe 描述的统计数据，Quantity 的均值为 13.02，UnitPrice 均值为 3.11，这也和我们分析的合理最大值能够呼应上。 接下来我们就继续洗掉非合理的数据。

```
df_goods = df_goods[df_goods.UnitPrice < 5]
df_goods = df_goods[df_goods.Quantity < 25]
df_goods
```

输出如下：  
![Drawing 19.png](https://s0.lgstatic.com/i/image6/M01/49/B5/Cgp9HWDcKuCAKvGZAAGmNKoTaEw676.png)

现在我们的数据集已经准备好，可以做进一步的分析了。

### 业务分析

#### 最受欢迎的商品

首先，我们分析一下最受欢迎的商品，以及它到底有多受欢迎。一般来说，我们可以先用 value\_counts 来查看。代码如下所示，我们使用 head 函数是因为我们先取前几条查看即可。

```
df_goods.Description.value_counts().head()
```

输出如下：

```
WHITE HANGING HEART T-LIGHT HOLDER    1669
JUMBO BAG RED RETROSPOT               1328
PARTY BUNTING                         1274
LUNCH BAG RED RETROSPOT               1206
ASSORTED COLOUR BIRD ORNAMENT         1157
Name: Description, dtype: int64
```

可以看到， WHITE HANDING HEART T-LIGHT HOLDER 这件商品是最受欢迎的，看起来像是一个手提的灯座。

为了更直观地分析，我们也可以直接把 value\_counts 返回的 Series 以直方图的形式表示出来。代码如下：

```
buying_count = df_goods.Description.value_counts()[:20]
sns.set()
sns.barplot(buying_count.index, buying_count.values)
```

输出如下：  
![Drawing 21.png](https://s0.lgstatic.com/i/image6/M01/49/B5/Cgp9HWDcKuyAaqfAAAE4SdIcnew104.png)

可以看到上面的图有点问题，由于我们的商品名称比较长，所以全部都重叠到了一起。像这样的情况，我们可以用以下两个方式解决：

-   把画布变长；
    
-   旋转 x 轴的刻度。
    

代码如下：

```
buying_count = df_goods.Description.value_counts()[:20]
sns.set()
plt.figure(figsize=(20,9))
sns.barplot(buying_count.index, buying_count.values)
plt.xticks(rotation = 90)
```

输出如下：  
![Drawing 23.png](https://s0.lgstatic.com/i/image6/M01/49/BE/CioPOWDcKvWAFw1SAAEu92ks_UI227.png)

可以看到，除了第一名的 WHITE HODLER 远高于第二名，后面的几种商品差距都并不是特别大。

#### 购买最多的国家

接下来，我们来分析订单主要都销往了哪些国家。首先，因为我们考虑的是订单维度的数据，所以第一步我们需要将数据表按照订单的维度进行聚合。另外，为了方便我们分析优质的用户，我们可以直接用订单号、国家客户 ID 进行聚合。

```
df_customer_country_map = df_goods[['CustomerID', 'InvoiceNo', 'Country']].groupby(['CustomerID', 'InvoiceNo', 'Country']).count()
df_customer_country_map = df_customer_country_map.reset_index(drop = False)
df_customer_country_map
```

输出如下：  
![Drawing 25.png](https://s0.lgstatic.com/i/image6/M01/49/B5/Cgp9HWDcKv6AT_3JAAHxpX1TvEU855.png)

聚合表已经有了，我们现在先将国家的频率数据整理出来。

```
countries = df_customer_country_map['Country'].value_counts()
```

为了展示国家类型的数据，plotly 提供了世界地图的工具，可以在世界地图上展示国家的频率信息。代码如下, 基本用法我在注释中给出了说明。

```
data = dict(type='choropleth',
locations = countries.index,
locationmode = 'country names', z = countries,
text = countries.index, colorbar = {'title':'订单数'},
colorscale=[[0, 'rgb(255,255,255)'],
             [0.1, "rgb(0, 255,255)"],
            [1, 'rgb(255,0,255)']])
layout = dict(title='订单国家分布')
fig3 = go.Figure(data = [data], layout = layout)
iplot(fig3, validate=False)
```

输出如下所示：  
![Drawing 27.png](https://s0.lgstatic.com/i/image6/M01/49/BE/CioPOWDcKwaALdFNAAInwFjfNvk010.png)

地图上国家的颜色代表了该国家的订单数，紫色代表订单数最多，而浅青色代表订单数最少。初步看大部分地区订单都较少，但欧洲有部分国家颜色较深，我们将其放大。如下所示：

![Drawing 29.png](https://s0.lgstatic.com/i/image6/M01/49/BE/CioPOWDcKw2AHfhJAAH3aTxonsk657.png)

从上图中可以看出，英国的订单数占到了绝大多数，旁边青色的德国和法国也有一定的订单，但相比英国也很少。基本可以说该家公司的客户基本都在英国，没有做国际贸易。

#### 优质用户

接下来我们分析优质的用户，我们已经有了订单维度的表，只需要按照 CustomerID 来进行聚合，并使用 count 函数来指定 InvoiceNo 和 Country 字段聚合之后显示计数。

```
df_customer_order_count = df_customer_country_map.groupby("CustomerID").count()
df_customer_order_count
```

输出如下：  
![Drawing 31.png](https://s0.lgstatic.com/i/image6/M01/49/B5/Cgp9HWDcKxaAcM18AAD2_MMTvAE811.png)

因为我们在 groupby 之后使用了 count，所以 InvoiceNo 和 Country 的值都是一样的，都代表该用户有多少条订单记录，也就是订单数。

接下来我们使用条形图来看下单数最多的 20 个用户, 代码如下：

```
df_customer_order_count = df_customer_order_count.sort_values(by="InvoiceNo",ascending=False).head(20)
df_customer_order_count = df_customer_order_count.reset_index(drop=False)
df_customer_order_count = df_customer_order_count.rename(columns = {"InvoiceNo":"OrderCount"})
fig = plt.figure(figsize=(20,9))
sns.barplot(df_customer_order_count.index, df_customer_order_count.OrderCount)
plt.xticks(df_customer_order_count.index, df_customer_order_count.CustomerID, rotation=90)
```

输出如下：  
![Drawing 33.png](https://s0.lgstatic.com/i/image6/M01/49/BE/CioPOWDcKx-AJlRDAACpL29WHSA971.png)

可以看到，前两名用户基本平分秋色，每个人都是 190 单左右，远超过第三名。不过整体来说，这 20 个用户都应该算优质用户。

#### 销售额随着时间变化的趋势

最后一步是分析销售额的趋势，首先在原始数据表中是没有销售额一列的。只有单元价格和数量，所以我们首先需要建立销售额一列：

```
df_goods["TotalPrice"] = df_goods["UnitPrice"] * df_goods["Quantity"]
df_goods
```

输出如下：  
![Drawing 34.png](https://s0.lgstatic.com/i/image6/M01/49/B5/Cgp9HWDcKyqAb7_LAAIIfA2w26o374.png)

可以看到，我们新建的 TotalPrice 一列已经成功了，在表格的最后。

第二步，我们将 InvoiceDate 字改为 datetime 类型，并设置为数据表的索引。

```
df_goods.InvoiceDate = pd.to_datetime(df_goods.InvoiceDate)
df_goods = df_goods.set_index("InvoiceDate")
df_goods
```

输出如下：  
![Drawing 35.png](https://s0.lgstatic.com/i/image6/M01/49/B5/Cgp9HWDcKzKAH9NxAAIKOd_n_-I002.png)

现在日期已经是索引的，然后我们只关注 TotalPrice， 所以这里我们首先将 TotalPrice 单独拿出来, 并且新增一个代表日期的列。虽然 InvoiceDate 也有日期，但是它也包含了时间，我们需要一个单纯日期的列用来做聚合。

```
df_cum = df_goods[["TotalPrice"]]
df_cum["date"] = df_cum.index.date
df_cum
```

输出如下：  
![Drawing 37.png](https://s0.lgstatic.com/i/image6/M01/49/BE/CioPOWDcKziAFCUrAAHbCe8T5jo902.png)

现在万事俱备，接下来我们首先用 date 列来聚合，含义就是把每一天的所有销售额都加起来，然后直接调用 plot 函数画图即可。

```
df_cum = df_cum.groupby("date").sum()
df_cum.plot()
```

输出如下：  
![Drawing 39.png](https://s0.lgstatic.com/i/image6/M01/49/B5/Cgp9HWDcK0CAMO2OAAOkF1Eqm2k976.png)

整体来看，销售额随着时间还是逐步增长的，说明该公司还具备一定的成长空间。

### 小结

至此，我们电商数据的可视化分析实战就结束了。 我们首先来回顾一下今天涉及的新内容：

-   使用 DataFrame 的 describe 做数据概览，数据概览报告的含义；
    
-   使用 isna().sum() 查看缺失值情况，并使用 dropna 删除缺失值；
    
-   使用字符串匹配的方法查找 DataFrame 中符合条件的记录；
    
-   使用 plotly 绘制可交互的图表来更高效的处理异常值；
    
-   使用 plt.xticks 的 rotation 参数来旋转 x 轴刻度，避免重叠；
    
-   使用 plotly 绘制世界频率地图。
    

到这里，玩转 Python 数据分析的所有理论知识部分就结束了。

接下来，我们将会用一整个模块，让你综合使用之前学习到的知识进行实战，加深印象，为投身数据分析行业做好准备。
