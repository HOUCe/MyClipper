---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


在上一节课中，我们学习了如何使用 pandas 的函数来从多种数据源：csv、excel 和 html 网页。其中不管是哪一种数据读取的方式，最终返回的都是一个 DataFrame 对象。

对于 DataFrame 对象，上一讲我们只是简单将其打印出来，这一讲我们来学习围绕 DataFrame 的基本操作（添加行、列，删除行、列，排序等），除了 DataFrame，本讲我们也会学习另外一个重要的 pandas 数据结构: Series。

我们首先介绍 pandas 中的三个最常见的概念：index、Series 和 DataFrame。

### 数据的“目录”： index

index 也叫索引，索引是计算机科学中非常常见的概念，你可能听起来会有点陌生，但其实应该很早之前就打过交道了。比如看一本书，书的目录就是书本内容的索引。所以通俗意义上，索引可以理解为就是存储了如何访问某块数据方式的数据。拿目录的例子来说，目录本身也是数据，但这个数据的内容是如何访问另一块数据（书的正文）。

在我们之前的学习中，我们也或多或少和索引打过交道。比如我们通过列表的下标访问列表的某个元素，比如 a\[5\] ，这个 5 也叫列表的索引。字典的场景中，我们通过 key 来访问字典中的某个元素，比如: student\["name"\]，这个 "name" 的字符串，也属于字典的索引。

### 一维数据序列：Series

Series 本身是一种数据类型，很像我们之前打过交道的列表，是存储多个数据元素的容器。事实上，我们也可以直接使用一个列表来创建一个 Series。但与列表不同的是，Series 一般由两部分组成：index 和 values。

-   values 容易理解，顾名思义就是存储了 Series 里面的所有元素的值，所以 values 部分可以认为就是和列表是等价的。
    
-   index 部分代表代表 Series 的**索引**。根据上面对于索引的定义，index 部分的数据就是为了能方便地定位到 values 里面的数据。
    

Series 有一个单独的索引项，这就使得它既支持类似列表一样的数字索引，也支持类似字典一样的用字符串或者其他 Python 对象来做索引。对于 Series，可以简单理解成是一个列表和字典的集合体。

接下来我们用几个实例来简单介绍一下 Series 的概念。

#### （1）直接从列表创建 Series

新建 chapter12 文件夹，打开 VS code 打开该文件夹，然后新建新的 notebook，命名为 chapter12.ipynb ，并保存到上述文件夹。

```
import pandas as pd
ser1 = pd.Series([1,3,5,7])
ser1
```

输出显示如下：

```
0    1
1    3
2    5
3    7
dtype: int64
```

输出有两列，第一列是 index，第二列是 values， values 就是我们传入的列表，而 index 则是对应的序号。当我们只使用列表来创建 Series 对象时，会生成默认的索引。即类似列表那样，每一个元素的位置作为索引。Series 对象也具备 index 和 values 属性，这样我们可以单独访问这两个部分。

```
print("values: ", ser1.values)
print("index: ", ser1.index)
```

从以下输出结果可以看到，values 其实就是我们传入的列表，而 index 则是一个 RangeIndex 的对象。

```
values:  [1 3 5 7]
index:  RangeIndex(start=0, stop=4, step=1)
```

对于这个 Series 对象，我们可以通过 index 的值来获得对应 values 里面的值。比如 index 等于1 对应的是 values 里面的 3。在代码中想要获得 1 对应的值就可以这样：

输出

#### （2）创建 Series，并指定索引

在上面的例子中，我们直接从一个列表创建了 Series，Series 为其分配了默认的 index，即元素在列表中的位置作为其对应的 index，比如 5 是列表 1,3,5,7 中的第三个数字，则它的 index 就是 2（从 0 开始数起）。

除了这种方式，Series 还支持我们在创建的时候指定对应的索引。

```
ser2 = pd.Series([1,3,5,7], index=["a", "b", "c", "d"])
ser2
```

输出为：

```
a    1
b    3
c    5
d    7
dtype: int64
```

可以看到，我们指定了一个由 4 个字符串的列表作为数字列表的索引。两个列表的元素是一一对应的关系，比如字符串 "a" 就是数字 1 的索引，字符串 "b" 就是数字 3 的索引。我们可以验证一下。

输出如下，可以看到我们通过索引 "d" ，确实拿到了数字 7。

现在我们来看一下 ser2 的 values 和 index 的值

```
print("values:", ser2.values)
print("index:", ser2.index)
```

输出

```
values: [1 3 5 7]
index: Index(['a', 'b', 'c', 'd'], dtype='object')
```

values 和第一个例子一样，但 values 这次就不是 RangeIndex 对象了，而是一个 Index 对象，里面保存了我们传入的作为索引的字符串数组。

总结一下， Series 可以看成是高级的列表或者字典，当不指定 index 的时候，Series 会生成默认的位置索引，这样的 Series 就像是一个列表。而当我们指定了 index 之后，则可以通过 index 列表中的元素来访问对应的 values 中的元素，就像字典的 key-value 结构一样。

整体来说，**Series 通过将 index 和 values 分别存储的机制，实现了列表和字典的结合。**

### 二维数据表：DataFrame

学习完了 Series，现在我们来看上一讲经常出现的 DataFrame。在上一讲的学习中，我们都是从各种文件中加载数据，之后直接存储为 DataFrame。这个部分我们来一步步地揭开 DataFrame 的神秘面纱。

#### （1）DataFrame 的组成

根据上一讲我们学习的内容，DataFrame 是一个由行和列组成的二维表格。相信你已经猜到了，DataFrame 其实就是由 Series 组成的，DataFrame 的某一行，或者某一列都是一个 Series。

口说无凭，我们通过代码来确认一下。将上一讲中的 tv\_rating.csv 文件拷贝到 chapter12 目录。并新建 Cell， 输入如下代码：

```
df_rating = pd.read_csv("tv_rating.csv")
df_rating
```

输出为：  
![Drawing 0.png](https://s0.lgstatic.com/i/image6/M00/3F/3E/CioPOWCc5n2AWoiKAAIKRi7SPjI316.png)

在上述表格中，无论是列（比如标题、评分）或者行（比如第二行、第三行）都是 Series。其中 title、rating、stars 则是列的索引。而 0、1、2...3599 是行的索引。

我们可以通过列名作为索引，来单独输出某一列，比如评分这一列。

```
ser_rating = df_rating["rating"]
print(ser_rating)
print("------------分割一下------------")
print(type(ser_rating))
```

输出结果为：

```
0       3.7分
1       4.0分
2       4.6分
3       3.4分
4       4.4分
        ... 
3595    4.0分
3596    4.0分
3597    1.0分
3598    2.0分
3599    4.6分
Name: rating, Length: 3600, dtype: object
------------分割一下------------
<class 'pandas.core.series.Series'>
```

可以看到，分数的这一列被打印出来，格式和上面学习的 Series 是一样的，左边是 index，右边是 values。之后我们通过 type 函数获得了 ser\_rating 变量的类型，确实也是 Series。

除了输出某一列，我们还可以用行索引，来单独输出某一行。比如我们输出第二行：刘老根第三部，对应的索引是 1。我们可以用如下的代码获取这一行。

```
ser_1 = df_rating.loc[1]
print(df_rating.loc[1])
print("------------分割一下------------")
print(type(df_rating.loc[1]))
```

输出为：

```
title                  刘老根第三部
rating                   4.0分
stars     主演赵本山,范伟,李静,闫学晶,王小宝
Name: 1, dtype: object
------------分割一下------------
<class 'pandas.core.series.Series'>
```

可以看到，我们拿到的 ser\_1 仍然是一个 Series 类型的对象。左边的 title、rating、stars是index，右边的"刘老根第三部""4.0分" 等是 values。因为 ser\_1 是一个 Series，所以如果我们要拿这一行中的某个数据，比如评分，直接写 ser\_1\["rating"\] 就可以实现。

对比通过行索引取出的行 Series 和 通过列索引取出的列 Series 不难发现，**列 Series 的 index 是 DataFrame 的行头，而行 Series 的 index 则是 DataFrame 的列名**。

#### （2）DataFrame 的创建

既然 DataFrame 是一个个 Series 组成的，那自然我们可以用 Series 来构造出 DataFrame。

构造 DataFrame 最常见的方式是用多个行 Series 的形式来创建，不同的行 Series 的长度应该是一致的（因为表格中每一行的元素个数都需要相等）。

我们以如下表格为例，将其直接创建为一个 DataFrame。

![Drawing 1.png](https://s0.lgstatic.com/i/image6/M00/3F/3E/CioPOWCc5pGAY4wvAABEPmw2qjE441.png)

新建 Cell ，输入如下代码：

```
index_arr = ["姓名", "年龄", "籍贯", "部门"]
ser_xiaoming = pd.Series(["小明", 22, "河北","IT部"], index= index_arr)
ser_xiaoliang = pd.Series(["小亮", 25, "广东","IT部"], index = index_arr)
ser_xiaoe = pd.Series(["小E", 23, "四川","财务部"], index=  index_arr)
df_info = pd.DataFrame([ser_xiaoming, ser_xiaoliang, ser_xiaoe])
df_info
```

输出  
![Drawing 2.png](https://s0.lgstatic.com/i/image6/M00/3F/3E/CioPOWCc5piAZtVwAAAxdFwHom8557.png)

可以看到，表格已经被成功的打印出来，这说明我们已经将内容正确构建出了 DataFrame。

### 基本操作

接下来这一节，我们来讲解一下 DataFrame 和 Series 的常见操作。

#### （1）添加行

DataFrame 提供了 append 的方法，用于添加一行，用法如下：

```
ser_xiaoh = pd.Series(["小红", 28, "福建", "财务部"], index = index_arr)
df_info = df_info.append(ser_xiaoh, ignore_index= True)
df_info
```

输出后可以看到，小红的记录已经追加到了末尾。  
![Drawing 3.png](https://s0.lgstatic.com/i/image6/M01/3F/36/Cgp9HWCc5qCACtioAABAKcY_wDE691.png)

#### （2）添加列

添加一列一般有两种情况，如果我们要添加的列，所有行的值都相同的话，我们可以直接以单个值赋值给新添加的列 Series 即可。如下所示：

```
df_info["考核结果"] = "合格"
df_info
```

输出为：  
![Drawing 4.png](https://s0.lgstatic.com/i/image6/M00/3F/3E/CioPOWCc5qmAW_O_AABSkQKX3zY822.png)

但如果新添加的列，每一行的内容不是完全一样时，就需要我们将一个新的 Series 赋值给 DataFrame 针对新列名的列 Series。

```
df_info["考核结果"] = pd.Series(["合格", "良好", "优秀", "良好"])
df_info
```

输出结果为：

![Drawing 5.png](https://s0.lgstatic.com/i/image6/M01/3F/36/Cgp9HWCc5rCAYrOFAABTPK_CVu0026.png)

可以看到，我们的列已经对应到了不同的行上面。有一点值得注意的是，我们第一次已经添加了“考核结果”这一列，每一行的值都是合格。后来我们对这一列赋值了一个新的 Series，修改了它的值。

所以，**当我们对 DataFrame 某个列名对应的列 Series 赋值，当列名不存在的时候，则会新建对应的列，而当列名存在时，则会修改原先列的值。**

所以这个技术既能创建新的一列，也可以修改已有的列。

#### （3）删除行或列

DataFrame 提供了 drop 方法来删除某一行或者某一列。

我们先以删除列举例，比如要删除刚才我们添加的“考核结果”这一列

```
df_info.drop(labels = "考核结果", axis=1, inplace= True)
df_info
```

输出结果如下，可以看到考核结果一列已经被删除。

![Drawing 6.png](https://s0.lgstatic.com/i/image6/M00/3F/3F/CioPOWCc5ryAc7NwAAA-94Kbhuk971.png)

接下来是删除行，以删除小 E 这一行为例：

```
df_info.drop(labels=2, axis=0, inplace=True)
df_info
```

输出结果如下，可以看到，小 E 那一行记录已经被成功删除。  
![Drawing 7.png](https://s0.lgstatic.com/i/image6/M00/3F/3F/CioPOWCc5sKAbGHzAAAz6desnM8702.png)

#### （4）单个单元格的查看与修改

在上面的操作中，整体还是以 Series 维度的操作为主。比如修改 Series，增加 Series。如果我们只需要查看和修改其中某一个单元格的内容，虽然也可以通过上面的方法，比如首先获得单元格所在的行 Series 或者列 Series，然后再用进一步的索引去获得单元格的内容。这样的方式一个比较麻烦，二是修改的时候可能会有问题。

对于单个单元格的查看和修改，推荐的方式是使用 DataFrame 的 loc 属性，可以一步到位指定定位到单元格，假设我们需要查看小亮的籍贯，以及修改为广西这个任务为例，演示 loc 函数的用法。

查看单元格：

输出为：

学会了查看之后，修改就比较简单了，直接给 loc 属性选出来的单元格赋值即可。

```
df_info.loc[1, "籍贯"] = "广西"
df_info
```

输出，可以看到，小亮的籍贯已经被修改为广西。

![Drawing 8.png](https://s0.lgstatic.com/i/image6/M01/3F/36/Cgp9HWCc5tCAS1kaAAA0AAgXGrA097.png)

#### （5）DataFrame 的排序

在数据分析的任务中，对数据集进行排序是非常常见的诉求。拿我们电视剧评分的数据集来说，可能我们希望分析高分的电影和低分的电影分别都有些什么特征。做这样的分析，首先第一步就是需要将我们的 DataFrame 按评分排序。我们以之前我们从 csv 加载的 DataFrame, df\_rating 为例。

DataFrame 提供了 sort\_values 方法来实现排序，用法如下。

```
df_rating.sort_values(by = "rating", inplace=True)
df_rating
```

输出为：  
![Drawing 9.png](https://s0.lgstatic.com/i/image6/M01/3F/36/Cgp9HWCc5tiAQfY6AAGfifkZwD0970.png)

可以看到，整个 DataFrame 不再是按行头的索引排序，而是按照电视剧的评分从低到高来排序了。

如果想看从高到低呢？自然也是支持的。只需要将是否升序排序的参数：ascending 设置为False 即可。

```
df_rating.sort_values(by="rating", inplace=True, ascending=False)
df_rating
```

输出如下所示，可以看到 DataFrame 已经变为评分从高到低排序了

![Drawing 10.png](https://s0.lgstatic.com/i/image6/M01/3F/36/Cgp9HWCc5uCASSpnAAGtWbCFw6U028.png)

#### （6）取前 N 个和后 N 个

在排序后，我们对数据表要进行分析，比如对高分进行分析，我们往往需要看多几个条目。但是每次输出 DataFrame 的时候，Notebook 一般只会选择前五个和末尾五个组成摘要进行表格的输出。如果默认的表格打印不满足我们的需求，我们可以使用 DataFrame 的 head 函数和 tail 函数来输出前 N 个和 后 N 个的数据。

举个例子，我们希望分别分析 20 条高分电视剧和 20 条低分电视剧。可以按如下方式实现：

输出结果为：  
![Drawing 11.png](https://s0.lgstatic.com/i/image6/M00/3F/3F/CioPOWCc5uuAen7UAALh4MirY1g688.png)

输出最后的 20 条的原理是类似的，只不过换成 tail 函数。

输出如下所示，可以看到这次输出了 20 条都是 1 分的数据。

![Drawing 12.png](https://s0.lgstatic.com/i/image6/M00/3F/3F/CioPOWCc5vOATptWAAM9wUAvcs8399.png)

#### （7）获取 DataFrame 的行数和列数

很多时候，从数据文件加载为 DataFrame 的时候，我们首先会看这个 DataFrame 有多少行、多少列。 DataFrame 提供了 shape 属性来返回行数和列数的信息。

shape 属性返回一个元组，这个数据结构我们之前没有学习过，不过你可以简单把他当一个列表用即可，shape 属性返回的元组有两个元素，第一个就是行数，第二个就是列数。

比如那 df\_rating 这个 DataFrame 为例，打印其行列数信息，代码如下：

```
shape = df_rating.shape
print("行数:", shape[0])
print("列数:", shape[1])
```

输出后和我们数据集的情况是匹配的。

### 小结

简单的总结一下今天学习到的内容。

首先，我们学习了一个计算机领域非常重要的概念——索引。这也是 pandas 中 DataFrame 很多操作的基础。索引之于数据就像目录之于书本内容，我们通过索引从一堆数据的拿出我们想要的一个或者多个数据。

然后，我们学习了 pandas 中的一维数据序列对象：Series。Series 融合了列表和字典的功能。通过把 index 和values 分开存储，让它既能按照列表使用，也能按照字典使用。

之后，我们学习了 DataFrame 的基本概念，DataFrame 是由 Series 组成的二维数据表。DataFrame 的一行或者一列都是一个 Series。自然我们可以用多个行 Series 或者多个列 Series 来创建 DataFrame。

最后，我们学习了 DataFrame 的基本操作。

-   添加行：用 append 方法
    
-   添加列：将新增加的列 Series 通过列名作为列索引赋值给 DataFrame 。
    
-   删除行或者列：用 drop 方法，通过 axis 参数控制删除行或删除列
    
-   单个单元格的查看与修改：loc 属性，中括号内第一个元素是行索引，第二个是列索引。
    
-   排序：用 sort\_values 方法， by 参数指定要用哪一列作为排序标准，ascending 参数决定要升序还是降序。
    
-   取前 N 个和后 N 个：head 和 tail 函数，N 就是函数的参数。
    
-   获取行数和列数：shape 属性。
    

课后练习：

为电视剧评分的 DataFrame，添加一列质量评级信息，列名为:"level"，规则是：

-   评分小于 2 分，则 level 为“低”；
    
-   大于等于2分，小于4分，则为“中”；
    
-   大于等于4分，则为“高”。
    

添加完后，打印出增加之后的 DataFrame。

___

答案：

```
shape = df_rating.shape
row_count = shape[0]
levels = []
for i in range(0, row_count):
    rating = df_rating.loc[i, "rating"]
    rating = rating.replace("分", "")
if rating < "2.0":
        levels.append("低")
elif rating < "4.0":
        levels.append("中")
else:
        levels.append("高")
df_rating["level"] = pd.Series(levels)
df_rating
```

执行之后，输出：  
![Drawing 13.png](https://s0.lgstatic.com/i/image6/M01/3F/36/Cgp9HWCc5v-ALrE8AAHGkz4dB2o796.png)
