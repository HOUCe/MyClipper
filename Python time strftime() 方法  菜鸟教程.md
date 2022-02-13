# [Python time strftime() 方法 | 菜鸟教程](https://www.runoob.com/python/att-time-strftime.html)

___

## 描述

Python time strftime() 函数接收以时间元组，并返回以可读字符串表示的当地时间，格式由参数 format 决定。

## 语法

strftime()方法语法：

time.strftime(format\[, t\])

## 参数

-   format -- 格式字符串。
-   t -- 可选的参数t是一个struct\_time对象。

## 返回值

返回以可读字符串表示的当地时间。

## 说明

python中时间日期格式化符号：

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

## 实例

以下实例展示了 strftime() 函数的使用方法：

## 实例

#!/usr/bin/python  
\# -\*- coding: UTF-8 -\*-

\# 通过导入 \_\_future\_\_ 包来兼容 Python3.x print  
\# 如果使用了 Python3.x 可以删除此行引入  
from \_\_future\_\_ import print\_function

from datetime import datetime

now \= datetime.now() \# current date and time

year \= now.strftime("%Y")  
print("year:", year)

month \= now.strftime("%m")  
print("month:", month)

day \= now.strftime("%d")  
print("day:", day)

time \= now.strftime("%H:%M:%S")  
print("time:", time)

date\_time \= now.strftime("%Y-%m-%d, %H:%M:%S")  
print("date and time:",date\_time)

以上实例输出结果为：

year: 2020 month: 09 day: 25 time: 10:24:28 date and time: 2020\-09\-25, 10:24:28
