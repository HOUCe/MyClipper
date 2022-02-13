---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


上一讲中，我们学习了 DataFrame 常见的数据查询技巧。有了这些技巧，我们已经可以通过各种角度来分析 DataFrame， 即便 DataFrame 包含非常多的数据。

但是在现实情况中，我们往往还会面临一个棘手的问题：现实工作中，因为在数据记录和数据存储环节偶尔会出现问题，比如互联网公司后端的行为日志记录系统时不时就会出现问题，导致部分数据的丢失。所以数据分析师拿到的原始数据中会存在很多字段或者记录是丢失的。为了不让这些缺失的数据影响数据分析的结果，在分析之前往往就需要进行数据清洗，对这些缺失的数据进行预处理。

本讲我们就来学习常见的数据清洗的技巧。

### 什么是缺失值

当我们从 CSV 文件或者其他数据源加载到 DataFrame 中时，往往会遇到某些单元格的数据是缺失的。当我们打印出 DataFrame 时，缺失的部分会显示为 NaN， 或者 None，或者 NaT（取决于单元格的数据类型），这样的值我们就称之为缺失值。

我们通过一个具体的例子来学习缺失值。按照之前每次课程的试验准备步骤，我们新建文件夹：chapter14，打开该文件夹，然后新建一个新的 Notebook，命名为 chapter14.ipynb 并保存在该文件夹中。

假设阿普闪购举办了一次全员英语能力考试，每个员工最后都有听力、阅读、写作、口试四个成绩。这里我们抽样了三个同事的分数数据，打算对其做一些简单的分析。如下所示

```
scores = [[20.26, 71.58, 27.06, 97.51],
       [40.61, 72.32, 56.54,  5.45],
       [72.44, 68.89,  6.65, 75.54]]
```

执行上述代码，接下来我们需要将分数数据导入到 DataFrame 中。代码如下：

```
import pandas as pd
index_arr = ["听力", "阅读", "写作", "口试"]
df_scores = pd.DataFrame(scores,
            index = ["小亮", "小明", "小E"],
            columns= index_arr)
df_scores
```

输出为：  
![Drawing 0.png](https://s0.lgstatic.com/i/image6/M01/40/A8/Cgp9HWCmKq6AcOFEAABFpvOeAns145.png)

现在，三位同事的分数已经被录入了，但这会儿你的 Mentor 希望你把小李的成绩纳入一起分析。但小李的我们只有听力成绩，不知道另外三项的成绩。

代码如下：

```
ser_xiaol = pd.Series([30.04,None,None,None], index=index_arr,name="小李")
df_scores = df_scores.append(ser_xiaol)
df_scores
```

执行代码后，输出如下：  
![Drawing 1.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmKrWASd3wAABam_gRqAY869.png)

可以看到，小李的阅读、写作和口试显示了 NaN，代表**数字类型**的缺失值。时间类型的缺失值一般显示为 NaT，而字符串类型的则显示为 None。

在实际项目中，缺失值可以说一直存在于原始的数据源中。如果我们在数据分析时不把它处理掉，很可能会得到错误的结果。

以这个例子来说：如果要计算写作科目的平均分，小李的 NaN 到底是当作 0，还是当作平均数，还是干脆就不把小李纳入计算，**都需要根据情况进行决策，来最大化降低缺失值对于分析结果的影响**。

接下来我们会介绍对于缺失值不同策略的实现方式。

### 查询缺失值

要处理缺失值，首先第一步是查询缺失值是否存在，以及数量情况如何。与上述例子不同，现实项目中我们是不知道 DataFrame 中是不是有缺失值以及到底有多少缺失值。

接下来，我们会学习如何查询 DataFrame 中的缺失值情况。为了更好地演示如何查看缺失值，我们再添加一条记录到 DataFrame。

```
ser_xiaowang = pd.Series([None, 91.00, 72.34, None], index = index_arr, name="小王")
df_scores = df_scores.append(ser_xiaowang)
df_scores
```

执行之后，最新的 DataFrame 如下图所示。  
![Drawing 2.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmKsKAEaChAABJZEgxFWo387.png)

接下来我们开始分析缺失值的情况

#### 1\. 按单元格查看缺失值情况

DataFrame 提供了 isna 函数，isna 函数返回一个新的 DataFrame， 行数和列数和原 DataFrame 相同，新的 DataFrame 全部由布尔型数据组成，原 DataFrame 的单元格的数据是缺失值的话，在新的 DataFrame 对应位置的单元格就是 True，否则为 False。

输出：  
![Drawing 3.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmKsiAaTemAAA0KGMlo4g788.png)

可以看到，小李的 阅读、写作、口试，以及小王的 听力、口试是 True，代表在原来的 DataFrame 中这些数据是缺失值。

#### 2\. 按列查看缺失值

由于现实项目中的 DataFrame 往往很大，我们不可能逐一去看 DataFrame 每个单元格是 True 还是 False，所以更常见的查看手段就是按列聚合缺失值的数量。

我们只需要在 isna 函数的基础上再调用一次 sum 函数，即可实现按列聚合。

输出为：  
![Drawing 4.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmKteAUfxFAAAY4QjlbeY070.png)

代表听力、阅读、写作三列都有一个缺失值，而口试一列有两个。

#### 3\. 按行查看缺失值

既然可以按列查看，自然也是可以按行查看的。按行查看可以帮助我们了解某个同事的缺失值情况。按行查看的实现方式和按列类似，只需要在 sum 函数的参数中传入 1 即可。

输出为：  
![Drawing 5.png](https://s0.lgstatic.com/i/image6/M01/40/B1/CioPOWCmKuKAIMFXAAAYWS3c09w606.png)

#### 4\. 过滤出有缺失值的列

有时候，我们希望单独将有缺失值的列过滤出来，查看大概情况，这时候配合使用 isna 函数和上一讲学习的 loc 函数就可以实现。

```
df_scores.loc[:, df_scores.isna().any()]
```

输出为：  
![Drawing 6.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmKumAc5SOAABIXIIRgZ8827.png)

因为目前我们的 DataFrame 每一列都至少包含一个缺失值，所以过滤列之后输出了所有记录。

#### 5\. 过滤出有缺失值的行

对应的，如果我们想过滤出有缺失值的行，同样也可以通过 loc 配合 isna 实现。

```
df_scores.loc[df_scores.isna().any(1),:]
```

输出为：  
![Drawing 7.png](https://s0.lgstatic.com/i/image6/M01/40/B1/CioPOWCmKvSAUxqcAAAkK1cvAns816.png)

可以看到，包含缺失值的小李和小王的记录被过滤了出来。

#### 6\. 缺失值的总个数

对 isna 返回的布尔 DataFrame 做 sum，则可以得到各列各行有多少个缺失值，如果再对这个结果再做一次 sum，则可以得到整个 DataFrame 包含多少个缺失值。

```
df_scores.isna().sum().sum()
```

输出为：

这代表整个 DataFrame 一共包含五个缺失值。

### 处理缺失值

在查询出缺失值后，接下来就是根据分析的场景和缺失值的情况，来决定怎么处理这些缺失值。

常见的缺失值处理方法有以下三种。

#### 1\. 缺失值删除

顾名思义，删除代表的就是我们直接将缺失值从 DataFrame 中删除，一般在缺失值比较少的情况下可以用删除来简单处理。

pandas 的 DataFrame 提供了一个强大的删除缺失值的方法：dropna， 通过传入恰当的参数，我们可以灵活地删除部分或者全部的缺失值。

（1）删除所有缺失值所在的行

输出为：  
![Drawing 8.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmKwKAID_fAAA0kJR2yR4310.png)

（2）删除所有缺失值所在的列

```
df_scores.dropna(axis = 1)
```

输出为：  
![Drawing 9.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmKweAc6mUAAARkvZQNuo582.png)

因为我们的 DataFrame 每一列都至少有一个缺失值，所以删除后 DataFrame 只剩下行索引。

（3）删除少于 X 个正常值的行

有时候，我们希望删除缺失值较多的行，保留有缺失值但数量比较少的行，可以通过指定 thresh 参数来实现。

```
df_scores.dropna(thresh=2)
```

输出为：  
![Drawing 10.png](https://s0.lgstatic.com/i/image6/M01/40/B1/CioPOWCmKw-ACp8QAAA_ua7tKp0134.png)

可以看到，小李的正常值只有 1 个，所以被删除。而小王的正常值有两个，所以被保留。

（4）参考某几列作为删除依据

有的时候，我们的数据表中不同的列权重（重要性）是不一样的。比如这次职工英语考试，最关键的是听力，所以我们希望只看听力这一列，如果听力是缺失值，则删除，其他列有缺失值则不删除。可以通过 subset 参数实现。

```
df_scores.dropna(subset=["听力"])
```

输出为：  
![Drawing 11.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmKxaAdzz6AAA_F_gjlFo185.png)

可以看到，小王的记录被删除，而小李的被保留，原因就是小李的听力成绩是在的，而我们通过 subset 参数指定了只看听力这一列的缺失值情况。

另外，需要注意一点的是，**dropna 方法默认不会改变调用它的 DataFrame**，而是会将删除缺失值后的 DataFrame 作为函数的返回值返回。所以上面的代码并没有实际修改到 df\_scores。如果需要实际修改 df\_scores ，则需要做一次赋值，比如： df\_scores = df\_scores.dropna()

#### 2\. 缺失值替换

除了删除之外，另一个主流的缺失值处理方式就是替换。简单来说就是将缺失值的部分替换为一个固定的值，来减少缺失值带来的对于分析结果的不确定性。当数据量大且缺失值的数量也不小的时候，使用填充策略相比删除策略能显著提升分析结果的准确性。

常见的缺失值替换策略有以下几种。

（1）全表固定值替换

最简单的缺失值替换方式，就是使用一个默认值来替换 DataFrame 中所有的缺失值。首先我们先看一下目前 DataFrame的缺失值情况

输出为：  
![Drawing 12.png](https://s0.lgstatic.com/i/image6/M01/40/B1/CioPOWCmKyCAS_VTAABJjTDy060581.png)

现在，我们用 33.0 这个数字来替换掉全部的缺失值。代码如下：

输出为：  
![Drawing 13.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmKyaAO3wVAABIWT2BdkQ018.png)

可以看到，所有的缺失值已经被替换为了 33.0。

（2）按列固定值替换

除了全局替换，我们也可以实现按列来替换缺失值，为了不影响 df\_scores 的值，这里我们用一个新的 DataFame 来测试。

```
df_scores_test = df_scores.copy(deep=True)
df_scores_test["听力"] = df_scores_test["听力"].fillna(60.0)
df_scores_test
```

执行之后，输出为：  
![Drawing 14.png](https://s0.lgstatic.com/i/image6/M01/40/B1/CioPOWCmKy2ATgkNAABKzf2iTGU608.png)

小王的听力成绩被填充为了 60分。

（3）按行固定值替换

按行填充和按列填充类似，只是索引单行就需要借助 loc 对象，因为按行列填充都会修改到原始 DataFrame，所以这里我们仍然使用 df\_scores\_test 来进行测试。

```
df_scores_test.loc["小李", :] = df_scores_test.loc["小李",:].fillna("50.0")
df_scores_test
```

输出为：  
![Drawing 15.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmKzSAEZ4GAABJZbWIP_E619.png)

可以看到，小李的阅读、写作、口试成绩都被填充为了50.0。

（4）最近有效值替换

在实际的项目中，除了使用固定值之外，还有一个常见的策略就是使用最近有效值来做替换。

什么叫最近有效值呢？就是在列的维度，当某一个单元格的数据是缺失值时，在该列往上搜索，碰到第一个有效值（非缺失值），就是最近有效值。

听起来比较绕，我们举例来说明一下，还是以 df\_scores 这个 DataFrame 为例，数据如下。

![Drawing 16.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmKz6ATFDbAABJjTDy060551.png)

-   小王的听力是缺失值，那在该列往上找，第一个有效值就是小李的听力成绩：30.04 ，所以 30.04 就是小王的听力的最近有效值。
    
-   同理可得小李的阅读的最近有效值是 68.89，以此类推。
    

pandas 中要实现最近有效值填充，给 fillna 函数传入 method 参数即可。代码如下：

```
df_scores.fillna(method="ffill")
```

输出为：  
![Drawing 17.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmK0aAcxhzAABOI511Amo515.png)

可以看到，几个缺失值的位置都被对应的最近有效值替换了。最近有效值灵活利用了列的数据特性，比起全局统一值的替换往往能达到更好的效果。

当我们设置 method="bfill" 的时候，pandas 就会用缺失值对应列，往下搜索的第一个有效值来填充。

#### 3\. 缺失值插值

在有的场景下，只使用最近有效值依然不能很好地满足分析的诉求。比如一些时间序列分析的场景，缺失值可能和前面或者后面的数据都有一定的关系。

如果可以结合缺失值前后的有效值的信息来推测缺失值，那准确性相比直接用最近有效值要高很多。pandas 提供了插值方法来实现这一目的。

插值简单来说就是通过已经有的点来拟合出一个函数关系（f），然后根据缺失值的位置（x）来去拟合出来的函数中拿到对应的 f(x) 值，然后用这个值去替换掉缺失值。这样我们认为这个 f(x) 是最有可能贴近真实的值的。

插值的方法有很多，最简单的有线性插值、临近点插值、立方插值等。这里以简单的线性插值为例来介绍 pandas 插值的用法。

假设我们有如下 Series：

```
ser_test = pd.Series([100,3, None, None, 9])
ser_test
```

执行之后输出：  
![Drawing 18.png](https://s0.lgstatic.com/i/image6/M01/40/B1/CioPOWCmK1SATthFAAAf5ngdOl8854.png)

目前 ser\_test 中有两个缺失值，想要通过线性插值来计算出这两个缺失值的话，我们可以拿到缺失值前后的两个数据点(1,3.0), (4, 9.0)，根据两点直线方程有：  
![Drawing 20.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmK2WAUvs-AAAfk3cdX-U187.png)

化简可得：y = 2x + 1， 将缺失值的行索引 2 和 3代入该函数，可以得到插值分别为 5 和 7。

以上是线上插值的原理，实际我们在写代码中并不需要手算，pandas 提供了 interpolate 函数可以帮助我们直接搞定。

现在我们来计算 ser\_test 的插值情况。

输出为：

![Drawing 21.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmK3CAVpviAAAfW2mN-O8302.png)

可以看到，填充的结果和我们手算的结果是一样的。

现在，我们使用插值的方法来填充 df\_scores 这个 DataFrame。当对 DataFrame 使用 interpolate时，实际上是每个列 Series 分别计算并插值的。代码如下：

输出为：  
![Drawing 22.png](https://s0.lgstatic.com/i/image6/M00/40/B1/CioPOWCmK5iAJ25AAABZ9xoEPdg877.png)

从结果上，可以看到绿框的两个缺失值成功替换为了线性插值的版本，而红框部分却仍然是用的最近有效值，这是为何呢？其实很简单，线性插值需要缺失值前后有效值的信息来拟合方程，而红框部分都缺少后面的有效值，所以无法拟合。**当线性插值无法拟合的时候，会默认采用最近有效值来填充**。

#### 4\. 处理重复值

除了常见的缺失值之外，实际项目中还经常遇到的异常数据问题就是重复值。企业的数据日志记录系统出现问题时，有时候会导致丢失数据，这就产生了缺失值的问题。有的时候会重复写入数据，这也产生了重复值的问题。

重复值指的是 DataFrame 中的两行全部或部分一样。

为了更好地演示如何处理重复值，我们先模拟一下重复值的场景，额外添加两条小王的记录进 DataFrame。

```
ser_xiaowang = pd.Series([None, 91.00, 72.34, None], index = index_arr, name="小王")
df_scores = df_scores.append(ser_xiaowang)
df_scores
```

输出为：  
![Drawing 23.png](https://s0.lgstatic.com/i/image6/M00/40/A9/Cgp9HWCmK6KAX1W7AABT43ifpDs141.png)

pandas 提供了 duplicated 函数来识别 DataFrame 中是否存在重复的行，执行 duplicated 后返回一个布尔类型的 Series，并将重复的行标记为 True， 其他为 False。用法如下：

输出：  
![Drawing 24.png](https://s0.lgstatic.com/i/image6/M00/40/B1/CioPOWCmK6mAQbJhAAAdKdL_9gQ730.png)

从结果中可以看到，第一个小王的记录，因为是第一次出现，不算重复，所以标记为 False，而第二个小王的记录因为是第二次出现了，所以标记为 True，也就是重复。

确认有重复的数据后，我们只需要调用 pandas 提供的 drop\_duplicates 方法即可删除这些重复值。

```
df_scores = df_scores.drop_duplicates()
df_scores
```

输出为：  
![Drawing 25.png](https://s0.lgstatic.com/i/image6/M00/40/B1/CioPOWCmK7CANpwXAABJUucLwl4598.png)

### 小结

到此，我们对于缺失值和重复值的处理就讲解完了。相信现在你拿到一个不完美的数据集，已经知道如何下手进行初步的分析和数据清洗了。

回顾一下，今天我们主要学习了如下关键点。

-   缺失值的概念：DataFrame 中缺少的部分数据，数字的显示为 NaN，字符串显示为 None，时间类型则显示为 NaT。
    
-   查看缺失值：
    
    -   按单元格查看：df.isna()
        
    -   按列查看：df.isna().sum()
        
    -   按行查看：df.isna().sum(1)
        
    -   有缺失值的列：df.loc\[:, df.isna().any()\]
        
    -   有缺失值的行：df\_scores.loc\[df\_scores.isna().any(1),:\]
        
    -   缺失值总个数：df.isna().sum().sum()
        
-   处理缺失值：
    
    -   删除缺失值：df.dropna()
        
    -   缺失值替换：df.fillna()
        
    -   缺失值插值：df.interpolate()
        
-   处理重复值：
    
    -   查看重复行：df.duplicated()
        
    -   删除重复行：df.drop\_duplicates()
        

到现在，pandas 的学习已经结束了大半儿。下一讲我们会专门学习数据分析领域常见的挑战：时间序列数据的分析与处理。
