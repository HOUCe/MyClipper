---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


上一讲中，我们学习了如何通过 Python 下载到网页的内容，并保存为 html 文件。但保存下来的 html 文件中还包括很多我们不需要的内容（那些复杂的代码标签等），如何才能拿到我们感兴趣的内容呢？ 比如电视剧的名字或者新闻标题等。

今天我就介绍下，如何通过 Python 从网页中获取我们感兴趣的数据。

### HTML 基础

在上一讲中，我们知道网页本质上是一种由 HTML 规则标记的文本文件。如果我们要从 HTML 中过滤我们需要的信息，首先第一步就是需要了解 HTML 标记规则的基础知识。

#### HTML 标签基础

HTML 页面其实是由很多基本的元素组合而成，这种元素一般叫作**标签**。形式如下：

主要分为三个部分。

-   开始标记：就是上面的 <标签名> 部分，代表下来的内容是一个标签元素。
    
-   标签内容：就是上面的网页内容部分，指的是在这个标签内部的网页内容，标签内部的网页内容的样式会受到标签的影响。
    
-   结束标记：也就是上面的 </标签名> 部分，代表这个标签已经结束了。
    

HTML 正是通过各种各样不同的标签来把普通的文字内容变成五彩斑斓的网页。

标签是怎么对文本的格式产生影响呢？下面用一个例子来说明，为了便于理解，这里的标签不是真实的 HTML 标签，不过工作原理是差不多的。

比如说，我们希望展示展示一行文字：“今天星期三，天气晴”，但为了醒目希望将其颜色变为红色。那我们可以增加红色的标签，如：

`<red>` 就是开始标记，代表接下来是一个 red 标签，“今天星期三，天气晴”则是 red 标签内部的网页内容，会被 red 标签所影响（文字变为红色）。`</red>` 为结束标记，则代表之后的文字就不是红色了。

另外，我们想强调天气晴，希望把这三个字加粗，所以我们可以把这三个字放在粗体的标签里。

```
<red>今天星期三，<bold>天气晴</bold></red>
```

bold 标签在 red 标签包含的内容里。这说明了标签的一个重要的特型，就是可以**嵌套存在**。简单地说就是一个标签包含的内容里可以包含其他的标签。在上面的例子中，我们把 bold 标签叫作 red 标签的**子标签**，因为它是被 red 标签包住的。

现在，我们希望将这句话居中显示，那我们可以把整段内容放在用来居中的标签中。为了方便查看，我们可以将开始标记、标签内容和结束标记分别放在一个新的行。

如下所示：

```
<center>
<red>
    今天星期三，
<bold>
      天气晴
</bold>
</red>
</center>
```

HTML 中，是否换行只是为了标签的美观，并不影响结果的正确性。

现在，我们可以回过头去看之前我们下载的煎蛋网的主页。

用 VS Code 打开 chapter07 的文件夹，并打开 jiandan.html。并搜索“普通感冒”（由于更新有延迟，你还用你 07 课时的暗号，即你 07 课时看到的第一行标题） 定位到新闻的区域。如下图所示。

![Drawing 0.png](https://s0.lgstatic.com/i/image6/M00/3A/8C/CioPOWB_41CAJoSlAApvC9a23Yo276.png)

可以看到，来自真实世界的网页基本都是一个个的标签互相嵌套而已（红框部分画出了一部分）。下一节，我们将学习 HTML 标签的几个主要的组成部分。

#### HTML 标签的属性

看我们的 jiandan.html，你会发现标签和我之前介绍的不太一样，具体就是开始标签的尖括号内除了标签名，还多了一些内容。这个部分的内容叫作：标签的属性。

具体的形式如下所示：

![Drawing 1.png](https://s0.lgstatic.com/i/image6/M00/3A/84/Cgp9HWB_41aAAC6GAACORzuj_mA839.png)

一个标签里面可能会有一个或者多个属性，每个属性的形式是都是属性名 = 属性值这样的形式。

为什么光用标签还不够，还需要标签的属性呢？举个例子：比如我们希望给网页中的文字设定不同的字号（字体大小）。我们总不能每个字号都弄一个标签。而是会选择设计一个“整合”的字体标签，然后把字号作为字体标签的属性来实现。比如这是一段 18 号大小的字：

```
<font size="18">这是一段18号大小的字</font>
```

#### 常见标签属性

HTML 标签的属性是学习爬虫的重点之一，我们本讲的核心任务就是从一大堆标签中选取出我们想要的内容，主要的方法就是通过标签名搭配标签属性。有的属性是某个标签特有的，比如 font 标签的 size 属性。有的属性是所有标签都有的，最常见的就是 id 和 class 这两个属性。

（1）id 属性

id 顾名思义，一般代表标签的“名字”。 类似我们的身份证号。比如下图中， div 标签的 id 为 wrapper， 下面一个 div 标签的 id 则叫 header。虽然 HTML 的标准定的比较宽松，比如不是每个标签都会有 id 属性，而且 id 的值可能也会有重复，但 id 属性仍然是我们在抓取的内容期间比较重要的参考。

![Drawing 2.png](https://s0.lgstatic.com/i/image6/M00/3A/8C/CioPOWB_416Ac05UAADUBI-k0So846.png)

（2）class 属性

class 属性，代表标签的类别。类别是做什么的呢？ HTML 有一套机制可以创建一组样式，并给这组样式起一个名字，比如 style\_a。然后只要有标签需要用到这组样式，只需要将 class 属性的值写为 style\_a ，那这组样式就自然对这个标签生效了。我们编写爬虫不需要关心这套流程是怎么实现的，只需要知道 class 是所有标签都有的属性即可。

如下图所示：箭头所指的 div 标签的 class 为 logo。代表这个页面的某个地方定义了 logo 这个样式组。

![Drawing 3.png](https://s0.lgstatic.com/i/image6/M00/3A/8C/CioPOWB_42aAR01HAACwlU_pyoU629.png)

至此，我们已经学完了 HTML 的基本要素，在这里总结一下：

1.  HTML 是由一个个标签组成的；
    
2.  标签可以嵌套，也就是每个标签内部可以有其他标签，里面的标签也叫作外面标签的子标签；
    
3.  在标签的开始标记中，可以指定标签的属性；
    
4.  id 和 class 属性是最广泛的使用的属性，每个标签都有这两个属性。
    

### 使用 BeautifulSoup 提取内容

BeautifulSoup 是一个 Python 库，用于分析 HTML。它和它的名字一样，用起来非常“香”。今天我们会通过使用 BeautifulSoup 去从上一节课我们下载到的 html 文件：jiandan.html 中提取所有新闻的标题。

首先，我们在工作目录新建 chapter08，然后把我们在 chapter07 目录中下载的 jiandan.html 拷贝过来。

打开 VS Code，并点击文件 → 打开文件夹，打开刚才创建的 chapter08 文件夹。打开了之后应该就可以在 VS Code 中看到 jiandan.html。

然后新建一个 Notebook，保存为 chapter08.ipynb。接下来我们将在这个 notebook 中对 jiandan.html 进行内容提取。

![Drawing 4.png](https://s0.lgstatic.com/i/image6/M00/3A/84/Cgp9HWB_426Ae6JlAADkCeyw4CE570.png)

> 因为煎蛋网每天都在更新，如果你希望使用和本讲完全一样的 jiandan.html ，可以直接去这里[下载](https://github.com/th-sails/python-data-course?fileGuid=xxQTRXtVcqtHK6j8)。

#### 安装 BeautifulSoup

和之前安装 selenium 的方式类似，我们去开始菜单 → Anaconda ，选择 Anaconda Prompt，出现命令行界面。苹果系统（Mac ）搜索终端即可。

在命名行界面，输入如下的安装命令（BeautifulSoup 的 Python 包名为 bs4, 简写）

回车执行，之后在询问会否确认时输入 y 回车，等待安装完毕，关闭即可。

![Drawing 5.png](https://s0.lgstatic.com/i/image6/M00/3A/8C/CioPOWB_43eAOkfsAADi_Z9vbto917.png)

#### 读取 html 文件到 Python

数据提取的第一步，我们首先需要将 html 文件加载到 Python 的变量中。在上一讲中，我们学习了通过文件对象来把 Python 变量写进文件里，这里我们来尝试用类似的代码来将文件中的内容读出来。

代码如下，可以看到和写入文件的代码非常类似。

```
fo = open("jiandan.html","r",encoding="utf-8")
html_content = fo.read()
fo.close()
print(html_content)
```

按 shift + enter 执行之后，输出结果如下所示（打印的 html 文本太长，这里仅截图示意）。

![Drawing 6.png](https://s0.lgstatic.com/i/image6/M00/3A/84/Cgp9HWB_436AQXW0AAHkviDKjUo958.png)

#### 创建 BeautifulSoup 对象

现在，我们已经将文件的内容读了出来，但是目前还是以一个超大的字符串的形式存储在变量中。想要对其进行有效的分析，第一步我们先要构造一个 BeautifulSoup 的对象。代码如下：

```
from bs4 import BeautifulSoup
doc = BeautifulSoup(html_content)
print(doc.title)
```

执行上述代码之后，BeautifulSoup 对象就被创建并存在变量 doc 中，为了测试是否创建成功，我们打印了 doc 对象的 title 属性，输出如下。

```
<title>
煎蛋 - 地球上没有新鲜事
</title>
```

#### BeautifulSoup 对象的基本使用

刚刚我们打印了 doc 对象的 title 属性，来测试对象是否创建成功。从打印的内容来看，想必你已经猜到了，doc 对象的 title 属性，就对应了网页的 title 标签。但是刚才我们打印的内容中，title 标签也被打印出来了。

那是否可以只取标签里面的内容呢？

（1）get\_text 方法

在 BeautifulSoup 中，我们可以通过标签对象的 get\_text 方法来获得标签中的内容。

```
title_label = doc.title
print(title_label.get_text())
```

输出如下：

可以看到，这次我们成功将标签内的文字提取了出来。

（2）find\_all 方法

BeautifulSoup 对象另一个常用方法是 find\_all， 用来在 html 文档中查到特定标签名以及特定属性值的标签，下面我举例来说明。

在上面，我们学习 html 标签的 class 属性的时候，截了一张这样的图：

![Drawing 7.png](https://s0.lgstatic.com/i/image6/M00/3A/84/Cgp9HWB_44uAPXdFAACwlU_pyoU838.png)

这个截图同样也是来自 jiandan.html 这个文件中，而目前我们的 doc 对象已经包含了这些内容。现在我们来尝试提取 class 等于 logo 的这个 div 标签，代码如下。

```
logo_label = doc.find_all("div", class_="logo")
print(logo_label)
```

输出结果为：

```
[<div class="logo">
<h1><a href="/" onclick="ga('send', 'pageview','/header/logo');">煎蛋</a></h1>
</div>]
```

可以看到，我们截图中的内容，箭头所指的 div 标签被成功的过滤了出来。

（3）获取标签对象的属性

通过标签对象的 attrs 属性，我们可以获取标签对象的属性的值，attrs 是一个字典，获取属性的值的方法如下：

```
label = logo_label[0]
print(label.attrs["class"])
```

输出：

### 过滤出煎蛋的新闻列表

现在，我们来使用之前的 doc 对象来过滤出新闻的标题列表。

#### （1）观察 html，提取初步的新闻标签列表

在使用 BeautifulSoup 进行内容过滤的第一步，首先要查看我们想要的内容所在的标签。我们在 VS Code 中打开 jiandan.html，搜索“普通感冒”（暗号，你在 07 课时看到的第一行标题关键词）到达新闻标题内容区域，如下图所示。

![Drawing 8.png](https://s0.lgstatic.com/i/image6/M00/3A/8C/CioPOWB_45aAT4TCAAde5ee8MOc271.png)

通过观察截图中的标签内容，很容易发现似乎每个新闻标题，都有一个对应的 div 标签，并且它的 class 是 indexs。沿着这个思路，我们可以先考虑使用 find\_all 将其过滤出来。通过如下代码。

```
index_labels = doc.find_all("div", class_="indexs")
print(len(index_labels))
```

输出：

说明找到了 24 个符合条件的 div 标签。一个网页的新闻也是 20 多条，说明有希望。

#### （2）提取单个标签的新闻标题

因为 index\_labels 是一个标签对象的列表，距离我们要过滤的新闻标题还有距离。下一步，我们就来分析如何从这些标签对象中抽取新闻标题。

首先，我们先取第一个来看一下。

```
first_object = index_labels[0]
print(first_object)
```

输出如下：

```
<div class="indexs">
<span class="comment-link" title="1小时 ago">8</span>
<h2><a href="http://jandan.net/p/108693" target="_blank">引发普通感冒的鼻病毒会将新冠病毒排挤出细胞！</a></h2>
<div class="time_s"><a href="http://jandan.net/p/author/majer">majer</a> · <strong><a href="http://jandan.net/p/tag/%e6%96%b0%e5%86%a0%e7%97%85%e6%af%92" rel="tag">新冠病毒</a></strong></div>
换而言之，如果你感冒了，在那段期间就不会被新冠病毒感染 </div>
```

现在我们的问题就转化为，如何从上述 html 中抽取出新闻的标题。

通过查看上述内容可以发现，我们的标题在第三行，在一个 a 标签内部。那是不是我们只需要过滤出这个 a 标签，再使用 get\_text 拿到里面的内容就好了？

你猜得没错。那要怎么过滤出第三行的 a 标签呢？可以看到这个 a 标签里有 target="\_blank" 这样一个属性，我们就可以使用 find\_all 将其过滤出来了。

```
a_labels = first_object.find_all("a",target="_blank")
my_label = a_labels[0]
print(my_label.get_text())
```

输出如下：

你看，新闻的标题被成功抽取了出来！

#### （3）提取新闻标题的列表

目前，我们已经实现了从第一个标签对象中提取新闻标题，但我们的列表中有 24 个标签对象。要怎么做呢？聪明的你应该想到了，我们只需要把“从标签对象抽取标题”这段逻辑写成一个函数，然后通过一个循环对列表的每个标签对象都调用这个函数即可。

代码如下：

```
def get_title(label_object):
    a_labels = label_object.find_all("a",target="_blank")
    my_label = a_labels[0]
return my_label.get_text()
for i in range(0,len(index_labels)):
    title = get_title(index_labels[i])
    print(title)
```

执行后输出：

```
引发普通感冒的鼻病毒会将新冠病毒排挤出细胞！
无厘头研究：植入虚假的记忆再抹去它们
什么是仇恨犯罪？
突发：LHCb发现了违背标准模型的现象
今日带货 20210324
舌战裸猿：IBM搞出了可以打辩论赛的AI
大吐槽：「我没醉，醉的是世界」
今年世界总发电量的0.6%被用于挖比特币
接种疫苗后还是感染新冠？不要为此惊讶
今日带货：蛋友家的血橙
科学家首次在野外检测到抗多药的超级真菌
未在iPhone12盒中搭配充电器，苹果被巴西消协罚200万美元
工程师将解决城市陷坑的问题
今日带货：粉面专场
科学家在碟子里培育出了泪腺，并让它哭泣
疯狂实验进行时：把志愿者禁闭在黑暗的空间里40天
今日带货 20210321
我们已向外星人发送了哪些消息？
脑力小体操：石头+剪刀 VS 石头+布
发霉啦：今天，我终于向母亲摊牌了
普渡大学的经济学家计算出世界各地幸福的价格
人类首次观察到木星上极光黎明风暴的成形过程
为女儿出头，母亲编辑假裸照败坏高中啦啦队队员的名誉
今日带货：淘宝京东蛋友推荐
```

可以看到，jiandan.html 中的所有新闻标题被我们成功过滤了出来，现在是不是感受到了 BeautifulSoup 的神奇之处了呢。

至此，我们就完成了使用 BeautifulSoup 对网页进行内容的提取。

### 小结

总结一下今天所学习到的内容。

（1）HTML 基础

-   HTML 页面是由一个个 HTML 标签组成的。
    
-   HTML 标签分为开始标记、结束标记以及中间的内容三部分。每个标签内部可以在嵌套其他的标签，也可以是普通的文本。
    
-   HTML 标签的开始标记中可以写标签的属性，id 和 class 是所有标签都有的，最常用的属性。
    

（2）BeautifulSoup 的使用

-   首先需要将 html 文件读到 Python 中，再用存储着文件内容的变量构造 BeautifulSoup 对象。
    
-   可以使用 BeautifulSoup 对象的 find\_all 方法，找到标签名和属性值都符合条件的标签对象，比如 doc.find\_all('div', class\_="logo") 代表找到标签名为 div， class 属性为 logo 的标签。
    
-   标签对象都有 get\_text 方法，用于返回标签内部的文本。
    

（3）网页数据抓取的分析方法

-   观察想要过滤内容所在的标签特征，过滤出包含文本的标签，然后调用 get\_text 拿到文本内容。
    
-   如果层次关系比较复杂，可以分步走，一步一步过滤出最终的内容。比如例子中，我们首先过滤了 class=indexs 的 div 标签，再从其中过滤出包含标题的 a 标签。
    

课后练习：

在课程代码的基础上，编写代码，提取出新闻的时间内容，如下图红框中部分。

提示：需要用到标签对象的 attrs 属性。

![Drawing 9.png](https://s0.lgstatic.com/i/image6/M00/3A/84/Cgp9HWB_462AdEvkAAS7Pw1Jr6I131.png)

___

答案：

```
comments = doc.find_all("span",class_="comment-link")
for i in range(0,len(comments)):
    comment = comments[i]
    print(comment.attrs["title"])
```

输出

```
1小时 ago
4小时 ago
8小时 ago
12小时 ago
14小时 ago
23小时 ago
1天 ago
1天 ago
1天 ago
2天 ago
2天 ago
2天 ago
2天 ago
3天 ago
3天 ago
3天 ago
4天 ago
4天 ago
4天 ago
5天 ago
5天 ago
5天 ago
5天 ago
6天 ago
```
