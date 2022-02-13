# [[Python学习笔记-009]Unix时间戳与带时区的时间字符串之间的相互转化 - veli - 博客园](https://www.cnblogs.com/idorax/p/13222242.html)

**问题：**如何使用Python3的datetime模块将下面的时间字符串转化为Unix时间戳？

**2020-07-01 08:15:21+00:00**

显然，这是一个带有时区(time zone)的可供人识别的时间字符串，对应的时区为UTC。我们不妨使用下列时间字符串格式将上面的字符串转换为datetime对象。

 运行如下：

![复制代码](https://common.cnblogs.com/images/copycode.gif)

$ python3
Python 3.5.2 (default, Apr 16 2020, 17:47:17) 
... \>>> 
>>> import datetime \>>> fmt = "%Y-%m-%d %H:%M:%S%z"
>>> s1 = "2020-07-01 08:15:21+00:00"
>>> dtobj = datetime.datetime.strptime(s1, fmt)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module> File "/usr/lib/python3.5/\_strptime.py", line 510, in \_strptime\_datetime
    tt, fraction \= \_strptime(data\_string, format)
  File "/usr/lib/python3.5/\_strptime.py", line 343, in \_strptime
    (data\_string, format))
ValueError: time data '2020-07-01 08:15:21+00:00' does not match format '%Y-%m-%d %H:%M:%S%z'
>>> 

![复制代码](https://common.cnblogs.com/images/copycode.gif)

查看源代码文件/usr/lib/python3.5/\_strptime.py L343：

![复制代码](https://common.cnblogs.com/images/copycode.gif)

   ...<snip>... 182    class TimeRE(dict): 183        """Handle conversion from format directives to regexes."""
   184    
   185        def \_\_init\_\_(self, locale\_time=None): 186            """Create keys/values.
   187    
   188            Order of execution is important for dependency reasons.
   189    
   190 """
   191            if locale\_time: 192                self.locale\_time = locale\_time 193            else: 194                self.locale\_time = LocaleTime() 195            base = super() 196            base.\_\_init\_\_({ 197                # The " \\d" part of the regex is to make %c from ANSI C work
   198                'd': r"(?P<d>3\[0-1\]|\[1-2\]\\d|0\[1-9\]|\[1-9\]| \[1-9\])",
   ...<snip>... 213                'z': r"(?P<z>\[+-\]\\d\\d\[0-5\]\\d)",
   ...<snip>... 302    def \_strptime(data\_string, format="%a %b %d %H:%M:%S %Y"): 303        """Return a 2-tuple consisting of a time struct and an int containing
   304        the number of microseconds based on the input string and the
   305        format string.""" ...<snip>... 340        found = format\_regex.match(data\_string) 341        if not found: 342            raise ValueError("time data %r does not match format %r" %
   343 (data\_string, format))
   ...<snip>...

![复制代码](https://common.cnblogs.com/images/copycode.gif)

发现L213对**z%**的约定是：

   213                'z': r"(?P<z>\[+-\]\\d\\d\[0-5\]\\d)",

因此，我们知道：

对z%来说是一个无效的输入！因为%z不认识冒号**（ : ）**。那么，将其修订为

再次运行：

![复制代码](https://common.cnblogs.com/images/copycode.gif)

$ python3
Python 3.5.2 (default, Apr 16 2020, 17:47:17) 
... \>>> 
>>> import datetime \>>> fmt = "%Y-%m-%d %H:%M:%S%z"
>>> s1 = "2020-07-01 08:15:21+0000"
>>> dtobj = datetime.datetime.strptime(s1, fmt) \>>> type(dtobj) <class 'datetime.datetime'\>
>>> dtobj
datetime.datetime(2020, 7, 1, 8, 15, 21, tzinfo=datetime.timezone.utc) \>>> dtobj.tzinfo
datetime.timezone.utc \>>> dtobj.tzname() 'UTC+00:00'
>>> print(s1)                    **\# <<< the input  of tz part (+0000)  excludes ':'** 
2020-07-01 08:15:21+0000
>>> print(dtobj)                 **#** **<<< the output of tz part (+00:00) includes ':'**
2020-07-01 08:15:21+00:00
>>> 

![复制代码](https://common.cnblogs.com/images/copycode.gif)

**_原来datetime.datetime类内置的\_\_str\_\_方法默认会针对时区（time zone）部分输出冒号（ : ） ，实在是诡异！这样的设计使得用户无法直接将包含了时区字符串的str(dtobj)通过datetime.datetime.strptime(str(dtobj), fmt)还原为datetime.datetime对象，因此不得不说是一种糟糕的设计！_**

言归正传，有了上面的分析和逼近，将带时区的时间字符串转换为Unix时间戳就很容易实现了。

**00 - 预备知识**

-   **什么是Unix时间戳？**来自维基百科的解释：

![](https://img2020.cnblogs.com/blog/1094457/202007/1094457-20200702223434697-1581392865.png)

来自网站https://www.unixtimestamp.com/的解释：

**The unix time stamp is a way to track time as a running total of seconds. This count starts at the Unix Epoch on January 1st, 1970 at UTC. Therefore, the unix time stamp is merely the number of seconds between a particular date and the Unix Epoch. It should also be pointed out that this point in time technically does not change no matter where you are located on the globe. This is very useful to computer systems for tracking and sorting dated information in dynamic and distributed applications both online and client side.** Unix时间戳是一种将时间总运行秒数用来跟踪时间的方法。此计数从1970年1月1日的Unix纪元开始。因此，Unix时间戳仅仅是特定日期和unix纪元之间的秒数。还应指出的是，无论你在地球上的哪个地方，时间点在技术上都不会改变。这对于那些用来跟踪和排序具有时序的信息的计算机系统来说非常有用，这些具有时序的信息存在于各种动态和分布式应用程序中，这些应用程序包括在线应用和客户端应用。

-   **Unix时间戳有什么用？**

首先，在不同的时区，可供人阅读的时间的字符串表示是不相同的。也就是说，同一个时间点，字符串表示因时区而有所不同；其次，不同的应用程序或编程语言，对同一个时间点给出的可供人阅读的时间字符串也是不相同的；那么，在需要根据时间序列对某些信息进行排序时，Unix时间戳就特别有用，因为：**第一、表示形式具有简洁性，其值在计算机中是一个数值；第二、表示语义具有唯一性，对同一个时间点的表示既确定且唯一。**

**01 - 将带时区的时间字符串转换为Unix时间戳**

设带时区的时间字符串为S (e.g. 2020-07-01 08:15:21+00:00), 转换步骤如下：

-   **将S中时区部分的冒号去掉， 存为S1（e.g. 2020-07-01 08:15:21+0000）；**
-   **将S1通过datetime.datetime.strptime(S1, '%Y-%m-%d %H:%M:%S%z')转换为datetime.datetime对象，存为dtobj；**
-   **调用dtobj.timestamp()得到对应的Unix时间戳(float类型）。**

 代码片段如下：

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# strtime to unix time
def strtime2unixtime(s, fmt='%Y-%m-%d %H:%M:%S%z'):
    l\_str \= s.split('+')
    l\_str\[\-1\] = l\_str\[-1\].replace(':', '')
    s1 \= '+'.join(l\_str)
    dtobj \= datetime.strptime(s1, fmt) return dtobj.timestamp()

![复制代码](https://common.cnblogs.com/images/copycode.gif)

图示如下：

![](https://img2020.cnblogs.com/blog/1094457/202007/1094457-20200702213255075-1136848887.png)

**02 - 将Unix时间戳转换为带有时区的时间字符串**

设Unix时间戳为ts (e.g. 1593695823.0),   转换步骤如下：

-   **使用datetime.utcfromtimestamp()将ts转换成datetime类的对象dtobj1；**
-   **使用dtobj1.replace()将tzinfo设置为timezone.utc, 存为对象dtobj2;**
-   **调用datetime.strftime(dtobj2, '%Y-%m-%d %H:%M:%S%z')将dtobj2转换成带有时区的时间字符串。**

图示如下：

![](https://img2020.cnblogs.com/blog/1094457/202007/1094457-20200702213322460-1472273166.png)

代码片段如下：

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# unix time to UTC or LOCAL strtime
def unixtime2strtime(ts, tz='utc', fmt='%Y-%m-%d %H:%M:%S%z'):
    dtobj1 \= datetime.utcfromtimestamp(ts) if tz == 'utc':
        dtobj2 \= dtobj1.replace(tzinfo=timezone.utc) else:  # local time zone
        dtobj2 = dtobj1.replace(tzinfo=timezone.utc).astimezone(tz=None) return datetime.strftime(dtobj2, fmt)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

完整的代码文件foo.py如下：

![复制代码](https://common.cnblogs.com/images/copycode.gif)

 1 #!/usr/bin/python3
 2 
 3 import sys 4 from datetime import datetime, timezone 5 
 6 
 7 # strtime to unix time
 8 def strtime2unixtime(s, fmt='%Y-%m-%d %H:%M:%S%z'):
 9     l\_str = s.split('+') 10     l\_str\[-1\] = l\_str\[-1\].replace(':', '') 11     s1 = '+'.join(l\_str) 12     dtobj = datetime.strptime(s1, fmt) 13     return dtobj.timestamp() 14 
15 
16 # unix time to UTC or LOCAL strtime
17 def unixtime2strtime(ts, tz='utc', fmt='%Y-%m-%d %H:%M:%S%z'): 18     dtobj1 = datetime.utcfromtimestamp(ts) 19     if tz == 'utc': 20         dtobj2 = dtobj1.replace(tzinfo=timezone.utc) 21     else:  # local time zone
22         dtobj2 = dtobj1.replace(tzinfo=timezone.utc).astimezone(tz=None) 23     return datetime.strftime(dtobj2, fmt) 24 
25 
26 # get unix time
27 def get\_curr\_unixtime(): 28     return datetime.now().timestamp() 29 
30 
31 # get readable time string
32 def get\_curr\_strtime(fmt='%Y-%m-%d %H:%M:%S%z'): 33     utcnow = datetime.utcnow() 34     dtobj = utcnow.replace(tzinfo=timezone.utc).astimezone(tz=None) 35     return datetime.strftime(dtobj, fmt) 36 
37 
38 def usage(s): 39     print("Usage: %s <cmd> <string>" % s, file=sys.stderr) 40     print("e.g.", file=sys.stderr) 41     print(" %s tounix '1970-01-01 00:00:01+00:00'" % s, file=sys.stderr) 42     print(" %s tounix '1970-01-01 00:00:01+0000'" % s, file=sys.stderr) 43     print(" %s tounix '1970-01-01 08:00:01+0800'" % s, file=sys.stderr) 44     print(" %s tostr 1" % s, file=sys.stderr) 45     print(" %s tostr 1593694070.5" % s, file=sys.stderr) 46     print(" %s now" % s, file=sys.stderr) 47 
48 
49 def main(argc, argv): 50     if argc < 2: 51 usage(argv\[0\]) 52         return 1
53 
54     cmd = argv\[1\] 55 
56     if cmd == 'tounix': 57         s = argv\[2\] 58 
59         ts = strtime2unixtime(s) 60         print("%s ==> %.1f" % (s, ts)) 61     elif cmd == 'tostr': 62         s = argv\[2\] 63 
64         fmt = '%Y-%m-%d %H:%M:%S%z'
65         ts = float(s) 66         strtime1 = unixtime2strtime(ts, tz='utc', fmt=fmt) 67         print("%.1f => %s" % (ts, strtime1)) 68         strtime2 = unixtime2strtime(ts, tz='auto', fmt=fmt) 69         print("%.1f => %s" % (ts, strtime2)) 70     elif cmd == 'now': 71         now = datetime.now() 72         utcnow = datetime.utcnow() 73 
74         fmt1 = '%Y-%m-%d %H:%M:%S%Z'
75         fmt2 = '%Y-%m-%d %H:%M:%S%z'
76         obj = utcnow.replace(tzinfo=timezone.utc).astimezone(tz=None) 77         print("timezone : %s" % obj.tzname()) 78         print("datetime : %s" % datetime.strftime(obj, fmt1)) 79         print("datetime : %s" % datetime.strftime(obj, fmt2)) 80 
81         now\_ts = now.timestamp() 82         utcnow\_ts = utcnow.timestamp() 83         print() 84         print('current       unix time = %.1f' % now\_ts) 85         print('current   utc unix time = %.1f' % utcnow\_ts) 86         print(' local\_ts - utc\_ts  = %.1f' % (now\_ts - utcnow\_ts)) 87 
88         print() 89         print('current unixtime: %.1f' % get\_curr\_unixtime()) 90         print('current datetime: %s' % get\_curr\_strtime()) 91     else: 92 usage(argv\[0\]) 93         return 1
94 
95     return 0 96 
97 
98 if \_\_name\_\_ == '\_\_main\_\_': 99     sys.exit(main(len(sys.argv), sys.argv))

![复制代码](https://common.cnblogs.com/images/copycode.gif)

运行foo.py如下：

![复制代码](https://common.cnblogs.com/images/copycode.gif)

huanli@idorax16:~$ ./foo.py tostr 1
1.0 => 1970\-01\-01 00:00:01+0000
1.0 => 1970\-01\-01 08:00:01+0800 huanli@idorax16:~$ ./foo.py now
timezone : CST
datetime : 2020\-07\-02 21:40:51CST
datetime : 2020\-07\-02 21:40:51+0800 current       unix time = 1593697251.3 current   utc unix time = 1593668451.3 local\_ts \- utc\_ts  = 28800.0 current unixtime: 1593697251.3 current datetime: 2020\-07\-02 21:40:51+0800 huanli@idorax16:~$ ./foo.py tounix '2020-07-02 21:40:51+0800'
2020\-07\-02 21:40:51+0800 ==> 1593697251.0 huanli@idorax16:~$ ./foo.py tostr 1593697251.0
1593697251.0 => 2020\-07\-02 13:40:51+0000
1593697251.0 => 2020\-07\-02 21:40:51+0800 huanli@idorax16:~$ ./foo.py tounix '2020-07-02 13:40:51+00:00'
2020\-07\-02 13:40:51+00:00 ==> 1593697251.0 huanli@idorax16:~$ ./foo.py tounix '2020-07-02 13:40:51+0000'
2020\-07\-02 13:40:51+0000 ==> 1593697251.0

![复制代码](https://common.cnblogs.com/images/copycode.gif)

**总结**：**Unix时间戳(Unix timestamp)****是**一种时间表示方式，其被定义为**从格林威治时间1970年01月01日00时00分00秒起至现在的总秒数**。**Unix时间戳与时区无关，对某一特定时间点的表示具有唯一确定性**。在实现Unix时间戳与带时区的时间字符串之间的相互转化中，我们使用如下关键调用：

![复制代码](https://common.cnblogs.com/images/copycode.gif)

1\. ts = dtobj.timestamp()                                                
   # 获取datetime类的对象dtobj的unix时间戳ts(浮点数)
2. dtobj = datetime.utcfromtimestamp(ts)                                 
   # 将unix时间戳ts(浮点数)转换为datetime类的对象dtobj。
   # 注意：不要使用datetime.fromtimestamp()

3. dtobj2 = dtobj1.replace(tzinfo=timezone.utc)                     # 将时区设定为UTC
4. dtobj2 = dtobj1.replace(tzinfo=timezone.utc).astimezone(tz=None) # 或者   本地时区，例如CST

5. s = datetime.str**f**time(dtobj2, '%Y-%m-%d %H:%M:%S%z')                  
   # 将datetime类的对象dtobj2格式化输出（**format**）为可供人阅读的带时区的时间字符串
6. dtobj2 = datetime.str**p**time(s, '%Y-%m-%d %H:%M:%S%z')                  
   # 将带时区的时间字符串s格式化解析(**p**arse)为datetime类的对象dtobj2

![复制代码](https://common.cnblogs.com/images/copycode.gif)

**参考资料：**

1.  博客：[python标准库datetime的astimezone设置时区时遇到的坑](https://blog.csdn.net/weixin_42146296/article/details/93764817 "垃圾中文技术性网站")
2.  博客：[datetime时区转换](http://www.dannysite.com/blog/122/)
3.  博客：[Python datetime模块参考手册](https://www.jianshu.com/p/ff329e881683)
4.  文档：[datetime --- 基本的日期和时间类型](https://docs.python.org/zh-cn/3/library/datetime.html)
