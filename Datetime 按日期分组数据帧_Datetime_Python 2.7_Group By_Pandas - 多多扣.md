# [Datetime 按日期分组数据帧_Datetime_Python 2.7_Group By_Pandas - 多多扣](http://duoduokou.com/datetime/40886662102961142217.html)

Datetime 按日期分组数据帧

> Datetime 按日期分组数据帧,datetime,python-2.7,group-by,pandas,Datetime,Python 2.7,Group By,Pandas,我有一个包含date列的熊猫数据框。该列的元素类型为pandas.tslib.Timestamp 我希望按日期对数据帧进行分组，但不包括比该日期更精细的时间戳信息（即按日期分组，其中所有2011年2月23日都分组）。我知道如何用SQL来表达这一点，但我对Pandas还是很陌生的 做了一些非常类似的事情，但我不理解代码，它使用datetime对象 从最开始，我甚至不知道如何从Pandas Timestamp对象检索日期。我可以转换为datetime对象，但这似乎非常迂回 根据请求，df.head

我有一个包含

```
date
```

列的熊猫数据框。该列的元素类型为

```
pandas.tslib.Timestamp
```

我希望按日期对数据帧进行分组，但不包括比该日期更精细的时间戳信息（即按日期分组，其中所有

```
2011年2月23日
```

都分组）。我知道如何用SQL来表达这一点，但我对Pandas还是很陌生的 做了一些非常类似的事情，但我不理解代码，它使用

```
datetime
```

对象 从最开始，我甚至不知道如何从Pandas Timestamp对象检索日期。我可以转换为

```
datetime
```

对象，但这似乎非常迂回

___

根据请求，

```
df.head（）
```

的输出：

___

不清楚您是否正在尝试分组和聚合（如在SQL中）或创建带有日期而不是时间戳的索引 如果您正在尝试分组和聚合，您可以这样做：

```
df.groupby(df.set_index('date').index.date).mean()
```

Timeseries索引具有day、date等datetime属性，它们将聚合timed列，因为它是唯一的数字列 如果要创建日期级别的索引，可以执行以下操作：

```
import datetime
df.set_index(['date', df.date.apply(lambda x: datetime.datetime.date(x))], inplace=True)
df.index.names = ['timestamp', 'daydate']
```

___

这将为您提供一个带有时间戳和日期的多索引。如果您不希望索引是永久性的，请删除inplace=参数。您可以使用

```
normalize
```

DatetimeIndex方法（该方法将索引设置为当天午夜）：

___

在0.15中，您可以访问dt属性，因此可以将其写成：

```
g = df.groupby(df['date'].dt.normalize())
```

___

这里欢迎您df.head（）的输出我指的是groupby和aggregate。你的方法似乎比Andy Hayden的方法更通用（即，有效期不超过几天）。谢谢你，我理解，这正是我想要的。另一种选择：

```
pd.DatetimeIndex（df[“date”]）。date
```

。一个优点是，许多您想要分组的常见内容都是内置的：

```
.month
```

、

```
.year
```

、

```
.hour
```

，等等。这种方法似乎忽略了时区，但patrickrm101的不忽略。真的吗？这听起来像一个bug，它会在github上发布一个问题

```
In [11]: df['date']
Out[11]: 
0   2011-12-03 02:48:52
1   2011-12-03 03:00:09
2   2011-12-03 03:04:04
3   2011-12-03 03:04:35
4   2011-12-03 03:04:56
Name: date, dtype: datetime64[ns]

In [12]: pd.DatetimeIndex(df['date']).normalize()
Out[12]: 
<class 'pandas.tseries.index.DatetimeIndex'>
[2011-12-03 00:00:00, ..., 2011-12-03 00:00:00]
Length: 5, Freq: None, Timezone: None
```

```
g = df.groupby(pd.DatetimeIndex(df['date']).normalize())
```

```
g = df.groupby(df['date'].dt.normalize())
```
