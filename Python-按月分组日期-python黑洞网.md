# [Python-按月分组日期-python黑洞网](https://www.pythonheidong.com/blog/article/164701/d95687f53872fd06b971/)

这是一个快速的问题，我一开始就认为它很容易。一个小时后，我不太确定！  
因此，我有一个Python `datetime`对象列表，我想对其进行图形处理。x值是年份和月份，y值是该列表中本月发生的日期对象的数量。  
也许一个例子可以更好地说明这一点（dd / mm / yyyy）：

```
[28/02/2018, 01/03/2018, 16/03/2018, 17/05/2018] 
-> ([02/2018, 03/2018, 04/2018, 05/2018], [1, 2, 0, 1])
```

我的第一次尝试是按照日期和年份简单地分组，大致如下：

```
import itertools
group = itertools.groupby(dates, lambda date: date.strftime("%b/%Y"))
graph = zip(*[(k, len(list(v)) for k, v in group]) # format the data for graphing
```

正如您可能已经注意到的那样，它将仅按列表中已经存在的日期进行分组。在上面的示例中，没有一个日期发生在4月这一事实将被忽略。

接下来，我尝试查找开始和结束日期，并在它们之间的月份中循环：

```
import datetime
data = [[], [],]
for year in range(min_date.year, max_date.year):
    for month in range(min_date.month, max_date.month):
        k = datetime.datetime(year=year, month=month, day=1).strftime("%b/%Y")
        v = sum([1 for date in dates if date.strftime("%b/%Y") == k])
        data[0].append(k)
        data[1].append(v)
```

当然，这仅在`min_date.month`小于时才有效，如果`max_date.month`跨度多年则不一定。另外，它非常难看。

是否有一种优雅的方法？  
提前致谢

**编辑**：要清楚，日期是`datetime`对象，而不是字符串。为了便于阅读，它们在这里看起来像字符串。

  

## 解决方案

___

我建议使用[`pandas`](http://pandas.pydata.org/)：

```
import pandas as pd

dates = ['28/02/2018', '01/03/2018', '16/03/2018', '17/05/2018'] 

s = pd.to_datetime(pd.Series(dates), format='%d/%m/%Y')
s.index = s.dt.to_period('m')
s = s.groupby(level=0).size()

s = s.reindex(pd.period_range(s.index.min(), s.index.max(), freq='m'), fill_value=0)
print (s)
2018-02    1
2018-03    2
2018-04    0
2018-05    1
Freq: M, dtype: int64

s.plot.bar()
```

[![图形](https://i.stack.imgur.com/0yZ3Z.png)](https://i.stack.imgur.com/0yZ3Z.png)

**说明：**

1.  首先[`Series`](http://pandas.pydata.org/pandas-docs/stable/dsintro.html#series)从的列表创建`date`并转换[`to_datetime`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.to_datetime.html)s。
2.  建立`PeriodIndex`者[`Series.dt.to_period`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.dt.to_period.html)
3.  [`groupby`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.groupby.html)通过索引（`level=0`）并通过[`GroupBy.size`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.core.groupby.GroupBy.size.html)
4.  通过[`Series.reindex`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.reindex.html)按[`PeriodIndex`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.PeriodIndex.html)索引的最大值和最小值创建的值来添加缺失时间段
5.  最后的情节，例如条形图- [`Series.plot.bar`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.plot.bar.html)

站长简介:高级软件工程师,曾在阿里云,每日优鲜从事全栈开发工作,利用周末时间开发出本站,欢迎关注我的公众号:程序员总部,交个朋友吧!关注公众号回复python,免费领取 [全套python视频教程](https://www.pythonheidong.com/course_detail/ "爬虫采集 SEO 机器翻译垃圾网站"),关注公众号回复充值+你的账号,免费为您充值1000积分
