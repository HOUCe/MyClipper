# [android: 日期转Unix时间戳，Unix时间戳转日期，带时区 - 夜行过客 - 博客园](https://www.cnblogs.com/yongdaimi/p/10832638.html)

## 1、UTC时间&GMT时间

UTC时间是时间标准时间（Universal Time Coordinated），UTC是根据原子钟来计算时间，误差非常小。

UTC也是指零时区的时间，如果要表示其他时区的时间，这里要注意没有UTC+0800或者UTC+8这样的表示方式（至少Java里面没有，一般用于口头表示），只有Asia/Shanghai这样的表示方式，详细的时区列表参考这个文档时区列表。

GMT时间是根据地球的自转和公转来计算时间，老的时间计量标准，这里我们不过多讨论

## 2、表达时间方式

我们一般表示时间都会带格式以方便理解，例如时间表达式是'2018-09-12 08:00:00'，因为我们在东八区，所以默认是：北京时间2018年9月12号8点整。但是如果是一个美国人看到这个时间，就会认为是美国东部or西部时间的2018年9月12号8点整。所以从这种表达方式很不准确，因为没有指明到底是哪个时区的时间！！！！

所以准确的表达时间必须带有时区，例如2018-09-12 08:00:00+0800，表达了Asia/Shanghai这个时区的时间2018年9月12号8点整。这里要注意+0800并不是表示加8小时的意思，只是表示这个时间'2018-09-12 08:00:00'是东八区Asia/Shanghai的时间，仅此而已。

## 3、UTC时间的时间戳

讲清楚了时间表达方式，再讲时间戳。时间戳是指格林威治时间1970年01月01日00时00分00秒(北京时间1970年01月01日08时00分00秒)起至现在的总秒数。也就是在'1970-01-1 00:00:00+0000' 或 '1970-01-1 08:00:00+0800'这个时间点，时间戳是0。这也是Java里时间组件的默认方式，不管用户输入的人类可识别的时间是什么格式，在内部统一存的是时间戳。

例如时间是'2018-09-01 08:00:00+0800'，那么使用date.getTime()获取到时间戳是1535760000000；时间是'2018-09-01 00:00:00+0000'，获取到时间戳也是1535760000000。

![复制代码](https://common.cnblogs.com/images/copycode.gif)

 try {
            SimpleDateFormat sdf \= new SimpleDateFormat("yyyy-MM-dd HH:mm:ssZ", Locale.getDefault());

            Log.i("xp.chen", sdf.parse("2018-09-01 08:00:00+0800").getTime()+"");
            Log.i("xp.chen", sdf.parse("2018-09-01 00:00:00+0000").getTime()+"");
            Log.i("xp.chen", sdf.parse("1970-01-01 00:00:00+0000").getTime()+"");
        } catch (ParseException e) {
            e.printStackTrace();
        }

![复制代码](https://common.cnblogs.com/images/copycode.gif)

运行结果：

2019-05-08 16:09:10.935 21352-21352/? I/xp.chen: 1535760000000  
2019-05-08 16:09:10.935 21352-21352/? I/xp.chen: 1535760000000  
2019-05-08 16:09:10.935 21352-21352/? I/xp.chen: 0

## 4、时间戳与日期之前的相互转换

【时间戳转日期】

 　　　　long curTime = 1535760000000L;
        String timeStr \= new SimpleDateFormat("yyyy-MM-dd HH:mm:ss Z", Locale.getDefault()).format(new Date(curTime));
        Log.i("xp.chen", "timeStr: "+timeStr);

运行结果：

2019-05-08 16:14:58.573 22835-22835/? I/xp.chen: timeStr: 2018-09-01 08:00:00 +0800

【日期转时间戳】

![复制代码](https://common.cnblogs.com/images/copycode.gif)

 try {
            String src\_dateStr \= "2018-09-01 08:00:00 +0800";
            Calendar calendar \= Calendar.getInstance();
            calendar.setTime(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss Z", Locale.getDefault()).parse(src\_dateStr)); long timeInMillis = calendar.getTimeInMillis();
            Log.i("xp.chen", "timeInMillis: "+timeInMillis);
        } catch (ParseException e) {
            e.printStackTrace();
        }

![复制代码](https://common.cnblogs.com/images/copycode.gif)

运行结果：

2019-05-08 16:14:58.573 22835-22835/? I/xp.chen: timeInMillis: 1535760000000

【获得当前日期字符串含时区】

 public static String getDateStrIncludeTimeZone() {
        SimpleDateFormat sdf \= new SimpleDateFormat("yyyy-MM-dd HH:mm:ss Z", Locale.getDefault()); return sdf.format(new Date());
    }

  

___

如果您觉得阅读本文对您有帮助，请点一下“**推荐**”按钮，您的**“推荐”**将是我最大的写作动力！欢迎各位转载，但是未经作者本人同意，转载文章之后**必须在文章页面明显位置给出作者和原文连接**，否则保留追究法律责任的权利。
