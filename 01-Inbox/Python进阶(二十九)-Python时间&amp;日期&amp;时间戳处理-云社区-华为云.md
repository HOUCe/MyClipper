# [Python进阶(二十九)-Python时间&amp;日期&amp;时间戳处理-云社区-华为云](https://bbs.huaweicloud.com/blogs/detail/232637)

#Python进阶(二十九)-Python时间&日期&时间戳处理  
##将字符串的时间转换为时间戳  
方法:

```
import time
#将其转换为时间数组
a = "2013-10-10 23:40:00"
timeArray = time.strptime(a, "%Y-%m-%d %H:%M:%S")
#转换为时间戳
timeStamp = int(time.mktime(timeArray))
#1381419600
print(timeStamp)

  
```

##字符串格式更改  
  如a = “2013-10-10 23:40:00”,想改为 a = “2013/10/10 23:40:00”  
  方法:先转换为时间数组,然后转换为其他格式

```
a = "2013-10-10 23:40:00"
timeArray = time.strptime(a, "%Y-%m-%d %H:%M:%S")
otherStyleTime = time.strftime("%Y/%m/%d %H:%M:%S", timeArray)
#2013/10/10 23:40:00
print(otherStyleTime)

  
```

##时间戳转换为指定格式日期  
###方法一:  
  利用localtime()转换为时间数组,然后格式化为需要的格式,如

```
timeStamp = 1381419600
timeArray = time.localtime(timeStamp)
otherStyleTime=time.strftime("%Y-%m-%d %H:%M:%S", timeArray)
#2013-10-10 23:40:00
print(otherStyleTime)

  
```

###方法二:

```
import datetime
timeStamp = 1381419600
dateArray = datetime.datetime.utcfromtimestamp(timeStamp)
otherStyleTime = dateArray.strftime("%Y-%m-%d %H:%M:%S")
#2013-10-10 15:40:00
print(otherStyleTime)

  
```

  注意：使用此方法时必须先设置好时区，否则有时差  
##获取当前时间并转换为指定日期格式  
###方法一:

```
import time
timeStamp = 1381419600
#获得当前时间时间戳
now = int(time.time())
#转换为其他日期格式,如:"%Y-%m-%d %H:%M:%S"
timeArray = time.localtime(timeStamp)
otherStyleTime = time.strftime("%Y-%m-%d %H:%M:%S", timeArray)
#2013-10-10 23:40:00
print(otherStyleTime)

  
```

###方法二:

```
import datetime
#获得当前时间
now = datetime.datetime.now()
#转换为指定的格式
otherStyleTime = now.strftime("%Y-%m-%d %H:%M:%S")
#2017-04-06 16:13:32
print(otherStyleTime)

  
```

##获得三天前的时间  
  方法:

```
import time
import datetime
#先获得时间数组格式的日期
threeDayAgo=(datetime.datetime.now() - datetime.timedelta(days = 3))
#转换为时间戳
# timeStamp = int(time.mktime(threeDayAgo.timetuple()))
#转换为其他字符串格式
otherStyleTime = threeDayAgo.strftime("%Y-%m-%d %H:%M:%S")
#2017-04-03 16:15:56
print(otherStyleTime)

  
```

  注:timedelta()的参数有:days,hours,seconds,microseconds  
##给定时间戳,计算该时间的几天前时间

```
import datetime
import time
timeStamp = 1381419600
#先转换为datetime
dateArray = datetime.datetime.utcfromtimestamp(timeStamp)
threeDayAgo = dateArray - datetime.timedelta(days = 3)
#2013-10-07 15:40:00
print(threeDayAgo)

  
```

##给定日期字符串，直接转换为datetime对象

```
dateStr = '2013-10-10 23:40:00'
datetimeObj=datetime.datetime.strptime(dateStr, "%Y-%m-%d %H:%M:%S")

  
```

  注：将字符串日期转换为datetime后可以很高效的进行统计操作，因为转换为datetime后，可以通过datetime.timedelta()方法来前后移动时间，效率很高，而且可读性很强。  
##计算两个datetime之间的差距

```
a = datetime.datetime(2014,12,4,1,59,59)
b = datetime.datetime(2014,12,4,3,59,59)
diffSeconds = (b-a).total_seconds()
#7200.0
print(diffSeconds)

  
```

  注：time.strftime，time.strptime，datetime.timedelta

##Python time strftime()方法  
  Python time strftime() 函数接收以时间元组，并返回以可读字符串表示的当地时间，格式由参数format决定。  
time.strftime(format\[, t\])  
format – 格式字符串。  
t – 可选的参数t是一个struct\_time对象。  
返回以可读字符串表示的当地时间。  
以下实例展示了 strftime() 函数的使用方法：

```

import time

t = (2009, 2, 17, 17, 3, 38, 1, 48, 0)
t = time.mktime(t)
print time.strftime("%b %d %Y %H:%M:%S", time.gmtime(t))

  
```

  以上实例输出结果为：  
Feb 17 2009 09:03:38  
##Python time strptime()方法  
  Python time strptime() 函数根据指定的格式把一个时间字符串解析为时间元组。  
time.strptime(string\[, format\])  
string – 时间字符串。  
format – 格式化字符串。  
返回struct\_time对象。  
以下实例展示了 strptime() 函数的使用方法：

```

import time

struct_time = time.strptime("30 Nov 00", "%d %b %y")
print "returned tuple: %s " % struct_time

  
```

  以上实例输出结果为：  
returned tuple: (2000, 11, 30, 0, 0, 0, 3, 335, -1)  
##datetime.timedelta  
  datetime.timedelta对象代表两个时间之间的的时间差，两个date或datetime对象相减时可以返回一个timedelta对象。  
构造函数：  
class datetime.timedelta(\[days\[, seconds\[, microseconds\[, milliseconds\[, minutes\[, hours\[, weeks\]\]\]\]\]\]\])  
所有参数可选，且默认都是0，参数的值可以是整数，浮点数，正数或负数。

!\[这里写图片描述\](https://img-blog.csdnimg.cn/img\_convert/46cc348062c27bf57424afe162b04ab4.png) !\[这里写图片描述\](https://img-blog.csdnimg.cn/img\_convert/f9c024e20306fb0e4e3e84a15aab3217.png)

文章来源: shq5785.blog.csdn.net，作者：No Silver Bullet，版权归原作者所有，如需转载，请联系作者。

原文链接：shq5785.blog.csdn.net/article/details/69396305

【版权声明】本文为华为云社区用户转载文章，如果您发现本社区中有涉嫌抄袭的内容，欢迎发送邮件至：[cloudbbs@huaweicloud.com](mailto:cloudbbs@huaweicloud.com)进行举报，并提供相关证据，一经查实，本社区将立刻删除涉嫌侵权内容。
