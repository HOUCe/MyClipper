---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


上一节，我们学习了函数。这一讲，我们来学习 Python 中另外一个重要的概念：类与对象。

### 类与对象

#### 类与对象有什么关系

你可能会奇怪，为什么要叫类与对象呢？是两个不同的东西吗？简单来说，类代表一个类别，而对象则代表类的一个实例。比如我们在变量与数据类型中学习的整型变量为例。

输出结果为：

我们创建了一个变量 a，并赋值为 3，然后打印了它的类型。可以看到，输出 int，代表整型变量。  
细心的你也发现了，输出里面有一个单词 class，它就是类别的意思。难道说？所谓的整型，是一个类？你猜得没错。这里的整型：int，就是一个类。而我们创建的整型变量 a，就是他的一个实例。

我们再举一些生活中的例子：

-   “人”是一个类，小明则是“人”这个类的一个实例，所以小明是一个对象；
    
-   “打车公司”是一个类，滴滴则是这个类的一个实例对象（也可以隐藏 实例 这个称呼）；
    
-   “酱油”是一个类，海天酱油 则是一个对象；
    
-   “车”是一个类，奔驰车则是一个对象。
    

细心的你不难发现，类与对象本质上是抽象与具象的关系，对象在类的基础上进行了适当的具象。所以在某个抽象关系中的对象也可能会成为另一个抽象关系中的类。比如上面奔驰车是车的一个对象，那同样可能存在，奔驰车是一个类，而 S350L 是一个对象。

#### Python 中的类

理解了类与对象，现在我们来看一下 Python 中的类，我们在开头的例子中提到，类是用来把有联系的数据和函数给组织起来的一种方式。

在类中，**数据被称为属性，而函数则被称为方法。每个类都可以有零个或者多个属性，也可以有零个或者多个方法。**

类的属性和方法的概念也很适合用来描述现实世界，比如“人”这个类，身高、体重、年龄等就是属性，而走路、吃饭、打球 等动作就是方法。

接下来，我们通过一个例子来演示怎么定义类，怎么使用类。

#### 定义一个“人”的类

现在我们尝试定义一个人的类。需要有年龄、性别、姓名三个属性，并提供两个方法：introduce 方法，打印一句介绍自己的话。get\_age 方法，返回当前对象的 age。

```
class Person:
def __init__(self):
        self.name = ""
        self.age = 0
        self.gender = ""
def introduce(self):
        print("Hello, 我是" + self.name)
        print("我今年 " + str(self.age) + " 岁")
        print("另外, 我是" + self.gender)
def get_age(self):
return self.age
xm = Person()
xm.name = "小明"
xm.age = "25"
xm.gender = "男生"
print(xm.get_age())
xm.introduce()
```

简单说一下上面的代码的主要逻辑，你主要看代码就好。

-   在定义环节：
    
    -   定义了一个类：Person（类名首字母用**大写**），并用 **init** 函数中初始化了三个属性：name，age，gender。（**init** 函数，前后都有两个下划线，代表**初始化函数**。简单来说就是，当我们创建这个类的对象的时候，这个函数会被自动执行，不用你再shift+enter。）
        
    -   定义了 introduce 方法，会把几个属性的值组合成一段自我介绍，这里我们通过方法参数 self.属性名 的形式来拿到类的属性的值。这种形式叫作\*\*点语法。\*\*点语法的本质就是找点前面的对象拿点后面的属性的值。比如 A.B 代表 A对象的B属性的值。
        
    -   定义了 get\_age 方法，把当前对象的 age 对象的值返回。可以看到，无论是 introduce 方法 还是 get\_age 方法，都有一个 self 参数。这是 Python 的语法规定，**类的方法的第一个参数都必须是 self**。这样方法内部就能通过对 self 使用点语法来获取属性的值。
        
-   使用环节：
    
    -   注意从我们使用的类的代码开始，我们这里就没有缩进。因为有缩进就会被认为还是在类的内部，但这里的代码是在外部的，所以不用缩进；
        
    -   我们创建了一个 Person 对象，存在变量 xm 中，之后逐一给它的属性赋值
        
    -   打印 xm 对象的 get\_age 方法返回的值
        
    -   调用 xm 对象的 introduce
        

运行后，输出如下所示：

```
25
Hello, 我是小明
我今年 25 岁
另外, 我是男生
```

可以看到，通过类的机制，我们成功地把数据和函数绑定在了一起。比如上面例子的数据：name、age、gender 和函数：get\_age、introduce。并且类提供了一种机制（self 参数），能够让类的方法可以访问类的属性。

#### 给属性赋值

回看刚刚的例子，我们创建了 Person 类的对象之后，逐一对他的属性赋值，每一次属性赋值都需要针对 xm 对象使用点语法，比较麻烦，有没有更好的方式呢？ 答案是肯定的。

我们稍微改造一下刚才的例子，改造后的代码如下所示：

```
class Person:
def __init__(self, name, age, gender):
        self.name = name
        self.age = age
        self.gender = gender
def introduce(self):
        print("Hello, 我是" + self.name)
        print("我今年 " + str(self.age) + " 岁")
        print("另外, 我是" + self.gender)
def get_age(self):
return self.age
xm = Person("小明", 25, "男生")
print(xm.get_age())
xm.introduce()
```

执行之后，输出和刚才是一致的。

```
25
Hello, 我是小明
我今年 25 岁
另外, 我是男生
```

不同在哪儿呢？是这里。

![Drawing 0.png](https://s0.lgstatic.com/i/image6/M00/37/96/CioPOWB35-qAUd1zAAdSWJhKJGY565.png)

现在我们不需要进行逐个属性的赋值，而是在构造对象的阶段就完成了几个属性的初始化。

这其中的奥妙就在 **init** 方法里，我们给 **init** 函数加了参数，然后用这些参数直接在 \_\_init\_\_的函数体中给我们的属性赋值。这样直接在创建对象的时候，把对应的值依次放在类名后的括号中，用逗号分隔。就能实现一次性地给属性都赋值，精简了代码。

### 常见的系统类

类和函数一样，类同样也有 Python 的开发者们提前写好提供给我们使用的类，一般称为系统类。上面举的例子都是我们自己实现的类，本节会重点介绍常用的系统类。

#### 列表(list)

列表相信大家都不陌生，在变量与数据类型一章已经介绍过，它是 Python 非常常用的数据类型。

这里介绍一下列表的基础使用，代码如下：

```
a = [90,1,23, 15]
print("Third number is ",a[2])
a.append(-1)
print("After append: ", a)
del a[1]
print("After delete second number: ", a)
a.remove(15)
print("After delete 15：", a)
a.sort()
print("After sort", a)
a.sort(reverse = True)
print("After reverse sort", a)
print("Length: ", len(a))
```

上面主要方法都有注释，这里不再展开，你可以结合以下输出和代码，加深理解。重要的是你要知道，如果之后让你做某个事（删除、排序等），你能直接使用 list 类对应的方法。而不需要自己写循环完成。

```
Third number is  23
After append:  [90, 1, 23, 15, -1]
After delete second number:  [90, 23, 15, -1]
After delete 15： [90, 23, -1]
After sort [-1, 23, 90]
After reverse sort [90, 23, -1]
Length:  3
```

#### 字符串(string)

字符串我们用的也是非常多的，在之前也介绍过一些基础用法，比如用 + 号来连接两个字符串，这里我们再介绍额外的一些用法。

```
a = "Hello,this is my home,welcome"
a = a + ",pp"
print("\nAfter append pp:\n", a)
a = a[3:]
print("\nAfter remove first 3 characters:\n", a)
a = a.replace("my","")
print("\nAfter remove 'my':\n", a)
a = a.replace("home", "company")
print("\nAfter replace 'home' to 'company':\n",a)
str_list = a.split(",")
print("\nList split from string by comma:\n", str_list)
```

输出

```
After append pp:
 Hello,this is my home,welcome,pp
After remove first 3 characters:
 lo,this is my home,welcome,pp
After remove 'my':
 lo,this is  home,welcome,pp
After replace 'home' to 'company':
 lo,this is  company,welcome,pp
List split from string by comma:
 ['lo', 'this is  company', 'welcome', 'pp']
```

#### 字典(dict)

字典和列表、字符串一样，也是 Python 中相对常用的数据类型，同样也是系统类。因为复杂一些，所以在变量与数据类型一节没有介绍。

字典和列表类似，也是存储多个变量的容器。但与列表不同的是，字典存储的不仅仅只有变量，还有变量之间的映射关系。

我们举个例子，假设我们要存储部门里三位技术顾问的年龄，可以用列表来完成，比如 a = \[30, 45, 50\], 但如果我们不仅要存储年龄，还得存储这三位工程师的名字。那用列表就无能为力了。

简单来说，我们希望存储一个名字-年龄的映射关系。在这个例子上就是要存储这种形式的数据：王叔：30，张叔：45，李叔：50。在 Python 中，这种有对应关系的两个变量我们称之为键值对。比如王叔就是 键(key)，而 30 就是值（value），一个键会唯一对应到一个值。

这里我们希望存储的就是三个键值对。而字典，就是专门用来存储键值对的容器。

我们以下面的例子来演示字典的使用，结合列表的例子可以帮助你快速理解。

```
d = {}
print("Empty dict:", d)
d = {"xiaoming" : 3.5 , "xiaohong": 4}
print("Two key-value pair dict:", d)
print("Xiaoming's company age:", d["xiaoming"])
d["xiaogang"] = 5.5
print("After append xiaogang:", d)
del d["xiaohong"]
print("After remove xiaohong:", d)
d["xiaoming"] = 9.3
print("After change xiaoming's value:", d)
```

输出

```
Empty dict: {}
Two key-value pair dict: {'xiaoming': 3.5, 'xiaohong': 4}
Xiaoming's company age: 3.5
After append xiaogang: {'xiaoming': 3.5, 'xiaohong': 4, 'xiaogang': 5.5}
After remove xiaohong: {'xiaoming': 3.5, 'xiaogang': 5.5}
After change xiaoming's value: {'xiaoming': 9.3, 'xiaogang': 5.5}
```

### 实战类与对象

通过一段时间的学习，你成功拿到了阿普尔星球最大的电商网站：阿普闪购的 OFFER！成为一名助理数据分析师。入职第一天，你的 mentor 交给你一项任务：记录公司部门的信息，每个部门都需要统计部门名称、员工列表、部门主管的姓名等信息。另外还有两个要求：

-   部门员工入职和离职都能方便的更新信息；
    
-   可以方便地查看某个部门的汇总信息。
    

问题分析：

-   目前我们需要记录的信息都是围绕部门这个实体的，所以我们可以用一个部门的类来记录，部门名称、员工列表和部门主管则都是这个类的属性。
    
-   另外的要求是有人员变动的时候方便更新信息，那其实就是部门类需要提供增加人员和减少人员的方法。
    
-   最后一个要求是方便查看部门汇总，那其实就是需要一个打印方法，来把部门的属性都打印出来。
    

我们接下来一步步来实现它。

#### （1）创建部门类

我们首先创建部门类和它的属性，部门名称和主管姓名，是字符串类型的变量，这两个属性我们通过初始化函数的参数来初始化。而员工列表是一个列表类型，我们先把它初始化成一个空列表。

```
class Department:
def __init__(self, dep_name, boss_name):
        self.dep_name = dep_name
        self.boss_name = boss_name
        self.stuff_list = []
```

其中：

-   dep\_name 是部门的名字
    
-   boss\_name 是部门主管的名字
    
-   stuff\_list 是员工列表，我们将其初始化为一个空列表。
    

要创建一个部门类的对象，只需要在类名后面传入具体的部门名和主管名即可。比如：

```
it_dep = Department("it部", "小明")
```

#### （2）实现增加员工的方法

目前员工列表，也就是 stuff\_list 属性还是空列表，为了实现往这个列表增加员工的名字，我们需要在部门类增加一个添加员工的方法，我们就命名为 add\_stuff。这个方法除了类方法必须带的 self 参数之外，只有一个参数：要添加的员工的姓名。

然后方法里面只需要把这个姓名添加到 stuff\_list 即可，添加数据到列表，只需要调用列表对象的 append 方法即可。

根据上述分析，我们给 Department 类增加 add\_stuff 方法。添加方法之后的类代码如下所示：

```
class Department:
def __init__(self,dep_name, boss_name):
        self.dep_name = dep_name
        self.boss_name = boss_name
        self.stuff_list = []
def add_stuff(self, name):
        self.stuff_list.append(name)
```

#### （3）实现删除员工的方法

部门会新增员工，比如员工入职，或者调入。也可能会减少员工，比如员工离职，比如转岗去其他部门，所以我们同样需要一个删除员工的方法。

类似添加员工的设计，删除员工的方法同样需要员工姓名的参数，在方法内部调用 stuff\_list 对象的 remove 方法来将员工从列表中移除。

添加之后的类代码如下所示：

```
class Department:
def __init__(self,dep_name, boss_name):
        self.dep_name = dep_name
        self.boss_name = boss_name
        self.stuff_list = []
def add_stuff(self, name):
        self.stuff_list.append(name)
def remove_stuff(self, name):
        self.stuff_list.remove(name)
```

#### （4）添加打印部门信息的方法

打印部门信息，就是将部门的三个属性直接打印出来，比较简单，这里我们直接实现：

添加之后的类代码如下所示

```
class Department:
def __init__(self,dep_name, boss_name):
        self.dep_name = dep_name
        self.boss_name = boss_name
        self.stuff_list = []
def add_stuff(self, name):
        self.stuff_list.append(name)
def remove_stuff(self, name):
        self.stuff_list.remove(name)
def print_dep_info(self):
        print("部门：",self.dep_name)
        print("主管：",self.boss_name)
        print("员工：",self.stuff_list)
```

#### （5）使用部门类

现在，我们的部门类已经开发完毕了，现在让我们来用一用它，看看它是不是好使。

首先是记录部门信息，假设现在先记录2个部门：it部和财务部。首先要分别创建两个部门的对象：

```
it_dep = Department("it部", "小明")
finance_dep = Department("财务部", "红姐")
```

然后，通过调用部门对象的 add\_stuff 方法录入员工的名字

```
it_dep.add_stuff("小亮")
it_dep.add_stuff("小七")
finance_dep.add_stuff("小E")
finance_dep.add_stuff("小洁")
```

现在，我们部门数据的记录就完成了，可以通过调用部门对象的 print\_dep\_info 来查看部门的详细信息，如查看 It 部的信息：

```
it_dep.print_dep_info()
finance_dep.print_dep_info()
```

执行后输出

```
部门： it部
主管： 小明
员工： ['小亮', '小七']
部门： 财务部
主管： 红姐
员工： ['小E', '小洁']
```

最后，当有人需要离职时，只需要调用 remove\_stuff 方法即可，假设 it 部的小七决定去创业，要离职，我们需要将他从部门列表中移除。

```
it_dep.remove_stuff("小七")
```

为了查看是否成功移除，我们再次调用打印部门信息的方法。

输出

```
部门： it部
主管： 小明
员工： ['小亮']
```

可以看到，小七已经不在员工列表中了。

至此，我们通过类与对象的方法，完成了部门信息统计的任务，并且可以非常方便地处理员工增加和减少的场景。

### 小结

总结一下，今天我们学习了 Python 除函数之外另一个重要的概念：类与对象。类通过属性和方法来将数据和代码组织在一起，对象则是类的实例。

举例来说，“人”是一个类，对于这个类而言：身高、体重、年龄是属性，走路、吃饭、睡觉是方法。一般来说名词多半是属性，而动词多半是方法。“小明”则是人这个类的一个对象。

之后，我们学习了 Python 中类的定义，主要有以下几个关键点：

-   所有方法都有 self 参数，一方面这是 Python 的语法规则，另一方面方法中访问属性也需要用到 self；
    
-   在 **init** 方法中，需要给属性赋值，避免后续使用属性的时候属性还没有值，导致程序出错；
    
-   可以给 **init** 方法增加参数，来让构造对象的时候就传入初始化属性的值，这样就可以避免在创建对象之后还逐个属性赋值，造成代码的冗余；
    
-   使用类时，通过 对象 = 类名(参数...) 的形式构造对象，这是 Python的语法规则，照做就好；
    
-   通过点语法来访问对象的属性或者执行对象的方法，同样，这也是 Python 的语法规则。
    

然后，我们学习了 Python 常见的三个系统类的基本用法。

-   列表：按顺序存储多个元素的类。
    
-   字符串：按顺序存储多个字符的类。
    
-   字典：存储对应关系（键值对）的类。
    

最后，我们通过一个统计部门信息的实战案例，练习了类与对象的使用方法和技巧。

至此，我们整个 Python 语言的理论知识部分就学习完毕啦！ 恭喜你，下一章，我们即将进入实战 Python 语言的环节，你是否已经摩拳擦掌了呢！别慌，先完成课后练习。

#### 课后练习

编写一个车的类，包含属性：公里数、油耗、马力。

包含方法：

-   introduce，打印出车辆的基本信息
    
-   print\_eco\_info, 打印环保信息，如果油耗大于 10，则打印不及格，大于 6 小于等于 10，则打印及格，小于等于 6 则打印优秀
    

___

答案：

```
class Car:
def __init__(self, miles, fuel, horsepower):
        self.total_miles = miles
        self.fuel = fuel
        self.horsepower = horsepower
pass
def introduce(self):
        print("我的里程：", self.total_miles)
        print("我的油耗：", self.fuel)
        print("我的马力：", self.horsepower)
def print_eco_info(self):
if self.fuel > 10 :
            print("不及格")
elif self.fuel > 6:
            print("及格")
else:
            print("良好")
car1 = Car(14000, 11, 200)
car2 = Car(2000,5, 175)
car1.introduce()
car1.print_eco_info()
print("")
car2.introduce()
car2.print_eco_info()
```

输出

```
我的里程： 14000
我的油耗： 11
我的马力： 200
不及格
我的里程： 2000
我的油耗： 5
我的马力： 175
良好
```
