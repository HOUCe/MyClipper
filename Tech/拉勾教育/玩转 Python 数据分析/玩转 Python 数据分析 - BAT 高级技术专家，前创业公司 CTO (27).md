---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


前面两节课，我们学习了主流的几种 matplotlib 的图形形式，主要包括折线图、散点图、直方图、条形图和饼图。现在我们已经可以画出样式比较多的图表。但我们画出的图表距离专业的图表来看，还有一些不足。

本讲我们就来学习 matplotlib 画图的几个重要的样式要素：脊柱、注解和图例。学好这些可以帮助我们画出更专业的图表。

### 脊柱

首先，我们来学习图像脊柱，脊柱可以理解为图像的坐标轴。之前的课程里我们简单学习过设置范围和名称，现在我们来系统地学习一下坐标轴的操作。

首先第一步还是准备好实验的环境。在工作目录下新建 chapter24 文件夹，并用 VScode 打开，然后新建 notebook, 并以 chapter24.ipynb 的名字保存在上述文件夹中。

之后导入我们的三板斧：

```
import matplotlib.pyplot as plt
import pandas as pd 
import numpy as np
```

#### 设置坐标轴的刻度

关于坐标轴的显示，之前我们主要学习了使用 plt.xlabel 来设置坐标轴的标题，用 xlim 来设置坐标轴的范围（y 轴同理）。但如果要把图像画得更加专业，我们还可以通过 xticks 来设置坐标轴的刻度。

xticks 除了常见的 fontsize 、color 等属性之外，还支持传入一个刻度的数组以及对应的标签信息，注意区分刻度和范围。范围决定的是轴上显示的数据范围，一般是一个最大值和一个最小值。而刻度决定的是在这样一个范围中哪些地方要显示刻度，以及显示的刻度文本是什么。一般来说，刻度的信息可以用 np.arange 函数方便地生成，因为它同时制定了开始、结束和步长。

举个例子来说，我们可以设置 y 轴的范围是 10 到 20，然后指定 15 到 20 的区域，每间隔 1 就展示一个刻度。代码如下：

```
x_ranges = np.arange(0, 10)
figure1 = plt.figure(figsize=(10, 10)) 
ax1 = figure1.add_subplot(1,1,1)
plt.title("y = 2x +1")
plt.xlabel("X")
plt.xlim([0, 20])
plt.ylabel("Y")
plt.ylim([0, 20])
plt.yticks(np.arange(15, 20, 1))
plt.plot(x_ranges, x_ranges * 2 + 1)
plt.show()
```

输出如下：  
![Drawing 1.png](https://s0.lgstatic.com/i/image6/M00/47/BF/Cgp9HWDRvvyANEpUAAAnDUQk_j4193.png)

接下来我们以一个更实际一点的例子来说明设置刻度的作用。我们以前一课中出现过的期中成绩直方图为例。上一节课中，我们画图的代码如下：

```
math_scores = np.array([71,65,70,96,64])
chinese_scores = np.array([84,75,68,83,57])
english_scores = np.array([55,78,76,91,64])
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
![Drawing 3.png](https://s0.lgstatic.com/i/image6/M00/47/C8/CioPOWDRvwOAIouoAAAzkK3P_5o312.png)

首先我们看横轴，因为我们需要传入 index， 所以现在刻度显示的是数字。光从数字上看很难看出不同的直方图组率属于哪个班级。另一方面，学生最后的成绩并不是看百分制，而是分为 ABCD 四档。总结下来，我们的任务就是：

-   横轴刻度用班级名称来显示；
    
-   纵轴刻度用 ABCD 四挡来显示。
    

刚才我们已经 plt.yticks 函数来指定了刻度的值和范围，这次我们要加上标签，标签一般是一个列表，列表的元素数量和刻度数需要一致。修改上述代码，如下所示：

```
...
category = ["一班", "二班", "三班", "四班", "五班"]
plt.xticks(np.arange(len(category)),category)
plt.yticks(np.arange(0, 100,25), ["D","C", "B", "A"])
index_category = np.arange(len(category))
...
```

输出如下：  
![Drawing 5.png](https://s0.lgstatic.com/i/image6/M00/47/BF/Cgp9HWDRvwuAVKynAAA0t48Qu20103.png)

可以看到，通过对轴的刻度进行定制，进一步提升了图表的专业性。

#### 轴的显示或隐藏

在我们之前画的图表中，虽然我们关注点主要放在 x 轴和 y 轴。但其实每一张 matplotlib 的图像都有上、下、左、右四根轴。拿上面的图来说：  
![Drawing 7.png](https://s0.lgstatic.com/i/image6/M00/47/C8/CioPOWDRvxGAY0RzAAAsygyajd8435.png)

每一根轴都可以设置隐藏或显示，默认全部显示。设置轴显示或者隐藏的主要通过之前介绍的子图对象（Axes）。

我们仍然使用期中成绩的图表来演示如何对轴进行隐藏或者显示，为了减少每次修改代码的成本，我们先把画图代码单独抽成一个函数。如下所示：

```
def draw_midterm_scores():
    math_scores = np.array([71,65,70,96,64])
    chinese_scores = np.array([84,75,68,83,57])
    english_scores = np.array([55,78,76,91,64])
    plt.xlabel("",fontsize = 14)
    plt.ylabel("平均分", fontsize = 14)
    plt.xticks(fontsize=14)
    plt.yticks(fontsize=14)
    plt.title("期中成绩条形图", fontsize=14)
    plt.rcParams["font.sans-serif"] = "SimHei"
    plt.rcParams["axes.unicode_minus"] = False
    category = ["一班", "二班", "三班", "四班", "五班"]
    plt.xticks(np.arange(len(category)),category)
    plt.yticks(np.arange(0, 100,25), ["D","C", "B", "A"])
    index_category = np.arange(len(category))
    bar_width = 0.25
    plt.bar(index_category - bar_width, chinese_scores, width=bar_width, color=[0,0,1])
    plt.bar(index_category, math_scores, width=bar_width, color=[0,1,0])
    plt.bar(index_category + bar_width, english_scores, width=bar_width, color=[1,0,0])
    plt.show()
```

这样我们之后创建完坐标轴，只需要调用 draw\_midterm\_scores 即可实现绘制期中成绩条形图。  
现在我们尝试隐藏上方和右边的轴。代码如下所示：

```
figure3 = plt.figure(figsize = (10, 5))
ax1 = figure3.add_subplot(1,1,1)
ax1.spines["top"].set_visible(False)
ax1.spines["right"].set_visible(False)
ax1.spines["bottom"].set_visible(True)
ax1.spines["left"].set_visible(True)
draw_midterm_scores()
```

输出如下：  
![Drawing 9.png](https://s0.lgstatic.com/i/image6/M00/47/BF/Cgp9HWDRvxuALT6BAAAxOX-_460930.png)

#### 移动坐标轴

在上述代码中，类似 ax1.spines\["left"\] 返回来的就是 Spine 对象，也就是脊柱。除了设置是否显示，另一个重要的功能就是轴的移动。我们可以用 set\_position 函数来移动坐标轴。

set\_position 接收一个列表作为参数，列表的第一个元素是位置类型，第二个元素是具体的位置数字。位置类型的取值有：

-   axes，代表位置移动是基于相对于坐标轴的倍数；
    
-   data，代表位置移动是基于轴上数据点的值。
    

上面的说法可能比较抽象，我们举几个具体的例子：

-   set\_position(\["axes", 0.5\])， 类型为 axes，位置数字为 0.5 ， 代表将当前的轴移动到坐标轴一半的位置。
    
-   set\_position(\["axes", -0.5\])，代表把轴以相反方向移动当前轴一半的距离
    
-   set\_position(\["data", 10\])，代表把轴移动到轴上 10 这个数据点的位置。
    

这次我们以 sin 函数为例来学习坐标轴位置设置的方法。首先第一步，我们基于之前我们实现的 sin 函数绘制，将 sin 函数封装为一个函数：

```
def draw_sin():
    x_ranges = np.arange(-20, 20, 0.2)
    plt.title("y = sin(x)")
    plt.xlabel("X")
    plt.xlim([-20, 20])
    plt.ylabel("Y")
    plt.ylim([-2.5, 2.5])
    plt.plot(x_ranges,np.sin(x_ranges) )
    plt.show()
```

之后，我们先看看默认的轴的位置。代码如下：

```
figure3 = plt.figure(figsize = (10, 5))
ax1 = figure3.add_subplot(1,1,1)
draw_sin()
```

输出如下：  
![Drawing 11.png](https://s0.lgstatic.com/i/image6/M00/47/C8/CioPOWDRvySAT2yeAABejY7BxZo795.png)

首先，我们将顶部和右边的轴隐藏，然后将 y 轴移动到 -5 这个数据点的位置。代码如下：

```
figure3 = plt.figure(figsize = (10, 5))
ax1 = figure3.add_subplot(1,1,1)
ax1.spines["top"].set_visible(False)
ax1.spines["right"].set_visible(False)
ax1.spines["left"].set_position(["data",-5])
draw_sin()
```

输出如下：  
![Drawing 13.png](https://s0.lgstatic.com/i/image6/M00/47/BF/Cgp9HWDRvyuAIerQAABRZnuv9Xg962.png)

可以看到，我们成功将 y 轴，也就是 left 对应的左轴，移动到了数据点 -5 的位置。

当然，改变坐标轴的位置也不是随便改，一般都有具体的意义，比如我们希望用更数学的方式来展示函数的曲线，那我们可以将 x 轴和 y 轴都移动到 0 点的位置。因为我们的数据是对称的，所以我们用 axes 的位置类型，移动坐标轴的一半也可以实现类似的效果。代码如下所示：

```
figure3 = plt.figure(figsize = (10, 5))
ax1 = figure3.add_subplot(1,1,1)
ax1.spines["top"].set_visible(False)
ax1.spines["right"].set_visible(False)
ax1.spines["left"].set_position(["data",0])
ax1.spines["bottom"].set_position(["axes",0.5])
draw_sin()
```

输出如下：  
![Drawing 15.png](https://s0.lgstatic.com/i/image6/M00/47/C0/Cgp9HWDRvzOAX763AABXCtqJZkw897.png)

可以看到，通过移动坐标轴，我们可以根据场景的不同画出更加易于理解的图像。

#### 坐标轴的常见样式

之前我们学习过，通过设置 plot 函数的参数，可以设置线条的颜色、线型和线宽。同样，matplotlib 的坐标轴也支持设置这三个属性。分别通过如下函数来调整。

-   set\_linewidth：设置轴的粗细。
    
-   set\_linestyle：设置轴的样式，具体样式的语法可以参考之前折线图中的 fmt 语法中，线型部分的取值。
    
-   set\_color：设置轴的颜色。
    

这三个函数使用相对简单，我们直接上例子，我们来对上面例子中的 y 轴进行一些样式的设置。代码如下：

```
figure3 = plt.figure(figsize = (10, 5))
ax1 = figure3.add_subplot(1,1,1)
ax1.spines["top"].set_visible(False)
ax1.spines["right"].set_visible(False)
ax1.spines["left"].set_position(["data",0])
ax1.spines["bottom"].set_position(["axes",0.5])
ax1.spines["left"].set_linewidth(3)
ax1.spines["left"].set_linestyle("-.")
ax1.spines["left"].set_color("r")
draw_sin()
```

输出如下：  
![Drawing 17.png](https://s0.lgstatic.com/i/image6/M00/47/C8/CioPOWDRvzuAUJ0bAABcC7ApjDA010.png)

### 图例

在完成脊柱的相关设置后，我们即将迎来图像体系知识的最后一个部分：图例与注解。现在虽然我们的图表整体已经专业了很多。但在有的场景下仍然缺乏足够的表现能力，简单地说就是没那么易懂。

举一个例子，假设我们需要在图像中同时画出 sin 和 cos 的函数曲线。首先，我们扩展之前的 draw\_sin 函数，扩展为 draw\_sin\_cos。

```
def draw_sin_cos():
    x_ranges = np.arange(-20, 20, 0.2)
    plt.title("Sin & Cos")
    plt.xlabel("X")
    plt.xlim([-20, 20])
    plt.ylabel("Y")
    plt.ylim([-2.5, 2.5])
    plt.plot(x_ranges,np.sin(x_ranges))
    plt.plot(x_ranges, np.cos(x_ranges), "-.r")
    plt.show()
```

然后基于我们刚才设计的坐标轴，画出图形：

```
figure3 = plt.figure(figsize = (10, 5))
ax1 = figure3.add_subplot(1,1,1)
ax1.spines["top"].set_visible(False)
ax1.spines["right"].set_visible(False)
ax1.spines["left"].set_position(["data",0])
ax1.spines["bottom"].set_position(["axes",0.5])
draw_sin_cos()
```

输出如下：  
![Drawing 19.png](https://s0.lgstatic.com/i/image6/M00/47/C8/CioPOWDRv0SAJgeDAAB7V3L2pmQ726.png)

图像虽然看起来没什么问题，但如果把这张图像放在数据分析报告中，看报告的人很难一眼就看出哪条是 sin，哪条是 cos，有一定的理解成本。

接下来我们就来学习如何通过图例和注解让图像更容易理解。

#### 添加图例

在 matplotlib 中，图例代表在图像中的一个小区域，用来专门说明图像中曲线的名称。添加图例一般有两个步骤：

1.  在 plot 的时候，指定曲线的 label，通过 plot 函数的label 参数指定；
    
2.  在 plot 结束，show 之前，调用 plt.legend ，通知 matplotlib 需要展示图例。
    

现在我们就来实操一下。

首先我们需要修改 draw\_sin\_cos 函数，在 plot 的时候传入函数的名称，以及添加调用 legend 函数，代码如下：

```
def draw_sin_cos():
    x_ranges = np.arange(-20, 20, 0.2)
    plt.title("Sin & Cos")
    plt.xlabel("X")
    plt.xlim([-20, 20])
    plt.ylabel("Y")
    plt.ylim([-2.5, 2.5])
    plt.plot(x_ranges,np.sin(x_ranges), label = "y = sin(x)")
    plt.plot(x_ranges, np.cos(x_ranges), "-.r", label = "y = cos(x)")
    plt.legend()
    plt.show()
```

执行代码更新函数，并再次执行调用该函数的绘图代码。输出如下：  
![Drawing 21.png](https://s0.lgstatic.com/i/image6/M00/47/C0/Cgp9HWDRv0-AFwRtAACWx252Bfs917.png)

可以看到，在右上角新增了一个说明区，并展示了曲线样式和对应的label 的关系。这样就能一目了然地知道到底哪条线是 sin 函数，哪条线是 cos 函数。

#### 设置图例的位置

legend 函数支持 loc 参数，用于设定图例展示的位置。有以下几种取值：

![Drawing 22.png](https://s0.lgstatic.com/i/image6/M00/47/C8/CioPOWDRv1aAX2MtAABvhg_eOso794.png)

比如我们将图例改到左上角，修改 draw\_sin\_cos 函数中，legend 函数的调用：

```
    ...
    plt.plot(x_ranges, np.cos(x_ranges), "-.r", label = "y = cos(x)")
    plt.legend(loc="upper left")
    plt.show()
```

重新执行关联的 Cell，输出如下：  
![Drawing 24.png](https://s0.lgstatic.com/i/image6/M00/47/C0/Cgp9HWDRv1-Af7ORAACRX3bEwGg780.png)

这里你也可以自行替换想放的位置来做实验。

### 注解

注解，顾名思义就是用来为图像标注解释的工具。本质上就是在绘制的图表上的部分区域添加文字，来帮助浏览者来快速理解图像的含义。

#### 简易注解

axes 对象，也就是之前我们说的子图对象，提供了 annotate 方法用来在图像中添加注解。最简易的用法就是我们指定一个数据点，以及对应的文本，然后 matplotlib 就会为我们将文本绘制在我们指点的数据点附近的合适的位置。

比如我们希望在之前的 Sin&Cos 的图中更清晰的表示哪条是 sin， 哪条是 cos。annotate 函数接受两个参数，第一个是要标注的字符串，第二个是点的位置。我们只需要选择合适的数据点添加上文本。

修改 draw\_sin\_cos 函数，代码如下：

```
    ...
    plt.plot(x_ranges,np.sin(x_ranges), label = "y = sin(x)")
    plt.plot(x_ranges, np.cos(x_ranges), "-.r", label = "y = cos(x)")
    plt.annotate("y = cos(x)", [0, np.cos(0)])
    plt.annotate("y = sin(x)", [-1.5, np.sin(-1.5)])
    plt.legend(loc="upper left")
    ...
```

添加代码后，重新执行相关的 Cell， 结果如下：  
![Drawing 26.png](https://s0.lgstatic.com/i/image6/M00/47/C8/CioPOWDRv2eAf-OXAACciEpEDY4558.png)

可以看到，我们的注解已经被成功添加到了曲线我们制定的数据点上。使用 plt.annotate 函数，可以让我们实现在曲线的任意位置添加文本。哪怕不附着在曲线上。

比如，我们可以在 （5,2）的位置添加文本“Hello”，在之前的 annotate 函数调用下添加

```
    plt.annotate("Hello", [5,2])
```

执行相关 Cell 后输出：  
![Drawing 28.png](https://s0.lgstatic.com/i/image6/M00/47/C8/CioPOWDRv26AGbFJAACVPXcc-us396.png)

可以看到，Hello 已经被添加到了指定位置。

#### 自定义注解

上图中注解虽然已经添加上去了，但缺点也很明显，文字和曲线有重叠导致看得有点不太清楚。plt.annotate 函数其实支持更加高级的用法，简单地来说就是三个参数。

-   xy：指定数据点的位置，是一个包含两个元素的列表，分别代表 x 和 y。
    
-   xytext: 注解文本的位置，是一个包含两个元素的列表，分别代表 x 和 y。
    
-   arrowprops：指向箭头的属性，是一个字典，用来说明箭头的样式。
    

简单来说，通过这三个属性我们可以设置文本稍微离开数据点，然后通过一个箭头指向数据点，避免文本和曲线重合的问题。对于上面的图像来说，我们可以让 cos 的文本偏上一些，sin 文本偏下一些。

修改 draw\_sin\_cos 的代码如下所示：

```
def draw_sin_cos():
    x_ranges = np.arange(-20, 20, 0.2)
    plt.title("Sin & Cos")
    plt.xlabel("X")
    plt.xlim([-20, 20])
    plt.ylabel("Y")
    plt.ylim([-2.5, 2.5])
    plt.plot(x_ranges,np.sin(x_ranges), label = "y = sin(x)")
    plt.plot(x_ranges, np.cos(x_ranges), "-.r", label = "y = cos(x)")
    plt.annotate("y = cos(x)", xy = [0, np.cos(0)], xytext = [2, np.cos(0) + 0.5],arrowprops={"facecolor":"red", "shrink":0.05}, fontsize=13)
    plt.annotate("y = sin(x)", xy=[-1.5, np.sin(-1.5)], xytext=[-6.5, np.sin(-1.5) - 1], arrowprops={"facecolor":"cyan", "shrink":0.05},fontsize=13)
    plt.legend(loc="upper left")
    plt.show()
```

执行该 Cell，以及下方的调用 draw\_sin\_cos 的 Cell 后，输出如下：  
![Drawing 30.png](https://s0.lgstatic.com/i/image6/M00/47/C0/Cgp9HWDRv32AWD7eAACfOxRlUX4147.png)

可以看到现在我们的文本已经不会和曲线重叠了，并且会将箭头指向我们指定的数据点。

通过搭配使用图例与注解，我们可以让我们画的图像更有表现力，也更容易理解。

### 小结

至此，我们今天的内容就基本上学完了，简单总结一下。

-   脊柱
    
    -   通过 plt.xticks/plt.yticks 设置图像的刻度以及刻度的标签。
        
    -   通过子图对象可以获取到 left/right/bottom/top 四个轴的对象，通过轴对象的 set\_visible 可以控制显示或者隐藏某个轴。
        
    -   通过子图对象的 set\_position 移动坐标轴。
        
    -   通过子图对象的 set\_linewidth 设置轴的线宽， set\_linestyle 设置线型，以及set\_color 设置轴的颜色。
        
-   图例
    
    -   通过设置 plot 函数的label参数指定轴的标签。
        
    -   通过 plt.legend 函数通知matplotlib绘制图例，并通过 loc 参数设置图例的位置。
        
-   注解
    
    -   通过 plt.annotate 函数可以在坐标系的任意位置添加文本。
        
    -   可以通过 xy 参数 xytext 参数以及 arrowprops参数来实现文本适当偏移数据点，并且添加箭头指向数据点。
        

关于 matplotlib 的基础知识，目前我们已经学习完了。下一讲我们会学习一套基于 matplotlib 开发的，绘制更复杂交互的图表的工具库：seaborn。

课后练习：

为期中考试直方图添加图例，注明不同颜色直方图对应的科目。以及添加注解，表明每科的最高分的位置，注解的文本就是“科目名称” +“最高分”。

___

答案：

```
def draw_midterm_scores():
    math_scores = np.array([71,65,70,96,64])
    chinese_scores = np.array([84,75,68,83,57])
    english_scores = np.array([55,78,76,91,64])
    plt.xlabel("",fontsize = 14)
    plt.ylabel("平均分", fontsize = 14)
    plt.xticks(fontsize=14)
    plt.yticks(fontsize=14)
    plt.ylim([0, 110])
    plt.title("期中成绩条形图", fontsize=14)
    plt.rcParams["font.sans-serif"] = "SimHei"
    plt.rcParams["axes.unicode_minus"] = False
    category = ["一班", "二班", "三班", "四班", "五班"]
    plt.xticks(np.arange(len(category)),category)
    plt.yticks(np.arange(0, 100,25), ["D","C", "B", "A"])
    index_category = np.arange(len(category))
    bar_width = 0.25
    plt.bar(index_category - bar_width, chinese_scores, width=bar_width, color=[0,0,1], label="语文")
    plt.bar(index_category, math_scores, width=bar_width, color=[0,1,0], label="数学")
    plt.bar(index_category + bar_width, english_scores, width=bar_width, color=[1,0,0], label="英语")
    plt.annotate("语文最高分",xy = [0-bar_width,84], xytext=[0,100],arrowprops={"facecolor":"cyan", "shrink":0.05},fontsize=13 )
    plt.annotate("数学最高分",xy = [3,96], xytext=[3 - bar_width*4,100],arrowprops={"facecolor":"orange", "shrink":0.05},fontsize=13 )
    plt.annotate("英语最高分",xy = [3+bar_width,91], xytext=[3 + bar_width*2,81],arrowprops={"facecolor":"red", "shrink":0.05},fontsize=13 )
    plt.legend()
    plt.show()
figure3 = plt.figure(figsize = (10, 5))
ax1 = figure3.add_subplot(1,1,1)
draw_midterm_scores()
```

输出如下：  
![Drawing 32.png](https://s0.lgstatic.com/i/image6/M00/47/C8/CioPOWDRv4eAEJykAABOq2JMscQ458.png)
