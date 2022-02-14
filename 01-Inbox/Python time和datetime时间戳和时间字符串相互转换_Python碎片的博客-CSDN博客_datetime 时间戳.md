# [Python time和datetime时间戳和时间字符串相互转换_Python碎片的博客-CSDN博客_datetime 时间戳](https://blog.csdn.net/weixin_43790276/article/details/90674297)

### Python time和[datetime](https://so.csdn.net/so/search?q=datetime)时间戳和时间字符串相互转换

[时间戳](https://so.csdn.net/so/search?q=%E6%97%B6%E9%97%B4%E6%88%B3)是指格林威治时间1970年01月01日00时00分00秒开始计算所经过的秒数，是一个浮点数。

time和datetime都是Python中的内置模块（不需要安装，直接可以使用），都可以对时间进行获取，对时间格式进行转换，如时间戳和时间字符串的相互转换。

现在我们就使用这两个模块来对时间格式进行转换。

**一、time获取当前时间**

```
print(time.localtime(time.time()))
```

运行结果：

```
time.struct_time(tm_year=2019, tm_mon=5, tm_mday=29, tm_hour=17, tm_min=3, tm_sec=28, tm_wday=2, tm_yday=149, tm_isdst=0)time.struct_time(tm_year=2019, tm_mon=5, tm_mday=29, tm_hour=17, tm_min=3, tm_sec=28, tm_wday=2, tm_yday=149, tm_isdst=0)
```

可以通过time.time()获取到当前的时间，默认是一个时间戳[浮点数](https://so.csdn.net/so/search?q=%E6%B5%AE%E7%82%B9%E6%95%B0)。

通过time.localtime()或time.localtime(time.time())都是获取到当前时间的struct\_time，里面分别对应了当前时间的年、月、日、时、分、秒、一周的第几天(周一是0,0-6)、一年的第几天(从1开始，1-366)、夏时令(是夏时令1，不是0，不知道-1)。

**二、time将时间戳转换成时间字符串**

```
time_str = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime(time.time()))
```

运行结果：

```
2019-05-29 17:08:35
```

**三、time将时间字符串转换成时间戳**

```
print(time.strptime(time_str, '%Y-%m-%d %H:%M:%S'))time_stamp = time.mktime(time.strptime(time_str, '%Y-%m-%d %H:%M:%S'))
```

运行结果：

```
time.struct_time(tm_year=2019, tm_mon=5, tm_mday=29, tm_hour=17, tm_min=8, tm_sec=35, tm_wday=2, tm_yday=149, tm_isdst=-1)
```

可以看到，不管是将时间戳转换成时间字符串，还是将时间字符串转换成时间戳，time模块都是通过struct\_time来过渡的，也就是说，都需要先转换成struct\_time,再用struct\_time转换成想要的结果。

**四、datetime获取当前时间**

```
from datetime import datetimeprint(datetime.now().timetuple())
```

运行结果：

```
2019-05-29 17:22:37.343784time.struct_time(tm_year=2019, tm_mon=5, tm_mday=29, tm_hour=17, tm_min=22, tm_sec=37, tm_wday=2, tm_yday=149, tm_isdst=-1)
```

可以通过datetime.now()获取到当前的时间，默认是一个datetime时间对象，样式是一个时间字符串的样式。

**注意：导包时导入的是datetime包下的datetime模块。**导包方式不同，使用时也不同。

通过datetime对象的timetuple()方法可以获取到时间的struct\_time。

**五、datetime将datetime对象转换成时间字符串和时间戳**

```
datetime_str = datetime.strftime(datetime.now(), '%Y-%m-%d %H:%M:%S')datetime_stamp = datetime.timestamp(datetime.now())
```

运行结果：

**六、datetime将时间字符串转换成时间戳**

```
datetime_stamp2 = datetime.timestamp(datetime.strptime(datetime_str, '%Y-%m-%d %H:%M:%S'))
```

运行结果：

```
1559121757.0
```

**七、datetime将时间戳转换成时间字符串**

```
datetime_str2 = datetime.strftime(datetime.fromtimestamp(datetime_stamp2), '%Y-%m-%d %H:%M:%S')
```

运行结果：

```
2019-05-29 17:22:37
```

在使用datetime进行时间戳和时间字符串之间的转换时，都是先转换成datetime对象，然后再做进一步的转转。

在实际工作中，我们也可以同时使用time和datetime两个模块，它们是可以混合使用的。

![](https://img-blog.csdnimg.cn/20190529170235810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzc5MDI3Ng==,size_16,color_FFFFFF,t_70)
