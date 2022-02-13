# [Python计算给定日期位于当年第几周 - ''竹先森゜ - 博客园](https://www.cnblogs.com/zhuminghui/p/12732808.html)

### 一、计算当前时间处于今年的第几周：

**方法一：**

import time print(time.strftime("%W"))  # 索引从0开始

**方法二：**

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import datetime # 得到一个元祖，元素分别为年、当前周数、当前处于周几
t = datetime.datetime.now().isocalendar()  # (2020, 16, 7)
y = t.isocalendar()\[0\] # 2020年
week\_count = t.isocalendar()\[1\] # 第16周
d = t.isocalendar()\[2\] # 周天

![复制代码](https://common.cnblogs.com/images/copycode.gif)

**方法三：**

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import time print(time.localtime())  # time.struct\_time(tm\_year=2020, tm\_mon=4, tm\_mday=19, tm\_hour=18, tm\_min=33, tm\_sec=41, tm\_wday=6, tm\_yday=110, tm\_isdst=0) # 通过索引取得所需的值
print(time.localtime()\[7\])  # 110 一年中的第110天 
print(time.localtime()\[7\]/7+1)  # 一年中的第几周 
 

![复制代码](https://common.cnblogs.com/images/copycode.gif)

### 二、计算指定日期位于当年第几周

![复制代码](https://common.cnblogs.com/images/copycode.gif)

import datetime # 2020年3月8日
y = 2020 m \= 3 d \= 8
print(datetime.datetime(int(y), int(m), int(d)).isocalendar()\[1\])

![复制代码](https://common.cnblogs.com/images/copycode.gif)
