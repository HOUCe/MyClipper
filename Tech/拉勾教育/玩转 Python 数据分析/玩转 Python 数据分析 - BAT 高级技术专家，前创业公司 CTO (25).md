---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


上一讲我们学习了 Matplotlib 画图的基本流程和一些常见图像属性的设置。本讲我们来学习在特征相关性分析中最常用的两种图形：折线图和散点图的用法，以及如何用折线图与散点图来分析特征之间相关性的方法。

### 折线图

折线图我们应该已经不陌生了，在上一讲我们绘制的图像都是折线图。Matplotlib 中，绘制折线图主要通过 plot 函数实现。

今天我们来学习折线图一些更深入的使用技巧和方法。

#### 绘制平滑的曲线

虽然我们都说折线图，但是并不代表 plot 函数只能画直线，我们以画 sin 函数为例。在工作目录下创建文件夹，名字为 chapter22，并在 VS code 中打开该文件夹。打开之后新建 Notebook，保存为 chapter22.ipynb。

sin 函数的范围是 -1 到 1，所以这次我们设定 y 轴的范围为 -5 到 5。x 轴的范围取 0 到 20。我们画出 x 取值 0 到 20 的正弦曲线。我们上一讲已经学习过画图的做法，稍加改动就能够实现这次的需求。代码如下：

```
x_ranges = np.arange(0, 20)
figure1 = plt.figure(figsize=(10, 10)) 
ax1 = figure1.add_subplot(1,1,1)
plt.title("y = sin(x)")
plt.xlabel("X")
plt.xlim([0, 20])
plt.ylabel("Y")
plt.ylim([-2.5, 2.5])
plt.plot(x_ranges,np.sin(x_ranges) )
plt.show()
```

输出如下：

![Drawing 1.png](https://s0.lgstatic.com/i/image6/M00/46/8C/Cgp9HWDLHFuACVqfAABmAuNAlvE627.png)

可以看到，曲线虽然画出来了，但是却不够平滑，比较生硬。本质的原因是我们的数据源，plot 函数本质就是把数据源每个点用线连起来，所以我们只需要提供更“密集”的数据源，也就是提高采样的频率，就能够获得更平滑的曲线。

用这个例子来说，y 的数据是由 x 决定的，所以我们只需要提供一个间隔更小的 x\_ranges 即可。之前的课程学过，numpy 的 arange 函数的第三个参数就是步长，默认是 1，这里我们只需要在同样的范围情况下，缩短步长就可以获得更密集的数据。我们可以测试一下：

```
print("步长为1：", np.arange(0,20))
print("步长为0.2：", np.arange(0, 20, 0.2)) 
```

输出如下：

```
步长为1： [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19]
步长为0.2： [ 0.   0.2  0.4  0.6  0.8  1.   1.2  1.4  1.6  1.8  2.   2.2  2.4  2.6
2.8  3.   3.2  3.4  3.6  3.8  4.   4.2  4.4  4.6  4.8  5.   5.2  5.4
5.6  5.8  6.   6.2  6.4  6.6  6.8  7.   7.2  7.4  7.6  7.8  8.   8.2
8.4  8.6  8.8  9.   9.2  9.4  9.6  9.8 10.  10.2 10.4 10.6 10.8 11.
11.2 11.4 11.6 11.8 12.  12.2 12.4 12.6 12.8 13.  13.2 13.4 13.6 13.8
14.  14.2 14.4 14.6 14.8 15.  15.2 15.4 15.6 15.8 16.  16.2 16.4 16.6
16.8 17.  17.2 17.4 17.6 17.8 18.  18.2 18.4 18.6 18.8 19.  19.2 19.4
19.6 19.8]
```

可以看到，当步长变为0.2 之后，生成的数组就变得很密集。  
将 x\_ranges 的生成增加步长，代码如下：

```
x_ranges = np.arange(0, 20, 0.2) 
figure1 = plt.figure(figsize=(10, 5)) 
ax1 = figure1.add_subplot(1,1,1)
plt.title("y = sin(x)")
plt.xlabel("X")
plt.xlim([0, 20])
plt.ylabel("Y")
plt.ylim([-2.5, 2.5])
plt.plot(x_ranges,np.sin(x_ranges) )
plt.show()
```

输出如下：  
![Drawing 3.png](https://s0.lgstatic.com/i/image6/M00/46/8C/Cgp9HWDLHGWAbEARAABowqriMSw624.png)

可以看到，数据曲线变得平滑了很多。

#### 连线规则

既然 plot 函数是把传入的点用线连接起来，那连接的顺序是什么呢？plot 函数的连接顺序并不是按照 x 从左到右画，而是严格按照传入的数据源的顺序，首先将 x\[0\], y\[0\] 连接到 x\[1\], y\[1\]， 然后再将 x\[1\] y\[1\] 连接到 x\[2\], y\[2\]，以此类推。所以理论上，我们在坐标轴中并不仅限于画函数图形，而是可以画任意图形。

接下来，我们通过四个点来在坐标轴上画一个三角形，来演示 plot 函数的连线规则。

```
 figure2 = plt.figure(figsize=(10, 5)) 
ax1 = figure2.add_subplot(1,1,1)
plt.title("Test Line")
plt.xlabel("X")
plt.xlim([0, 20])
plt.ylabel("Y")
plt.ylim([0, 10])
plt.plot([2, 10, 5,2],[2, 2, 5,2])
plt.show()
```

输出如下：  
![Drawing 5.png](https://s0.lgstatic.com/i/image6/M00/46/8C/Cgp9HWDLHG6AZ54pAABCcQ0atxM652.png)

根据我们传入的点，上述图形的绘制顺序如下，1 → 2 → 3。

![Drawing 7.png](https://s0.lgstatic.com/i/image6/M00/46/8C/Cgp9HWDLHHSAW5iSAABbJjjeSdI711.png)

#### fmt 语法

折线图的最后一个话题，我们来介绍一下它格式设置的语法。折线图常见的格式设置主要包含三种：

-   marker，也就是点的样式，这里的点指的就是数据源中传入的点；
    
-   line，线的样式，比如有实线、虚线；
    
-   color，也就是线条的颜色，这个我们之前通过参数 c 设置过。
    

我们先简单来感受一下样式的设置，在刚才的 cell 中，将 plot 函数增加一个字符串的参数。如下所示：

```
plt.plot([2, 10, 5,2],[2, 2, 5,2], "o--r")
```

三角形将会变成如下所示：  
![Drawing 9.png](https://s0.lgstatic.com/i/image6/M00/46/95/CioPOWDLHH-AUjA8AABCSvFTmiQ591.png)

主要有三个变化：

-   线的连接处变成了圆点，这个就是 marker；
    
-   线变成了虚线；
    
-   颜色变成了红色。
    

这些都是通过 "o--r" 这个字符串实现的，这个字符串，就称之为 fmt 参数。

fmt 参数本质就是通过一个字符串来一次性设置三种样式。它的格式如下：

```
fmt = "[marker][line][color]"
```

中括号表示可以只写其中的一项或者两项。  
（1）marker 的语法

三个部分我们逐一来看，首先是 marker，可以有如下几种取值：

![Drawing 10.png](https://s0.lgstatic.com/i/image6/M01/46/95/CioPOWDLHIeAb3LWAABYg-5bsWg530.png)

设置 marker 的样式除了在 fmt 字符串中设定其样式外，plot 函数还支持 markersize 参数用于指定 marker 的大小。

可以将上述 plot 函数中的 fmt 字符串的第一个字符替换为其他样式，测试样式的效果。比如我们替换为六边形，代码如下：

```
plt.plot([2, 10, 5,2],[2, 2, 5,2], "H--r", markersize=10)
```

输出如下：  
![Drawing 12.png](https://s0.lgstatic.com/i/image6/M01/46/95/CioPOWDLHJOATUqXAABDGCoVUbo293.png)

可以看到，定点的样式已经变成了六边形。

（2）line 的语法

![Drawing 13.png](https://s0.lgstatic.com/i/image6/M01/46/95/CioPOWDLHJqAZUXhAAAw6AksfGU905.png)

在之前的例子中，line 的类型我们使用的是线段虚线（--），可以修改之前的 plot 函数调用，来感受不同线条的样式，比如这里我们改成线段-点混合虚线（-.）。

```
plt.plot([2, 10, 5,2],[2, 2, 5,2], "H-.r", markersize=10)
```

输出如下：  
![Drawing 15.png](https://s0.lgstatic.com/i/image6/M01/46/95/CioPOWDLHKGAaKhdAABA98iKXLk515.png)

（3）颜色的语法

设置线条和 marker 的颜色，之前我们介绍过可以通过传入 RGB 三个数字的形式来指定。在 fmt 语法中也可以通过颜色的简写来指定，对应关系如下。

![Drawing 16.png](https://s0.lgstatic.com/i/image6/M01/46/95/CioPOWDLHKeAZa49AAA4Tv3Pt2k338.png)

这次我们来改个颜色试试，还是改一下 fmt 的参数，将颜色标记改为 g，即绿色，如下所示：

```
plt.plot([2, 10, 5,2],[2, 2, 5,2], "H-.g", markersize=10)
```

输出如下：  
![Drawing 18.png](https://s0.lgstatic.com/i/image6/M01/46/8D/Cgp9HWDLHK-AEYEGAABEdNn3Kek298.png)

### 散点图

散点图在前一节我们已经画过，和折线图最核心的区别就是，**散点图是直接将点画出来，而不会用线去连接**。在很多离散数据的场景，用折线图往往不能很好地反应特征之间的相关性，尤其是在数据点比较多的情况下，用折线图往往就会非常凌乱。这个时候直接将数据点画出来往往更加直观。

散点图作图的流程和折线图完全一样，只是在绘图的时候，散点图调用的方法是 plt.scatter， 而折线图调用的是 plt.plot。plt.scatter 函数的用法和 plt.plot 很类似，同样是传入 x 轴的数组和 y 轴的数组，而且 scatter 函数同样支持设置点的样式，**但不支持 fmt 语法**。设置散点图的点参数可以使用 scatter 函数的 marker 参数，取值和 fmt 中介绍的 marker 的表格一致。

现在我们直接基于之前画正弦函数（sin 函数）的例子，来看一下散点图的效果：

```
x_ranges = np.arange(0, 20, 0.2) 
figure1 = plt.figure(figsize=(10, 5)) 
ax1 = figure1.add_subplot(1,1,1)
plt.title("y = sin(x)")
plt.xlabel("X")
plt.xlim([0, 20])
plt.ylabel("Y")
plt.ylim([-2.5, 2.5])
plt.scatter(x_ranges,np.sin(x_ranges), marker="*")
plt.show()
```

输出如下：  
![Drawing 20.png](https://s0.lgstatic.com/i/image6/M01/46/95/CioPOWDLHLiAGj5TAABr8HZPCYs940.png)

### 相关性分析实战

散点图在异常数据分析与特征相关性分析中非常常见。接下来我们通过一个实际的案例来学习如何使用散点图来进行异常值和相关性的分析。

#### 数据准备

首先下载数据源（链接:[https://pan.baidu.com/s/1HalPbk8BdkpmaRfcEAu21A](https://pan.baidu.com/s/1HalPbk8BdkpmaRfcEAu21A?fileGuid=xxQTRXtVcqtHK6j8)提取码: 9ccd ），将下载到的 data.csv 保存到 chapter22 目录中。

然后，我们在 notebook 中查看一下数据。

```
import pandas as pd
df_goods = pd.read_csv("data.csv")
df_goods
```

输出如下：

![Drawing 21.png](https://s0.lgstatic.com/i/image6/M01/46/8D/Cgp9HWDLHMOAIw24AADjvG6mls4296.png)

data.csv 存储了欧洲某礼品电商公司销售的订单数据。从上图中可以看到有 54w 条数据，本次我们重点关注以下三列：

![2021617-175615.png](https://s0.lgstatic.com/i/image6/M01/46/8D/Cgp9HWDLHMuAfBoDAABHZY00ops483.png)

#### 异常数据剔除

首先，我们来分析一下 Quantity 这个列是否存在异常值。异常值指的就是和绝大多数值差得很远，或者明显不符合常识的值。对于 Quantity 列来说，小于 0 的肯定是异常值，如果大多数订单都只是 10 到 20， 那如果有记录是几千、几万，那肯定也是异常值。

一般来说，异常值分析主要包含两个方面：

-   分析是否存在异常值；
    
-   分析异常值存在的比例。
    

为什么要分析比例呢？ 如果异常值的比例太高，那说明这个列就没有分析的价值，我们就需要想其他的办法帮助我们找到利于分析的数据。

使用散点图，上述两个步骤都可以轻松完成。

我们尝试绘制 Quantity 与时间的散点图，这样就能看出随着时间的推移的 Quantity 数据的分布。

（1）清洗 Quantity 异常值

第一步，我们需要将订单时间，也就是 InvoiceDate 转换成 datetime 的格式，这样才能将其作为横轴。代码如下：

```
df_goods.InvoiceDate = pd.to_datetime(df_goods.InvoiceDate)
```

然后，我们画出 Quantity 沿着下单日期的分布图。

```
plt.figure(figsize = [18,6])
plt.scatter(df_goods.InvoiceDate, df_goods.Quantity)
```

上述代码中，我们没有创建子图，也没有设置 x/y 轴的属性，当没有这些代码的时候。Matplotlib 会自动为我们创建一个充满画布的子图，然后用我们传入数据的范围来作为轴的尺度。

上述代码输出如下：

![Drawing 23.png](https://s0.lgstatic.com/i/image6/M01/46/8D/Cgp9HWDLHN-ACBe7AABAG1Umt2s792.png)

从图中可以清晰地看出 Quantity 数据存在异常值，一部分是小于 0 的，一部分远大于大多数值。我们首先剔除小于 0 的值，以及大于 10000 的值，再来看一下分布情况。

代码如下：

```
df_goods = df_goods.loc[(df_goods.Quantity > 0) & (df_goods.Quantity < 10000)]
plt.figure(figsize = [18,6])
plt.scatter(df_goods.InvoiceDate, df_goods.Quantity)
```

输出如下：  
![Drawing 25.png](https://s0.lgstatic.com/i/image6/M01/46/8D/Cgp9HWDLHOeAT5-0AAB-hZ33S44988.png)

第一轮异常值剔除后，我们现在在 0~10000 的范围上可以看得更加细致，从上图中，我们不难发现，完全可以以 800 作为阈值来剔除异常值。

代码如下：

```
df_goods = df_goods.loc[(df_goods.Quantity > 0) & (df_goods.Quantity < 800)]
df_goods
```

输出如下：  
![Drawing 26.png](https://s0.lgstatic.com/i/image6/M01/46/8D/Cgp9HWDLHPGAFe5lAADzhOuVugY875.png)

异常值剔除之后还剩 53w 条记录，影响不大，也符合我们从散点图中观察得到的结论。

（2）清洗 UnitPrice 异常值

用类似的方法看 UnitPrice 的分布：

```
plt.figure(figsize = [18,6])
plt.scatter(df_goods.InvoiceDate, df_goods.UnitPrice)
```

输出如下：

![Drawing 28.png](https://s0.lgstatic.com/i/image6/M01/46/8D/Cgp9HWDLHPmAOrJJAAA-eiAJzP4086.png)

可以看到， UnitPrice 仍然存在异常值，我们首选初步排除 < 0 以及大于 1000 的。然后再看一次。

```
df_goods = df_goods.loc[(df_goods.UnitPrice > 0) & (df_goods.UnitPrice < 1000)]
plt.figure(figsize = [18,6])
plt.scatter(df_goods.InvoiceDate, df_goods.UnitPrice)
```

输出如下：

![Drawing 30.png](https://s0.lgstatic.com/i/image6/M00/46/8D/Cgp9HWDLHQCAf1ztAAC58uUJnbM327.png)

这次的 UnitPrice 虽然分布均衡了一些，但不难发现底部那一条的数据点的密集程度还是远超过上方的区域，从点的稀疏程度来看，我们可以认为超过 50 的都算异常值。

在之前的基础上做以下调整。

```
df_goods = df_goods.loc[(df_goods.UnitPrice > 0) & (df_goods.UnitPrice < 50)]
df_goods
```

输出如下：  
![Drawing 31.png](https://s0.lgstatic.com/i/image6/M00/46/8D/Cgp9HWDLHQeATTL5AADw0RbmYeM891.png)

可以看到，在过滤完 Quantity 和 UnitPrice 的异常值后，数据表还剩 52w条记录，整体还是不影响分析的。

#### 特征相关性分析

在剔除完异常值之后，我们接下来的任务是分析 UnitPrice 和 Quantity 之间是否存在一定的相关性。相关性分析之前我们已经学习过使用 NumPy 的 corrcoef 函数来计算，今天我们尝试通过散点图的形式来分析。

一般来说，要分析两个样本量很高特征的相关性，我们可以直接把其中一个特征作为横轴，另一个特征作为纵轴，然后将样本数据以点的形式画出来。

代码如下所示：

```
plt.figure(figsize = [18,6])
plt.scatter(df_goods.UnitPrice, df_goods.Quantity)
```

输出如下：

![Drawing 33.png](https://s0.lgstatic.com/i/image6/M00/46/8D/Cgp9HWDLHRKAf4L6AAB7SKMKmt0995.png)

上图中，我们以 UnitPrice 为横轴，Quantity 作为纵轴。从图中可以看到，随着 UnitPrice 增加，Quantity 对应就减少，尤其是超过 20块之后更加明显，而随着 UnitPrice 降低，Quantity 增加，UnitPrice 在 0 块到 6 块这个区间，Quantity 的数量最高。说明东西越便宜，卖的数量就越多。

从上面的推论上，不难看出 UnitPrice 和 Quantity 存在一定的负相关性，但这个相关性不是线性的。通过 NumPy 的 corrcoef 函数我们只能得到一个相关性的系数，而通过散点图往往能解读出更多更有用的信息。

### 小结

我们来复习一下今天学习的内容。首先，我们学习了折线图的几个实用技巧：

-   如何绘制平滑的曲线；
    
-   如何利用连线规则来画出任意的图形；
    
-   如何利用 fmt 语法来指定折线图的样式。
    

然后，我们学习了散点图的基本用法，非常简单，就是将 plot 函数替换为 scatter 函数即可。

最后，我们通过一个相关性分析的案例实战，体会了如何使用散点图来做异常值的清洗和特征相关性的分析。

课后作业：

在坐标系内绘制一个以(50,200) 作为左上角点的，宽度为 150 的正方形，点的样式为 8 边形，线的样式为虚线点，颜色为青色。

___

答案：

```
figure3 = plt.figure(figsize=(5, 5))
ax1 = figure3.add_subplot(1,1,1)
plt.title("Test Line")
plt.xlabel("X")
plt.xlim([0, 400])
plt.ylabel("Y")
plt.ylim([0, 400])
plt.plot([50, 200, 200,50, 50],[200, 200, 50,50,200], "8-.c")
plt.show()
```

输出  
![Drawing 35.png](https://s0.lgstatic.com/i/image6/M00/46/95/CioPOWDLHR6ACZMTAABNHqfN9AQ620.png)
