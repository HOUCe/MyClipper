# [Python中时间与时间戳之间的转换_null的专栏-CSDN博客_python 时间转时间戳](https://blog.csdn.net/google19890102/article/details/51355282)

对于时间数据，如`2016-05-05 20:28:54`，有时需要与[时间戳](https://so.csdn.net/so/search?q=%E6%97%B6%E9%97%B4%E6%88%B3)进行相互的运算，此时就需要对两种形式进行转换，在Python中，转换时需要用到`time`模块，具体的操作有如下的几种：

-   将时间转换为时间戳
-   重新格式化时间
-   时间戳转换为时间
-   获取当前时间及将其转换成时间戳

## 1、将时间转换成时间戳

将如上的时间`2016-05-05 20:28:54`转换成时间戳，具体的操作过程为：

-   利用`strptime()`函数将时间转换成时间数组
-   利用`mktime()`函数将时间数组转换成时间戳

```
#coding:UTF-8
import time

dt = "2016-05-05 20:28:54"

#转换成时间数组
timeArray = time.strptime(dt, "%Y-%m-%d %H:%M:%S")
#转换成时间戳
timestamp = time.mktime(timeArray)

print timestamp
```

## 2、重新格式化时间

重新格式化时间需要以下的两个步骤：

-   利用`strptime()`函数将时间转换成时间数组
-   利用`strftime()`函数重新格式化时间

```
#coding:UTF-8
import time

dt = "2016-05-05 20:28:54"

#转换成时间数组
timeArray = time.strptime(dt, "%Y-%m-%d %H:%M:%S")
#转换成新的时间格式(20160505-20:28:54)
dt_new = time.strftime("%Y%m%d-%H:%M:%S",timeArray)

print dt_new

```

## 3、将时间戳转换成时间

在时间戳转换成时间中，首先需要将时间戳转换成localtime，再转换成时间的具体格式：

-   利用`localtime()`函数将时间戳转化成localtime的格式
-   利用`strftime()`函数重新格式化时间

```
#coding:UTF-8
import time

timestamp = 1462451334

#转换成localtime
time_local = time.localtime(timestamp)
#转换成新的时间格式(2016-05-05 20:28:54)
dt = time.strftime("%Y-%m-%d %H:%M:%S",time_local)

print dt

```

## 4、按指定的格式获取当前时间

利用`time()`获取当前时间，再利用`localtime()`函数转换为localtime，最后利用`strftime()`函数重新格式化时间。

```
#coding:UTF-8
import time

#获取当前时间
time_now = int(time.time())
#转换成localtime
time_local = time.localtime(time_now)
#转换成新的时间格式(2016-05-09 18:59:20)
dt = time.strftime("%Y-%m-%d %H:%M:%S",time_local)

print dt
```
