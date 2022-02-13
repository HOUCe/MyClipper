# [Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位） - Just Code](https://justcode.ikeepstudying.com/2019/10/python-%E6%97%B6%E9%97%B4%E5%A4%84%E7%90%86-%E6%97%B6%E9%97%B4%E6%88%B3-%E6%97%A5%E6%9C%9F%E6%A0%BC%E5%BC%8F%E5%8C%96-%E6%97%A5%E6%9C%9F%E5%92%8C%E6%97%B6%E9%97%B4%E6%88%B3%E4%BA%92%E7%9B%B8/)

[Home](https://justcode.ikeepstudying.com/)[Python / Wxpython](https://justcode.ikeepstudying.com/category/python-2-wxpython/)Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）

## 一、相关术语的解释

___

-   **_UTC time_** Coordinated Universal Time，世界协调时，又称 格林尼治天文时间、世界标准时间。与UTC time对应的是各个时区的local time，东N区的时间比UTC时间早N个小时，因此UTC time + N小时 即为东N区的本地时间；而西N区时间比UTC时间晚N个小时，即 UTC time – N小时 即为西N区的本地时间； 中国在东8区，因此比UTC时间早8小时，可以以UTC+8进行表示。
-   **_epoch time_** 表示时间开始的起点；它是一个特定的时间，不同平台上这个时间点的值不太相同，对于Unix而言，epoch time为 1970-01-01 00:00:00 UTC。
-   **_timestamp（时间戳）_** 也称为Unix时间 或 POSIX时间；它是一种时间表示方式，表示从格林尼治时间1970年1月1日0时0分0秒开始到现在所经过的毫秒数，其值为float类型。 但是有些编程语言的相关方法返回的是秒数（Python就是这样），这个需要看方法的文档说明。需要说明的是时间戳是个差值，其值与时区无关。

## 二、时间的表现形式

___

常见的时间表示形式为：

-   时间戳
-   格式化的时间字符串

Python中还有其它的时间表示形式：

-   time模块的**_time.struct\_time_**
-   datetime模块的**_datetime类_**

关于datetime模块的datetime类会在下面做详细讲解，这里简单说下time.struct\_time。

time.struct\_time包含如下属性：

下标/索引

属性名称

描述

0

tm\_year

年份，如 2017

1

tm\_mon

月份，取值范围为\[1, 12\]

2

tm\_mday

一个月中的第几天，取值范围为\[1-31\]

3

tm\_hour

小时， 取值范围为\[0-23\]

4

tm\_min

分钟，取值范围为\[0, 59\]

5

tm\_sec

秒，取值范围为\[0, 61\]

6

tm\_wday

一个星期中的第几天，取值范围为\[0-6\]，0表示星期一

7

tm\_yday

一年中的第几天，取值范围为\[1, 366\]

8

tm\_isdst

是否为夏令时，可取值为：0 , 1 或 -1

属性值的获取方式有两种：

-   可以把它当做一种特殊的有序不可变序列通过 **_下标/索引_** 获取各个元素的值，如t\[0\]
-   也可以通过 **_.属性名_** 的方式来获取各个元素的值，如t.tm\_year。

需要说明的是struct\_time实例的各个属性都是只读的，不可修改。

## 三、 time模块

___

time模块主要用于时间访问和转换，这个模块提供了各种与时间相关的函数。

### 1\. 函数列表

方法/属性

描述

time.altzone

返回与utc时间的时间差，以秒为单位（西区该值为正，东区该值为负）。其表示的是本地DST 时区的偏移量，只有daylight非0时才使用。

time.clock()

返回当前进程所消耗的处理器运行时间秒数（不包括sleep时间），值为小数；该方法Python3.3改成了time.process\_time()

time.asctime(\[t\])

将一个tuple或struct\_time形式的时间（可以通过gmtime()和localtime()方法获取）转换为一个24个字符的时间字符串，格式为: “Fri Aug 19 11:14:16 2016″。如果参数t未提供，则取localtime()的返回值作为参数。

time.ctime(\[secs\])

功能同上，将一个秒数时间戳表示的时间转换为一个表示当前本地时间的字符串。如果参数secs没有提供或值为None，则取time()方法的返回值作为默认值。ctime(secs)等价于asctime(localtime(secs))

time.time()

返回时间戳（自1970-1-1 0:00:00 至今所经历的秒数）

time.localtime(\[secs\])

返回以指定时间戳对应的本地时间的 struct\_time对象（可以通过下标，也可以通过 .属性名 的方式来引用内部属性）格式

time.localtime(time.time() + n\*3600)

返回n个小时后本地时间的 struct\_time对象格式（可以用来实现类似crontab的功能）

time.gmtime(\[secs\])

返回指定时间戳对应的utc时间的 struct\_time对象格式（与当前本地时间差8个小时）

time.gmtime(time.time() + n\*3600)

返回n个小时后utc时间的 struct\_time对象（可以通过 .属性名 的方式来引用内部属性）格式

time.strptime(time\_str, time\_format\_str)

将时间字符串转换为struct\_time时间对象，如：time.strptime(‘2017-01-13 17:07’, ‘%Y-%m-%d %H:%M’)

time.mktime(struct\_time\_instance)

将struct\_time对象实例转换成时间戳

time.strftime(time\_format\_str\[, struct\_time\_instance\])

将struct\_time对象实例转换成字符串，如果struct\_time\_instance不指定则取当前本地时间对应的time\_struct对象

### 2\. 练习

##### 获取时间戳格式的时间

##### 获取struct\_time格式的时间

`2`

`time.struct_time(tm_year``=``2017``, tm_mon``=``2``, tm_mday``=``4``, tm_hour``=``14``, tm_min``=``2``, tm_sec``=``34``, tm_wday``=``5``, tm_yday``=``35``, tm_isdst``=``0``)`

`4`

`time.struct_time(tm_year``=``2017``, tm_mon``=``2``, tm_mday``=``4``, tm_hour``=``6``, tm_min``=``2``, tm_sec``=``56``, tm_wday``=``5``, tm_yday``=``35``, tm_isdst``=``0``)`

##### 获取字符串格式的时间

`2`

`'Sat Feb 04 14:06:42 2017'`

`4`

`'Sat Feb 04 14:06:47 2017'`

`5`

`>>> time.strftime(``'%Y-%m-%d'``)`

##### 时间戳格式转struct\_time格式时间

`04`

`>>> t2` `=` `time.localtime(t1)`

`06`

`time.struct_time(tm_year``=``2017``, tm_mon``=``2``, tm_mday``=``4``, tm_hour``=``14``, tm_min``=``7``, tm_sec``=``56``, tm_wday``=``5``, tm_yday``=``35``, tm_isdst``=``0``)`

`07`

`>>> t3` `=` `time.gmtime(t1)`

`09`

`time.struct_time(tm_year``=``2017``, tm_mon``=``2``, tm_mday``=``4``, tm_hour``=``6``, tm_min``=``7``, tm_sec``=``56``, tm_wday``=``5``, tm_yday``=``35``, tm_isdst``=``0``)`

##### 字符串格式转struct\_time格式时间

`01`

`>>> time.strptime(``'Sat Feb 04 14:06:42 2017'``)`

`02`

`time.struct_time(tm_year``=``2017``, tm_mon``=``2``, tm_mday``=``4``, tm_hour``=``14``, tm_min``=``6``, tm_sec``=``42``, tm_wday``=``5``, tm_yday``=``35``, tm_isdst``=``-``1``)`

`03`

`>>> time.strptime(``'Sat Feb 04 14:06:42 2017'``,` `'%a %b %d %H:%M:%S %Y'``)`

`04`

`time.struct_time(tm_year``=``2017``, tm_mon``=``2``, tm_mday``=``4``, tm_hour``=``14``, tm_min``=``6``, tm_sec``=``42``, tm_wday``=``5``, tm_yday``=``35``, tm_isdst``=``-``1``)`

`05`

`>>> time.strptime(``'2017-02-04 14:12'``,` `'%Y-%m-%d %H:%M'``)`

`06`

`time.struct_time(tm_year``=``2017``, tm_mon``=``2``, tm_mday``=``4``, tm_hour``=``14``, tm_min``=``12``, tm_sec``=``0``, tm_wday``=``5``, tm_yday``=``35``, tm_isdst``=``-``1``)`

`07`

`>>> time.strptime(``'2017/02/04 14:12'``,` `'%Y/%m/%d %H:%M'``)`

`08`

`time.struct_time(tm_year``=``2017``, tm_mon``=``2``, tm_mday``=``4``, tm_hour``=``14``, tm_min``=``12``, tm_sec``=``0``, tm_wday``=``5``, tm_yday``=``35``, tm_isdst``=``-``1``)`

`09`

`>>> time.strptime(``'201702041412'``,` `'%Y%m%d%H%M'``)`

`10`

`time.struct_time(tm_year``=``2017``, tm_mon``=``2``, tm_mday``=``4``, tm_hour``=``14``, tm_min``=``12``, tm_sec``=``0``, tm_wday``=``5``, tm_yday``=``35``, tm_isdst``=``-``1``)`

#### struct\_time格式转字符串格式时间

`1`

`>>> time.strftime(``'%Y-%m-%d %H:%M'``, time.localtime())`

#### struct\_time格式转时间戳格式时间

`1`

`>>> time.mktime(time.localtime())`

### 3\. 时间格式转换

时间戳格式的时间 与 字符串格式的时间 虽然可以通过ctime(\[secs\])方法进行转换，但是字符串格式不太适应中国国情。因此，整体而言，它们 不能直接进行转换，需要通过struct\_time作为中介，转换关系如下：

![Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）](http://justcode.ikeepstudying.com/wp-content/uploads/2019/10/1063221-20170320110359861-1880368659.png "Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）")

Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）

说明：上面的 ‘%H:%M:%S’ 可以直接用 ‘%X’ 代替。

## 四、 datetime模块

___

datetime模块提供了处理日期和时间的类，既有简单的方式，又有复杂的方式。它虽然支持日期和时间算法，但其实现的重点是为输出格式化和操作提供高效的属性提取功能。

### _1\. datetime模块中定义的类_

datetime模块定义了以下几个类：

类名称

描述

datetime.date

表示日期，常用的属性有：year, month和day

datetime.time

表示时间，常用属性有：hour, minute, second, microsecond

datetime.datetime

表示日期时间

datetime.timedelta

表示两个date、time、datetime实例之间的时间间隔，分辨率（最小单位）可达到微秒

datetime.tzinfo

时区相关信息对象的抽象基类。它们由datetime和time类使用，以提供自定义时间的而调整。

datetime.timezone

Python 3.2中新增的功能，实现tzinfo抽象基类的类，表示与UTC的固定偏移量

**_需要说明的是：这些类的对象都是不可变的。_**

##### 类之间的关系：

### 2\. datetime模块中定义的常量

常量名称

描述

datetime.MINYEAR

datetime.date或datetime.datetime对象所允许的年份的最小值，值为1

datetime.MAXYEAR

datetime.date或datetime.datetime对象所允许的年份的最大值，只为9999

### 3\. datetime.date类

#### datetime.date类的定义

`1`

`class` `datetime.date(year, month, day)`

year, month 和 day都是是必须参数，各参数的取值范围为：

参数名称

取值范围

year

\[MINYEAR, MAXYEAR\]

month

\[1, 12\]

day

\[1, 指定年份的月份中的天数\]

#### 类方法和属性

类方法/属性名称

描述

date.max

date对象所能表示的最大日期：9999-12-31

date.min

date对象所能表示的最小日志：00001-01-01

date.resoluation

date对象表示的日期的最小单位：天

date.today()

返回一个表示当前本地日期的date对象

date.fromtimestamp(timestamp)

根据跟定的时间戳，返回一个date对象

#### 对象方法和属性

对象方法/属性名称

描述

d.year

年

d.month

月

d.day

日

d.replace(year\[, month\[, day\]\])

生成并返回一个新的日期对象，原日期对象不变

d.timetuple()

返回日期对应的time.struct\_time对象

d.toordinal()

返回日期是是自 0001-01-01 开始的第多少天

d.weekday()

返回日期是星期几，\[0, 6\]，0表示星期一

d.isoweekday()

返回日期是星期几，\[1, 7\], 1表示星期一

d.isocalendar()

返回一个元组，格式为：(year, weekday, isoweekday)

d.isoformat()

返回‘YYYY-MM-DD’格式的日期字符串

d.strftime(format)

返回指定格式的日期字符串，与time模块的strftime(format, struct\_time)功能相同

#### 实例

`02`

`>>>` `from` `datetime` `import` `date`

`05`

`datetime.date(``9999``,` `12``,` `31``)`

`11`

`datetime.date(``2017``,` `2``,` `4``)`

`12`

`>>> date.fromtimestamp(time.time())`

`13`

`datetime.date(``2017``,` `2``,` `4``)`

`23`

`datetime.date(``2016``,` `2``,` `4``)`

`25`

`>>> d.replace(``2016``,` `3``,` `2``)`

`26`

`datetime.date(``2016``,` `3``,` `2``)`

`28`

`time.struct_time(tm_year``=``2017``, tm_mon``=``2``, tm_mday``=``4``, tm_hour``=``0``, tm_min``=``0``, tm_sec``=``0``, tm_wday``=``5``, tm_yday``=``35``, tm_isdst``=``-``1``)`

`40`

`'Sat Feb  4 00:00:00 2017'`

`41`

`>>> d.strftime(``'%Y/%m/%d'``)`

### 4\. datetime.time类

#### time类的定义

```
class datetime.time(hour, [minute[, second, [microsecond[, tzinfo]]]])
```

hour为必须参数，其他为可选参数。各参数的取值范围为：：

参数名称

取值范围

hour

\[0, 23\]

minute

\[0, 59\]

second

\[0, 59\]

microsecond

\[0, 1000000\]

tzinfo

tzinfo的子类对象，如timezone类的实例

#### 类方法和属性

类方法/属性名称

描述

time.max

time类所能表示的最大时间：time(23, 59, 59, 999999)

time.min

time类所能表示的最小时间：time(0, 0, 0, 0)

time.resolution

时间的最小单位，即两个不同时间的最小差值：1微秒

#### 对象方法和属性

对象方法/属性名称

描述

t.hour

时

t.minute

分

t.second

秒

t.microsecond

微秒

t.tzinfo

返回传递给time构造方法的tzinfo对象，如果该参数未给出，则返回None

t.replace(hour\[, minute\[, second\[, microsecond\[, tzinfo\]\]\]\])

生成并返回一个新的时间对象，原时间对象不变

t.isoformat()

返回一个‘HH:MM:SS.%f’格式的时间字符串

t.strftime()

返回指定格式的时间字符串，与time模块的strftime(format, struct\_time)功能相同

#### 实例

`01`

`>>>` `from` `datetime` `import` `time`

`04`

`datetime.time(``23``,` `59``,` `59``,` `999999``)`

`08`

`datetime.timedelta(``0``,` `0``,` `1``)`

`10`

`>>> t` `=` `time(``20``,` `5``,` `40``,` `8888``)`

`22`

`datetime.time(``21``,` `5``,` `40``,` `8888``)`

`25`

`>>> t.strftime(``'%H%M%S'``)`

`27`

`>>> t.strftime(``'%H%M%S.%f'``)`

### 5\. datetime.datetime类

#### datetime类的定义

```
class datetime.datetime(year, month, day, hour=0, minute=0, second=0, microsecond=0, tzinfo=None)
```

year, month 和 day是必须要传递的参数， tzinfo可以是None或tzinfo子类的实例。

各参数的取值范围为：

参数名称

取值范围

year

\[MINYEAR, MAXYEAR\]

month

\[1, 12\]

day

\[1, 指定年份的月份中的天数\]

hour

\[0, 23\]

minute

\[0, 59\]

second

\[0, 59\]

microsecond

\[0, 1000000\]

tzinfo

tzinfo的子类对象，如timezone类的实例

如果一个参数超出了这些范围，会引起ValueError异常。

#### 类方法和属性

类方法/属性名称

描述

datetime.today()

返回一个表示当前本期日期时间的datetime对象

datetime.now(\[tz\])

返回指定时区日期时间的datetime对象，如果不指定tz参数则结果同上

datetime.utcnow()

返回当前utc日期时间的datetime对象

datetime.fromtimestamp(timestamp\[, tz\])

根据指定的时间戳创建一个datetime对象

datetime.utcfromtimestamp(timestamp)

根据指定的时间戳创建一个datetime对象

datetime.combine(date, time)

把指定的date和time对象整合成一个datetime对象

datetime.strptime(date\_str, format)

将时间字符串转换为datetime对象

#### 对象方法和属性

对象方法/属性名称

描述

dt.year, dt.month, dt.day

年、月、日

dt.hour, dt.minute, dt.second

时、分、秒

dt.microsecond, dt.tzinfo

微秒、时区信息

dt.date()

获取datetime对象对应的date对象

dt.time()

获取datetime对象对应的time对象， tzinfo 为None

dt.timetz()

获取datetime对象对应的time对象，tzinfo与datetime对象的tzinfo相同

dt.replace(\[year\[, month\[, day\[, hour\[, minute\[, second\[, microsecond\[, tzinfo\]\]\]\]\]\]\]\])

生成并返回一个新的datetime对象，如果所有参数都没有指定，则返回一个与原datetime对象相同的对象

dt.timetuple()

返回datetime对象对应的tuple（不包括tzinfo）

dt.utctimetuple()

返回datetime对象对应的utc时间的tuple（不包括tzinfo）

dt.timestamp()

返回datetime对象对应的时间戳，返回值是一个类似time.time()返回的浮点型值。需要注释的是，该方法是Python 3.3才新增的

dt.toordinal()

同date对象

dt.weekday()

同date对象

dt.isocalendar()

同date对象

dt.isoformat(\[sep\])

返回一个‘%Y-%m-%d%H:%M:%S.%f’格式的字符串

dt.ctime()

等价于time模块的time.ctime(time.mktime(d.timetuple()))

dt.strftime(format)

返回指定格式的时间字符串

#### 实例

`01`

`>>>` `from` `datetime` `import` `datetime, timezone`

`04`

`datetime.datetime(``2017``,` `2``,` `4``,` `20``,` `44``,` `40``,` `556318``)`

`06`

`datetime.datetime(``2017``,` `2``,` `4``,` `20``,` `44``,` `56``,` `572615``)`

`07`

`>>> datetime.now(timezone.utc)`

`08`

`datetime.datetime(``2017``,` `2``,` `4``,` `12``,` `45``,` `22``,` `881694``, tzinfo``=``datetime.timezone.utc)`

`10`

`datetime.datetime(``2017``,` `2``,` `4``,` `12``,` `45``,` `52``,` `812508``)`

`12`

`>>> datetime.fromtimestamp(time.time())`

`13`

`datetime.datetime(``2017``,` `2``,` `4``,` `20``,` `46``,` `41``,` `97578``)`

`14`

`>>> datetime.utcfromtimestamp(time.time())`

`15`

`datetime.datetime(``2017``,` `2``,` `4``,` `12``,` `46``,` `56``,` `989413``)`

`16`

`>>> datetime.combine(date(``2017``,` `2``,` `4``), t)`

`17`

`datetime.datetime(``2017``,` `2``,` `4``,` `20``,` `5``,` `40``,` `8888``)`

`18`

`>>> datetime.strptime(``'2017/02/04 20:49'``,` `'%Y/%m/%d %H:%M'``)`

`19`

`datetime.datetime(``2017``,` `2``,` `4``,` `20``,` `49``)`

`20`

`>>> dt` `=` `datetime.now()`

`22`

`datetime.datetime(``2017``,` `2``,` `4``,` `20``,` `57``,` `0``,` `621378``)`

`41`

`datetime.date(``2017``,` `2``,` `4``)`

`43`

`datetime.time(``20``,` `57``,` `0``,` `621378``)`

`45`

`datetime.time(``20``,` `57``,` `0``,` `621378``)`

`47`

`datetime.datetime(``2017``,` `2``,` `4``,` `20``,` `57``,` `0``,` `621378``)`

`49`

`datetime.datetime(``2016``,` `2``,` `4``,` `20``,` `57``,` `0``,` `621378``)`

`51`

`time.struct_time(tm_year``=``2017``, tm_mon``=``2``, tm_mday``=``4``, tm_hour``=``20``, tm_min``=``57``, tm_sec``=``0``, tm_wday``=``5``, tm_yday``=``35``, tm_isdst``=``-``1``)`

`53`

`time.struct_time(tm_year``=``2017``, tm_mon``=``2``, tm_mday``=``4``, tm_hour``=``20``, tm_min``=``57``, tm_sec``=``0``, tm_wday``=``5``, tm_yday``=``35``, tm_isdst``=``0``)`

`61`

`'2017-02-04T20:57:00.621378'`

`62`

`>>> dt.isoformat(sep``=``'/'``)`

`63`

`'2017-02-04/20:57:00.621378'`

`64`

`>>> dt.isoformat(sep``=``' '``)`

`65`

`'2017-02-04 20:57:00.621378'`

`67`

`'Sat Feb  4 20:57:00 2017'`

`68`

`>>> dt.strftime(``'%Y%m%d %H:%M:%S.%f'``)`

`69`

`'20170204 20:57:00.621378'`

### 6\. 使用datetime.datetime类对时间戳与时间字符串进行转换

![Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）](http://justcode.ikeepstudying.com/wp-content/uploads/2019/10/1063221-20170205083810667-126583354.png "Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）")

Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）

### 7\. datetime.timedelta类

timedelta对象表示连个不同时间之间的差值。如果使用time模块对时间进行算术运行，只能将字符串格式的时间 和 struct\_time格式的时间对象 先转换为时间戳格式，然后对该时间戳加上或减去n秒，最后再转换回struct\_time格式或字符串格式，这显然很不方便。而datetime模块提供的timedelta类可以让我们很方面的对datetime.date, datetime.time和datetime.datetime对象做算术运算，且两个时间之间的差值单位也更加容易控制。  
这个差值的单位可以是：天、秒、微秒、毫秒、分钟、小时、周。

#### datetime.timedelta类的定义

```
class datetime.timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, hours=0, weeks=0)
```

所有参数都是默认参数，因此都是可选参数。参数的值可以是整数或浮点数，也可以是正数或负数。内部值存储days、seconds 和 microseconds，其他所有参数都将被转换成这3个单位：

-   1毫秒转换为1000微秒
-   1分钟转换为60秒
-   1小时转换为3600秒
-   1周转换为7天

然后对这3个值进行标准化，使得它们的表示是唯一的：

-   microseconds : \[0, 999999\]
-   seconds : \[0, 86399\]
-   days : \[-999999999, 999999999\]

#### 类属性

类属性名称

描述

timedelta.min

timedelta(-999999999)

timedelta.max

timedelta(days=999999999, hours=23, minutes=59, seconds=59, microseconds=999999)

timedelta.resolution

timedelta(microseconds=1)

#### 实例方法和属性

实例方法/属性名称

描述

td.days

天 \[-999999999, 999999999\]

td.seconds

秒 \[0, 86399\]

td.microseconds

微秒 \[0, 999999\]

td.total\_seconds()

时间差中包含的总秒数，等价于: td / timedelta(seconds=1)

#### 实例

`03`

`>>> datetime.timedelta(``365``).total_seconds()` `# 一年包含的总秒数`

`05`

`>>> dt` `=` `datetime.datetime.now()`

`06`

`>>> dt` `+` `datetime.timedelta(``3``)` `# 3天后`

`07`

`datetime.datetime(``2017``,` `2``,` `8``,` `9``,` `39``,` `40``,` `102821``)`

`08`

`>>> dt` `+` `datetime.timedelta(``-``3``)` `# 3天前`

`09`

`datetime.datetime(``2017``,` `2``,` `2``,` `9``,` `39``,` `40``,` `102821``)`

`10`

`>>> dt` `+` `datetime.timedelta(hours``=``3``)` `# 3小时后`

`11`

`datetime.datetime(``2017``,` `2``,` `5``,` `12``,` `39``,` `40``,` `102821``)`

`12`

`>>> dt` `+` `datetime.timedelta(hours``=``-``3``)` `# 3小时前`

`13`

`datetime.datetime(``2017``,` `2``,` `5``,` `6``,` `39``,` `40``,` `102821``)`

`14`

`>>> dt` `+` `datetime.timedelta(hours``=``3``, seconds``=``30``)` `# 3小时30秒后` 

`15`

`datetime.datetime(``2017``,` `2``,` `5``,` `12``,` `40``,` `10``,` `102821``)`

## 五、时间格式码

___

_time模块的struct\_time以及datetime模块的datetime、date、time类都提供了strftime()方法，该方法可 以输出一个指定格式的时间字符串。_

具体格式由一系列的格式码（格式字符）组成，Python最终调用的是各个平台C库的strftme()函数，因此各平台对全套格式码的支持会有所不同，具体情况需要参考该平台上的strftime(3)文档。下面列出了C标准（1989版）要求的所有格式码，它们在所有标准C实现的平台上都可以工作：

![Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）](http://justcode.ikeepstudying.com/wp-content/uploads/2019/10/1063221-20170205104434573-1343126060.png "Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）")

Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）

_python中时间日期格式化符号：_

-   %y 两位数的年份表示（00-99）
-   %Y 四位数的年份表示（000-9999）
-   %m 月份（01-12）
-   %d 月内中的一天（0-31）
-   %H 24小时制小时数（0-23）
-   %I 12小时制小时数（01-12）
-   %M 分钟数（00=59）
-   %S 秒（00-59）
-   %a 本地简化星期名称
-   %A 本地完整星期名称
-   %b 本地简化的月份名称
-   %B 本地完整的月份名称
-   %c 本地相应的日期表示和时间表示
-   %j 年内的一天（001-366）
-   %p 本地A.M.或P.M.的等价符
-   %U 一年中的星期数（00-53）星期天为星期的开始
-   %w 星期（0-6），星期天为星期的开始
-   %W 一年中的星期数（00-53）星期一为星期的开始
-   %x 本地相应的日期表示
-   %X 本地相应的时间表示
-   %Z 当前时区的名称
-   %% %号本身

calendar

获取年历

`1`

`# 返回一个多行字符串格式的year年年历，3个月一行，间隔距离为c`

获取日历

`2`

`>>>` `print` `calendar.month(``2017``,` `11``)`

获取一周一行的月日历

`2`

`>>>` `print` `calendar.monthcalendar(``2017``,``11``)`

`3`

`[[``0``,` `0``,` `1``,` `2``,` `3``,` `4``,` `5``], [``6``,` `7``,` `8``,` `9``,` `10``,` `11``,` `12``], [``13``,` `14``,` `15``,` `16``,` `17``,` `18``,` `19``], [``20``,` `21``,` `22``,` `23``,` `24``,` `25``,` `26``], [``27``,` `28``,` `29``,` `30``,` `0``,` `0``,` `0``]]`

获取日期为星期几

`1`

`# 获取2017年11月11日是星期几(0-6代表星期一到星期日)`

`2`

`>>>` `print` `calendar.weekday(``2017``,``11``,``11``)`

datetime

获取当前日期时间

`02`

`>>>` `print` `datetime.datetime.now()`

`03`

`2017``-``11``-``11` `17``:``21``:``56.076882`

`05`

`>>>` `print` `datetime.datetime.now().date()`

`07`

`>>>` `print` `datetime.date.today()`

`10`

`>>>` `print` `datetime.datetime.now().strftime(``"%Y-%m-%d %H:%M:%S"``)`

获取当前日期的后几天/前几天

`02`

`>>>` `print` `datetime.date.today()` `+` `datetime.timedelta(days``=``1``)`

`04`

`>>>` `print` `datetime.date.today()` `+` `datetime.timedelta(``1``)`

`07`

`>>>` `print` `datetime.date.today()` `-` `datetime.timedelta(days``=``1``)`

`09`

`>>>` `print` `datetime.date.today()` `-` `datetime.timedelta(``1``)`

获取本周/本月最后一天及第一天

`01`

`>>> today` `=` `datetime.date.today()`

`03`

`>>>` `print` `today` `-` `datetime.timedelta(today.weekday())`

`06`

`>>>` `print` `today` `+` `datetime.timedelta(``6``-``today.weekday())`

`09`

`>>>` `print` `datetime.date(today.year, today.month,` `1``)`

`12`

`>>> first_day_weekday, last_day_num` `=` `calendar.monthrange(today.year, today.month)`

`13`

`>>>` `print` `datetime.date(today.year, today.month, last_day_num)`

获取当天最小时间/最大时间

`2`

`>>>` `print` `datetime.datetime.combine(datetime.date.today(), datetime.time.``min``)`

`5`

`>>>` `print` `datetime.datetime.combine(datetime.date.today(), datetime.time.``max``)`

`6`

`2017``-``11``-``11` `23``:``59``:``59.999999`

time

获取当前时间戳

获取本地时间

`1`

`>>>` `print` `time.localtime(time.time())`

`2`

`time.struct_time(tm_year``=``2017``, tm_mon``=``11``, tm_mday``=``11``, tm_hour``=``18``, tm_min``=``13``, tm_sec``=``57``, tm_wday``=``5``, tm_yday``=``315``, tm_isdst``=``0``)`

获取格式化时间

`01`

`# 格式化成%Y-%m-%d %H:%M:%S形式`

`02`

`>>>` `print` `time.strftime(``"%Y-%m-%d %H:%M:%S"``, time.localtime())`

`04`

`# 格式化成%a %b %d %H:%M:%S %Y形式`

`05`

`>>>` `print` `time.strftime(``"%a %b %d %H:%M:%S %Y"``, time.localtime())`

`06`

`Mon Nov` `11` `18``:``58``:``40` `2017`

`08`

`>>> format_time` `=` `time.strptime(``"2017-11-11 18:58:39"``,` `"%Y-%m-%d %H:%M:%S"``)`

`09`

`>>>` `print` `time.mktime(format_time)`

各个时间格式之间的转换

-   datetime <=> date
    
    `2`
    
    `>>>` `print` `datetime.datetime.now().date()`
    
    `5`
    
    `>>> today` `=` `datetime.date.today()`
    
    `6`
    
    `>>>` `print` `datetime.datetime.combine(today, datetime.time.``min``)`
    
-   datetime <=> timestamp
    
    `2`
    
    `>>>` `print` `time.mktime(datetime.datetime.now().timetuple())`
    
    `5`
    
    `>>>` `print` `datetime.datetime.fromtimestamp(``1510396755.0``)`
    
-   datetime <=> string
    
    `2`
    
    `>>>` `print` `datetime.datetime.now().strftime(``"%Y-%m-%d %H:%M:%S"``)`
    
    `5`
    
    `>>>` `print` `datetime.datetime.strptime(``"2017-11-11 18:39:15"``,` `"%Y-%m-%d %H:%M:%S"``)`
    
-   datetime <=> timetuple
    
    `2`
    
    `>>>` `print` `datetime.datetime.now().timetuple()`
    
    `3`
    
    `time.struct_time(tm_year``=``2017``, tm_mon``=``11``, tm_mday``=``11``, tm_hour``=``18``, tm_min``=``50``, tm_sec``=``57``, tm_wday``=``5``, tm_yday``=``315``, tm_isdst``=``0``)`
    
    `5`
    
    `>>> time_tuple` `=` `datetime.datetime.now().timetuple()`
    
    `6`
    
    `>>>` `print` `datetime.datetime.fromtimestamp(time.mktime(time_tuple))`
    
-   10时间戳获取方法：
    
    强制转换是直接去掉小数位。
    
-   13位时间戳获取方法：（1）默认情况下python的时间戳是以秒为单位输出的float
    
    通过把秒转换毫秒的方法获得13位的时间戳：
    
    `2`
    
    `millis` `=` `int``(``round``(time.time()` `*` `1000``))`
    
    round()是四舍五入。
    
    （2）
    
    `3`
    
    `current_milli_time` `=` `lambda``:` `int``(``round``(time.time()` `*` `1000``))`
    
    `6`
    
    `>>> current_milli_time()`
    
    13位时间 戳转换成时间：
    
    `2`
    
    `>>> now` `=` `int``(``round``(time.time()``*``1000``))`
    
    `3`
    
    `>>> now02` `=` `time.strftime(``'%Y-%m-%d %H:%M:%S'``,time.localtime(now``/``1000``))`
    
-   Python timestamp to datetime
    
    `1`
    
    `from` `datetime` `import` `datetime`
    
    `3`
    
    `dt_object` `=` `datetime.fromtimestamp(timestamp)`
    
    `4`
    
    `print``(``"dt_object ="``, dt_object)`
    
    `5`
    
    `print``(``"type(dt_object) ="``,` `type``(dt_object))`
    
    output
    
    `1`
    
    `dt_object` `=` `2018``-``12``-``25` `09``:``27``:``53`
    
    `2`
    
    `type``(dt_object)` `=` `<``class` `'datetime.datetime'``>`
    
-   Python datetime to timestamp
    
    `1`
    
    `from` `datetime` `import` `datetime`
    
    `4`
    
    `timestamp` `=` `datetime.timestamp(now)`
    
    `5`
    
    `print``(``"timestamp ="``, timestamp)`
    
-   字符串转日期 Converting string into datetime
    
    `1`
    
    `from` `datetime` `import` `datetime`
    
    `3`
    
    `datetime_object` `=` `datetime.strptime(``'Jun 1 2005  1:33PM'``,` `'%b %d %Y %I:%M%p'``)`
    
-   计算两个datetime之间的差距
    
    `1`
    
    `a` `=` `datetime.datetime(``2014``,``12``,``4``,``1``,``59``,``59``)`
    
    `2`
    
    `b` `=` `datetime.datetime(``2014``,``12``,``4``,``3``,``59``,``59``)`
    
    `3`
    
    `diffSeconds` `=` `(b``-``a).total_seconds()`
    

## 六、总结

___

那么Python中处理时间时，使用time模块好，还是用datetime模块好呢？我觉得还是看个人习惯吧，没有什么绝对的好坏之分。就我个人而言，datetime模块基本上可以满足需要，且用起来确实比较方便。对于time模块，在以下几种情况下比较实用：

-   取当前时间的时间戳时会用到time.time()方法，当然也可以通过datetime.datetime.now().timestamp()来获取，只是显得复杂一点；
-   同样，如果想获取当前本地时间的字符串格式（如生成一个备份文件或日志文件的名称）直接使用time.strtime(format)会更简洁一点，当然也可以通过datetime.datetime.now().strftime(format)来实现；
-   但是，如果需要把一个时间字符串转换为一个时间戳，且使用的是Python 2时，应该使用time模块，因为datetime.timestamp()方法是在Python 3.3才新增的方法。

![Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）](http://justcode.ikeepstudying.com/wp-content/uploads/2019/10/83878c91171338902e0fe0fb97a8c47a.png "Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）")

Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）

![Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）](http://justcode.ikeepstudying.com/wp-content/uploads/2019/10/DatetimeFormatString.png "Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）")

Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）

![Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）](http://justcode.ikeepstudying.com/wp-content/uploads/2019/10/9a365df8872f7e90bb471174d21ae73d.png "Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）")

Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）

![Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）](http://justcode.ikeepstudying.com/wp-content/uploads/2019/10/%E6%97%B6%E9%97%B4%E6%93%8D%E4%BD%9C.png "Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）")

Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）

![Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）](http://justcode.ikeepstudying.com/wp-content/uploads/2019/10/83878c91171338902e0fe0fb97a8c47a.jpg "Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）")

Python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）

本文：[python: 时间处理, 时间戳, 日期格式化, 日期和时间戳互相转换, 时间模块, 日期模块, time, date, php timestamp（10位和13位）](http://justcode.ikeepstudying.com/2019/10/python-%e6%97%b6%e9%97%b4%e5%a4%84%e7%90%86-%e6%97%b6%e9%97%b4%e6%88%b3-%e6%97%a5%e6%9c%9f%e6%a0%bc%e5%bc%8f%e5%8c%96-%e6%97%a5%e6%9c%9f%e5%92%8c%e6%97%b6%e9%97%b4%e6%88%b3%e4%ba%92%e7%9b%b8/)

2541 total views , 2 views today
