---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


“工欲善其事，必先利其器。”在课程正式开始之前，我们先搭建一套高效的 Python 工作环境为后续数据分析做准备。

关于高效作业，对于需要编写 Python 代码进行数据分析的工作而言，主要涉及两个方面。

**1\. 一款具备强大的自动完成和错误提示的开发工具。**

Python 丰富的函数库和组件库是这门语言强大的核心原因，但我们不可能去记忆所有的方法名和参数名，往往只能记住一些常用的或者某个方法开头的几个字母。这个时候**一个好的开发工具就需要能聪明地“猜”出你想输入的代码**，并给出候选列表方便你选择（类似于输入法的字词提示功能）。

另外，**当你输入错误的时候，这个工具能够提示你具体是哪里错了，建议改成什么，从而大幅提升编写效率**。在别人还在查到底是哪个单词拼错了导致代码跑不起来的时候，你已经写完一个完整的模块了。

**2\. 掌握快捷键。**

Python 数据分析需要边写边看结果，甚至每写两行代码就需要点击运行、新建文本段落、代码段落等操作。所以熟练地掌握快捷键，可以使绝大多数的操作都不需要鼠标，手不用离开键盘就能完成，起到事半功倍的效果。

### 说干就干，Let's hack!

接下来，我就先带你配置一套比传统方式更高效的数据分析开发工具，而快捷键则会在之后的教学过程一步步教给大家。

整个配置过程相比传统的环境安装稍微多了几步，不过并不复杂，你只需要跟着一步一步操作就可以。

> 本课时搭建环境的版本说明如下：  
> Anaconda3.0  
> VS Code 1.51.1  
> 实际并无太多版本限制，你安装最新版即可。

#### 第一步，数据科学增强版的 Python 环境：Anaconda

Anaconda 是一个 Python 数据科学工具包，里面包含了 Python 做数据计算最常用的库和工具，属于必装软件。目前它已经非常成熟，并且整套 Anaconda 可以免费提供给个人使用。

**1**. 用浏览器访问 Anaconda 的个人版页面：[h](https://www.anaconda.com/products/individual?fileGuid=xxQTRXtVcqtHK6j8)[ttps://www.anac](https://www.anaconda.com/products/individual?fileGuid=xxQTRXtVcqtHK6j8)[on](https://www.anaconda.com/products/individual?fileGuid=xxQTRXtVcqtHK6j8)[da.com/products/individual](https://www.anaconda.com/products/individual?fileGuid=xxQTRXtVcqtHK6j8) ，点击 Download，页面会自动跳转到具体的下载页面，如图所示：

![Drawing 0.png](https://s0.lgstatic.com/i/image6/M01/2D/6F/Cgp9HWBmjziAHkoCAAJzVFPWD-c336.png)

**2**. 根据自己的设备类型 （Mac/Windows），选择合适的安装包版本。无论 Windows 还是Mac， 都选择 Graphical Installer，它代表图形化的安装器，之后更易于使用。

> 本篇教程后续步骤以 Windows 为例。

**3**. 下载之后双击安装包进行安装（如图所示），直接点击 Next。

![Drawing 1.png](https://s0.lgstatic.com/i/image6/M00/2D/77/CioPOWBmj1OAPltdAAJG0cCuo4w472.png)

**4**. 接下来就是使用协议界面，点击 I Agree，代表同意使用协议。

![Drawing 2.png](https://s0.lgstatic.com/i/image6/M00/2D/77/CioPOWBmj1mAOKfCAAM0jYlBpcU374.png)

**5**. 之后连续 Next，可以看到选择安装位置的界面，如果没有特殊的需求，直接默认位置就好，继续点击 Next。

![Drawing 3.png](https://s0.lgstatic.com/i/image6/M00/2D/6F/Cgp9HWBmj1-AZ7mOAAH8fGuG76g018.png)

**6**. 最后一个配置界面是高级选项，不用更改，直接点击 Install，等待 2~3 分钟之后，即可完成安装。

安装完毕之后，可以从程序中找到 Anaconda Navigator，点击打开就可以看到整套 Anaconda3 的所有工具（如下图所示）：

![Drawing 4.png](https://s0.lgstatic.com/i/image6/M00/2D/77/CioPOWBmj2mAY44ZAAJ4bjvt15I883.png)

其中 Notebook 是数据分析应用范围最广泛的工具，也是本次课程的主力工具，但它却不是一款足够有效率的工具，因为它缺乏智能的代码输入联想、自动完成和错误提示。而有效率的分析师是不会容忍自己用“记事本”写代码的。

所以，接下来，我会带你在自己的电脑中配置一个智能、强大的 Notebook（此时安装好的Anaconda3 页面先不关闭）。

#### 第二步，飞一般的代码编辑器：VS Code

VS Code（ Visual Studio Code），是微软开发的跨平台代码编辑器，靠着其强大的插件生态，目前已经成为全球最流行的代码编辑器。本次我们就通过 VS Code，来解决 Notebook 开发效率的问题。

首先按照以下的步骤安装和配置 VS Code。

**1**. 下载：用浏览器访问[https://code.visualstudio.com/](https://code.visualstudio.com/?fileGuid=xxQTRXtVcqtHK6j8)，网页会直接识别当前的操作系统，直接点击下载按钮，下载安装包。

**2**. 安装：下载完毕后，双击安装包进行安装，全部默认配置即可。

**3**. 安装中文语言包【可选，习惯英文的同学可以跳过】：启动 VS Code，进入插件 Tab（左侧边栏最后下方的图标），输入 【Chinese】，出现的第一个插件，点击 Install 安装。安装完成后，重启 VS Code 即可生效。

![Drawing 5.png](https://s0.lgstatic.com/i/image6/M00/2D/77/CioPOWBmj3GAU1p2AAD2IWnRzOU216.png)

**4**. 安装 Python 插件：依旧是在插件面板，输入 【Python】，安装列表中的第一个插件。

![Drawing 6.png](https://s0.lgstatic.com/i/image6/M00/2D/6F/Cgp9HWBmj4eAIa0-AAB94MABrDY329.png)

至此，基础的 VS Code 环境已经配置完毕，在下一步我们会开始用 VS Code 编写Python 代码。

#### 第三步，配置 VS Code 使用 Anaconda 的Python环境

现在，我们开始实际地玩一玩 VS Code。

打开 VS Code，选择【文件】-【新建文件】，会建立一个默认的文本文件，按 CTRL +s 保存，文件名为【hello.py】。

> 后缀名一定要是 .py，因为 VS Code 要根据文件的后缀名来匹配合适的工具链。

保存之后，如果 VS Code 识别到 Python 文件，我们上一步安装的 Python 插件就会开始工作，寻找本机的Python 环境，结果会展示在下方的状态栏上。

![Drawing 7.png](https://s0.lgstatic.com/i/image6/M00/2D/77/CioPOWBmj46Ac9KZAABFRVP-_yA643.png)

状态栏上的是 Python 3.8.5 64-bit(conda)，说明目前 VS Code 已经在使用我们第一步 Anaconda 安装的 Python 环境。如果不是，可以点击这行字，在弹出的列表中选择带 Conda 字样的Python 环境。

> Anaconda的 Python环境包含了丰富的科学计算的库，所以是做数据分析的首选。

确认环境之后，我们即可进入最后一步。

#### 第四步，Jupyter in VS Code

见证奇迹的时刻来临了，我们进入 VS Code 的插件 Tab（左侧边栏最下方的图标），输入 Jupyter 安装由微软官方出品的 Jupyter 插件（前几个有 Microsoft 字眼的）。

![Drawing 8.png](https://s0.lgstatic.com/i/image6/M00/2D/6F/Cgp9HWBmj5aAKvGrAABUqOcg_gM988.png)

安装完成之后，重启 VS Code（如果显示是禁用，那就是安装好了，直接操作后续即可）。按 【CTRL+P】 弹出命令面板，输入【>Jupyter】，此时会列出所有 Jupyter 插件支持的操作，选择 【Jupyter: Create New Blank Jupyter Notebook】，如下图所示。

![Drawing 9.png](https://s0.lgstatic.com/i/image6/M00/2D/6F/Cgp9HWBmj52AAWOfAAD8hk8ikao110.png)

选择之后，VS Code 内部就出现了一个类似 Notebook 的编辑界面，和传统的网页版 Notebook不同，VS Code 中的Notebook 具备强大的代码提示和自动完成的功能。接下来，我们来学习一下它的主要操作。

打开编辑界面，我们将 Notebook 可操作性的区域分为三个部分：主操作区、Cell 操作区、 边栏操作区。

![Drawing 11.png](https://s0.lgstatic.com/i/image6/M01/2D/78/CioPOWBmkAmAGFRfAAIZXU8Ix7A173.png)

**主操作区**：主要用来控制整个 Notebook 的一些行为，从左到右的功能依次为：

-   运行全部 Cell；
    
-   运行上一个 Cell；
    
-   运行下一个 Cell；
    
-   重启 Kernel；
    
-   中断 Kernel；
    
-   插入 Cell；
    
-   清空结果；
    
-   展示活动的变量；
    
-   保存；
    
-   导出。
    

> Kernel 代表在后台实际运行Python代码的服务。

**边栏操作区**：不同位置的“+”号代表在不同位置插入 Cell。

**Cell 操作区**：主要用来控制当前 Cell 的行为，从左到右依次为：

-   运行 Cell（快捷键：【Shift + Enter】）；
    
-   逐行运行 Cell；
    
-   切换为文本模式（切换为文本模式后，点击"{}"图标可切换回代码模式）。
    

Cell是Notebook 中的核心概念，直译过来是“单元格”，但 Notebook 中的Cell 却不能用单元格简单概括，所以本文统一用Cell 描述，一个 Notebook 由多个 Cell 组成。

Cell 一共有两种类型：

-   **代码Cell**，主要用来编写 Python 代码，每个代码 Cell 都可以单独执行，并且执行结果会展示在 Cell的下方。
    
-   **文本 Cell**，顾名思义，用来编写文本, 对于数据分析工作而言，除了代码本身，分析的思路、推导的逻辑同样非常重要，文本 Cell 就是用来承载这些内容。
    

这也是 Notebook 区别于 IPython 最大的地方，可以实现代码和文本的混排，来最大化的呈现数据分析的产出。

### Notebook 的基本操作

接下来，我们通过一个具体的任务，学习一下 Notebook 的基本操作。这些操作在后续的课程中会经常用到，我们先通过几个简单的小任务初步熟悉一下。

任务目标如下。

1.  创建一个 Notebook，保存为 my\_practice.ipynb。
    
2.  添加一个 Cell，通过代码打印“I am using Notebook”, 并运行。 在之后的课程中，我们每学完一个小阶段，都会通过新建一个 Cell 来编写代码测试我们学习的内容。
    
3.  添加一个Cell，并转换成文本 Cell，输入文字“我的数据分析启程了！耶！”。在之后的课程中，我们希望把自己编写代码和分析的思路写下来的时候，往往就需要添加一个 Cell 并转换为文本模式这样来记录。这样最后我们的代码和分析文字都在同一个文件（Notebook）中，非常清晰。
    
4.  添加一个 Cell，通过代码打印 1+1 的结果。
    

让我们来一步步实现上面的任务吧。

第一步，按【CTRL + P】（Mac 对应【CMD + P】）， 调出 VS Code 的命令面板，输入【> Jupyter】可以看到 Notebook 插件支持的命令，其中比较常用的几个如下。

> **1\. Create New Black Jupyter Notebook：** 创建新的空白 Notebook 工作区。  
> **2\. Export to PDF**：将当前的Notebook 导出为 PDF，在后续写数据分析报告的时候会用到。  
> **3\. Import Jupyter Notebook**：导入已有的Notebook。用来导入已有的 Notebook 文件，后续课程的所有 Notebook 我都会放到 Github 上共享，可以下载后导入运行。

![Drawing 12.png](https://s0.lgstatic.com/i/image6/M00/2D/78/CioPOWBmj9yAeI5CAADzIYou8DI414.png)

今天我们首先选择第一个，创建一个新的Notebook，创建之后按 【CTRL + S】 保存，文件名输入：my\_practice.ipynb。

第二步，我们要新建 Cell，我们点击边栏操作区的 + 号即可新建 Cell， 然后我们输入以下代码：

![Drawing 13.png](https://s0.lgstatic.com/i/image6/M01/2D/78/CioPOWBmkBaACcSMAACLIOFD0sg833.png)

```
print("I am using Notebook")
```

点击 Cell 操作区中的“运行 Cell”，可以看到我们代码被执行，并打印出了上述字符串。效果如下图所示。

![Drawing 14.png](https://s0.lgstatic.com/i/image6/M01/2D/78/CioPOWBmkCSAY9SxAABiiVr6B9o327.png)

第三步，我们类似第二步首先新建一个 Cell，并点击 Cell 操作区中的 M 图标，切换为文本模式，并输入“我的数据分析启程了！耶！”。输入完毕后鼠标点击 Cell 之外的任意区域即可退出编辑模式，进入预览模式（双击 Cell 可重新进入编辑模式）。这样，我们的第三步就完成了。 如图所示。

![Drawing 15.png](https://s0.lgstatic.com/i/image6/M01/2D/70/Cgp9HWBmkCuAd18YAACRFjQAhXU645.png)

第四步，就很简单了，我们直接新建一个 Cell， 并输入以下代码：

运行 Cell，可以看到打印了“2”，至此，我们的任务已经全部完成。整个过程如图所示。

![Drawing 16.png](https://s0.lgstatic.com/i/image6/M01/2D/79/CioPOWBmkDKANTuZAAC7vrBVucI709.png)

### 小结

至此，你已经在自己电脑上配置出一套面向数据分析的Python 开发环境，也知道如何新建 Notebook，以及在 Notebook中添加代码 Cell 来输入代码、文本 Cell 来输入文字。

回顾下今天的内容，我主要强调了这几点：

-   对于需要编写 Python 代码进行数据分析的工作而言，一款具备强大的自动完成和错误提示的开发工具、掌握常用的快捷键会让你事半功倍；
    
-   使用 Anaconda + VS Code + Jupyter Notebook 插件，可以搭建一套具备代码提示和自动完成的、高效的、面向数据分析的开发环境；
    
-   你要学会 VS Code Notebook 的基本操作，这些基础我们后续都会用到。
    

搭建环境是用Python 做数据分析的第一步，为了让你学习更顺畅，**我们的视频观看形式附带了实操视频**。如果你在搭建环境中还有什么问题，可以写在留言区，我会帮你扫清障碍，让你快速上手 Python。

下一讲，我们将正式进入 Python 语言的世界，开始学习 Python 语言的基础：变量与数据类型。

> **彩蛋**  
> 本节课里会用到的快捷键：  
> 【CTRL + P】打开 VS Code 的命令面板；  
> 【Shift + Enter】运行当前 Cell；  
> 【Esc】退出当前 Cell 的编辑状态，退出编辑状态之后可以用方向键上下移动 Cell。
