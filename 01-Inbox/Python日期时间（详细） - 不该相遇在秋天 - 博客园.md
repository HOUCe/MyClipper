# [Python日期时间（详细） - 不该相遇在秋天 - 博客园](https://www.cnblogs.com/fengyumeng/p/13529277.html)

### 获取当前时间戳

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import time

t \= time.time()

millis1 \= int(t) print('10位时间戳：{}'.format(millis1))

millis2 \= int(t \* 1000) print('13位时间戳：{}'.format(millis2))

![复制代码](https://common.cnblogs.com/images/copycode.gif)

打印结果：  
10位时间戳：1594800086  
13位时间戳：1594800086816

### 格式化时间

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import time

now \= time.strftime("%Y-%m-%d %H:%M:%S") print('当前时间格式化：{}'.format(now))

now2 \= time.strftime('%Y-%m-%d %H:%M:%S',time.localtime(1594368331)) print('指定时间格式化：{}'.format(now2))

![复制代码](https://common.cnblogs.com/images/copycode.gif)

打印结果：  
当前时间格式化：2020-07-15 16:08:57  
指定时间格式化：2020-07-10 16:05:31

格式化符号表：  
%y 两位数的年份表示（00-99）  
%Y 四位数的年份表示（000-9999）  
%m 月份（01-12）  
%d 月内中的一天（0-31）  
%H 24小时制小时数（0-23）  
%I 12小时制小时数（01-12）  
%M 分钟数（00=59）  
%S 秒（00-59）  
%a 本地简化星期名称  
%A 本地完整星期名称  
%b 本地简化的月份名称  
%B 本地完整的月份名称  
%c 本地相应的日期表示和时间表示  
%j 年内的一天（001-366）  
%p 本地A.M.或P.M.的等价符  
%U 一年中的星期数（00-53）星期天为星期的开始  
%w 星期（0-6），星期天为星期的开始  
%W 一年中的星期数（00-53）星期一为星期的开始  
%x 本地相应的日期表示  
%X 本地相应的时间表示  
%Z 当前时区的名称  
%% %号本身

### 格式化时间转时间戳

import time

str \= '2020-07-10 16:05:31' ts \= time.mktime(time.strptime(str,"%Y-%m-%d %H:%M:%S")) print('格式化时间转时间戳：{}'.format(int(ts)))

打印结果：  
格式化时间转时间戳：1594368331

### 时间元组+拆分时间

#### 当前时间元组

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import time
t\_tuple \= time.localtime() print('当前时间元组：{}'.format(t\_tuple)) print('当前年：{}'.format(t\_tuple.tm\_year)) print('当前月：{}'.format(t\_tuple.tm\_mon)) print('当前日：{}'.format(t\_tuple.tm\_mday)) print('当前时：{}'.format(t\_tuple.tm\_hour)) print('当前分：{}'.format(t\_tuple.tm\_min)) print('当前秒：{}'.format(t\_tuple.tm\_sec)) print('当前周几：{}'.format(t\_tuple.tm\_wday)) print('当前是一年中第几天：{}'.format(t\_tuple.tm\_yday)) print('当前是否是夏令时：{}'.format(t\_tuple.tm\_isdst))

![复制代码](https://common.cnblogs.com/images/copycode.gif)

打印结果：  
当前时间元组：time.struct\_time(tm\_year=2020, tm\_mon=7, tm\_mday=15, tm\_hour=16, tm\_min=59, tm\_sec=45, tm\_wday=2, tm\_yday=197, tm\_isdst=0)  
当前年：2020  
当前月：7  
当前日：15  
当前时：16  
当前分：59  
当前秒：45  
当前周几：2  
当前是一年中第几天：197  
当前是否是夏令时：0

#### 时间戳转时间元组

import time

t\_tuple2 \= time.localtime(1594368331) print('时间戳转时间元组：{}'.format(t\_tuple2))

打印结果：  
时间戳转时间元组：time.struct\_time(tm\_year=2020, tm\_mon=7, tm\_mday=10, tm\_hour=16, tm\_min=5, tm\_sec=31, tm\_wday=4, tm\_yday=192, tm\_isdst=0)

#### 格式化时间转时间元组

import time

tts \= time.strptime('2018-09-30 11:32:23', '%Y-%m-%d %H:%M:%S') print('格式化时间转时间元组：{}'.format(tts))

打印结果：  
格式化时间转时间元组：time.struct\_time(tm\_year=2018, tm\_mon=9, tm\_mday=30, tm\_hour=11, tm\_min=32, tm\_sec=23, tm\_wday=6, tm\_yday=273, tm\_isdst=-1)

#### 时间元组转时间戳

import time

tt \= time.mktime((2020, 7, 10, 16, 5, 31, 0, 0, 0)) print('时间元组转时间戳：{}'.format(int(tt)))

打印结果：  
时间元组转时间戳：1594368331

### 获取今天日期

下面两种写法 效果相同 前者是格式化时间的方法 可以传入任意时间戳指定返回任意格式  后者是专门为获取今天日期提供的方法

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import time import datetime

today1 \= time.strftime("%Y-%m-%d") print(today1)

today2 \= datetime.date.today() print(today2)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

打印结果：  
2020-07-15  
2020-07-15

today方法的结果还可以进一步转化成元组：

import datetime

d \= datetime.date.today().timetuple() print(d)

打印结果：  
time.struct\_time(tm\_year=2020, tm\_mon=7, tm\_mday=15, tm\_hour=0, tm\_min=0, tm\_sec=0, tm\_wday=2, tm\_yday=197, tm\_isdst=-1)

### 时间戳转日期格式

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import time import datetime

d1 \= time.strftime('%Y-%m-%d',time.localtime(1594804107)) print('时间戳转日期：{}'.format(d1))

d2 \= datetime.date.fromtimestamp(1594804107) print('时间戳转日期：{}'.format(d2))

![复制代码](https://common.cnblogs.com/images/copycode.gif)

打印结果：  
2020-07-15  
2020-07-15

### 两个日期相差天数

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import datetime

a \= datetime.date(2020, 7, 1)
b \= datetime.date(2020, 7, 15) print('b的日期减去a的日期：{}'.format(b.\_\_sub\_\_(a).days)) print('a的日期减去b的日期：{}'.format(a.\_\_sub\_\_(b).days))

![复制代码](https://common.cnblogs.com/images/copycode.gif)

打印结果：  
b的日期减去a的日期：14  
a的日期减去b的日期：-14

### 获取几天/小时/分 之前/之后的时间

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import datetime # 3小时之后
later\_hours = datetime.datetime.now() + datetime.timedelta(hours=3) print(later\_hours.strftime('%Y-%m-%d %H:%M:%S')) # 3小时之后的时间戳
print(int(later\_hours.timestamp())) # 10分钟之后
later\_minutes = datetime.datetime.now() + datetime.timedelta(minutes=10) print(later\_minutes.strftime('%Y-%m-%d %H:%M:%S')) # 10分钟之后的时间戳
print(int(later\_minutes.timestamp())) # 7天之后
later\_days = datetime.datetime.now() + datetime.timedelta(days=7) print(later\_days.strftime('%Y-%m-%d %H:%M:%S')) # 7天之后的时间戳
print(int(later\_days.timestamp()))

![复制代码](https://common.cnblogs.com/images/copycode.gif)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import datetime # 3小时之前
before\_hours = datetime.datetime.now() - datetime.timedelta(hours=3) print(before\_hours.strftime('%Y-%m-%d %H:%M:%S')) # 3小时之前的时间戳
print(int(before\_hours.timestamp())) # 10分钟之前
before\_minutes = datetime.datetime.now() - datetime.timedelta(minutes=10) print(before\_minutes.strftime('%Y-%m-%d %H:%M:%S')) # 10分钟之前的时间戳
print(int(before\_minutes.timestamp())) # 7天之前
before\_days = datetime.datetime.now() - datetime.timedelta(days=7) print(before\_days.strftime('%Y-%m-%d %H:%M:%S')) # 7天之前的时间戳
print(int(before\_days.timestamp()))

![复制代码](https://common.cnblogs.com/images/copycode.gif)

### 获取上个月开始结束时间

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import datetime # 上个月第一天
date = datetime.datetime.today()
year,month \= date.year,date.month if month == 1:
    startDate \= datetime.date(year-1, 12, 1) else:
    startDate \= datetime.date(year, month-1, 1) print(startDate) # 上个月最后一天
today = datetime.datetime.today()
endDate \= datetime.date(today.year, today.month, 1) - datetime.timedelta(days=1) print(endDate)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

打印结果：  
2020-06-01  
2020-06-30

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import datetime # 上个月开始时间戳
if month == 1:
    startTime \= int(time.mktime(datetime.date(year-1,12,31).timetuple())) else:
    startTime \= int(time.mktime(datetime.date(datetime.date.today().year,datetime.date.today().month-1,1).timetuple())) print(startTime) # 上个月结束时间戳
endTime = int(time.mktime(datetime.date(datetime.date.today().year,datetime.date.today().month,1).timetuple())) - 1
print(endTime)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

打印结果：  
1590940800  
1593532799

### 获取本月开始结束时间

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import datetime import time # 本月第一天
date = datetime.datetime.today()
year,month \= date.year,date.month
startDate \= datetime.date(year, month, 1) print(startDate) # 本月最后一天
today = datetime.datetime.today() if month == 12:
    endDate \= datetime.date(year, month, 31) else:
    endDate \= datetime.date(today.year, today.month + 1, 1) - datetime.timedelta(days=1) print(endDate)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

打印结果：  
2020-07-01  
2020-07-31

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import datetime import time # 本月开始时间戳
startTime = int(time.mktime(datetime.date(datetime.date.today().year,datetime.date.today().month,1).timetuple())) print(startTime) # 本月结束时间戳
if month == 12:
    endTime \= int(time.mktime(datetime.date(datetime.date.today().year + 1,1, 1).timetuple())) - 1
else:
    endTime \= int(time.mktime(datetime.date(datetime.date.today().year,month+1, 1).timetuple())) - 1
print(endTime)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

打印结果：  
1593532800  
1609430399

### 获取本周上周时间

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import datetime import time # 本周一的时间
today = datetime.datetime.today()
monday\_date \= today - datetime.timedelta(today.isoweekday()-1)
t \= monday\_date.strftime('%Y-%m-%d') print(t) # 本周一的时间戳
ts = time.mktime(time.strptime(t,"%Y-%m-%d")) print(int(ts)) # 上周一的时间
last\_monday\_date = today - datetime.timedelta(days=today.weekday()+7)
ta \= last\_monday\_date.strftime('%Y-%m-%d') print(ta) # 上周一的时间戳
tas = time.mktime(time.strptime(ta,"%Y-%m-%d")) print(int(tas)) # 上周的结束时间
last\_over\_date = today - datetime.timedelta(days=today.weekday()+1)
o \= last\_over\_date.strftime('%Y-%m-%d') print(o) # 上周的结束时间戳
oa = time.mktime(time.strptime(o,"%Y-%m-%d"))
oat \= int(oa)+86400-1
print(oat)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

打印结果：  
2020-07-13  
1594569600  
2020-07-06  
1593964800  
2020-07-12  
1594569599

### 获取今年去年时间

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import datetime import time

today \= datetime.datetime.today() # 今年开始时间
start\_year\_date = datetime.datetime(today.year, 1, 1)
syd \= start\_year\_date.strftime('%Y-%m-%d') print(syd) # 今年开始时间戳
print(int(start\_year\_date.timestamp())) # 今年结束时间
end\_year\_date = datetime.datetime(today.year,12,31,23,59,59)
eyd \= end\_year\_date.strftime('%Y-%m-%d') print(eyd) # 今年结束时间戳
print(int(end\_year\_date.timestamp())) # 去年开始时间
last\_year\_date = datetime.datetime(today.year-1, 1, 1)
lyd \= last\_year\_date.strftime('%Y-%m-%d') print(lyd) # 去年开始时间戳
print(int(last\_year\_date.timestamp())) # 去年结束时间
last\_year\_end\_date = datetime.datetime(today.year, 1, 1,23,59,59) - datetime.timedelta(days=1)
lyed \= last\_year\_end\_date.strftime('%Y-%m-%d') print(lyed) # 去年结束时间戳
print(int(last\_year\_end\_date.timestamp()))

![复制代码](https://common.cnblogs.com/images/copycode.gif)

打印结果：  
2020-01-01  
1577808000  
2020-12-31  
1609430399  
2019-01-01  
1546272000  
2019-12-31  
1577807999
