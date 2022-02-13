---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


现在，我们已经学完了 Python 语言几大核心组成部分：变量与数据类型、分支结构与循环结构、函数以及类与对象。

本节我们将通过完成一个稍微复杂的实战案例来把这些知识点串起来，加深你对相关知识点的理解。另外，在学习本章的过程中，可能会需要你经常去翻前面几章学到的内容哟。

### 任务描述

转眼间，你在阿普闪购已经实习了两个月。这段时间你对于工作日渐得心应手，还收获了两个好朋友：财务部的实习生小 E 和后端技术实习生阿强。最近午饭的时候，小 E 总是抱怨自己最近工作太忙了老是漏处理一些事情。要是有个软件可以记录所有的待办事项，能看到哪些事情做了哪些事情没做就好了，这样就不会漏了。

刚学完 Python 的你毫不犹豫地决定要帮小 E 这个忙，为她设计一个简单的待办管理工具。

经过充分了解了小E的需求之后，你列出了基本的需求点：

-   新增待办；
    
-   删除已有的待办；
    
-   标记一个待办为已完成；
    
-   查看所有待完成和已完成的待办，并且能够区分展示。
    

### 准备知识

这次为了完成给小 E 设计的软件几乎需要用到你所有用过的知识，还需要一些从没接触过的简单内容，我先介绍下。

（1）导入系统的库函数

Python 有好多库函数，如果你想用现成的函数，可以用这个 from-import 语句把对应的函数调出来。比如我们要用到 os.path 模块中的 exists 函数，你可以这么写：

```
from os.path import exists
```

一般在库函数的介绍页面都会给出函数所在的模块名。

（2）获得用户输入的内容

很多时候我们的代码都需要处理用户输入的内容。比如这个例子中，我们需要用户输入待办内容和命令。Python 中的 input 函数可以实现这样的功能。形式如下：

```
变量 = input("提示用户输入的时候展示信息")
```

该代码执行之后，会在 VSCode 上方出现一个输入框，用户输入内容按回车之后，用户输入的内容就会存储在等号右边的变量中。

（3）清除之前打印过的内容

如果我们打印的内容过多，用户看起来就比较费劲。比如在下面的例子中，当我们选择删除待办的时候，我们希望按顺序打印出待办让用户选择，此时就需要先将之前打印的命令菜单清除，这样看起来更清晰些。Python 中清除打印的内容通过 clear\_output 实现。

用法如下。

```
from IPython.display import clear_output
clear_output()
```

### 任务分解

一切准备就绪，现在我们开始分析一下如何完成我们的任务。我们在日常工作的时候，当要做的事情比较复杂的时候，首先第一步就是需要先排个计划，第一步做什么，第二步做什么。

软件设计也是一样的，软件实现的计划一般都是利用分而治之的思想，把一个复杂的任务拆成几个简单一些的子任务，把子任务做完了之后再组合起来完成一开始的复杂任务。

软件设计的计划，大概分为三个步骤：

1.  根据功能的需求，看看是否可以拆成相对独立的几个子需求；
    
2.  针对每个子需求，编写代码实现；
    
3.  将各个子需求的代码组合到一起，完成整个软件的功能。
    

实现子需求的代码，我们一般也称之为**模块**。

这个章节，我们主要完成第一步：根据需求拆分出不同的子需求。

我们首先来畅想一下，我们到时候会怎样使用这个任务管理工具：首先，我们需要看看我们目前有哪些待办还没有完成，以及要看一下我们之前完成了哪些待办。除了查看，我们还会需要添加新的待办。或者完成了某个待办的时候，要把对应的待办标记为已完成。最后，对于一些我们不需要做了的待办，我们可以将其删除。

综上所述，我们大概可以将这次的软件拆分为三个相对独立的子需求：

1.  管理待办：用来保存待办数据，以及支持增加、删除等操作。
    
2.  打印菜单与待办：用于打印菜单以及当前待办。
    
3.  处理用户输入：获取用户的输入，执行相应的命令。
    

为了实现最终的软件，我们需要开发三个模块来实现这三个子需求，我们命名为：

-   待办管理模块（完成管理待办子需求的处理）
    
-   打印模块（完成打印菜单与待办子需求的处理）
    
-   整合模块（完成用户输入子需求的处理）
    

接下来我们一步步实现这三个模块。

### 任务实现

接下来，我们开始正式进行任务的实现。打开 VSCode，新建一个 notebook，并保存为 chapter05.ipynb。

> 注意：接下来章节的代码块和 notebook 中的 Cell 都是一一对应的关系，每一个 Cell 的代码输入完毕后都要记得按 shift + enter 执行哦。

#### （1）实现待办管理

根据我们在类与对象一章学习的知识，同时包含数据和函数的概念最适合用类来表示，我们可以取名为 Task。

根据上述分析，我们不难推导出 Task 类的基本结构。

-   属性：文本内容、状态(初始值是未完成)
    
-   方法：标记已完成。
    

基于此，我们可以写出 Task 类的实现，新建 Cell 输入以下代码：

```
class Task:
def __init__(self, text):
        self.text =text
        self.status = "未完成"
def mark_finished(self):
        self.status = "已完成"
```

上述代码实现了当个待办的基本操作：记录待办文本和完成状态，并提供了改变完成状态的方法。

现在，我们已经实现了待办，那待办管理这个模块的功能是不是就算完成了呢？ 答案是否定的。比如添加待办、删除待办这两个核心的操作还没有完成，要完成这两个操作，我们首先需要一个变量来存储待办，结合变量与数据类型学到的知识，什么样类型的变量最适合存储所有待办呢？因为我们的待办是有多个的，我们学习过能够存储多个数据的数据类型就只有列表和字典，然而这个例子并不涉及映射关系，所以列表是最好的选择。

我们使用列表来存储所有待办，然后再通过函数来实现添加、删除和标记。整体来看，这些功能我们可以用一个类来表示，取名为 TaskList。同时，我们需要分开展示已完成和未完成的待办，对应的函数也可以放在该类中一并完成。

基于上面的分析，TaskList 类的基本结构为。

-   属性：一个列表变量
    
-   方法：增加待办，删除待办，获得已完成的待办列表，获得未完成的待办列表
    

代码如下所示。新建 Cell，输入如下的代码。

```
class TaskList:
def __init__(self):
        self.tasks = []
def add_task(self, task):
        self.tasks.append(task)
def remove_task(self, idx):
del self.tasks[idx]
def get_finished_tasks(self):
        result = []
for i in range(0,len(self.tasks)):
if self.tasks[i].status =="已完成":
                result.append(self.tasks[i])
return result
def get_unfinished_tasks(self):
        result = []
for i in range(0,len(self.tasks)):
if self.tasks[i].status =="未完成":
                result.append(self.tasks[i])
return result
```

现在，待办管理模块已经完成了。接下来我们实现打印模块。

#### （2）实现打印模块

打印模块，分为两个部分，一个是打印菜单，一个是打印当前的待办。当前的待办需要区分已完成和未完成。打印部分整体功能相对简单，我们直接使用两个函数来实现。

首先是打印菜单，我们的菜单分为两部分组成，一部分是问候语，一部分则是命令列表和对应的序号，用于让用户输入序号来决定要执行哪个命令。目前我们的命令有 4 个：

-   1.添加待办
    
-   2.删除待办
    
-   3.完成待办
    
-   4.退出
    

基于上面的分析，我们打印菜单的代码可以类似如下实现。新建 Cell，输入以下代码。

```
def print_menu():
    print("Hello~\n")
    print("请问你希望做什么呢？")
    print("1. 添加待办")
    print("2. 删除待办")
    print("3. 标记待办已完成")
    print("4. 退出")
```

然后是打印已完成和未完成的列表，简单来说就是拿到两个列表，一个是已经完成的待办列表，另一个是未完成待办的列表，我们的任务是把这两个列表的待办内容都打印出来。在程序层面，要做的事情就是逐个访问列表中的元素，然后用 print 语句打印出来。说到逐个，你应该反应过来了，可以通过循环来实现。

新建 Cell，输入如下代码。

```
def print_tasks(finished, unfinished):
    print("未完成的待办：")
for i in range(0,len(unfinished)):
        print(i, unfinished[i].text)
    print("")
    print("已完成的待办：")
for i in range(0,len(finished)):
        print(i, finished[i].text)
    print("")
```

最后，我们还需要打印全部待办。因为打印全部待办是服务于删除待办功能和标记完成功能的，所以不仅需要打印待办的文本，还需要打印序号。所以我们还是用和上面类似的循环来把全部待办的列表打印出来即可，只是打印的同时也要打印循环变量 i。

代码如下：

```
def print_all_tasks(all_tasks):
for i in range(0,len(all_tasks)):
        print(i, all_tasks[i].text)
```

#### （3）实现整合模块

最后一个部分了，见证奇迹的时刻马上就要来临了，加油！

整合模块所需要首要问题就是：我们之前的程序都是运行一次，做一些打印之后就退出了。但这次我们做的是一个小软件，退不退出是用户说了算，比如从之前的菜单来看(此处代码不用输入)：

```
    print("1. 添加待办")
    print("2. 删除待办")
    print("3. 标记待办已完成")
    print("4. 退出")
```

用户可以不断地输入 1 来添加待办，也可以不断输入 3 来将一些待办标记为完成。一直到用户输入 4，代码才终止运行。那要怎么实现这样的逻辑呢？

这就需要用到之前我们学到的循环登场了，刚才描述的逻辑本质上就是一个循环。当用户按1时，接着提示用户输入待办，用户输入完成后，菜单回到初始状态，等待用户选择使用哪个功能，用户可以继续按1来增加，也可以按2来删除。不断循环，一直到用户按4时，程序退出。

写成代码，大概就是这样（仅是为了说明结构，不能直接运行）

```
while True:
    打印未完成待办
    打印已完成待办
    打印菜单
    等待用户输入命令
if 用户输入的是 1:
        等待用户输入待办内容
        添加待办
elif 用户输入的是 2:
        打印所有待办以及序号
        等待用户输入要删除的序号
        删除对应的待办
elif 用户输入的是 3:
        打印所有待办以及序号
        等待用户输入要标记完成的序号
        标记用户输入的待办为完成
elif 用户输入的是 4:
break
```

接下来我们就来一步步实现上述流程。

首先我们新建 Cell，创建一个 TaskList 对象，用于实际管理所有待办。

```
my_task_list = TaskList()
```

运行之后，继续新建一个 Cell，在新的 Cell 中将我们刚才描述的循环逻辑用实际的 Python 代码实现，如下所示

```
while True:
    clear_output()
    finished_tasks = my_task_list.get_finished_tasks()
    unfinished_tasks = my_task_list.get_unfinished_tasks()
    print_tasks(finished_tasks, unfinished_tasks)
    print_menu()
    command = input("请输入操作序号")
if command == "1":
        text = input("请输入希望添加的待办内容")
        task = Task(text)
        my_task_list.add_task(task)
elif command == "2":
        clear_output()
        print_all_tasks(my_task_list.tasks)
        idx = input("请输入想删除的待办序号")
        idx = int(idx)
        my_task_list.remove_task(idx)
elif command == "3":
        clear_output()
        print_all_tasks(my_task_list.tasks)
        idx = input("请输入希望标记完成的待办序号")
        idx = int(idx)
        my_task_list.tasks[idx].mark_finished()
elif command == "4":
break
```

仔细阅读代码，不难发现，我们在（1）和（2）中准备的待办管理和打印模块都被使用上了。  
至此，我们整个软件已经完成。

### 试用一下

现在，我们来测试一下我们编写的待办管理软件的功能。逐个运行我们的 Cell，整合模块的代码 Cell 应该放在最后运行。

因为本讲代码较多，所以我把 Cell 整体的情况贴在下方，你可以查看你输入的代码是否正确。记住，需要从上到下逐个运行 Cell。

![Drawing 0.png](https://s0.lgstatic.com/i/image6/M00/37/96/CioPOWB36QWANa7nAAI1bZ61cnM474.png)  
![Drawing 1.png](https://s0.lgstatic.com/i/image6/M00/37/96/CioPOWB36QuAAe5IAAIAa_49ivc396.png)  
![Drawing 2.png](https://s0.lgstatic.com/i/image6/M00/37/96/CioPOWB36RGAVnPEAAKs3XjG8D0794.png)

运行完整合模块的代码 Cell 之后，可以看到界面如下所示。注意红框部分，一个地方是我们打印的当前待办以及菜单，上面的红框则是我们的 input 函数在等待用户输入，并且提示用户输入操作序号。

![Drawing 3.png](https://s0.lgstatic.com/i/image6/M01/37/8E/Cgp9HWB36RiAJlErAAHYV8No-YM246.png)

#### （1）试试添加待办

我们首先添加待办，输入 1 按回车。这个时候输入框的提示变为：“请输入希望添加的待办内容”

![Drawing 4.png](https://s0.lgstatic.com/i/image6/M01/37/8E/Cgp9HWB36R6AXeT9AABDJ23zTAA409.png)

我们输入“处理报销的发票”，按回车。现在，在未完成的待办中，已经有了刚才我们添加的待办。到这里，说明我们的循环已经成功执行了一次，并且走的是 添加待办的分支。

![Drawing 5.png](https://s0.lgstatic.com/i/image6/M00/37/96/CioPOWB36SaAPtEfAAHPMfhNRLg448.png)

以同样的方式再添加一个待办，假设为：完成总结会的 ppt。添加完毕后，界面回到初始状态，我们在未完成的待办中已经打印了我们添加的两条内容。

![Drawing 6.png](https://s0.lgstatic.com/i/image6/M01/37/8E/Cgp9HWB36S2AZLz3AAGGQU--gZI221.png)

#### （2）试试删除待办

这个时候，我们输入 2 按回车，代表希望删除待办。输入框的提示变为：“请输入想删除的待办序号”，并且在打印的区域，我们之前打印的待办和菜单都已经消失，变为只显示所有待办以及对应的序号。

![Drawing 7.png](https://s0.lgstatic.com/i/image6/M00/37/96/CioPOWB36T-AYy6-AAFuVPx9zNU275.png)

我们输入 1 按回车，把 ppt 这个待办删除，可以看到回到了初始状态，并且在未完成的待办只剩下一条。

![Drawing 8.png](https://s0.lgstatic.com/i/image6/M00/37/96/CioPOWB36TSABg8bAAEG9ecOJPA026.png)

#### （3）试试将待办标记完成

这次，我们输入 3 回车，进入标记待办已完成的分支。这个时候输入框的提示变为：请输入希望标记完成的待办序号。 然后打印区，也就是下方红框的区域，打印出了所有未完成的待办。

![Drawing 9.png](https://s0.lgstatic.com/i/image6/M00/37/96/CioPOWB36UWAQGIeAAFs-6QcWBQ533.png)

由于目前只有一个待办，我们输入 0 ，回车，代表设置第 0 个待办为已完成。这个时候可以看到“处理报销的发票”已经到了已完成的待办下面，这代表我们的操作成功的执行了。

![Drawing 10.png](https://s0.lgstatic.com/i/image6/M00/37/8E/Cgp9HWB36VKAAIY8AAEfPxWMLtc403.png)

#### （4）试试退出

刚才我们做所有操作的时候，都可以看到这个 Cell 的状态是星号，我们在环境准备一章中讲过，Cell 的状态是星号，代表 Cell 一直在运行，这也符合我们的设定。

因为我们在最后的整合模块中，通过一个 while 循环，来让代码一直在等待输入命令——执行命令中一直循环。

![Drawing 11.png](https://s0.lgstatic.com/i/image6/M00/37/8E/Cgp9HWB36VmAGdyKAAH0md-XUUA180.png)

现在，我们按照菜单的提示，输入 4 ，代表退出程序，可以看到 Cell 的状态变回了数字，并且输入框也消失不见了，说明我们的整合模块 Cell 已经执行完毕，退出。

![Drawing 12.png](https://s0.lgstatic.com/i/image6/M00/37/8E/Cgp9HWB36V6AJKfJAAIzdg-ONOc827.png)

至此，我们为小 E 制作的待办管理软件就正式宣告完成了。小 E 再也不用担心会忘记事情啦~

> 现实世界中，为了降低用户的使用成本，最后交付的肯定是带窗口的可以直接使用鼠标操作的软件。本课程基于 Notebook 的软件仅仅是出于教学目的，毕竟要运行软件还得配置个Notebook，肯定也是不友好的 :D

### 小结

这堂课，我们完成了学习 Python 以来最复杂的一个实战案例，但另一方面，这个实战案例也包含了要使用 Python 解决复杂问题所需要的大部分基本能力。换句话说，花点时间，吃透这个案例，那你就能使用 Python 做很多很多的事情了。

现在，我们来回顾一下我们这个案例涉及的内容：

-   准备阶段，学习了 input 函数和 clear\_output 函数的使用，input 函数用来显示对话框，接收用户的输入。clear\_output 用来将之前打印的内容清除
    
-   分解阶段，我们通过对需求的分析，拆分为待办管理模块、打印模块和整合模块三个部分。
    
-   实现模块：
    
    -   待办管理模块，通过实现待办类 Task，以及待办管理类 TaskList。来提供主要的数据存储和操作功能。
        
    -   打印模块，主要通过使用 TaskList 类中的相关方法，实现打印未完成和已完成待办。同时也实现了菜单的打印。
        
    -   整合模块，我们通过一个 while 循环，并在循环体中调用打印模块和待办管理模块的相关函数，实现了 打印菜单-》用户输入命令-》执行命令的循环，执行命令的部分我们通过一个 if-elif-else 语句，来对用户输入的不同命令进行处理。
        

到现在，我们 Python 基础的理论+实践就全部学完了，下一个部分，我们开始系统性的学习如何用 Python 获取用户分析的数据。

#### 课后习题

实现一个新的命令：修改待办的文本。

___

习题答案：

分为以下几个步骤：

（1）修改打印菜单的函数，添加上修改待办的选项。直接追加在末尾即可，对应命令 5

```
def print_menu():
    print("Hello~\n")
    print("请问你希望做什么呢？")
    print("1. 添加待办")
    print("2. 删除待办")
    print("3. 标记待办已完成")
    print("4. 退出")
    print("5. 修改待办")
```

（2）在命令处理的 if-elif-else 语句中，增加判断 command 等于 5 的分支。对于修改待办而言，类似标记待办已完成，首先打印出当前所有待办和序号，让用户输入序号选择要处理的待办。然后需要新增一个 input 函数，让用户输入该待办的新内容。之后将用户输入的新内容替换掉原先的待办的内容。增加的部分代码如下：

```
elif command == "5":
        clear_output()
        print_all_tasks(my_task_list.tasks)
        idx = input("请输入希修改的待办序号")
        idx = int(idx)
        new_text = input("请输入新的待办内容")
        my_task_list.tasks[idx].text = new_text
```
