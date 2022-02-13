---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


上一节课中，我们学习了 pandas 中两个核心的数据结构：Series 和 DataFrame，之后还学习了 DataFrame 的常见操作，比如对列、行的增删查改。

但 DataFrame 的能力远不止于此，今天我们会围绕数据分析中各种各样的查询需求，来系统性介绍 DataFrame 强大的数据查询与过滤能力。

### 使用 \[\] 查询元素

中括号\[\]， 是 pandas 中最基础的索引器。索引器是指我们提供索引，然后索引器就返回索引对应的内容。其实我们早在变量与数据类型一讲中已经打过交道。

比如一个列表 a， 我们想要访问第三个元素则可以写 a\[2\] ， 这里的 2 就是索引，\[\] 就是索引器。a\[2\] 就能为我们访问列表 a 中索引 2 对应的元素，也就是列表的第三个元素。

以字典为例的话，如果我们有如下字典： b = {"name":"小亮", "age": 24}。我们希望访问字典 b 的 age 字段，则可以写 b\["age"\]，字符串 "age" 是索引，\[\]是索引器，最终 b\["age"\] 返回了索引对应的内容：24。

DataFrame 和 Series 同样支持 \[\]. 具体的行为是：

-   对 Series 使用\[\]，返回索引对应的元素；
    
-   对 DataFrame 使用\[\]，返回列名等于索引的那一列，以 Series 的形式。
    

#### 准备数据

我们通过例子来加深一下印象。在课程目录新建文件夹 chapter13。 之后在 VSCode 中打开该文件夹，并新建一个 Notebook，保存在该文件夹中并命名为 chapter13.ipynb.

我们还是以 chapter12 中用到的部门信息为例，首先将创建部门信息 DataFrame 拷贝过来。如下所示。

```
import pandas as pd
index_arr = ["姓名", "年龄", "籍贯", "部门"]
ser_xiaoming = pd.Series(["小明", 22, "河北","IT部"], index= index_arr)
ser_xiaoliang = pd.Series(["小亮", 25, "广东","IT部"], index = index_arr)
ser_xiaoe = pd.Series(["小E", 23, "四川","财务部"], index=  index_arr)
df_info = pd.DataFrame([ser_xiaoming, ser_xiaoliang, ser_xiaoe])
df_info
```

执行之后输出：

![Drawing 0.png](https://s0.lgstatic.com/i/image6/M01/3F/37/Cgp9HWCc6BOAKc9ZAAAxYu6nngg934.png)

这样，我们实验的数据就准备好了。

#### 简单索引

接下来，我们通过实验来加深一下 \[\] 选择器的理解。

首先是对 DataFrame 使用 \[\] 索引器。

```
ser_name = df_info["姓名"]
print(type(ser_name))
print(ser_name)
```

输出为：

```
<class 'pandas.core.series.Series'>
0    小明
1    小亮
2    小E
Name: 姓名, dtype: object
```

从输出的结果可以看出。通过 \[\] 索引器和"姓名"这个索引，拿到的是一个 Series 类型。并且内容就是上一步构造的 DataFrame 中，姓名那一列对应的 Series。

对一个 Series 对象使用 \[\] 索引器，则会返回索引对应的具体数据。比如当我们希望拿索引 1 对应的数据，从上面的例子中可以看到，索引 1 对应的数据是“小亮”这个名字。我们通过代码来验证一下。

```
result = ser_name[1]
print(result)
```

输出：

代码的运行结果和我们预期的结果一致。

#### 多重索引

\[\]除了可以传入单一索引实现数据选择，还支持传入一个索引列表来获得原始数据集的一个子集。规则如下：

-   当对 DataFrame 的 \[\] 索引器传入一个索引列表时，返回一个新的 DataFrame，这个 DataFrame 只包含索引列表中的列，相当于是原 DataFrame 的子 DataFrame；
    
-   同理对于 Series，传入索引列表时，返回一个子 Series，包含索引列表对应的数据。
    

我们继续通过代码来加深理解：

```
sub_df_info = df_info[["姓名", "年龄"]]
print(type(sub_df_info))
print(sub_df_info)
```

输出为：

```
<class 'pandas.core.frame.DataFrame'>
   姓名  年龄
0  小明  22
1  小亮  25
2  小E  23
```

传入索引列表之后，结果仍然是一个 DataFrame，并且这个 DataFrame 只包含了姓名、年龄这两列。

对于 Series 而言也是类似，比如我们希望同时挑选索引为 0 和 2 的两条记录，可以用如下方式实现。

```
sub_ser_name = ser_name[[0,2]]
print(type(sub_ser_name))
print(sub_ser_name)
```

输出为：

```
<class 'pandas.core.series.Series'>
0    小明
2    小E
Name: 姓名, dtype: object
```

可以看到，结果仍然是一个 Series 类型，但只包含了两条记录，分别是索引 0 对应的小明和索引 2 对应的小 E。从另一个角度看，Series 的数字索引是可以不连续的，这个也是和列表的一个重要区别。

#### 范围选择

在学习如何使用 \[\] 进行范围选择之前，我们先给我们的 DataFrame 添加新的两条记录，方便演示功能效果。代码如下：

```
ser_xiaoli = pd.Series(["小李", 22, "云南","设计部"], index=  index_arr)
ser_xiaowang = pd.Series(["小王", 20, "福建","设计部"], index=  index_arr)
df_info = df_info.append(ser_xiaoli, ignore_index= True)
df_info = df_info.append(ser_xiaowang, ignore_index= True)
df_info
```

输出如下所示，可以看到新的记录已经添加成功了，并且被分配了默认的索引。

![Drawing 1.png](https://s0.lgstatic.com/i/image6/M01/3F/37/Cgp9HWCc6CaAcdY4AABJ-KC0cVc920.png)

现在进入正题，\[\] 索引器支持常见的范围选择有以下几种：

（1）df\[n:m\], 选择第 n 条到第 m 条之间的记录。示例代码：

```
# 取第 2 到 4 条，不包含4，也就是第 2、第 3 条记录
df_info[2:4]
```

输出为：  
![Drawing 2.png](https://s0.lgstatic.com/i/image6/M00/3F/37/Cgp9HWCc6DaANplPAAAqNY-0V_E639.png)

（2）df\[:m\], 选择前 m 条记录。示例代码：

输出为：  
![Drawing 3.png](https://s0.lgstatic.com/i/image6/M00/3F/3F/CioPOWCc6DyABSMhAAAlHDZUVOc307.png)

> 如果不写 m ， 直接写 df\[:\] 的话，代表返回所有的记录。

（3）df\[::n\], 间隔 n 条选择。示例代码：

输出为：  
![Drawing 4.png](https://s0.lgstatic.com/i/image6/M00/3F/3F/CioPOWCc6ESAbQ1XAAA0POj-X_U382.png)

输出  
![Drawing 5.png](https://s0.lgstatic.com/i/image6/M00/3F/3F/CioPOWCc6EqAEVzBAABJqBaQ4xc502.png)

（4）df\[::-1\], 从最后一条开始逐一选择。

输出为：

![Drawing 6.png](https://s0.lgstatic.com/i/image6/M00/3F/37/Cgp9HWCc6FCATJEEAABA5ywq1V0591.png)

在范围选择的应用中，Series 的用法和 DataFrame 是一致的。

至此，我们就学习完了 \[\] 选择器的常见用法。

### loc 与 iloc

在学习 \[\] 选择器的过程中，如果我们想查询 DataFrame 中某个单元格的数据，那往往都需要分三步走：

1.  查看单元格所在行的索引；
    
2.  拿到单元格所在列的列 Series；
    
3.  用 1 拿到的索引去 2 拿到的 Series 中查询出具体的数据。
    

比如我们希望打印小亮的部门，可能就需要这样做：

输出为：  
![Drawing 7.png](https://s0.lgstatic.com/i/image6/M00/3F/3F/CioPOWCc6FeAJBoRAABJXfyFJ9A998.png)

可以看到，小亮的行索引是 1，然后我们获取部门的列 Series 并进行查询：

```
dep_series = df_info["部门"]
print("小亮的部门：" + dep_series[1])
```

输出如下。

整个过程还是比较麻烦的，并且需要人肉记得我们要查询的数据所在行的索引，非常容易出错。有没有更好的方式呢？答案是肯定的。

pandas 除了 \[\] 索引器之外，还提供了一套非常强大的数据查询方式：loc 和 iloc。

#### loc 基础用法

loc 和 iloc 是 pandas 的一种特殊的对象，专门用于查询 DataFrame 中的数据。

首先是 loc，基本用法如下：

看了以上的形式，相信聪明的你已经发现了，使用 loc 对象我们可以一次性执行行索引+列索引，这样就使得定位单元格的内容可以直接一行代码就搞定。

拿上面的例子来说，如果要查询小亮的部门，用 loc 直接这么写即可：

```
print("小亮的部门：" + df_info.loc[1, "部门"])
```

输出为：

可以看到，通过 loc， 我们将之前的三步缩短成了两步，省去了先取 Series 出来的环节。

loc 对象的 \[\] 索引器支持所有 DataFrame 的 \[\] 索引器的能力。具体来说就是 loc 对象的行索引部分和列索引部分都可以分别使用我们第一部分介绍的多种索引、范围选择的语法。

举个例子来说明一下，比如我们任务是在上面的 df\_info 表中，从后往前选择每个同事的姓名和年龄两列。

上述任务规定了两个条件：一个是需要从后往前，即我们取行的时候，需要使用范围选择中学到的技术；另一个是只取姓名和年龄两列，需要用到我们之前在多重索引中学到的技术。

具体的代码如下：

```
df_info.loc[::-1, ["姓名", "年龄"]]
```

输出为：  
![Drawing 8.png](https://s0.lgstatic.com/i/image6/M00/3F/37/Cgp9HWCc6GCAN1m5AAAjUvSO9eE853.png)

#### iloc 基础用法

iloc 的用法和 loc 非常类似，区别是 iloc 仅支持传入整数索引。简单来说，loc 是需要传入行索引和列索引的名称，而 iloc 则需要传入第几行、第几列这样的数字。基本用法如下：

拿上面的例子来说，假设我们要用 iloc 对象来打印小亮的部门，可以这么做。

```
print("小亮的部门：" + df_info.iloc[1, 3])
```

输出为：

从输出来看，效果和使用 loc 对象是一样的。但是从易用性的层面，loc 显然比 iloc 更加容易使用且不容易出错。使用 iloc 每次都需要去数我们所要的数据在第几行、第几列，非常容易出错。所以一般情况下，我们都推荐直接使用 loc。但也有一些场景，不知道行索引，但明确要拿第一个元素的场合就需要使用 iloc。

总体来说 iloc 是和 loc 打配合使用的， loc 最常用。

#### 条件查询

我们回过头去看单元格查询的三个步骤，虽然通过 loc 对象，我们省去了先取 Series 再取数据的冗余步骤，但是第一步：查看小亮所在的行的索引。这一步对于我们的例子来说是很简单的，毕竟一共也就这么几行。但如果我们的表里面数据有几十万、上百万，我们不可能逐一去看小亮所在的行的索引到底是什么。

这个时候就需要用到 loc 对象的一个重要特性：条件索引。

loc 的条件索引的具体用法如下所示，它和普通的 loc 用法区别最大的就是**将行索引部分替换为条件表达式。**

比如，我们希望获得年龄大于等于 23 岁的员工的信息。使用条件查询，我们可以这样写：

```
df_info.loc[df_info["年龄"] >= 23, :]
```

输出为：  
![Drawing 9.png](https://s0.lgstatic.com/i/image6/M00/3F/3F/CioPOWCc6GmAPf2rAAAmo0Oc0So360.png)

条件表达式往往是判断 DataFrame 的某个列满足某个条件，比如是否大于或等于，等等。这样我们就不用每次都要看我们想要数据的行索引是什么，而是直接通过写合适的条件表达式就可以筛选出我们想要的数据。

拿我们最开始的任务来说，我们要查询小亮的部门，有了条件表达式，我们不再需要关心小亮所在行的行索引。而是可以这么写：

```
df_info.loc[df_info["姓名"] == "小亮", "部门"]
```

输出：

```
1    IT部
Name: 部门, dtype: object
```

可以看到，小亮的部门被成功查询出来了，而且还是在我们完全不知道他行索引的前提下。不过，我们希望查询的是一个值，但这里的结果似乎是一个 Series，这是因为**一旦在 loc 中使用了条件表达式，它返回的结果就会是 Series**，因为会存在满足条件的行有多个的情况。

在这个例子里，我们知道表中只有一个小亮，所以直接从结果 Series 取第一个就可以。这里我们不关心结果中的行索引，所以可以直接使用 iloc 取第一个即可。（Series 的 iloc 和 DataFrame 的 iloc 作用类似，即不关心索引，而是按照**第几个**这样的排序来取）

综上所述，我们通过条件查询来打印小亮的部门，代码如下：

```
print("小亮的部门：" + df_info.loc[df_info["姓名"] == "小亮", "部门"].iloc[0])
```

输出为：

#### 高级条件查询

当我们筛选数据的时候，一个条件不能满足要求，就需要组合多个条件来筛选出我们想要的数据。组合多个条件时，最常见的两个逻辑关系就是：逻辑与和逻辑或。

（1）逻辑与

假设有两个条件：A 和 B，A & B 代表逻辑与，逻辑与的意思是 A 和 B 两个条件需要同时满足，则 A & B 才算满足。

举个例子，我们希望查询 IT 部中 25 岁以下的员工信息。这里就有两个条件，一个是部分是 IT部，另一个是年龄小于 25，这两个条件需要同时满足。

```
df_info.loc[(df_info["部门"] == "IT部") & (df_info["年龄"] <25), :]
```

输出  
![Drawing 10.png](https://s0.lgstatic.com/i/image6/M00/3F/3F/CioPOWCc6HOAO85lAAAb_cqP8eI579.png)

（2）逻辑或

假设有两个条件，A 和 B，A | B 代表逻辑或，意思是 A 和 B 只需要有一个条件满足，则 A | B 就满足。

举个例子，我们希望查询出所有财务部和设计部的员工。这里有两个条件，一个是部门等于财务部，一个是部门等于设计部，只需要满足其中一个条件就需要打印出来。代码如下：

```
df_info.loc[(df_info["部门"] == "财务部") | (df_info["部门"] == "设计部"), :]
```

输出为：  
![Drawing 11.png](https://s0.lgstatic.com/i/image6/M00/3F/37/Cgp9HWCc6H2AO2BSAAA2IQXkuyk406.png)

### 小结

至此，关于 DataFrame 和 Series 的数据查询技术就已经全部讲完了，我们在这里简单地回顾一下。

首先，我们学习了使用 \[\] 来查询 DataFrame 的 Series 的内容，关键点如下。

-   针对 Series 使用 \[\]， 返回传入的索引对应的元素；针对 DataFrame 使用 \[\] ，返回传入的索引对应的列 Series。
    
-   当传给 \[\] 索引器的索引是多个索引，即一个索引列表时，DataFrame 会返回包含索引列表中指定的列的子 DataFrame，而 Series 则会返回索引列表中索引对应的元素组成的子 Series。
    
-   \[n:m\]，代表查询从 n 到 m 中间的这一段记录，不写 n 时，代表查询前 m 条数据，n 和 m 都不写时，返回查询全部数据。
    
-   \[::n\]，代表每隔 n 条返回一条，一般用于基于固定的频率采样数据集；\[::-1\] 代表从后向前逐一返回。
    

之后，我们学习了查询数据更强大的 loc 和 iloc，关键点如下。

-   loc 后接\[\] ，可以一次性传入行索引和列索引，使用逗号隔开，实现了直接取单元格的数据；行索引和列索引都遵循第一部分介绍的各种规则，如多重索引、范围选择等。
    
-   iloc 和 loc 类似，只是传入的不是索引，而是第几行、第几列这样的整数。
    
-   loc 的行索引部分可以替换为条件表达式，来实现通过条件来选择行，而不是通过固定的行索引。
    
-   条件表达式可以组合，& 代表逻辑与，| 代表逻辑或。
    

学完了数据查询，相信你目前在拿到超大 DataFrame 的时候，也有足够的技巧去进行初步的分析。在接下来的章节我会将你更多的进阶分析，来更好地应对工作中的实际项目。

课后习题：

筛选出年龄在 22~23 的员工的姓名和籍贯，不包含设计部的员工。

___

答案：

```
df_info.loc[(df_info["年龄"] >= 22) & (df_info["年龄"] <= 23) & (df_info["部门"] != "设计部"), ["姓名", "籍贯"]]
```

输出  
![Drawing 12.png](https://s0.lgstatic.com/i/image6/M00/3F/37/Cgp9HWCc6IiAVYOyAAAVGDfTKCQ340.png)
