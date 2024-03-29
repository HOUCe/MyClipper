---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


上一讲中，我们学习了 NumPy 的基础知识，包含最基础的数据结构 ndarray 的学习以及常见的 NumPy 的方法。你感觉怎么样呢？

NumPy 这个部分我们可能会稍微多地接触一些数学知识，不过不用担心，都是我们以前学过的，我们只需要回忆、复习一下基本概念即可。毕竟现在绝大多数运算都不需要我们手算，直接调用 NumPy 的函数即可完成。所以不要害怕，继续冲！

今天我们将会学习 NumPy 更多强大的特性，一个是常见的统计学分析，然后会学习使用 NumPy 如何支持线性代数的基本运算。

为什么这里要提线性代数呢？因为基于矩阵和行列式的线性代数是绝大多数数值分析方法的基础，NumPy 提供了一套非常强大处理线性代数的函数，可以让我们轻松完成绝大多数运算，我们只需要清楚相关的概念即可完成学习，不需要复习具体的规则。

最后，我会介绍如何使用 NumPy 完成数据分析中高频出现的场景：相关性分析。

### 计算多维数组的统计学特征

首先，我们来介绍如何使用 NumPy 来计算多维数组的统计学特征。常见的统计学特征有：极值、中位数、均值、方差等。

首先我们在课程目录新建文件夹 chapter18，之后打开 VS code，选择文件 → 打开文件夹，选中刚才创建的文件夹打开。

之后创建 Notebook，并保存为 chapter18.ipynb，保存在 chapter18 文件夹中。

#### 最大值与最小值

最常见的统计就是统计多维数组的最大值和最小值，最大值的最小值我们简称为极值。NumPy 提供了 amax/amin 两个函数来计算多维数组的极值。

用法如下：

```
arr = np.arange(12).reshape(3,4)
print("原始数组：\n", arr)
print("\n")
print("最大值：", np.amax(arr))
print("最小值：", np.amin(arr))
```

输出为：

```
原始数组：
 [[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
最大值： 11
最小值： 0
```

上述代码中，我们直接传入了数组作为参数，所以 amax 和 amin 函数就返回了多维数组所有维度的最大值和最小值。当我们额外传入轴参数（axis参数）时，则会返回一个数组。

这里解释一下轴参数的含义：对于二维数组，轴(axis) 等于 0 时，代表沿着行的方向，向右逐个计算每一列的极值，简称为横轴方向。当 axis 等于 1 时，则按照列的方向向下，逐个计算每一行的极值，简称为纵轴方向。

用法示例如下：

```
arr = np.arange(12).reshape(3,4)
print("原始数组：\n", arr)
print("\n")
print("横轴方向最大值：", np.amax(arr, axis = 0))
print("横轴方向最小值：", np.amin(arr, axis = 0))
print("纵轴方向最大值：", np.amax(arr, axis = 1))
print("纵轴方向最小值：", np.amin(arr, axis = 1))
```

输出如下：

```
原始数组：
 [[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
横轴方向最大值： [ 8  9 10 11]
横轴方向最小值： [0 1 2 3]
纵轴方向最大值： [ 3  7 11]
纵轴方向最小值： [0 4 8]
```

#### 极值的差

在很多统计任务中，我们经常需要计算某个数组，或者某一列、某一行的最大值与最小值的差，来看数据的差异性。比如最大值和最小值相差比较小，说明数据是相对比较平稳的，可能区分度不大。反之如果差值很大，则说明数据变动很剧烈，值得进一步分析差异性。

NumPy 提供了 ptp 函数可以帮助我们直接计算出最大值与最小值的差，和 amax/amin 一样。ptp 也支持 axis 参数可以分行或者列来计算差异。

代码示例：

```
arr = np.random.randint(0, 100, [3,4])
print("原始数组：\n", arr)
print("\n")
print("数组极值差：", np.ptp(arr))
print("横轴方向极值差：", np.ptp(arr, axis=0))
print("纵轴方向极值差：", np.ptp(arr, axis=1))
```

输出（因为用了随机数组，实际你执行时候的结果和下面可能不同）：

```
原始数组：
 [[45 33 26 79]
 [48 78 43 78]
 [45 17 31  6]]
数组极值差： 73
横轴方向极值差： [ 3 61 17 73]
纵轴方向极值差： [53 35 39]
```

数组的最大值是 79，最小值是 6 ，所以数组的极值差是 73。

横轴方向上，第一列最大值是 48，最小值是 45 ，所以极值差是 3。所以横轴方向极值差的第一个数是 3，之后的逻辑以此类推。

#### 中位数

中位数可以这样理解，把数组排序之后，在数字中间的数字就是数组的中位数。如果数组的个数是偶数，则中位数就是在中间两个数字的平均值。NumPy 提供了 median 函数来计算中位数，同样也支持 axis 参数。

代码示例：

```
arr= np.arange(12).reshape(3,4)
print("原始数组：\n", arr)
print("\n")
print("数组中位数：", np.median(arr))
print("横轴方向中位数：", np.median(arr, axis=0))
print("纵轴方向中位数：", np.median(arr, axis=1))
```

输出如下：

```
原始数组：
 [[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
数组中位数： 5.5
横轴方向中位数： [4. 5. 6. 7.]
纵轴方向中位数： [1.5 5.5 9.5]
```

从纵轴方向时，每次计算的一维数组都有 4 个元素（偶数个），所以最终的中位数是中间两个数字的均值，所以出现了小数：1.5，5.5，9.5。

#### 平均值

平均值比较容易理解，这里就不多做解释，NumPy 提供了 mean 函数来计算数组的平均值，同样也支持 axis 参数。代码示例如下：

```
arr = np.random.randint(0, 100, [3,4])
print("原始数组：\n", arr)
print("\n")
print("数组平均值：", np.mean(arr))
print("横轴方向平均值：", np.mean(arr, axis=0))
print("纵轴方向平均值：", np.mean(arr, axis=1))
```

输出如下：

```
原始数组：
 [[87 34 63 26]
 [ 2 11 28 95]
 [16  8 84 74]]
数组平均值： 44.0
横轴方向平均值： [35.         17.66666667 58.33333333 65.        ]
纵轴方向平均值： [52.5 34.  45.5]
```

#### 标准差与方差

在数据统计中，衡量数据差异度还有两个常用的指标就是标准差与方差，标准差与方差的计算方法相似，标准差是方差的平方根。

NumPy 提供了 std 方法和 var 方法来分别计算标准差和方差。

同样，两个方法都支持 axis 参数，代码示例如下。

```
arr= np.arange(12).reshape(3,4)
print("原始数组：\n", arr)
print("\n")
print("数组标准差：", np.std(arr))
print("横轴方向标准差：", np.std(arr, axis=0))
print("纵轴方向标准差：", np.std(arr, axis=1))
print("\n")
print("数组方差：", np.var(arr))
print("横轴方向方差：", np.var(arr, axis=0))
print("纵轴方向方差：", np.var(arr, axis=1))
```

输出如下：

```
原始数组：
 [[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
数组标准差： 3.452052529534663
横轴方向标准差： [3.26598632 3.26598632 3.26598632 3.26598632]
纵轴方向标准差： [1.11803399 1.11803399 1.11803399]
数组方差： 11.916666666666666
横轴方向方差： [10.66666667 10.66666667 10.66666667 10.66666667]
纵轴方向方差： [1.25 1.25 1.25]
```

至此，我们已经学习完了 NumPy 中最常见的描述性数据统计的部分。

### 线性代数支持

线性代数是数学的一个分支，主要围绕研究解决线性系统的种种问题。线性系统可以简单理解成解多元一次的线性方程组。这类问题在现实生活中有非常多的应用。

举一个简单的例子，我们都知道学生的考试成绩和学生每天的作业量、玩游戏看电视的时间、早上起来的时间等都有一定的关联，但如何把这种关联具体化、数字化，来进一步通过学生日常的生活习惯来预测出他考试的分数，这就是线性代数研究的范畴。

在数据分析的领域，除了描述性的数据统计，我们在数据分析中还会用到推论型的数据统计分析。而对于推论型的数据分析，比如回归分析、曲线拟合等任务，线性代数知识都必不可少。

本节我们会介绍 NumPy 强大的线性代数计算的工具包。

线性代数的内容很多，但学习本篇文章只需要了解矩阵的一些基本知识即可。

NumPy 提供了一个基础的数据结构来支持线性代数的运算：matrix，也就是矩阵。matrix 可以理解为一种特殊的 ndarray，怎么特殊呢？就是 matrix 只支持二维的。而 ndarray 则支持任意多维。

#### 矩阵的创建

矩阵的创建和 ndarray 基本类似，最常见的就是传入列表来创建。比如在现实场景中，我们收集了团队员工对于公司餐厅服务的打分，打分项包含环境、服务态度、卫生情况、味道等四个维度。这个打分表最终就很适合表示为一个 Nx4 的矩阵，N 代表成员的个数。代码示例：

```
mat1 = np.matrix([[1,2,3], [4,5,6],[7,8,9]])
mat2 = np.matrix([1,2,3,4,5,6,7,8,9])
print("矩阵1：\n", mat1)
print("矩阵2：\n", mat2)
```

输出如下：

```
矩阵1：
 [[1 2 3]
 [4 5 6]
 [7 8 9]]
矩阵2：
 [[1 2 3 4 5 6 7 8 9]]
```

前文有说到，矩阵只支持二维，所以对于 mat2，我们虽然传入的是一个一维数组，但创建矩阵之后就被转换为 1 x 9 的二维矩阵。

除了通过列表来初始化，矩阵还支持通过字符串的形式初始化，行与行之间用分号隔开，行内的数组用空格隔开。示例如下：

```
mat3 = np.matrix("1 2 3;4 5 6;7 8 9")
print(mat3)
```

输出如下：

```
[[1 2 3]
 [4 5 6]
 [7 8 9]]
```

#### 矩阵的基础属性

矩阵对象同 numpy 对象一样，也支持常见的 NumPy 方法，比如我们之前学习的描述性统计方法：mean、std 等。比如在之前例子里，我们想知道员工自己打出的均值、方差，或者像看打分维度的（味道、环境）的均值、方差，就需要轴来计算矩阵的统计信息。另外，矩阵还有一个非常常见的操作，求矩阵的转置，NumPy 也提供了支撑，具体用法如下：

```
mat = np.matrix([[1,2,3], [4,5,6],[7,8,9]])
print("原始矩阵：\n", mat)
print("\n")
print("转置矩阵：\n", mat.T)
print("\n")
print("矩阵的均值：", np.mean(mat))
print("矩阵的标准差：", np.std(mat))
print("矩阵的求和：", np.sum(mat))
print("矩阵的求和（横轴方向）：", np.sum(mat, axis=0))
```

输出如下：

```
原始矩阵：
 [[1 2 3]
 [4 5 6]
 [7 8 9]]
转置矩阵：
 [[1 4 7]
 [2 5 8]
 [3 6 9]]
矩阵的均值： 5.0
矩阵的标准差： 2.581988897471611
矩阵的求和： 45
矩阵的求和（横轴方向）： [[12 15 18]]
```

我们在最后演示了按轴的方向求和，其实就是暗示了，矩阵和多维数组一样，所以相关的统计函数都支持 axis 参数。

#### 矩阵常见运算

矩阵常见的主要还是加、减、乘的方法。矩阵的加减方法是比较容易理解的，就是两个形状一致的矩阵对应未知的元素分别相加和相减，得到的矩阵就是计算的结果。举个加法的例子，假设员工分别对公司的两个餐厅都打了分，那就有两个 Nx4 的矩阵，现在我们希望把每个员工的所有打分都汇总，其实就是把两个矩阵进行相加。矩阵的加减法实现的代码如下：

```
mat = np.matrix("1 2 3; 4 5 6")
mat1 = np.matrix("7 8 9; 10 11 12")
print("矩阵相加的结果：\n", mat + mat1)
print("矩阵相减的结果：\n", mat - mat1)
```

输出如下：

```
矩阵相加的结果：
 [[ 8 10 12]
 [14 16 18]]
矩阵相减的结果：
 [[-6 -6 -6]
 [-6 -6 -6]]
```

矩阵的乘法则有一定的规则，如下所示：

![image.png](https://s0.lgstatic.com/i/image6/M00/43/96/Cgp9HWC58TuAfQXlAABqS4Jh4GE246.png)

用文字描述的话，就是用第一个矩阵的第一行，乘以第二个矩阵的每一列成为结果矩阵的第一行，乘的过程是元素两两相乘再相加。然后用第一个矩阵的第二行乘以第二个矩阵的每一列，成为结果矩阵的第二行，以此类推。

矩阵的乘法为什么会有这么奇怪的规则呢？其中一个角度是，为了简化线性方程组的写法，感兴趣的同学可以看一下[阮一峰的这篇文章](https://www.ruanyifeng.com/blog/2015/09/matrix-multiplication.html?fileGuid=xxQTRXtVcqtHK6j8)。

NumPy 的矩阵对象可以直接使用\*符号来实现矩阵的乘法。下面的代码我们实现了一个 2x3 的矩阵 乘以一个 3x4的矩阵，得到了一个 2x4 的矩阵。

```
mat = np.matrix("1 2 3;4 5 6")
mat1 = np.matrix("1 2 3 4; 5 6 7 8; 9 10 11 12")
print("矩阵相乘的结果：\n", mat*mat1)
```

输出如下：

```
矩阵相乘的结果：
 [[ 38  44  50  56]
 [ 83  98 113 128]]
```

#### 矩阵求逆

矩阵的逆矩阵代表这样的一种矩阵，它与原矩阵相乘之后得到的是单位矩阵。比如存在矩阵 A，如果矩阵 B 满足 A\*B=I（I 为单位矩阵），则我们就说矩阵 B 是矩阵 A 的逆矩阵，也可以表示为 A\-1。

当然，不是所有矩阵的逆矩阵都存在，有的矩阵本身是不可逆的，通过计算矩阵行列式的值是否等于 0 可以判断一个矩阵是否可逆。如果行列式的值等于 0 ，则说明矩阵不可逆，这种不可逆的矩阵也称为奇异矩阵。

NumPy 提供了方便的判断以及计算方法：np.linalg.det 函数可以计算矩阵行列式的值，而 np.linalg.inv 可以求矩阵的逆矩阵。不过 NumPy 的矩阵求逆有一个限制：只支持方阵。换句话说就是行数和列数相等的矩阵。

首先我们测试一下我们常用的矩阵是否可逆：

```
mat = np.matrix("1 2 3;4 5 6;7 8 9")
print("行列式：", np.linalg.det(mat))
```

输出如下：

```
行列式： -9.51619735392994e-16
```

输出结果是一个非常非常小的数组（末尾的 e-16 代表前面的数字乘以 10 的 -16 次方）。因为 np.linalg 工具包是一个通过各种近似算法来计算结果的函数库，所以会存在一定的误差。所以这个输出我们可以认为基本等于 0，可以得出，这个矩阵是一个奇异矩阵，不可逆。

对于奇异矩阵，一般情况下只需要随便修改一个数字很可能结果就会不一样，比如我们把最后一位改成 10 ，再来看一下行列数的值。

```
mat = np.matrix("1 2 3;4 5 6;7 8 10")
print("行列式：", np.linalg.det(mat))
```

输出如下：

约等于 -3，不等于 0 ，说明该矩阵是非奇异矩阵，存在逆矩阵。接下来我们就来计算这个矩阵的逆矩阵。计算出结果之后，我们将其与原矩阵相乘，测试结果是否是单位矩阵。

```
mat = np.matrix("1 2 3;4 5 6;7 8 10")
mat_inv = np.linalg.inv(mat)
print("原矩阵：\n", mat)
print("逆矩阵：\n", mat_inv)
print("两者相乘：\n", mat*mat_inv)
```

输出如下：

```
原矩阵：
 [[ 1  2  3]
 [ 4  5  6]
 [ 7  8 10]]
逆矩阵：
 [[-0.66666667 -1.33333333  1.        ]
 [-0.66666667  3.66666667 -2.        ]
 [ 1.         -2.          1.        ]]
两者相乘：
 [[ 1.00000000e+00 -4.44089210e-16 -1.11022302e-16]
 [ 4.44089210e-16  1.00000000e+00 -2.22044605e-16]
 [ 4.44089210e-16  8.88178420e-16  1.00000000e+00]]
```

可以看到，逆矩阵被成功的计算出来，并有了相乘之后的结果。所谓单位矩阵，就是指对角线的元素是 1 ，而其他元素是 0 的矩阵。分析上面两者相乘的结果，对角线的元素都是 1， 非对角线的元素虽然不是 0，但都是极小的数字（末尾都跟着 e-16 ，代表乘以10 的-16 次方），按照之前系统误差的处理方式，这类小数可以认为是 0。

经过误差的排除后，不难看出两者相乘的结果就是单位矩阵，这说明这里计算出的逆矩阵是正确的。

#### 求解线性方程组

在回归分析，分类分析等诸多数据分析方法中，我们都会和线性模型打交道，比如我们要通过学生早起的时间，每天学习时间，每天游戏时间来估算他的期末考试成绩，本质上最后就是解一个线性方程组。这个具体的案例我们会在下一讲进行介绍，而求解线性方程组则是基础中的基础。

线性方程组，又名为多元一次方程组。形式如下：

![Drawing 3.png](https://s0.lgstatic.com/i/image6/M00/43/4A/CioPOWC4de6Aco1qAAAKUhqOqz8765.png)![Drawing 6.png](https://s0.lgstatic.com/i/image6/M01/43/41/Cgp9HWC4dfSABzSIAAA1FbdDWmY891.png)

所有的系数 a 其实就构成了一个 mxm的矩阵， 记做 A，b1,b2,..bm 也构成了一个 nx1 的矩阵，记做 b，x1,x2...xm 也是一个 nx1的矩阵，记做 x。所以上面整个方程组可以转换为 Ax - b = 0。所谓的求解方程组，就是在矩阵 A 和 矩阵 b 已知的基础上，求出矩阵 x 的值。

NumPy 的 linalg 模块提供了 solve 方法，可以直接为我们求出 x。假设矩阵 A = \[\[1,0\], \[2,3\]\] b = \[\[4\], \[5\]\] ，现在我们编写代码来计算 x。

```
A = np.matrix("1 0; 2 3")
b = np.matrix("4;5")
x = np.linalg.solve(A, b)
print("A 矩阵：\n", A)
print("b 矩阵：\n", b)
print("x 矩阵：\n", x)
```

输出如下：

```
A 矩阵：
 [[1 0]
 [2 3]]
b 矩阵：
 [[4]
 [5]]
x 矩阵：
 [[ 4.]
 [-1.]]
```

感兴趣的同学可以手工验算一下计算的结果是否正确。结果的 x 是一个 2 x 1 的矩阵，表示形式就是 \[\[4\], \[-1\]\], 这 b 的格式是一样的。

### 相关性分析

最后一个部分，我们介绍另一个 NumPy 的经典功能：相关性分析。

相关性分析是数据分析中非常常用的方法，简单来说就是分析两个数据序列的相关性。数据序列可以简单理解成两个数量一样的一维数组。如果两个数据序列相关性强，则代表我们就可以用其中一个数据序列来预测另一个数据序列的趋势。

相关性分析在现实生活中应用非常广泛，比如数据分析的经典案例，男人去超市买啤酒的时候，都会顺便买尿布。超市通过数据分析，发现了这个规律，之后将两个商品捆绑在一起售卖并给予一定的优惠，进一步提升了销量。

上述的“数据分析”本质就是一种相关性分析，我们通过分析各个不同的商品购买记录两两之间的相关性，就能够发现相关性强的商品，之后将其捆绑销售，就能达到提升销量的目的。

假设存在两个数据序列，a 和 b， 它们的相关性一共有三种情况：

-   正相关性，代表 a 和 b 的趋势一致。a 数组的某个位置的元素比上一个元素显著变大，则对应 b 数组对应位置的元素比上一个也会显著变大。反之亦然；
    
-   负相关性，代表两个数据序列的趋势相反，a 数组某个未知的元素比上一个变大，而对应同样未知的 b 数组比上一个元素反倒会显著变小。反之亦然；
    
-   无相关性，a 和 b 中不同位置的数据变化是独立的，没有规律。
    

举个具体的例子，当 a 和 b 有正相关性时， a\[x\] 假设是 a 中比较大的值，那 b\[x\] 也是 b 中比较大的值。

NumPy 提供了 corrcoef 来计算两个数组的相关性。

假设我们传入两个数组：a 和 b，corrcoef 函数返回的是一个 2 x 2 的数组，从左到右，从上到下依次是: a 和 a 的相关性，a 和 b 的相关性，b 和 a 的相关性，以及 b 和 b 的相关性。

这个 2x2 的数组也称为相关性矩阵。相关性的取值范围为 -1 到 1。**其绝对值代表相关的程度，而正负号则代表是正相关还是负相关。**

#### 正相关性

首先我们来测试一下正相关性的场景，代码示例如下。

```
import numpy as np
np.random.seed(1)
a = np.random.randint(0, 100, 800)
b = a + np.random.normal(0, 10, 800)
print(np.corrcoef(a, b))
```

输出如下：

```
[[1.         0.94577193]
 [0.94577193 1.        ]]
```

从相关性矩阵的定义可以得出，右上角就是 a 和 b 的相关性，94% 代表高度相关。毕竟我们的 b 只是在 a 的基础上加了一些干扰项。

a 和 b 正相关，如果我们以 a 为横轴，b 为竖轴，将两个数组的点拼起来成一个点集，比如第一个点是 x = a\[0\], y = b\[0\] 以此类推。就可以发现当 a 的值大时，b 的值也大，a 的值小时，b 的值也小，整个点集的形状就会近似一条斜率为 1 的直线。

代码如下（画图技术下一部分才会学习，这里只看图就好）

```
import matplotlib 
import matplotlib.pyplot as plt
matplotlib.style.use("ggplot")
plt.scatter(a, b,c='b' )
plt.show()
```

输出如下：

![Drawing 7.png](https://s0.lgstatic.com/i/image6/M01/43/41/Cgp9HWC4dg-AR_R2AAFi7uhyXBI823.png)

#### 负相关性

负相关性从之前的例子可以知道，简单地来说就是 a 大的时候，b 要小。我们可以简单给 b 乘以 -1 就可以模拟出负相关的场景，代码如下：

```
b = b*-1
print(np.corrcoef(a, b))
```

输出如下：

```
[[ 1.         -0.94577193]
 [-0.94577193  1.        ]]
```

可以看到，相关性还是 94%，但这次是 -94%。 相关性系数的符号决定是了正相关还是负相关。

画成图的话，负相关的样子就是 a 大的时候，b 小。图像的形状就会很像正相关的曲线旋转90度的样子。代码如下：

```
plt.scatter(a, b,c='b' )
plt.show()
```

输出如下：

![Drawing 8.png](https://s0.lgstatic.com/i/image6/M01/43/41/Cgp9HWC4dhiANv_6AAFmnE60BlU818.png)

#### 无相关性

无相关性就很容易理解了，就是 a 和 b 一点关系都没有，a 大的时候，b 可能大，也可能小。我们只需要直接生成两个两个随机数组基本就是无相关性的数组。代码如下：

```
a =np.random.randint(0, 100, 800)
b = np.random.randint(0, 50, 800)
print(np.corrcoef(a, b))
```

输出如下：

```
[[1.         0.02301641]
 [0.02301641 1.        ]]
```

相关性是 0.02, 从绝对值来看非常小，基本可以认为没有相关性了。画成图来看一看。

```
plt.scatter(a, b,c='b' )
plt.show()
```

输出如下：

![Drawing 9.png](https://s0.lgstatic.com/i/image6/M00/43/4A/CioPOWC4diGAFVVMAALDSBjfKik850.png)

可以看到，横竖轴的取值基本没有任何规律，也就是没有相关性。

### 小结

至此，我们今天的内容就学习完毕了，总结一下。

首先，我们学习了常见的计算 NumPy 数组统计学特征的方式。

-   极值计算：amax/amin
    
-   极值差：ptp
    
-   中位数：median
    
-   均值：mean
    
-   方差与标准差：var 和 std
    

上述方案都支持传入 axis 参数来沿着某个轴计算子数组的值。

然后，我们学习了线性代码的常见操作。

-   矩阵的创建：np.matrix
    
-   矩阵的基础属性：mean、std、sum （同 ndarray）与 T（转置）
    
-   矩阵的常见运算：加减乘直接使用 +/-/\* 运算符进行，乘法需要两个矩阵的形状满足乘法的规则
    
-   矩阵求逆：首先使用 np.linalg.det 计算行列式，不等于 0 则可以使用 np.linalg.inv 计算逆矩阵
    
-   求解线性方程组：np.resolve
    

最后，我们单独讲解了数值分析中一种常见分析方法——相关性分析。通过调用 np.corrcoef 可以计算出相关性矩阵。从相关性矩阵中，通过绝对值来判断相关性的程度，通过正负号来判断是正相关还是负相关。

课后作业：

使用 NumPy，求解以下方程组的 x1,x2,x3

![202163-142252.png](https://s0.lgstatic.com/i/image6/M00/43/4A/CioPOWC4di-AH9UOAAAuzoYAmW8038.png)

___

答案：

```
A = np.matrix("7 10 -9; 1 3 5 ; 5 1 -3")
b = np.matrix("12;-9;11")
x = np.linalg.solve(A, b)
print(x)
```
