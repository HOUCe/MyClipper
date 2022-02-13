---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


在第 16 讲中，我们使用 pandas 对电商的用户行为数据做了一些简单的分析，并为短信营销提供了人群维度和时间维度的参考。不知你在分析完后是否有意犹未尽的感觉，是否总觉得那份数据集中还隐藏了很多的信息在向你招手？

现在我们已经完成了 NumPy 相关的学习，习得了一身从数值维度切入进行数据分析的本领。本讲我将会带你继续杀回电商数据集，尝试更进一步，挖掘出更多的信息。

### 任务背景

通过上一次你对营销短信的人群与时段的出色分析，阿普闪购的推广活动圆满达成了目标。当日订单量不断刷新新高，但与此同时，另一个问题也暴露出来：部分供应商对此次活动准备不足，导致生产跟不上了，很多商品发货周期变长，用户的整体满意度也受到了影响。

接下来，即将迎来下一次的大促节日，这次节日同以往不同，用户在开始活动的前三天内只能浏览商品、收藏或者添加购物车，不能下单，等三天过后才开始秒杀下单。为了不再重蹈之前准备不足的覆辙。你作为阿普闪购的数据分析师再一次临危受命。

这一次，任务很明确：**根据用户前三天预热期的行为记录，预测出每款商品的销量，这样可以提前知会供应商按照预测销量进行准备，避免之前准备不足的问题。**

这次，我们能拿到的数据和上次也是一样的。

-   用户行为表：最近 6 个月的用户行为记录。
    
-   vip 会员表：用户 vip 会员开通情况。
    
-   用户信息表：用户的相关信息。
    

三个表的字段说明如下所示。

用户行为表：

![Drawing 0.png](https://s0.lgstatic.com/i/image6/M01/44/B4/Cgp9HWDAdPyAFRqBAAB6v0DV6Vg387.png)

vip 会员表：

![Drawing 1.png](https://s0.lgstatic.com/i/image6/M01/44/BC/CioPOWDAdQGAFd2kAAA9MYvr6j0538.png)

用户信息表：

![Drawing 2.png](https://s0.lgstatic.com/i/image6/M01/44/BC/CioPOWDAdQeAJxxmAACGe0G0_-g395.png)

### 问题分析

从任务说明中可以发现，我们的核心任务是从其他行为（点击、添加收藏、添加购物车）预测商品的销量。这和我们上一次的分析维度不同，这次主要是从商品维度进行分析。

我们有三个数据源，VIP 会员表和用户信息表基本都是用户的信息。而用户行为表中则包含了商品的信息，所以这次我们重点从用户行为表入手。但面临的挑战是：用户行为表是面向行为的，每一行代表一个用户的一次行为，可能是点击、可能是加入购物车，等等。我们首先需要从这个表中抽象出一个面向商品的表，然后再尝试建立预测销量的模型。

### 数据清洗

接下来进入数据清洗环节。在工作目录新建文件夹 chapter20，之后将 chapter16 中用到的 data\_set 文件夹拷贝到 chapter20 文件夹中。

在 VS code 打开chapter20 文件夹，并新建 Notebook，保存为 chapter20.ipynb。

首先，导入必要的工具包以及加载用户行为表。

```
import pandas as pd
import numpy as np
df_user_log = pd.read_csv("data_set/user_behavior_time_resampled.csv")
df_vip_user = pd.read_csv("data_set/vip_user.csv")
```

你可能已经忘记了 user\_log 表的基本情况，我们打印出来看一下。

输出为：  
![Drawing 3.png](https://s0.lgstatic.com/i/image6/M01/44/B4/Cgp9HWDAdROAe26AAAD_78GuvhY590.png)

该表一共有 1098w 行记录，用户的每一次行为是就是一行。其中本次任务我们主要感兴趣的是 item\_id（用于区分商品），以及 action\_type（用户行为的标记）。

现在我们来查看一下这个表的缺失值记录。

```
df_user_log.isnull().sum()
```

输出：

```
user_id            0
item_id            0
cat_id             0
seller_id          0
brand_id       18132
time_stamp         0
action_type        0
timestamp          0
click              0
cart               0
order              0
fav                0
dtype: int64
```

brand\_id 有 18132 个缺失值，但我们这次的任务并不需要考虑 brand\_id，所以不需要处理。

下一步就是查看 action\_type 的取值，从数据集的描述来看，action\_type 只存在 0-3 的取值，由于这个字段对我们很重要，所以我们需要确认是否存在异常值。

```
df_user_log["action_type"].value_counts()
```

输出为：

```
0    9709458
2     659577
3     600738
1      15293
Name: action_type, dtype: int64
```

可以看到，action\_type 的取值没有异常值，并且点击行为都远高于其他三种行为，其中加到购物车的数量最少。

目前看来数据集已经基本满足我们的要求，接下来进入下一个阶段：特征工程。

### 特征工程

#### 为什么需要特征工程

在绝大多数需要建立模型的数据分析任务里，特征工程都是必不可少的环节。顾名思义，特征工程就是指准备特征、筛选特征的工作，只是这项工作具有一定的重要性和复杂度，所以也会叫作工程。

以线性回归模型为例，通过上一节的学习，我们都知道线性回归模型本质上是从找出自变量和因变量的关系。现实问题中，因变量是很好找的，因为往往因变量就是我们要预测的任务，比如在这个例子中，因变量就是我们的预测目标：商品的销量。

另一方面，**自变量，也称为模型的特征**，往往没那么明显。一个原因是因变量往往会受非常多的因素所影响，有直接的，也有间接的。另一个原因是我们拿到的数据集上往往没有现成的特征给我们。这需要我们从原始数据集中经过分析、变换之后才能得到我们想要的特征。

#### 特征分析

特征工程的第一步，是根据我们已有的数据（哪怕不是直接可用的数据）以及我们的因变量，来预设出一组特征。第二步，就是从原始数据中计算出这些特征。第三步则是用这些特征对模型进行训练，如何训练出的模型有问题，则需要重新回到第一步，尝试重新尝试其他的特征。

我们这次的因变量是商品的销量，能够作为参考的数据是用户在预热期的三天的行为。用户在浏览商品时，不能下单的话，主要的行为就是：点击商品、把商品添加到购物车，以及收藏商品。那对于商品来说，商品的点击数、加购物车的次数，以及收藏的次数是否可以作为销量的特征呢？

判断特征是否有效可以从特征的值改变是否会影响因变量的值这个标准出发。这三个特征，不管是点击数，还是收藏数，还是加购物车的次数越多，就说明商品越受欢迎，则说明越可能有用户来购买，反之亦然。所以这三个特征都可以作为我们这次任务的特征。

除了商品维度的特征，商品所在店铺的特征是否对商品的销量有帮助呢？比如在一些商品数比较多的店铺，往往用户搜索进入的概率就大，那商品被用户看到的概率也就变大了。另一方面，店铺的 VIP 用户多寡似乎也能说明店铺是否优质，优质的店铺的商品理论上来说也更好卖一些。

综上所述，我们初步总结了五个可能对商品销量有影响的特征。

商品维度：

-   点击数；
    
-   添加到购物车的次数；
    
-   收藏数。
    

商品所在店铺的维度：

-   店铺的商品数；
    
-   店铺所拥有的 VIP 用户数。
    

#### 商品特征计算

首先我们尝试计算商品的特征，从三张表的描述可以得知，在用户行为表中，有用户对商品的行为记录，所以我们从用户行为表入手来计算商品的特征。

基本的思路就是把用户行为表按照 item\_id 的维度聚合起来，然后点击数、加购物车数、收藏数和购买数都应该是聚合后的表的列。但目前用户行为表中没有这四列，相关的信息都由 action\_type 来表示了。

所以第一步，我们需要把 action\_type 的内容展开成四列来分别表示：点击、加购物车、收藏和下单。

规则就是如果 action\_type 是收藏时，则收藏列为 1，其他三列为 0；action\_type 为下单时，下单一列为 1，其他三列为 0, 以此类推。

这里就涉及我们在生成新列的时候，不仅是从现有列直接计算生成，而是需要有一定的逻辑判断（判断 action\_type ）的值，所以我们可以用 Series 的 apply 函数来实现，apply 函数可以把一个函数执行到指定的 Series 上，然后把函数的返回值作为新的 Series 返回。

代码如下：

```
df_user_log["click"] = df_user_log.action_type.apply(lambda l:1 if l==0 else 0)
df_user_log["cart"] = df_user_log.action_type.apply(lambda l:1 if l==1 else 0)
df_user_log["order"] = df_user_log.action_type.apply(lambda l:1 if l==2 else 0)
df_user_log["fav"] = df_user_log.action_type.apply(lambda l:1 if l==3 else 0)
df_user_log
```

解释一下上面的代码，这里我们用到了 lambda 表达式。lambda 表达式是一种精简的函数表示法。形式如下：

在 apply 函数中使用 lambda 表达式，Series 会对每个元素都执行一次这个表达式，当前执行的元素就是 lambda 表达式的参数，也就是 l。然后每一次执行，我们都会判断 l 的值（也就是action\_type）来判断是返回 0 还是 1。

最后，所有 lambda 表达式的结果汇总起来成一个新的 Series，作为 apply 函数的返回值，然后我们将其赋值给我们创建的新列。

上述代码的输出如下，红框部分即为我们添加的列。

![Drawing 4.png](https://s0.lgstatic.com/i/image6/M01/44/B4/Cgp9HWDAdSKAD0I6AADT2CSAToU921.png)

我们能看到 click 这一列都是 1 ，而其他三列都是 0，click 行为本身就远比其他三个多，理论上也说得通，为了避免有 Bug，我们还是查看一下这几列的数据分布。

```
print("click 数据分布：\n",df_user_log.click.value_counts())
print("order 数据分布：\n",df_user_log.order.value_counts())
print("fav 数据分布：\n",df_user_log.fav.value_counts())
print("cart 数据分布：\n",df_user_log.cart.value_counts())
```

输出为：

```
click 数据分布：
1    9709458
0    1275608
Name: click, dtype: int64
order 数据分布：
0    10325489
1      659577
Name: order, dtype: int64
fav 数据分布：
0    10384328
1      600738
Name: fav, dtype: int64
cart 数据分布：
0    10969773
1       15293
Name: cart, dtype: int64
```

从上面的数据可以看出，基本还是符合预期的。  
之后，我们从原始的行为表中筛选出我们感兴趣的字段。

```
df_clean = df_user_log[["item_id", "click", "cart", "order","fav"]]
df_clean
```

输出为：  
![Drawing 5.png](https://s0.lgstatic.com/i/image6/M01/44/B5/Cgp9HWDAdTeADUucAAFAbPPN_tM071.png)

筛选了之后，行是没有变化的，但只剩下五列了。接下来，我们将这个表以 item\_id 聚合，同一个 item\_id 的点击数、下单数、加购物车数和收藏数进行求和。这样我们就可以得到每个商品的汇总结果了。

```
df_item = df_clean.groupby(["item_id"]).sum()
df_item
```

输出为：  
![Drawing 6.png](https://s0.lgstatic.com/i/image6/M01/44/B5/Cgp9HWDAdT-AexVBAADkkVhOqxI168.png)

可以看到，输出的表中 item\_id 已经成为索引。并且结果表只剩下 75w 行记录，核心原因就是我们把 item\_id 相同的记录聚合了，所以现在 df\_item 已经是一个商品维度的特征表。

我们以上面的 1113162 号商品为例，数值显示他被点击过 17次，没有被加过购物车，被购买过两次，以及被收藏过一次。

至此，我们商品维度的特征就准备完毕了。

#### 店铺特征计算

店铺特征包含两个，需要分别进行计算。但一致的是，我们都希望得到一个店铺 id，也就是seller\_id 作为 index 的表，而特征则是表的列。

（1）店铺拥有的 vip 用户数

首先我们回顾一下 vip 用户表。

输出为：  
![Drawing 7.png](https://s0.lgstatic.com/i/image6/M01/44/B5/Cgp9HWDAdUqAUEFnAAE1mAP_a9Q396.png)

表里的 merchant\_id 就是商家的 id，但是在用户行为表中，商家的 id 字段为 seller\_id。所以为了后续对应方便，我们首先需要将该列重命名为 seller\_id。

然后我们要计算每一个商家的 VIP 用户数，该表存储的是用户是否是某个商家的 VIP，label 字段为 1 代表是。这里我们只考虑商家维度，所以我们只需要简单按照 seller\_id 聚合该表，让 seller\_id 一致的记录对应的 label 字段求和，即可得到每个商家的 VIP 用户数。

```
df_vip_user = df_vip_user.rename( columns={"merchant_id" : "seller_id"})
df_brand_vip_users = df_vip_user[["seller_id", "label"]].groupby("seller_id").sum()
df_brand_vip_users
```

输出为：  
![Drawing 8.png](https://s0.lgstatic.com/i/image6/M00/44/BD/CioPOWDAdVSAG3YBAACLpFjqkHM366.png)

聚合完毕后，表的索引就会是 seller\_id ，label 列则代表 VIP 用户的数量，比如最后一行记录的含义就是 id 为 4993 的店铺，有 4 个 VIP 用户。这样，我们第一个店铺特征表已经准备完毕，表的名字为：df\_brand\_vip\_user。

（2）计算每个店铺的商品数

计算店铺的商品数，首先我们可以从用户行为表中筛选出 seller\_id 和 item\_id。 因为我们只关心店铺的商品数，所以我们可以先按 item\_id 去重，这样留下来的记录就是商品的总数。然后我们在这个表的基础上，将 item\_id 列都赋值为 1 ，然后再按 seller\_id 聚合，让 item\_id 做求和的运算，就可以得出每个 seller\_id 对应的商品数。

代码如下：

```
df_seller_item_count = df_user_log[["seller_id", "item_id"]]
df_seller_item_count = df_seller_item_count.drop_duplicates("item_id")
df_seller_item_count["item_id"] = 1
df_seller_item_count = df_seller_item_count.groupby("seller_id").sum()
df_seller_item_count = df_seller_item_count.rename(columns = {"item_id" : "item_count"})
df_seller_item_count
```

输出为：  
![Drawing 9.png](https://s0.lgstatic.com/i/image6/M00/44/BD/CioPOWDAdVyAebgTAACfvacgRqA705.png)

可以看到，我们第二个店铺特征的表也准备好了，index 为 seller\_id， 有一列 item\_count 代表店铺的商品总数。

#### 商品-店铺特征关联

接下来，我们就把两个店铺特征表拼接到我们的商品特征表中。

先来看一下商品特征表。

输出为：  
![Drawing 10.png](https://s0.lgstatic.com/i/image6/M00/44/B5/Cgp9HWDAdWKAfB8oAADpCIEDDBU354.png)

可以看到，目前商品特征表中只有 item\_id，没有 seller\_id。 所以没有办法直接关联店铺特征表。所以第一步，我们首先需要把商品特征表增加 seller\_id 的字段。

要增加 seller\_id，只需要我们用类似之前的方法，从原始的行为表中抽取出 seller\_id 和 item\_id 的对应关系表，然后再将对应关系表拼接进商品特征表中即可。代码如下：

```
df_brand_item_map = df_user_log[["item_id", "seller_id"]]
df_brand_item_map = df_brand_item_map.drop_duplicates("item_id")
df_item = df_item.merge(df_brand_item_map, how="left", on="item_id")
df_item
```

输出：  
![Drawing 11.png](https://s0.lgstatic.com/i/image6/M00/44/B5/Cgp9HWDAdWyAYZw6AAD16QpZvtY187.png)

可以看到，现在我们商品特征表已经多了 seller\_id 的字段，现在我们可以将两个店铺特征表拼接进商品特征表了。

首先拼接店铺的 VIP 用户数表。代码如下：

```
df_item = df_item.merge(df_brand_vip_users,how = "left", on="seller_id")
df_item
```

输出：  
![Drawing 12.png](https://s0.lgstatic.com/i/image6/M00/44/B5/Cgp9HWDAdXOAFSiHAAD8qQyG2l4993.png)

从输出的来看，店铺 VIP 会员数已经拼接到表里了，但是出现了空值，这也是符合预期的。毕竟 VIP 用户表里不是每个店铺都有 VIP 用户。为了不影响后续的模型，我们通过之前学习的填补缺失值的方法，给空值填为 0，这也是符合逻辑的。

```
df_item["label"] = df_item["label"].fillna(0)
df_item
```

输出：  
![Drawing 13.png](https://s0.lgstatic.com/i/image6/M00/44/BE/CioPOWDAdXmAP-xYAAD1e2V4Jo0896.png)

可以看到，通过 fillna 之后，缺失值被替换为了0。

下一步，我们来拼接店铺的商品数特征表。代码如下：

```
df_item = df_item.merge(df_seller_item_count, how = "left", on = "seller_id")
df_item
```

输出：  
![Drawing 14.png](https://s0.lgstatic.com/i/image6/M00/44/BE/CioPOWDAdX-AduUrAADJaIVZlp0877.png)

现在，我们的商品特征表已经完成了，不仅有商品的点击、加购物车、下单和收藏记录，还有商品所在店铺的 VIP 用户数和店铺的商品数。下一步我们就基于这张表建立模型进行分析。

### 回归分析

激动人心的时刻马上就要到了，现在我们通过特征工程的环节，已经整理好了能够预测商品销量的特征表，现在我们建立线性回归模型来进行分析。

#### 拆分训练、测试集合

在训练之前，我们还需要想一个问题，训练出模型后，我们怎么衡量模型的好坏呢？总不能等到真正要在生产环境用上模型的时候才进行测试。

一般常见的做法是，将现有的数据拆成两份，比如 70% 一份，30% 一份。 70% 的数据用来训练模型，训练完之后用剩下的 30% 的数据进行测试。这样就能够在模型上线之前就能够衡量模型的好坏。sklearn 提供了现成的 train\_test\_split 函数，可以帮我们实现数据集的分割。

首先第一步，我们要从商品特征表中拆开自变量和因变量。

-   因变量是很明显的，就是 order 列，代表商品被下单的数量。
    
-   自变量就是除了 order 列之外的所有数值特征，也就是从商品特征表删掉 item\_id，seller\_id 和 order 列，剩下的都可以作为自变量。
    

代码如下：

```
from sklearn.model_selection import train_test_split
X = df_item.drop(columns=["order", "seller_id","item_id"])
y = df_item["order"]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.20, random_state=42)
X_train
```

输出：

![Drawing 15.png](https://s0.lgstatic.com/i/image6/M00/44/BE/CioPOWDAdYeATsL7AAE8k-NpbQ8829.png)

我们商品特征表本身有 75w 行。这里打印出的训练集有 60w 行，刚好占比 80%。可以看到 train\_test\_split 函数干得还不错。

#### 建立线性回归模型

接下来的步骤就比较简单，我们建立回归模型，然后调用 fit 方法来从 X\_train 和 y\_train 中训练模型。

```
linear_model = LinearRegression()
linear_model.fit(X_train, y_train)
```

训练结束之后，我们可以用 linear\_model 来 predict X\_test，然后拿 predict 函数返回的结果和 y\_test 比较，就能知道我们模型的误差。

```
y_pred = linear_model.predict(X_test)
print("Scores:", linear_model.score(X_test, y_test))
print("intercept:", linear_model.intercept_)
print("coef_:", linear_model.coef_)
```

输出：

```
Scores: 0.4417000916369106
MAE: 0.9939945446528016
intercept: 0.32490105754921395
coef_: [-1.09570715e-03  7.64421017e+00  7.20958736e-01  6.07684870e-04
 -3.10573289e-04]
```

模型的相关性分数说 0.44， 对于现实世界中的回归问题来说，这也算是个不错的成绩，说明我们的特征还是很大程度能够影响下单量。MAE 是 0.99，说明对于测试集而言，我们模型预测的结果和实际的真实结果非常接近，说明模型的拟合还是比较好的。

从模型系数中可以看到，我们的前三个特征：click、fav、cart 的系数比较大，后两个特征 label 和 item\_count 的系数比较小，说明对于商品的下单量而言，前三个特征更加重要，关联性更强，后两者则对结果影响相对较小。

有时候凭我们的主观判断，对于特征的重要性判断可能是不准的，我们可以都一并带上，然后在模型训练的环节，通过拟合算法自动去找到不同特征的重要性。这样就比人的判断靠谱多了。

我们来测试一下我们的模型，假设某个商品在预热期间，一共有 100 次点击，2 次加购物车，6 次收藏，这个商品所在的店铺有 10 个vip 用户，一共有 30 个商品。那根据我们的模型来预测这个商品的下单数：

```
print(linear_model.predict([[100, 2 ,6, 10, 30]]))
```

输出：

代表可能会被买 19 次。

### 小结

至此，我们今天的实战就学习完毕了，总结一下，今天我们主要学习了：

-   回归类型任务的分析思路与方法；
    
-   常见的特征工程的思路与方法；
    
-   通过 df.groupby("xxx").sum() 的形式来聚合数据表；
    
-   通过 df.merge 函数来拼接数据表，前提是两个数据表有相同的字段；
    
-   通过 train\_test\_split 来将数据集拆分为训练集和测试集；
    
-   通过 LinearRegression 来建立线性回归模型。
    

通过本节的学习，相信你对回归预测类型的分析能够初步找到初步感觉。当然这一块是非常大的领域，挑战也很多。本系列课程重点还是以带大家入门为主，有兴趣的同学可以自行深入研究。

课后习题：

尝试调整用于训练的特征，看看是否能够进一步降低 MAE。

___

答案（其中一种）：

既然从结果系数来看，label 和 item\_count 对结果的贡献较小，可以从训练特征表中删除，这样训练出的模型的 MAE 是 0.95。
