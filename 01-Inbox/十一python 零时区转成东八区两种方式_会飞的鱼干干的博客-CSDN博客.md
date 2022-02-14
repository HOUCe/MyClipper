# [十一:python 零时区转成东八区两种方式_会飞的鱼干干的博客-CSDN博客](https://blog.csdn.net/qq_36066039/article/details/113522661)

```
def str_to_timestamp(str_time=None, fmt='%Y-%m-%dT%H:%M:%S.%fZ'):        t = datetime.datetime.strptime(str_time, fmt)t += datetime.timedelta(hours=8)return int(time.mktime(t.timetuple()))def str_to_timestamp_3(str_time=None, fmt= '%Y-%m-%dT%H:%M:%S.%fZ'):        d = datetime.datetime.strptime(str_time, fmt)timeStamp = int(time.mktime(d.timetuple())) + 8 * 3600
```
