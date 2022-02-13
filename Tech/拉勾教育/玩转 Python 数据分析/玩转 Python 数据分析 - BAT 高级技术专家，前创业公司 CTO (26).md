---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


上节课中，我们学习了折线图和散点图，并且学习了如何可视化地进行特征的异常数据清洗和相关性分析。今天我们来学习另一类可视化的图表类型，主要有**直方图、条形图和饼图**，这些图更常见的用法是用于分析数据集内部的数据分布，以及数据取值频率等。

### 直方图

直方图是数据分析中经常出现的概念，又称为质量分布图，由一系列高度不等的细长矩形排列在横轴上来表示数据分布的情况（如下图所示）。一般来说，横轴表示数据的类别，纵轴表示数据的频率。直方图本质上是对于连续变量的概率分布图。

![Drawing 1.png](https://s0.lgstatic.com/i/image6/M00/47/6E/CioPOWDQTeiALI6EAAAsg3v26tI206.png)

（直方图示例）

要画直方图，我们第一步首先是要将所有数据分组，组的数量就是直方图中竖长方形的数量。直方图的分组要求是不重叠且相邻的，并且一般是等距的。

直方图在很多领域都有广泛的应用，除了数据分析。直方图也常常被用来分析图像的特征，甚至可以用来做一些基本的图像分类。因为图像是由不同的像素点组成的，每个像素点的取值就是颜色值 0~255， 通过将图像的所有像素点都以直方图的形式展现，比如按照像素值分成 255 个组，就能根据直方图的形状来推测图像大概是什么。

#### 画直方图

在 matplotlib 中，可以通过 plt.hist 函数画直方图。接下来我们生成一个符合正态分布的数组，然后将其用直方图的形式表现出来。通过这个例子来学习直方图的使用方法。

首先是大家已经非常熟悉的准备工作：在工作目录新建 chapter23， 并在 VScode 中打开，然后新建 notebook，保存为 chapter23.ipynb。

首先导入必要的三大件：

```
import matplotlib.pyplot as plt
import pandas as pd 
import numpy as np
```

然后初始化 NumPy 的随机数，并生成以 5 为中心，宽度为正负 10 的 1000 个符合正态分布的随机数，作为我们画图的数据源。

```
np.random.seed(100)
hints = np.random.normal(5, 10, 1000)
```

这里由于数字太多，就不打印了，直接逐一执行上述两个 Cell 即可。

我们在 NumPy 的部分学习了 ndarray 支持常见的统计方法，所以我们可以通过查看数组的均值和标准差来看这些数据是否符合我们设定的正态分布。

```
print(hints.mean(), hints.std())
```

输出如下：

```
4.832278426560908 10.458427194167
```

可以看到，均值在我们设定的中心（5）附近，标准差约等于我们设定的宽度（10，所以数据基本没有问题。  
接下来就是关键部分，画直方图，代码如下：

```
figure1 = plt.figure(figsize = (10, 5))
figure1.add_subplot(1,1,1)
plt.xlabel("值",fontsize = 14)
plt.ylabel("频率", fontsize = 14)
plt.xticks(fontsize=14)
plt.yticks(fontsize=14)
plt.title("正态分布的直方图", fontsize=14)
plt.rcParams["font.sans-serif"] = "SimHei"
plt.rcParams["axes.unicode_minus"] = False
plt.hist(hints, bins=10, color=[0,0,1])
plt.show()
```

输出如下：

![Drawing 3.png](https://s0.lgstatic.com/i/image6/M00/47/6E/CioPOWDQTieAbTzuAAA_FMVXMxU995.png)

从上图中不难发现，整个数据看起来就是正态分布的形状，随机的极值点到了-30 到 40。从直方图的台阶上看，相对于中心对称区域的高度都差不多（频率基本一致）。所以从直方图的几何特征上能够真实地反映出数据的概率分布特性。

看到这里，想必你也已经发现，plt.hist 函数的 bins 参数非常关键，bins 的值代表了要把数据分成几组，也就代表了图上一种会有多少个长方形。bins 越大，体现得就越精确，但相应的分布特征可能就越不明显。比如我们可以把上述代码的 bins 改成 50 。

```
...
plt.hist(hints, bins=50, color=[0,0,1])
...
```

输出如下：  
![Drawing 5.png](https://s0.lgstatic.com/i/image6/M00/47/65/Cgp9HWDQTi6Ad_XBAABJK-QFJF0223.png)

可以看到，分组变多后，直方图展示了更多关于原始数据的信息，但是也出现了较多的锯齿，但整体仍然是正态分布的形状。

直方图另外一个非常有用的参数就是 edgecolor，即每个长方形的边框颜色。上面的直方图中间区域是一片蓝，有时候并不利于做进一步的数据分析。这个时候我们可以设置 plt.hist 函数的 edgecolor 属性，来让每个长方形都有一个边框颜色。

修改上述代码如下：

```
...
plt.hist(hints, bins=50, color=[0,0,1], edgecolor=[0,0,0])
...
```

输出如下：  
![Drawing 7.png](https://s0.lgstatic.com/i/image6/M00/47/6E/CioPOWDQTjSAGxf4AABW2qiMfwA190.png)

可以看到，设置边框颜色之后，整个直方图看着更清晰了。

#### 展示多个直方图

在很多分析场景，我们希望同时分析 2~3 个数据源的频率分布，这背后的技术就是把 2~3 个直方图在同一个坐标系中展示。

为了模拟多个直方图，我们首先生成另外两个数据源。

```
hints_1 = np.random.normal(0, 8, 500)
hints_2 = np.random.normal(15, 10, 800)
```

接下来我们三次调用 plt.hist 即可，为了区分，我们使用三种不同的颜色来画。

```
figure1 = plt.figure(figsize = (10, 5))
figure1.add_subplot(1,1,1)
plt.xlabel("值",fontsize = 14)
plt.ylabel("频率", fontsize = 14)
plt.xticks(fontsize=14)
plt.yticks(fontsize=14)
plt.title("正态分布的直方图", fontsize=14)
plt.rcParams["font.sans-serif"] = "SimHei"
plt.rcParams["axes.unicode_minus"] = False
plt.hist(hints, bins=50, color=[0,0,1], edgecolor=[0,0,0])
plt.hist(hints_1, bins=50, color=[0,1,0], edgecolor=[0,0,0])
plt.hist(hints_2, bins=50, color=[1,0,0], edgecolor=[0,0,0])
plt.show()
```

输出如下：  
![Drawing 9.png](https://s0.lgstatic.com/i/image6/M00/47/65/Cgp9HWDQTj2ASsqPAABw14Gc_9I242.png)

图像虽然画出来了，但是有重叠的地方因为遮挡的关系，绿色和蓝色的分布看不见了，这个时候我们可以使用 plt.hist 函数的 alpha 属性，来让三个图都有一定的透明度，解决这个问题。

修改相关代码如下：

```
...
plt.hist(hints, bins=50, color=[0,0,1], edgecolor=[0,0,0], alpha = 0.5)
plt.hist(hints_1, bins=50, color=[0,1,0], edgecolor=[0,0,0], alpha = 0.5)
plt.hist(hints_2, bins=50, color=[1,0,0], edgecolor=[0,0,0], alpha = 0.5)
...
```

输出如下：  
![Drawing 11.png](https://s0.lgstatic.com/i/image6/M00/47/66/Cgp9HWDQTkaAcquQAACT5rGUcSQ366.png)

增加透明度之后，即便有所遮挡也可以看到了。不过你会发现好像最终的颜色比我们设置的蓝、绿、红要多一些，这背后的原因就是因为我们设置的透明度，不同的颜色区域叠在一块的时候不会完全遮挡，而是会用重叠区域的颜色混合成一种新的颜色。所以看上去好像颜色变多了。

### 条形图

#### 区别条形图和直方图

条形图，又称为柱状图，有的地方也把横版的称为条形图，竖版的称为柱状图，这里我们统称条形图。

条形图和直方图类似，也是通过一个个细长的长方形来表示数据的频率，所以很容易搞混这两个概念。

我们可以从下面这个表格来理解条形图和直方图的区别。

![Drawing 12.png](https://s0.lgstatic.com/i/image6/M00/47/6E/CioPOWDQTkyAD03iAABEzydCagU302.png)

很多情况下，条形图往往能表示比直方图更多维度的数据。下面是一个常见的条形图的例子。

![Drawing 14.png](https://s0.lgstatic.com/i/image6/M00/47/6E/CioPOWDQTlKAHU4dAAA_b5IjL9Q989.png)

在上图的例子中，可以看到有两个维度的类别，首先是男和女，然后是这两个类别分别在四个年龄段的分布情况。条形图一定程度上可以表示类似这样离散的三维数据，所以在部分场景，尤其是类别不多的场景下，用条形图来表示就显得非常的直观。像这样的类别我们称之为二维的类别。

#### 绘制条形图

通过 plt.bar 函数可以实现条形图的绘制。接下来我们通过一个实际的案例来学习条形图的绘制，某学校的初中二年级举行了期中考试。五个班的平均分如下：

![Drawing 15.png](https://s0.lgstatic.com/i/image6/M00/47/66/Cgp9HWDQTlmAKW7uAAAygmnmDTw788.png)

现在我们通过条形图来比对各个班的平均分的分布情况。

第一步，我们首先将数据导入到 notebook 中，以列的维度。

```
math_scores = np.array([71,65,70,96,64])
chinese_scores = np.array([84,75,68,83,57])
english_scores = np.array([55,78,76,91,64])
```

然后就用 plt 的常规模板进行画图。这里的 x 轴的数据源我们直接使用五个类别的值即可，即是一班、二班等。代码如下：

```
figure2 = plt.figure(figsize = (10, 5))
figure2.add_subplot(1,1,1)
plt.xlabel("",fontsize = 14)
plt.ylabel("平均分", fontsize = 14)
plt.xticks(fontsize=14)
plt.yticks(fontsize=14)
plt.title("期中成绩条形图", fontsize=14)
plt.rcParams["font.sans-serif"] = "SimHei"
plt.rcParams["axes.unicode_minus"] = False
category = ["一班", "二班", "三班", "四班", "五班"]
plt.bar(category, chinese_scores, color=[0,0,1])
plt.bar(category, math_scores, color=[0,1,0])
plt.bar(category, english_scores,  color=[1,0,0])
plt.show()
```

输出如下：

![Drawing 17.png](https://s0.lgstatic.com/i/image6/M00/47/6E/CioPOWDQTmCAPUx0AABE_-s6560775.png)

条形图已经画出来了，但是我们三门课的形状都画到一起了，图像的堆叠次序是根据画图的顺序来决定的。所以当英语的成绩比前面高的时候，红色的条形就遮住了前面的。

这种情况该怎么办呢？根据之前对条形图的定义，这里我们应该需要将三个图形并列排放。具体的实现方式分为两步：

1.  x 轴的数据源切换为数字，这样才能有宽度的概念；
    
2.  在绘制三个 bar 的时候，分别指定它们的相对位置与宽度。
    

修改后完成的代码如下：

```
figure2 = plt.figure(figsize = (10, 5))
figure2.add_subplot(1,1,1)
plt.xlabel("",fontsize = 14)
plt.ylabel("平均分", fontsize = 14)
plt.xticks(fontsize=14)
plt.yticks(fontsize=14)
plt.title("期中成绩条形图", fontsize=14)
plt.rcParams["font.sans-serif"] = "SimHei"
plt.rcParams["axes.unicode_minus"] = False
category = ["一班", "二班", "三班", "四班", "五班"]
index_category = np.arange(len(category))
bar_width = 0.25
plt.bar(index_category - bar_width, chinese_scores, width=bar_width, color=[0,0,1])
plt.bar(index_category, math_scores, width=bar_width, color=[0,1,0])
plt.bar(index_category + bar_width, english_scores, width=bar_width, color=[1,0,0])
plt.show()
```

输出如下：

![Drawing 19.png](https://s0.lgstatic.com/i/image6/M00/47/66/Cgp9HWDQTmiAL1WVAABAlI3w04E754.png)

这样，就能够比较明确地反映出学生期中考试的考试情况了，明显四班（序号 3）的成绩优于其他班，并且英语的考试成绩整体优于另外两科。

#### 水平条形图

水平条形图和普通的条形图逻辑都是类似的，只是绘制的方向变成了自左向右，绘制的函数名称为 plt.barh。其实就相当于把普通条形图的坐标轴的 x 轴和 y 轴调换一下。不过有区别的是，水平条形图一般不用于比较二维的类别，因为没有普通的条形图清晰。水平条形图多用于一维的类别。

比如下面的例子中，我们单独将各个班的数学成绩用水平条形图来展示：

```
figure3 = plt.figure(figsize = (10, 5))
figure3.add_subplot(1,1,1)
plt.xlabel("",fontsize = 14)
plt.ylabel("平均分", fontsize = 14)
plt.xticks(fontsize=14)
plt.yticks(fontsize=14)
plt.title("数学成绩条形图", fontsize=14)
plt.rcParams["font.sans-serif"] = "SimHei"
plt.rcParams["axes.unicode_minus"] = False
category = ["一班", "二班", "三班", "四班", "五班"]
plt.barh(category, math_scores, color=[1,0,1])
plt.show()
```

输出如下：  
![Drawing 21.png](https://s0.lgstatic.com/i/image6/M00/47/6E/CioPOWDQTnCAWndgAAA-TbFThT4053.png)

#### 堆叠条形图

堆叠条形图和之前我们画的条形图，多个条叠在一起的图不同。堆叠条形图也是解决二维类别的数据，和普通条形图核心的区别是不同子类的图形是叠在一起的（上下叠在一起，不是重叠）。堆叠条形图的好处是可以清晰地反应每个子类数据的占比。

绘制堆叠条形图的方式和普通条形图一样，也是通过 plt.bar 函数，区别只是堆叠条形图在绘制上叠的数据时，需要额外指定 bottom 参数。我们通过一个例子来学习一下。

某公司销售部门统计了最近四周的签单数据，目前在职的销售一共两名，销售 A 的签单数为 \[10,23, 5, 11\] ，销售 B 的签单数为 \[3,12,6, 5\]。 我们通过堆叠条形图来展示两个销售对于部门总销量的占比，代码如下所示：

```
figure4 = plt.figure(figsize = (10, 5))
figure4.add_subplot(1,1,1)
plt.xlabel("",fontsize = 14)
plt.ylabel("签单量", fontsize = 14)
plt.xticks(fontsize=14)
plt.yticks(fontsize=14)
plt.title("签单堆叠条形图", fontsize=14)
plt.rcParams["font.sans-serif"] = "SimHei"
plt.rcParams["axes.unicode_minus"] = False
category = ["第一周", "第二周", "第三周", "第四周"]
sales_a = [10,23, 5, 11]
sales_b = [3,12,6, 5]
plt.bar(category, sales_a, color=[1,0,1])
plt.bar(category, sales_b, color=[0,0,1], bottom=sales_a)
plt.show()
```

输出如下：  
![Drawing 23.png](https://s0.lgstatic.com/i/image6/M00/47/66/Cgp9HWDQTnyATcRqAABII_9U_O0287.png)

可以看到销售 A（紫色）和销售 B（蓝色）的数据在同一个条形上展示了，事实上撇开两个销售的占比不谈，单看条形图的高度反映的就是该部门的总签单量，然后不同的颜色代表两个销售各自对总签单量的共享。所以从上图中不难看出，销售 A 的贡献明显更大一些。

### 饼图

本讲最后一节是饼图，饼图虽然在表示内容的维度与丰富度上相比直方图和条形图会差一些。但是在反映数据占比的可理解性上是最好的，这也是为什么在很多数据分析的报告中做占比分析最常见的就是饼图。

通过 plt.pie 函数可以绘制饼图。pie 函数由以下几个关键参数实现功能。

-   x：饼图每一块的占比列表，列表有几个元素就代表饼图分几块。
    
-   explode：凸出显示，也是一个列表，和 x 一一对应，代表具体某一块是否要凸出显示。
    
-   colors：列表，和 x 一一对应，代表每一块的颜色。
    
-   labels：标签文本列表，和 x 一一对应，代表每一块的标题。
    
-   autopct：代表百分比文本的格式。
    
-   startangle： 默认饼图是从角度为 0 的位置逆时针开始画，这里可以指定初始角度。
    
-   labeldistance：文本标签距离饼图的距离。
    
-   pctdistance：百分比标签距离饼图中心的距离。
    

接下来我们通过一个案例来学习饼图的绘制方式。

假设我们对一个年级的学生，统计历史上得过三好学生称号的情况，完全没得过的占比为 40%， 得过一次的占比为 30%， 二次的为 25%， 三次以上的为 5%。现在我们用饼图表示这个数据，并将得过三次以上的凸出显示。

代码如下：

```
figure5 = plt.figure(figsize = (10, 5))
figure5.add_subplot(1,1,1)
plt.rcParams["font.sans-serif"] = "SimHei"
plt.rcParams["axes.unicode_minus"] = False
category = ["没有得过", "得过一次", "得过二次", "得过三次以上"]
size = [40, 30, 25, 5] 
color = ["r", "g", "b", "c"]
explode = [0,0,0,0.1]
plt.pie(size,explode=explode,colors=color,labels=category,labeldistance= 1.1, autopct="%1.1f%%", startangle=90,pctdistance=0.6)
plt.show()
```

执行之后输出如下：  
![Drawing 25.png](https://s0.lgstatic.com/i/image6/M00/47/66/Cgp9HWDQToiAL7LZAABPtDWzAB4157.png)

### 小结

至此，今天我们的课程就学习完毕了。我们一起来回顾一下今天学习的内容。

首先我们学习了直方图的概念以及绘制的方式。直方图的关键就是对连续数据进行分组统计。主要的技术点如下：

-   通过 plt.hist 函数可以实现绘制直方图；
    
-   通过 edgecolor 属性可以设置直方图的边框颜色；
    
-   通过 alpha 属性可以再绘制多个直方图的时候半透明显示，互不干扰。
    

然后我们学习了条形图，首先对比了条形图和直方图的区别。然后展示了条形图的绘制技巧。主要包括：

-   通过 plt.bar 绘制条形图；
    
-   通过 width 参数，以及对 x 轴的数据加偏移的形式，来实现多个子类的条形图并排显示；
    
-   通过 plt.barh 绘制水平条形图；
    
-   通过 plt.bar 配合 bottom 属性来绘制堆叠条形图。
    

最后，我们学习了饼图的绘制方式，plt.pie 方法的参数比较多。核心的就是占比列表（x 轴数据源），以及对应的文字标题信息、颜色信息、凸出显示信息等。

课后习题：

将“堆叠条形图”小节的销售的案例，用饼图绘制。因为有 4 周的数据，所以需要绘制 4 个饼图。

___

答案：

```
week_category = ["第一周", "第二周", "第三周", "第四周"]
sales_a = [10,23, 5, 11]
sales_b = [3,12,6, 5]
figure6 = plt.figure(figsize = (10, 10))
plt.rcParams["font.sans-serif"] = "SimHei"
plt.rcParams["axes.unicode_minus"] = False
for i in range(0,4):
    figure6.add_subplot(2,2,i + 1)
    plt.title(week_category[i])
    category = ["销售A", "销售B"]
    size = [sales_a[i], sales_b[i]] 
    color = ["b", "g"]
    explode = [0,0]
    plt.pie(size,explode=explode,colors=color,labels=category,labeldistance= 1.1, autopct="%1.1f%%", startangle=90,pctdistance=0.6)
plt.show()
```

输出如下：  
![Drawing 27.png](https://s0.lgstatic.com/i/image6/M00/47/66/Cgp9HWDQTpCAAWEeAABrn2nHqks590.png)
