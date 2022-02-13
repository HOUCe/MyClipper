---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=482#/detail/pc?id=4660
author: 
---

# [测试开发入门与实战 - 测试专家，VIPTEST社区联合创始人 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=482#/detail/pc?id=4660)


Python 语言是一门动态的、面向对象编程的语言，它凭借入门简单、功能强大等优势，受到越来越多开发人员的追捧，已成为一门长期霸榜前三位的热门编程语言。

它的语法非常简洁，同样的功能，相比 Java 等老牌编程语言，Python 花费更少的代码行数便可将其实现；对初学者也非常友好，它的代码可读性和可调试性很强，在复杂情况下，初学者也可以将重心放在编程对象和解决问题的思维方法上，而不必去过多关心语言的语法和类型。

所以，在转型测试开发过程中，你必须掌握好 Python 这一编程语言。

### Python 安装

Python 的最新版本是 3.8.3. 你可以点击进入 [Pyhton 官网](https://wiki.python.org/moin/BeginnersGuide/Download)下载安装包直接安装。

如果你当前系统用的是 Python 2，那么你需要安装最新版本 Python 3，安装完毕后，你可以通过输入以下命令行，来查看你系统的默认版本号。

如果发现显示的是 Python 2.x, 你可以通过如下方式更改默认值。

#### 1.如果你的电脑是 Windows 系统

-   通过“Windows + R”快捷键组合打开运行；
    
-   输入“sysdm.cpl”；
    
-   点击“环境变量（N）”，在弹出的对话框中，找到“系统变量”；
    
-   选中“Path”，并将 Python 的路径更改为 Python3 这一安装路径；
    
-   再次在命令行输入“python”查看输出的版本号。
    

#### 2.如果你的电脑是 MacOS 系统

-   打开 Terminal， 在命令行中输入“which python 3”，  
    你将看到如下类似输出 /usr/local/bin/python 3；
    
-   在 Terminal 继续输入“open ~/.bash\_profile ” ，并修改文件如下 alias python="/usr/local/bin/python3"；
    
-   保存并关闭文件，然后运行如下命令  
    source ~/.bash\_profile；
    
-   在 Terminal 里继续输入命令行入“python”，查看输出的版本号。
    

为了更有效率地开发，Python 安装好后，你还需要点击进入 [Pycharm 官网](https://www.jetbrains.com/pycharm/)，下载并配置集成开发环境。

### Python 标准数据类型

不像其他语言，Python 中的定义变量无须进行类型声明。Python 的标准数据类型有：Numbers（数字）、String（字符串）、List（列表）、Tuple（元组）和 Dictionary（字典）。

下面举个小例子来看下这几种类型的用法：

```
total_num = 100
welcome_words = "欢迎来到蔡超的测试开发课"
student_list = ["Kevin", "Emily", "Ray"]
unique_student = ("Kevin", "Emily", "Ray")
course_rank = {"math": "Kevin", "logic": "Emily", "English": "Ray"}
```

在 Python 中，List 和 Dictionary 分别有很多种内置用法，在此介绍其中最常用的部分。

#### 1.List 常用操作

```
语法：len（list）
举例：len(list1)
语法：list[N]
举例：list1[0] 
语法：list.append（x）
举例：list1.append（'Ray'）
语法：list.extend(x)
举例：list1.extend([2,3,4])
     print(list1) 
语法：list.insert(i, x) 
举例：list1.insert(0, 'Ray') 
语法：list.remove(x)
举例：list1.remove('Kevin')
语法：list1.pop(i)
举例：list1.pop(0) 
语法：list.clear()
举例：list1.clear()
语法：list.count(x)
举例：list1.count("Kevin")
语法：list.sort(key=None, reverse=False)
举例：list1.sort(reverse=true) 
语法：list.reverse()
举例：list1.reverse() 
```

#### 2.Dictionary 常用操作

```
dict1 = {"math": "Kevin", "logic": "Emily"}
dict1.get("math")
my_dict.get("English", "Ray")
dict1.keys()
dict1.values()
for k, v in dict1.items():
    print('{key} -- {value}'.format(key=k, value=v))
dict1["math"] = "Ray" 
del dict1["math"] 
del dict1
```

### Python 控制流

控制流非常重要，你的代码要如实表现业务逻辑，就必须掌握控制流，控制流中最常见的是分支语句和循环。

在 Python 里，常用的控制流关键字如下。

#### 1.while 循环

while 循环的作用在于，当某个条件成立时，一直执行循环语句， 直至循环条件不成立为止。

```
while 判断条件(condition)：
    执行语句(statements)
    ……
```

![3.png](https://s0.lgstatic.com/i/image/M00/53/FF/CgqCHl9ojIaAUDXOAADml4yLrEM692.png)

while 循环图

当你无法确定循环的次数时，通常使用 while 循环指定一个循环执行条件。

#### 2.for 循环

当我们需要对列表、字典等进行遍历操作时，我们通常会用到 for 循环。

```
for iterating_var in sequence:
   执行语句(statements)
   ……
```

![4.png](https://s0.lgstatic.com/i/image/M00/53/F4/Ciqc1F9ojH2AVe_4AADcELueers072.png)

for 循环图

当对列表、字典进行遍历时，或者当你能确定循环的次数时，通常使用 for 循环。

#### 3.if 语句

if...else...语句用处非常广泛，当业务逻辑需要判断某个条件是否成立时，就可以用 if...else...语句。

```
if 条件：
  执行条件语句A(statements)
else：
  执行条件语句B(statements)
```

![5.png](https://s0.lgstatic.com/i/image/M00/53/F4/Ciqc1F9ojE6AXnwQAACxphJ6AFY080.png)

if 语句图

#### 4.控制流应用案例

了解了 Python 的控制流后，下面直接看个例子加深下印象，我建立了一个名为 test.py 的文件，内容如下：

```
my_students = ["Kevin", "Emily"]
course_rank = {"math": "Kevin", "logic": "Emily", "English": "Ray"}
if __name__ == "__main__":
    my_students_rank = {}
for k, v in course_rank.items():
if( v in my_students):
            my_students_rank[v] = k
    p_len = len(my_students_rank)
while(p_len >0):
for p in my_students_rank.keys():
            print('{person} are good at {course}'.format(person=p, course=my_students_rank[p]))
            p_len -= 1
```

那么这段代码是什么含义呢？我来逐句解释一下：

```
my_students = ["Kevin", "Emily"]
course_rank = {"math": "Kevin", "logic": "Emily", "English": "Ray"}
if __name__ == "__main__":
    my_students_rank = {}
for k, v in course_rank.items():
if( v in my_students):
            my_students_rank[v] = k
    p_len = len(my_students_rank)
while(p_len >0):
for p in my_students_rank.keys():
            print('{person} is good at {course}'.format(person=p, course=my_students_rank[p]))
            p_len -= 1
```

我们来执行下这段语句：

可以看到如下结果：

```
Kevin is good at math
Emily is good at logic
```

了解了 While 语句、for 语句、if...else...语句，你就可以使用代码来表述真实业务场景了。

但你会发现这些代码完全是流水账似的，没有函数也没有模块，这显然不符合代码规范。那么，我们就必须学习下模块和函数。

### 函数、模块、包

上面我们讲了 Python 里的基本语法语句的使用，下面我们看下这些基本语法语句是如何被使用的。通常我们的代码为了方便调用，都会以函数、模块、包等形式存在。

#### 1.函数

函数就是能实现一定功能的代码语句的集合。在 Python 中定义函数很简单：

跟其他语言一样， Python 函数定义同样支持无形参、有形参、可变参数等；而函数可以有返回值，也可以没有返回值。下面我们来一一学习：

-   **无形参—不需要参数输入**
    

```
def print_log():
    print(''' Welcome to Kevin's class ! ''')
if __name__ == "__main__":
    print_log()
```

-   **有形参—函数接受用户参数**
    

```
def is_true(x):
return x >0
def min_number(x, y):
if x>=y:
       x,y = y, x
return x 
def sum_number(*args):
    total = 0
for k in args:
        total +=k
return total
def count_student(**kwargs):
for k, v in kwargs.items():
        print('{0} - {1}'.format(k, v))
if __name__ == "__main__":
    total = sum_number(1, 2)
    print(total)
    min = min_number(1, 2)
    print(min)
    count_student(math='kevin', logic='emily')
```

#### 2.模块

模块是为了编写可维护的代码，而把函数分组放到不同文件里的行为。在 Python 中，一个 .py文件 就是一个模块，一个模块可以包括一个或多个功能，模块又可以被一个或多个其他模块引用。

使用模块的好处很多， 我讲典型的两个：

-   **既提高了编程的效率，也增强了代码的可维护性。**
    

把模块导入当前模块，当前模块即可拥有模块已经实现的功能。如果模块的功能本身需要更改，我们只需要更改模块定义的地方即可，其他地方都无须更改。

-   **不同模块的函数名和变量名可以重名。**
    

有了模块，避免了函数名和变量名之间的冲突，例如如下的文件结构：

```
myproject
  |--module1.py
  |--module2.py
```

假设我在 module1.py 里和 module2.py 里，同时定义一个名字为 take\_picture() 的函数。这两个不同模块的函数虽然都叫 take\_picutre，但其行为可以不相同，也不会相互影响。

看到这里你可能会问，那如果模块名也相同怎么办呢？

#### 3.包

为了解决这个问题，Python 又定义了包（Package）。包就是一个目录文件，它必须包含一个名为 \_\_init\_\_.py 的文件。

如下就是一个包结构，在一个包里，不同层级目录下可以包含名字相同的模块。

```
myproject
  |-- web
    |-- module.py
    |-- __init__.py
  |-- API
    |-- module.py
    |-- __init__.py
  |-- __init__.py
```

你可以看出，在 web 层级和 API 层级它们都包含着名字相同的模块 module.py，以下列出不同包下的模块引用方式：

```
from web.module import Module
from API.module import Module
```

可以看出来，函数、模块、包的作用是把代码模块化，方便我们调用和写出更高效的编写代码。

### 模块的导入

有了函数、模块和包， 客观上我们就可以写出符合规范的代码。

那么，一个模块是如何被其他模块调用的呢？有如下三种方式：

#### 1\. 直接导入模块

采用这种方式导入后，就可以直接使用

这一方式来调用 take\_picture 函数，如果 module1 里含有其他函数，在 module1 被导入后，均可以通过 module1.xxx() 的方式来使用。

#### 2\. 采用 from...import 方式导入

有时候我们并不想把一个模块的所有功能都导入进来，假设我只想使用 take\_picture 这一个方法，那么我可以使用 from...import 的方式：

```
from module1 import take_picture
```

采用这种方式导入后，如果我要使用 take\_picture 函数，我可以直接在代码里以如下方式使用：

可以注意到，这种情况我就不必要写模块名字了。

但假设这个模块里还有个 xxx 函数，我在没有导入的情况下，是无法直接通过 module1.xxx() 或者 xxx() 这样的方式使用的。

#### 3\. 采用 from...import\* 方式导入

如果你想一次性地导入一个模块下的所有函数， 你可以使用如下方式：

采用这种方式导入后，你可以直接使用这个模块下的所有函数。

#### 4\. 动态导入

上面介绍的三种导入方式都属于静态导入，这个很好理解。

但在实际应用中，也会有在程序运行时才知道要具体导入哪个模块的情况（例如，测试框架自动查找测试用例并导入测试用例所属的模块），这时就需要动态导入。

动态导入常常用 importlib 来完成，常用的动态导入有以下两种方式。

-   **从模块直接导入**
    

```
import importlib 
mod = importlib.import_module（ "a.b"）
```

-   **根据模块名，文件名导入**
    

```
import importlib.util
spec = importlib.util.spec_from_file_location("a.b", "/path/to/file.py")
md = importlib.util.module_from_spec(spec)
spec.loader.exec_module(md)
```

这个方式比较常用。下面我来举个具体的例子， 假设现在我们的项目目录情况如下:

```
myproject
  |-- tests
    |-- a.py
    |-- __init__.py
  |-- b.py
```

在模块 a.py 里，我定义了一个函数：

```
def hello():
    print('i am module a!')
```

现在，我想在模块 b 中使用 hello() 这个函数， 要怎么操作呢？

```
import os
import glob
import importlib.util
def find_modules_from_folder(folder):
    absolute_f = os.path.abspath(folder)
    md = glob.glob(os.path.join(absolute_f, "**/*.py"))
return [(os.path.basename(f)[:-3], f) for f in md if os.path.isfile(f) and not f.endswith('__init__.py')]
def import_modules_dynamically(mod, file_path):
    spec = importlib.util.spec_from_file_location(mod, file_path)
    md = importlib.util.module_from_spec(spec)
    spec.loader.exec_module(md)
return md
if __name__ == "__main__":
    module = find_modules_from_folder('.')
for m in module:
        mod = import_modules_dynamically(m[0], m[1])
        mod.hello()
```

这个代码有点复杂，我先给定一个文件夹，然后通过函数 find\_modules\_from\_folder 来得到这个文件夹下的模块，及其对应的文件路径，然后我再通过 spec\_from\_file\_location 来动态加载。

### 小结

本节课我主要介绍了 Python 的一些基础编程知识，其中涉及变量、函数、控制流的定义，及其相关操作，最后介绍了模块的导入，这些知识是你使用 Python 编程时的必会知识，非常助于你后续解锁更复杂的操作。

好了，本节的内容就到这里，我希望你仔细研读本节内容，特别是对于 [Python 标准库](https://www.python.org/dev/peps/pep-0008/)和 Python 的基础语言的掌握，一定要多学多练，这样你才能跟上下一课时“05 | 告别 CURD，拥抱 Python 高阶编程”的步伐~。

___

[“测试开发工程师名企直推营” 入口，免费领取 50G 资料包](https://shenceyun.lagou.com/t/eka)
