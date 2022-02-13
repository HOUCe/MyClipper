---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


在很多数据分析任务中，经常会遇到处理时间相关的数据。比如电商网站经常需要根据下单记录来分析不同时间段的商品偏好，以此来决定网站不同时间段的促销信息；又或者是通过对过去十年的金融市场的数据进行分析，来预测某个细分版本的未来走势。在这些任务中，时间信息的处理都是重中之重。

时间数据的处理不同于对常见的数字、字符串等数据的处理方式，时间数据处理起来往往会比较复杂。

比如数据表中有一个表示时间的字符串："2018/02/01"，我们希望提取其年、月、日，就需要去解析，分割该字符串。而往往我们会遇到各种不同格式的表示，比如"01/02/2018"，或者 "2018-2-1"， 等等。如果要完全实现针对不同格式的兼容，往往需要书写大量琐碎的代码。而这还只是最简单的提取年月日。其他比如时间的加减，都不是简单就能够完成的。

pandas 作为数据分析最强大的工具集，自然也提供了一套非常强大的处理时间数据的工具，本讲我们就来具体介绍。

### 核心概念：时间和时间序列

pandas 提供了丰富的处理时间的工具和类，其中最常用的有以下几种。

-   **Timestamp**：代表某一个时间点。比如用户某个购物订单下单的时间，或者某次网页点击的时间。
    
-   **DatetimeIndex**：代表一个时间点的序列，换句话说就是多个 Timestamp 构成的列表。DatetimeIndex 可以作为 Series 和 DataFrame 的索引。
    
-   **Timedelta**：单个时长。比如 2 个小时，4 分钟等都算时长，时长具有不同的单位，常见的单位有天、时、分、秒等等。本质上，时长代表两点时间点（Timestamp）的距离。
    
-   **TimedeltaIndex**：多个时长数据的序列。类似 DatetimeIndex 和 Timestamp 的关系。TimedeltaIndex 就是多个 Timedelat 组成的列表，也可以作为 Series 和 DataFrame 的索引。
    
-   **DataOffset**：时间在日历维度的偏移。比如 2018 年 2 月 1 日早上 6 点，在日历上偏移一点就是 2018 年 1 月 31 日早上 6 点。DataOffset 提供了各种方便的偏移方式，比如按照工作日偏移。星期五早上 10 点，偏移一个工作日，可以自动返回下周一早上 10 点。
    

在使用 pandas 做时间处理的时候，最常见的场景就是：

1.  将来自数据源的时间描述（比如字符串或者整型）等表示，转化为 Timestamp类型；
    
2.  使用 Timestamp 类型来访问时间的各种属性，比如年月日、星期几等；
    
3.  使用 Timestamp 配合 Timedelta 来做时间相关的计算和加减等，如果是在日历维度的计算，则配合 DataOffset 一起使用；
    
4.  如果需要从时间的维度来筛选 DataFrame 里的记录，则需要先将时间列设置为 DatetimeIndex， 然后按照普通索引的用法通过时间来筛选。
    

接下来，我来逐一介绍下这 4 种场景的实现方式。

首先我们创建 chapter15 的文件夹，用 VS code 打开，并新建 chapter15.ipynb，保存到该文件夹中。

### 时间数据的解析

时间数据的解析本质就是将各种不同类型的时间表示都统一转换为 pandas 的 Timestamp 类型。因为只有转换为 Timestamp 之后才能进行后续的操作。

pandas 提供了 to\_datetime 方法，来将各种不同类型的时间数据转换为 Timestamp 类型。

#### （1）解析字符串

字符串是最常见的数据源中存储时间的方式，to\_datetime 函数近乎支持所有主流的时间字符串标记法，比如：

```
import pandas as pd
pd_time = pd.to_datetime("2018-08-29 17:17:22")
print(type(pd_time),pd_time)
pd_time1 = pd.to_datetime("2018-08-29 5:17pm")
print(type(pd_time1), pd_time1)
pd_time2 = pd.to_datetime("08/29/2018")
print(type(pd_time2), pd_time2)
pd_time3 = pd.to_datetime("Aug 29, 2018")
print(type(pd_time3), pd_time3)
```

执行之后，输出：

```
<class 'pandas._libs.tslibs.timestamps.Timestamp'> 2018-08-29 
 2018-08-29 17:17:00
<class 'pandas._libs.tslibs.timestamps.Timestamp'> 2018-08-29 00:00:00
<class 'pandas._libs.tslibs.timestamps.Timestamp'> 2018-08-29 00:00:00
<class 'pandas._libs.tslibs.timestamps.Timestamp'> 2018-08-29 00:00:00
```

从上面输出的结果可以看到，to\_datetime 函数返回的是 Timestamp 类型。并且该函数默认就支持从常见的用字符串表示的时间格式中解析出 Timestamp 结构。

如果我们想解析的时间字符串不是常见的类型呢？比如中文环境中，类似“2018 年 8 月 29 日”这样的表示方法还是会经常遇到的。答案是可以的。

to\_datetime 支持我们自定义时间格式字符串来进行解析。在时间格式字符串中，%Y 表示年份，%m 代表月，%d 代表日。

比如要解析刚才的中文时间，对应的格式字符串就是: "%Y年%月%日"。代码如下：

```
pd_time4 = pd.to_datetime("2018年8月29日", format="%Y年%m月%d日")
print(type(pd_time4), pd_time4)
```

执行之后，输出如下。因为我们没有指定时分秒，所以这个部分默认为 0 。

```
<class 'pandas._libs.tslibs.timestamps.Timestamp'> 2018-08-29 00:00:00
```

#### （2）解析整型/浮点型时间戳

在很多数据系统中，时间也经常以时间戳的形式存在。时间戳一般指的是 1970 年 1 月 1 日到某个时间点的秒数。比如一个特定的时间点：北京时间的 2021-05-09 21:06:44， 对应的时间戳就是：1620565604，代表从 1970 年 1 月 1 日零时零分零秒到 2021 年 5 月 29 日下午 9 点 6 分 44 秒一共有 1620565604 秒。

to\_datetime 同样支持直接将时间戳转换为 Timestamp 类型，用法如下：

```
time_value = 1620565604
pd_time5 = pd.to_datetime(time_value, unit="s")
print(type(pd_time5), pd_time5)
```

输出：

```
<class 'pandas._libs.tslibs.timestamps.Timestamp'> 2021-05-09 13:06:44
```

Timestamp 对象已经正确构建，但是为什么是 13 点 06 分，而不是刚才的 21 点 06 分？ 原因是通过 to\_datetime 默认是格林威治时间，也就是零时区，落后北京时间 8 小时。如果算上 8 小时的偏移，13+8 就正好是 21 点 06 分了。如果我们希望在构造 Timestamp 对象时就指定时区，可以调用 tz\_localize 指定。

```
pd_time6 = pd.to_datetime(time_value, unit="s").tz_localize("Asia/Shanghai")
print(type(pd_time6), pd_time6)
```

输出：

```
<class 'pandas._libs.tslibs.timestamps.Timestamp'> 2021-05-09 13:06:44+08:00
```

可以看到，这次输出的内容多了一个 +08:00 代表已经带上了时区。

#### （3）直接构造 Timestamp 对象

除了上述两种方式外，我们可以直接构建 Timestamp 对象。比如通过指定年月日，或者直接获取程序运行的时间。主要包括以下用法：

```
pd_time7 = pd.Timestamp(year=2018, month=8, day=29, hour= 21)
print(type(pd_time7), pd_time7)
pd_time8 = pd.Timestamp("now")
print(type(pd_time8), pd_time8)
```

输出：

```
<class 'pandas._libs.tslibs.timestamps.Timestamp'> 2018-08-29 21:00:00
<class 'pandas._libs.tslibs.timestamps.Timestamp'> 2021-05-09 21:54:38.064474
```

### 时间属性的提取

当我们获取到 Timestamp 对象之后，就可以通过 Timestamp 对象提供的方法来轻松获取各种时间的属性了。常见的属性获取方法如下所示：

```
print("当前时间对象：", pd_time8)
print("星期几，星期一为0：", pd_time8.dayofweek) 
print("星期几，字符串表示：", pd_time8.day_name())
print("一年中的第几天：", pd_time8.dayofyear)
print("这个月的有几天：",pd_time8.daysinmonth)
print("今年是否是闰年", pd_time8.is_leap_year)
print("当前日期是否是本月最后一天", pd_time8.is_month_end)
print("当前日期是否是本月第一天", pd_time8.is_month_start)
print("当前日期是否是本季度最后一天", pd_time8.is_quarter_end)
print("当前日期是否是本季第一天", pd_time8.is_quarter_start)
print("当前日期是否是本年度最后一天", pd_time8.is_year_end)
print("当前日期是否是本年度第一天", pd_time8.is_year_start)
print("当前第几季度：", pd_time8.quarter)
print("当前的时区：", pd_time8.tz)
print("本年第几周：", pd_time8.week)
print("年：", pd_time8.year)
print("月：", pd_time8.month)
print("日：",pd_time8.day)
print("小时：", pd_time8.hour)
print("分钟：", pd_time8.minute)
print("秒：", pd_time8.second)
```

输出：

```
当前时间对象： 2021-05-09 21:54:38.064474
星期几，星期一为0： 6
星期几，字符串表示： Sunday
一年中的第几天： 129
这个月的有几天： 31
今年是否是闰年 False
当前日期是否是本月最后一天 False
当前日期是否是本月第一天 False
当前日期是否是本季度最后一天 False
当前日期是否是本季第一天 False
当前日期是否是本年度最后一天 False
当前日期是否是本年度第一天 False
当前第几季度： 2
当前的时区： None
本年第几周： 18
年： 2021
月： 5
日： 9
小时： 21
分钟： 54
秒： 38
```

使用方法比较直观，这里就不展开解释。

### 时间数据的计算

pandas 中，时间数据的计算值的是时间数据的加减，比如在一个时间点上增加几小时、几分钟、或者几天，几个月来得到加了之后的时间。因为时间并不像数字运算一样简单，而是有很多潜在的规则在里面，比如一分钟 60 秒，一小时 60 分钟，一天 24 小时，一个月可能有 28 天，也可能有 30、31 天，等等，如果我们手写计算逻辑将会非常复杂。

pandas 提供了一套强大的时间计算机制来让我们不用关系背后的规则就能完成时间的计算。pandas 的时间计算是通过 Timestamp 对象和 Timedelta 对象混合运算来实现。Timedelta 可以理解成一个时间段，或者说，时间长度。最常见的运算有以下两种类型：

-   两个 Timestamp 对象相减，可以得到一个 Timedetla 对象；
    
-   一个 Timestamp 对象 加上或者减去一个 Timedelta 对象，可以获得一个新的 Timestamp 对象。
    

所以要实现时间的运算，我们首先要创建 Timedelta 对象。

#### Timedelta 对象的创建

Timedelta 对象和 Timestamp 对象类似，也支持多种形式的创建。

（1） 从字符串来创建

Timedelta 对象支持解析多种描述时长的格式。我们通过代码来展示：

```
delta1 = pd.Timedelta('0.5 days')
print("半天：", delta1)
delta2 = pd.Timedelta("2 days 3 hour 20 minutes")
print("2天零3小时20分钟", delta2)
delta3 = pd.Timedelta("1 days 20:36:00")
print("1天零8小时36分钟：", delta3)
```

执行之后输出：

```
半天： 0 days 12:00:00
2天零3小时20分钟 2 days 03:20:00
1天零8小时36分钟： 1 days 20:36:00
```

（2）从单元时间创建

除了通过一定格式的字符串来创建 Timedelta 对象之外，我们还可以通过设置函数的参数来创建 Timedelta 对象，比如这样表示：

```
delta4 = pd.Timedelta(days = 1.5)
print("1天半：", delta4)
delta5 = pd.Timedelta(days = 10, hours= 9)
print("十天零九小时：", delta5)
```

输出：

```
1天半： 1 days 12:00:00
十天零九小时： 10 days 09:00:00
```

（3）从时间缩写创建

还有一种简洁的形式来创建 Timedelta，就是通过数字+缩写的形式。缩写主要有以下几种：

-   W：代表周、星期
    
-   D：代表天
    
-   H：代表小时
    
-   M：代表分钟
    
-   S：代表秒
    

具体使用方法如下：

```
delta6 = pd.Timedelta("2W3D")
print("两周零三天：", delta6)
delta7 = pd.Timedelta("6H30M12S")
print("6小时30分钟12秒：", delta7)
```

输出

```
两周零三天： 17 days 00:00:00
6小时30分钟12秒： 0 days 06:30:12
```

#### 执行时间的计算

在学会如何创建 Timedelta 对象之后，要做时间的计算就非常简单了。我们直接上代码：

```
current_time = pd.Timestamp("now")
print("当前时间：", current_time)
two_week_ago = current_time - pd.Timedelta("2W")
print("两周前：", two_week_ago)
future_time = current_time + pd.Timedelta("30D7H")
print("30天零7小时之后的时间：",future_time)
```

执行之后，输出：

```
当前时间： 2021-05-09 23:46:09.346063
两周前： 2021-04-25 23:46:09.346063
30天零7小时之后的时间： 2021-06-09 06:46:09.346063
```

除了计算 Timedelta 和 Timestamp 外，两个 Timestamp 也能相减，得出一个时长（也就是 Timedelta）。

```
national_day = pd.to_datetime("2020-10-01 08:00:00")
delta8 = current_time - national_day
print("距离去年国庆已经过了：", delta8)
```

执行之后，输出：

```
距离去年国庆已经过了： 220 days 15:46:09.346063
```

### 时间数据作为索引

除了两个时间点的各种操作之外， pandas 还支持将时间数据作为索引，这样就能够支持各种时间维度的选择。为什么这个特性非常重要，我们以一个例子来说明。

首先从课程的 [Github 仓库](https://github.com/th-sails/python-data-course?fileGuid=xxQTRXtVcqtHK6j8)的 chapter15 目录中，下载 order\_record.csv 文件，并将其保存在你 chapter15 的工作目录中。

我们首先先将数据集加载出来，看看里面有什么：

```
df_log = pd.read_csv("order_record.csv")
df_log
```

输出：  
![Drawing 0.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmLPWANV81AAEs8yzzvmw295.png)

这是一个电商网站用户购买的记录数据，一共有一千条内容。从最后一列时间列来看，时间跨度在 2018 年 6 月到 11 月都有。

如果我们希望能够方便地进行时间维度的分析，比如查看 9 月 1 日到 9 月 15 日的记录，或者 8 月到 9 月的记录。那可以考虑将 time 一列转化为 DatetimeIndex。这样我们就能够直接对时间进行索引。

#### 设置 DatetimeIndex

将字符串的时间一列转化为 DatetimeIndex， 一般分为两步：第一步首先将时间一列转化为 Timestamp 对象。

```
df_log["time"] = pd.to_datetime(df_log["time"])
df_log["time"]
```

执行之后输出：

```
0     2018-08-29 17:17:22.300959410
1     2018-08-29 20:59:58.841378430
2     2018-08-01 19:20:06.479644547
3     2018-08-01 17:25:58.912202131
4     2018-06-02 11:00:51.123221777
                   ...             
995   2018-11-08 11:10:13.586269568
996   2018-11-08 19:10:55.214335543
997   2018-11-08 16:54:37.687285776
998   2018-11-08 17:46:17.253211617
999   2018-06-26 20:38:07.950590072
Name: time, Length: 1000, dtype: datetime64[ns]
```

可以看到，目前 time 列的数据类型已经转换为 Timestamp。

第二步就是将新的 time 这一列设置成索引。

```
df_log.set_index("time", inplace=True)
df_log
```

执行之后，输出：  
![Drawing 1.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmLQGAXxNyAAE6ix-TAKc850.png)

可以看到，现在时间列已经替代了之前默认的数字序号，成为 DataFrame 新的行索引。

现在我们可以查看一下 DataFrame 的索引类型。

输出：

```
DatetimeIndex(['2018-08-29 17:17:22.300959410',
'2018-08-29 20:59:58.841378430',
'2018-08-01 19:20:06.479644547',
'2018-08-01 17:25:58.912202131',
               ...
'2018-11-08 16:54:37.687285776',
'2018-11-08 17:46:17.253211617',
'2018-06-26 20:38:07.950590072'],
              dtype='datetime64[ns]', name='time', length=1000, freq=None)
```

可以看到，目前 df\_log 表的索引就是我们开头介绍的 DatetimeIndex 类型。

#### 基于时间筛选和过滤数据

在设置完 DatetimeIndex 之后，我们在之前提到的根据时间维度筛选就小菜一碟了。我们直接可以使用之前学习的 loc 索引器， 然后在行索引部分以字符串的形式写时间范围（开始时间和结束时间之间以冒号链接），具体用法见代码：

（1）选择从 9 月 1 日到 9 月 15 日的数据

```
df_log.loc["2018-09-01" : "2018-09-15",:]
```

输出（只截取了部分）：  
![Drawing 2.png](https://s0.lgstatic.com/i/image6/M00/40/B2/CioPOWCmLQmAC1TbAAGfv2hpww8974.png)

（2）选择从 8月到9月的数据

```
df_log.loc["2018-08" : "2018-09", :]
```

输出：  
![Drawing 3.png](https://s0.lgstatic.com/i/image6/M01/40/A9/Cgp9HWCmLRCAfoIHAAE_ZC7i9-I121.png)

（3）选择从 8 月 1 日到 9 月 2 日下午两点之前的数据

```
df_log.loc["2018-08-01" : "2018-09-02 14:00:00", :]
```

输出：  
![Drawing 4.png](https://s0.lgstatic.com/i/image6/M00/40/B2/CioPOWCmLRWATUtZAAE9sbrRYvo747.png)

可以看到，当我们把 Timestamp 作为索引时，就可以非常简单地实现各种不同时间范围的筛选，并且时间范围的写法也非常自由。

### 小结

关于时间常用的处理技术至此就学习完毕了，我们来复习一下今天学习的内容。

#### 1\. 基本概念：

pandas 的时间处理体系主要包含这几个类。

-   Timestamp 代表时间点。
    
-   DatetimeIndex 代表多个 Timestamp构成的索引列表。
    
-   Timedelta 代表时间长度，用于做时间的计算
    
-   TimedeltaIndex 用于将 Timedelta做索引，但不常用。
    

#### 2\. 数据的解析：

通过 to\_datetime 函数，可以将各类时间字符串、时间戳等表示形式转换为Timestamp 对象。同时也可以自定义时间格式字符串，用%Y、%m、%d 等格式字符来自定义解析。

#### 3\. 时间属性的提取：

Timestamp 对象提供了丰富的访问时间各种维度信息的能力，比如当前时间是星期几、在一年中是第几天，等等，具体见上面的示例代码。

#### 4\. 时间数据的计算：

在某个时间点上加减时间，需要用 Timedelta 对象来描述时间的长度。同样，Timedelta 对象也能从各种不同的数据生成，比如字符串、单位时间等。Timedelta 同时也可以表示两个 Timestamp 相减后的差。

#### 5\. 时间数据作为索引：

当我们希望从时间维度去筛选数据表中的数据的时候，可以将时间相关的列转换成 DatetimeIndex， 这样可以在行索引中直接写时间范围来筛选数据，非常方便。

学完了本讲，我们 pandas 相关的学习已经进入了尾声，是不是已经迫不及待想要用 pandas 做一个略微复杂的练习了呢？ 下一讲我们将会融合最近几讲学习的内容，完成一个较为完整的数据分析。

思考题

思考一下，Timedelta 为什么不能按月创建？

___

答案：

Timedelta 代表一个绝对的时间长度，而一个月的天数是不固定的。
