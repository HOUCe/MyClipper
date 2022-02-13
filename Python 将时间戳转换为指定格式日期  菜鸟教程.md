# [Python 将时间戳转换为指定格式日期 | 菜鸟教程](https://www.runoob.com/python3/python-timstamp-str.html)

 [![Document 对象参考手册](https://www.runoob.com/images/up.gif) Python3 实例](https://www.runoob.com/python3/python3-examples.html)

给定一个时间戳，将其转换为指定格式的时间。

**注意时区的设置。**

### 当前时间

## 实例 1

import time now = int(time.time()) timeArray = time.localtime(now) otherStyleTime = time.strftime("%Y-%m-%d %H:%M:%S", timeArray) print(otherStyleTime)

执行以上代码输出结果为：

2019\-05\-21 18:02:49

## 实例 2

import datetime now = datetime.datetime.now() otherStyleTime = now.strftime("%Y-%m-%d %H:%M:%S") print(otherStyleTime)

执行以上代码输出结果为：

2019\-05\-21 18:03:48

### 指定时间戳

## 实例 3

import time timeStamp = 1557502800 timeArray = time.localtime(timeStamp) otherStyleTime = time.strftime("%Y-%m-%d %H:%M:%S", timeArray) print(otherStyleTime)

执行以上代码输出结果为：

2019\-05\-10 23:40:00

## 实例 4

import datetime timeStamp = 1557502800 dateArray = datetime.datetime.utcfromtimestamp(timeStamp) otherStyleTime = dateArray.strftime("%Y-%m-%d %H:%M:%S") print(otherStyleTime)

执行以上代码输出结果为：

2019\-05\-10 23:40:00

 [![Document 对象参考手册](https://www.runoob.com/images/up.gif) Python3 实例](https://www.runoob.com/python3/python3-examples.html)
