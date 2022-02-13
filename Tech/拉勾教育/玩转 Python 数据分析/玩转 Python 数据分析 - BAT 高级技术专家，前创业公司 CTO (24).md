---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


现在，数据分析的三座大山我们已经翻过了两座，学完了 NumPy 和 Pandas 之后，我们基本具备了如何以表格的形式进行常规的数据分析，以及对于数据部分进行一些统计分析、回归分析等。但数据分析还有另一个重要的任务，就是把数据分析的内容与结果以图表的形式呈现出来。

毕竟数据分析的结果是要给别人看的，丰富多彩的图表在表达能力上肯定超过干巴巴的表格与数字。

Python 提供了非常强大的数据可视化的工具，本部分我们就来逐步学习如何在 Python 中实现数据的可视化。

### 课前准备：安装 Matplotlib 与 seaborn 工具包

安装的方式和之前一样，我们打开开始菜单 → Anaconda3 → Anaconda Prompt。 输入 conda install matplotlib 并回车。界面如下所示：  
![image.png](https://s0.lgstatic.com/i/image6/M01/45/3C/Cgp9HWDDD62APXwdAAFO5x9PKZs952.png)

输入 y 确认，完成安装。

接下来，我们安装 seaborn 工具包，继续输入 conda install seaborn 并回车，并在如下界面中输入 y 回车，确认安装。  
![image (1).png](https://s0.lgstatic.com/i/image6/M01/45/39/Cgp9HWDDBoCAKm75AAFsx4EdXOk957.png)

安装完毕后，关闭命令行窗口。

### 基本概念

要能够熟练地使用 Matplotlib 来画图，首先要理解 Matplotlib 图像的三层结构。

Matplotlib 图表结构如下图所示：

![image.png](https://s0.lgstatic.com/i/image6/M00/45/45/CioPOWDDEI-AbJ52AABfRcbQy0k031.png)

从外向内的方向来看，依次如下。

-   画板（Canvas），Matplotlib 所有图像都显示在一个 Canvas 之上，但 Canvas 一般都是系统放置好的，我们在代码中并不需要显式地创建 Canvas。
    
-   画布（Figure），Canvas 上方的一层是画布，画布一般就需要我们手动创建了，一般在画图的代码编写中，第一件事就是创建画布，并指定画布的大小，材质等信息。
    
-   子图（Axes），Axes 的意思是坐标轴。原本的意思为一个画布上可以有多个坐标轴，但我们习惯性地认为一个坐标轴就是一个子图，所以我们也把 Axes 叫作子图。一个画布上面可以添加多个子图。
    

### 绘图流程

介绍完了 Matplotlib 的基本概念，现在我们来介绍一下绘图的基本流程。可以用如下的图表示：  
![image (2).png](https://s0.lgstatic.com/i/image6/M01/45/41/CioPOWDDBoqABCLlAAGPQoAMLlA856.png)

接下来我们通过绘制 y=2x + 1 的函数图，来逐一讲解每个步骤的实现方法。首先在工作目录创建本讲的目录，chapter21， 然后在 VS code 中打开，并新建 Notebook，保存为 chapter21.ipynb.

#### （1）导入 Matplotlib 和 numpy

为什么需要 numpy？ 原因是基本上所有绘图的数据源要么是 pandas 的 Series，要么是 numpy 的 ndarray。这里为了方便讲解，我们使用更轻量级的 ndarray 来作为主数据源。

```
import matplotlib.pyplot as plt
import numpy as np
```

> 这里把 Matplotlib 简写为 plt 和 numpy 简写为 np，虽然这个简写可以任意指定，不过基本上用 plt 和 np 已经成为行业标准，网上大量的示例代码都采用了这种风格，所以一般情况下建议保持一致。

#### （2）准备图的数据源

我们要画图，首先就要思考我们的数据源是什么。因为图本身只是一个数据源更好的呈现方式。准备数据源，本质就是我们要思考我们想把什么东西画出来。在这个例子中，我们希望画出 y = 2x + 1 的函数曲线，那我们就要决定定义域（x 的范围）是多少。因为只要 x 确定了， y 也就可以直接算出来。

另外，在一般情况下，画图传入的只能是离散的值，不能是连续的值。比如我们想画 x 取值在 0 到 5 之间的函数曲线，那我们无法直接告诉 Matplotlib 我们想要这个范围的曲线，而是需要以一定的步长从这个范围中采样，生成一个离散的数据点列表告诉 Matplotlib。

比如我们可以按 1 采样，就是 \[0,1,2,3,4\] 也可以是按 0.5 采样。也就是 \[0. , 0.5, 1. , 1.5, 2. , 2.5, 3. , 3.5, 4. , 4.5\]。

这里我们画 0 到 10 范围内的曲线，按 1 采样，根据 NumPy 部分学习到的内容，使用 arange 函数可以轻松搞定。

```
x_ranges = np.arange(0, 10)
x_ranges
```

输出：

```
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

现在，我们要画图的基本数据点就准备好了。

#### （3）创建画布

通过 plt.figure 函数可以创建一块画布。并且可以通过 figsize 参数指定画布的大小，figsize 接受一个两个元素的列表，第一个元素为画布的宽，第二个元素为画布的高，单位是英寸。

在接下来的代码中，我们创建了一个 18x6 大小的画布，并将其保存在变量 figure1 中。

```
figure1 = plt.figure(figsize=(18, 6),facecolor=[0,0,1])
```

上述代码可以先不执行，不然会生成一个空的图。一般来说 Matplotlib 的画图代码中，创建画布、创建子图，设置子图标题、坐标轴属性以及绘制步骤往往都放在一个 Cell 中，避免中间放在不同 cell 中间会画出不完整的图像。

#### （4）创建子图

创建完画布之后，下一个步骤就是创建子图。在步骤 2 的基础上，添加如下的代码，创建一个充满整块画布的子图。

```
ax1 = figure1.add_subplot(1,1,1)
```

add\_subplot 函数参数里面的三个 1 是什么意思呢？ 我们在后续的子图布局会展开讲，这里就不再赘述。

#### （5）设置子图标题

在创建子图之后，直接使用 plt.title 函数即可指定当前子图的标题。将上述代码添加到步骤 2 的 cell 中。

这里你可能会有疑问，我们没有传入子图对象 ax1，他是怎么知道设置的是哪个子图的？原因是 Matplotlib 内部有一个状态机的机制，在你每次执行 add\_subplot 的时候，系统都会记录当前你操作的子图，这样你执行 plt 的这些方法都是针对当前子图生效。

#### （6）设置坐标轴的属性。

坐标轴的常见属性有标题、范围和刻度。一般来说指定的范围之后可以直接默认推导出刻度，所以主要还是以设置标题和范围为主。在步骤 2 的 cell 中，添加如下的代码。

```
plt.xlabel("X")
plt.xlim([0, 20])
plt.ylabel("Y")
plt.ylim([0, 20])
```

这里我们设置的范围，x 轴和 y 轴都是 0 到 20。这里的范围和数据源范围的关系是什么呢？这里设置的是坐标轴的范围，而数据源指定的是绘制曲线的范围。比如刚才我们设置的曲线范围是 0 到 10，而这里的轴范围指定了 0 到 20，所以待会儿我们画出的曲线理论上只会在坐标系的左边部分出现。

继续不用执行，看下面的步骤。

#### （7）绘图并显示

接下来就到了最关键的步骤：绘图。绘图的本质就是在我们准备好画布和轴之后将数据源告诉 Matplotlib 的过程，这里要用到 plt.plot 函数。

一般情况下 plot 函数接受两个参数，一个是 x 轴的数据，一个是对应的 y 轴的数据。x 轴的数据我们在步骤 2 已经创建好了，而我们要画的是 y = 2x + 1 的曲线，所以 y 轴的数据直接用 x轴的数据乘以 2 + 1 即可。因为 x\_range 是一个 NumPy 的ndarray，所以我们可以直接运算。

在调用完 plot 函数后，我们再调用 plt.show 函数，来将我们刚才做的全部工作展示出来。

整个绘图过程，完成的代码如下：

```
figure1 = plt.figure(figsize=(18, 6))
ax1 = figure1.add_subplot(1,1,1)
plt.title("y = 2x +1")
plt.xlabel("X")
plt.xlim([0, 20])
plt.ylabel("Y")
plt.ylim([0, 20])
plt.plot(x_ranges, x_ranges * 2 + 1)
plt.show()
```

执行之后输出如下：

![image (3).png](https://s0.lgstatic.com/i/image6/M00/45/3C/Cgp9HWDDD1mAKe0SAAENVDjMpdI238.png)

可以看到，我们的曲线已经被画出来了，并且和我们预期的一致，曲线只在左边部分出现，因为我们的数据源只包含了 0 到 10 的数据。画布大小也和我们设置的一样，宽是高的三倍，对于图的标题，坐标轴的标题设置也都生效。

但细心的你可能发现了问题，虽然我们的图确实展示了我们的数据在坐标轴的位置关系。但是 x 轴的刻度宽度却远比 y 轴的宽，本质原因是我们设定了坐标的 x 轴 比 y 轴长，但是两个轴的范围都是一样的，都是 0 到 20，这样就使得函数曲线看上去有点变形。

在对 Matplotlib 的绘图经验不足的时候，我们很难一次性把参数设置对，经常都需要先画出图来，然后再调整参数。

#### （8）根据绘图结果调整参数

要绘制出比例一致的图形，有多种方式，本质只要坐标系的大小和 x 轴、y 轴的范围成比例即可。这里我们简单地将画布大小调整为 10x10, 这样两边比例就一致了。

```
figure1 = plt.figure(figsize=(10, 10)) 
ax1 = figure1.add_subplot(1,1,1)
plt.title("y = 2x +1")
plt.xlabel("X")
plt.xlim([0, 20])
plt.ylabel("Y")
plt.ylim([0, 20])
plt.plot(x_ranges, x_ranges * 2 + 1)
plt.show()
```

输出如下：

![image (4).png](https://s0.lgstatic.com/i/image6/M01/45/44/CioPOWDDD2aAZz5-AADok6uY4UQ925.png)

现在的图能够比较真实地反应函数 y=2x+1 的形状了。

### 子图的布局

本节我们介绍子图的布局方式。Matplotlib 的子图默认是按照行列的方式来布局的。核心就是通过画布对象的 add\_subplot 函数。

add\_subplot 函数的形式如下：

```
add_subplot(行数，列数，当前子图索引)
```

行数和列数代表子图的位置在画布中如何划分。  
比如如果是行数和列数都是 2 的话，代表把画纸拆分成 2x2，一共四个格子，每个格子可以放置一个子图。当前子图索引参数指的是告诉 Matplotlib 接下来要设置的是这个 2x2 的划分里的第几幅子图。

这里要注意的是，当前子图索引和绝大多数 Python 索引不同，这里的索引是从 1 开始的，顺序对应网格的顺序是从左到右，从上到下。

我们还是通过例子来学习一下 add\_subplot 的用法，我们接下来创建一个 2x2 的划分，并分别逆序添加四幅子图，第一幅添加在最后，以此类推。

```
figure2 = plt.figure(figsize=[18,9])
ax1 = figure2.add_subplot(2,2, 4)
plt.title("fig1")
ax2 = figure2.add_subplot(2,2,3)
plt.title("fig2")
ax3 = figure2.add_subplot(2,2,2)
plt.title("fig3")
ax4 = figure2.add_subplot(2,2,1)
plt.title("fig4")
plt.show()
```

输出如下：

![image (5).png](https://s0.lgstatic.com/i/image6/M01/45/3C/Cgp9HWDDD3mAZB5-AACq6oTal0Y244.png)

因为这次添加的子图我们并没有做属性的设置和数据的填充，所以画的都是默认的空白图。但我们为每个子图都设置了标题来区分，可以看到第一个子图在右下角，第二个在左下角，这与我们添加时设置的子图索引是匹配的。

### 颜色与线条样式

Matplotlib 支持丰富的样式，其中最基本的就是控制画出的图形颜色和线条的粗细。

一般来说有以下几个原则：

-   画布的样式，通过设置 figure 函数的参数决定；
    
-   子图的样式，通过设置 add\_subplot 函数的参数决定；
    
-   线条的样式，通过设置 plot 函数的参数决定。
    

另外，在 Matplotlib 中，颜色往往由三个元素的列表来表示，分别代表颜色 R、G、B 三个通道的分量，范围都是 0-1.。

学习过计算机图像处理的同学应该并不陌生，比如纯红色就对应 \[1, 0 , 0\], 绿色就是 \[0, 1, 0\]，蓝色就是 \[0,0,1\]。比如还有一些颜色是三种颜色混合运算的，比如青色就是 \[0,1,1\]。 感兴趣的同学可以参考[RGB 颜色表 (360doc.com)](http://www.360doc.com/content/12/0115/10/7695942_179488602.shtml?fileGuid=xxQTRXtVcqtHK6j8)里面的颜色值除以 255，就可以得到 Matpliblot 中可以使用的色值。

接下来，我们还是通过一个例子来学习如何改变图像的各种颜色，任务是将刚才 y=2x + 1 的图像，做以下修改：

-   画布颜色修改为红色；
    
-   子图颜色修改为蓝色；
    
-   线条颜色修改为绿色；
    
-   线条粗细变为 3。
    

实现的代码如下：

```
figure1 = plt.figure(figsize=(10, 10), facecolor=[1, 0, 0]) 
ax1 = figure1.add_subplot(1,1,1, facecolor = [0, 0, 1])
plt.title("y = 2x +1")
plt.xlabel("X")
plt.xlim([0, 20])
plt.ylabel("Y")
plt.ylim([0, 20])
plt.plot(x_ranges, x_ranges * 2 + 1, linewidth = 3, color=[0,1,0])
plt.show()
```

输出为：

![image (6).png](https://s0.lgstatic.com/i/image6/M01/45/44/CioPOWDDD4KALmGxAAEDuNnCTPY687.png)

从上图中可以看到，各个部分都和我们刚才的设置保持了一致。

### 实战：显示评分曲线

接下来我们通过一个小实战来演练一下今天学习的内容。

任务说明：将国产电视剧评分的前 100 条记录的评分情况用折线图表示出来。

#### 准备数据源

把前 100 条记录的评分用折线图画出来，那, x 轴就是电视剧的序号，0 到 100，y 轴就是具体序号对应的电视剧的评分。

首先我们从 chapter12 目录中，把 tv\_rating.csv 拷贝到 chapter21 目录中。然后新建 Cell，加载数据集。如下所示：

```
import pandas as pd
df_rating = pd.read_csv("tv_rating.csv")
df_rating
```

输出：

![image (7).png](https://s0.lgstatic.com/i/image6/M01/45/3C/Cgp9HWDDD4uAPdysAAH7VNNcmEk211.png)

现在评分一列是字符串表示的，而且还有一个“分”字，而我们要送到 Matplotlib 中的数据肯定需要是数字，所以需要对这一列进行处理，去掉“分”字，并把数据转换为浮点数。我们把处理后的数据存为一个新的列：rating\_num。

```
df_rating["rating_num"] = df_rating.rating.apply(lambda x:float(x.replace("分","")))
df_rating
```

这里继续使用了 lambda 表达式来处理列，针对 rating 列的每一项，我们首先删除“分”字，然后转换为 float。转换后的 Series 存储为 rating\_num。  
输出如下：

![image (8).png](https://s0.lgstatic.com/i/image6/M01/45/3C/Cgp9HWDDD5aAO8nyAAIcX4rbsR4756.png)

可以看到，我们的新列已经添加成功。由于只处理前 100 条，所以我们将前 100 条保存为一个新的 DataFrame：df\_head\_100。

```
df_head_100 = df_rating[:100]
df_head_100
```

输出如下：

![image (9).png](https://s0.lgstatic.com/i/image6/M01/45/44/CioPOWDDD5-AOCCCAAGqUMQ8QGE099.png)

至此，我们的数据源就准备完毕了。

#### 绘制评分图

在数据源准备好之后，按照我们之前介绍的步骤直接绘图即可。

代码如下：

```
figure = plt.figure(figsize=[18,9])
ax1 = figure.add_subplot(1,1,1)
plt.rcParams["font.sans-serif"] = "SimHei"
plt.title("电视剧评分折线图")
plt.xlabel("序号")
plt.xlim([0, 100])
plt.ylabel("评分")
plt.ylim([0,5])
plt.plot(df_head_100.index, df_head_100.rating_num)
plt.show()
```

输出：

![image (10).png](https://s0.lgstatic.com/i/image6/M01/45/3C/Cgp9HWDDD6aAKbwSAAVpfMGG6QU102.png)

### 小结

到现在，我们关于 Matplotlib 第一课的内容就学习完毕了。简单复习一下今天的内容。

-   我们学习了 Matplotlib 的基本概念：画板、画布、子图。
    
-   学习了 Matplotlib 绘图的基本步骤：
    
    -   准备要用来可视化展现的数据源；
        
    -   创建画布，指定大小和画布颜色，默认为白色；
        
    -   创建子图，可以指定划分和位置，以及颜色；
        
    -   设置子图的标题；
        
    -   设置子图的轴标题，范围信息；
        
    -   设置数据源，执行绘图；
        
    -   显示图形。
        
-   学习了 Matplotlib 的颜色体系：通过三个元素的列表分别代表颜色的 R G B 分量，每个分量的取值范围都是 0 到 1。
    
-   学习了子图的布局方式，通过 add\_subplot 函数的参数决定了对画布的划分方式，以及选择当前创建子图的位置。
    

Matplotlib 默认绘制的是折线图，也就是本讲我们打交道的类型，下一讲我们会介绍更多更有意义图形绘制方式。

课后习题：

将实战案例的画布拆成上下两部分，上面依旧显示 0 到 100 的评分，下面部分则显示序号 100 到 200 之间的评分。

___

习题答案：

```
figure = plt.figure(figsize=[18,9])
ax1 = figure.add_subplot(2,1,1)
plt.rcParams["font.sans-serif"] = "SimHei"
plt.title("电视剧评分折线图 (0-100)")
plt.xlabel("序号")
plt.xlim([0, 100])
plt.ylabel("评分")
plt.ylim([0,5])
plt.plot(df_head_100.index, df_head_100.rating_num)
ax1 = figure.add_subplot(2,1,2)
plt.rcParams["font.sans-serif"] = "SimHei"
plt.title("电视剧评分折线图 (100-200)")
plt.xlabel("序号")
plt.xlim([100, 200])
plt.ylabel("评分")
plt.ylim([0,5])
plt.plot(df_head_100_200.index, df_head_100_200.rating_num)
plt.show()
```

输出如下：

![2021611-154431.png](https://s0.lgstatic.com/i/image6/M00/45/46/CioPOWDDFGqABwYzAAb6hZ38-yE126.png)
