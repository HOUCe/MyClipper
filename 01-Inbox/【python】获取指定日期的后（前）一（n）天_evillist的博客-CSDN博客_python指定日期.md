# [【python】获取指定日期的后（前）一（n）天_evillist的博客-CSDN博客_python指定日期](https://blog.csdn.net/evillist/article/details/50522505)

开始在网上找到了，获取今天，明天和前天日期的代码。

\>>> import [datetime](https://so.csdn.net/so/search?q=datetime) #导入日期时间模块  
\>>> today = datetime.date.today() #获得今天的日期  
\>>> print today #输出今天日期  
2014-01-04   
\>>> yesterday = today - datetime.timedelta(days=1) #用今天日期减掉时间差，参数为1天，获得昨天的日期  
\>>> print yesterday  
2014-01-03   
\>>> tomorrow = today + datetime.timedelta(days=1) #用今天日期加上时间差，参数为1天，获得明天的日期  
\>>> print tomorrow  
2014-01-05 

上面的代码可以看出，“+”表示获取后面的时间，“-”表示获取钱的时间，days=n即可获取前（后）n天。而我需要的是，获得指定日期（如：20151028）的前（后）n天。想到把20151028转换成日期格式，替换上面代码中的today就行，即下面红色字体。20151028的后一天是20151029，需要把datetime格式的数据，截取前面一段，可以用日期的格式化。

```
date = datetime.datetime(2015, 10, 28) + datetime.timedelta(days=n)time_format = cur_date.strftime('%Y%m%d')
```
