# [Python datetime 格式化字符串：strftime()_Shomy的专栏-CSDN博客_dt.strftime](https://blog.csdn.net/shomy_liu/article/details/44141483)

___

Python 的`datetime`模块 其实就是`date`和`time` 模块的结合， 常见的属性方法都比较常用  
比如：  
`datetime.day`,`datetime.month`,`datetime.year` 分别表示一个`datetime`对象的日，月，年；如下

```
from datetime import datetime
dt=datetime.now() #创建一个datetime类对象
print dt.year,dt.month,dt.day

'''
输出为：
2015 3 8
'''
```

下面着重记录一下 另一个方法： `strftime()` 用来格式化`datetime` 对象, 有时候会十分的方便：  
先看一下格式： `datetime.strftime('%' )` %后面跟一个控制字符，如下：

格式符

说明

%a

星期的英文单词的缩写：如星期一， 则返回 `Mon`

%A

星期的英文单词的全拼：如星期一，返回 `Monday`

%b

月份的英文单词的缩写：如一月， 则返回 `Jan`

%B

月份的引文单词的缩写：如一月， 则返回 `January`

%c

返回`datetime`的字符串表示，如`03/08/15 23:01:26`

%d

返回的是当前时间是当前月的第几天

%f

微秒的表示： 范围: \[0,999999\]

%H

以24小时制表示当前小时

%I

以12小时制表示当前小时

%j

返回 当天是当年的第几天 范围\[001,366\]

%m

返回月份 范围\[0,12\]

%M

返回分钟数 范围 \[0,59\]

%P

返回是上午还是下午–`AM` or `PM`

%S

返回秒数 范围 \[0,61\]。。。手册说明的

%U

返回当周是当年的第几周 以**周日**为第一天

%W

返回当周是当年的第几周 以**周一**为第一天

%w

当天在当周的天数，范围为\[0, 6\]，6表示星期天

%x

日期的字符串表示 ：03/08/15

%X

时间的字符串表示 ：23:22:08

%y

两个数字表示的年份 15

%Y

四个数字表示的年份 2015

%z

与utc时间的间隔 （如果是本地时间，返回空字符串）

%Z

时区名称（如果是本地时间，返回空字符串）

___

举几个列子：

```
from datetime import datetime
dt = datetime.now()  
print   '时间：(%Y-%m-%d %H:%M:%S %f): ' , dt.strftime( '%Y-%m-%d %H:%M:%S %f' )  
print   '时间：(%Y-%m-%d %H:%M:%S %p): ' , dt.strftime( '%y-%m-%d %I:%M:%S %p' )  
print   '星期缩写%%a: %s '  % dt.strftime( '%a' )  
print   '星期全拼%%A: %s '  % dt.strftime( '%A' )  
print   '月份缩写%%b: %s '  % dt.strftime( '%b' )  
print   '月份全批%%B: %s '  % dt.strftime( '%B' )  
print   '日期时间%%c: %s '  % dt.strftime( '%c' )  
print   '今天是这周的第%s天 '  % dt.strftime( '%w' )  
print   '今天是今年的第%s天 '  % dt.strftime( '%j' )  
print   '今周是今年的第%s周 '  % dt.strftime( '%U' ) 
print   '今天是当月的第%s天 '  % dt.strftime( '%d' )

'''
输出如下：
--------------------------------------------------------------
时间：(%Y-%m-%d %H:%M:%S %f):  2015-03-08 23:30:42 181000
时间：(%Y-%m-%d %H:%M:%S %p):  15-03-08 11:30:42 PM
星期缩写%a: Sun 
星期全拼%A: Sunday 
月份缩写%b: Mar 
月份全批%B: March 
日期时间%c: 03/08/15 23:30:42 
今天是这周的第0天 
今天是今年的第067天 
今周是今年的第10周 
今天是当月的第08天 
---------------------------------------------------
'''
```

有很多都是常用的说，可以记录一下 以后方便很多; 不用自己再去倒腾时间日期啥的了==
