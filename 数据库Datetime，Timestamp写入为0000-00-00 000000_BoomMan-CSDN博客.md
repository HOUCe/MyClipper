# [数据库Datetime，Timestamp写入为0000-00-00 00:00:00_BoomMan-CSDN博客](https://blog.csdn.net/boom_man/article/details/73197823)

## 在遇到数据库问题时心得

1.查看控制台报错原因,百度,自己读。  
2.打开log4j的语句显示功能,或你所使用的mybatis,Hibernate的语句显示  
3.查看数据库结果与语句执行是否一致,可以自己测试一遍。  
4.url控制相关，一般乱码问题在这里。

___

### 1.数据库字段设置成[Timestamp](https://so.csdn.net/so/search?q=Timestamp),但是写入结果为0000-00-00 00:00:00

原代码:  
Timestamp用法:

```

 Timestamp nowdate1 = new Timestamp(System.currentTimeMillis());

  Date date = new Date();
  Timestamp nowdate1 = new Timestamp(date.getTime());
```

错误原因:我们可以打印出nowdate1值，会发现它和数据库中的字段不同，它比数据库中的时间更长。  
解决办法：  
1.数据库版本降级，降到5.5即可，这个可能是高版本安全性更高的原因  
2.在写入数据时写成如下格式

```

String nowTime = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date());
Timestamp.valueOf(nowTime);
```

### 2.数据库出现java.sql.SQLException: Cannot convert value ‘0000-00-00 00:00:00’

出现原因:这是在读取数据库数据时会出现的错误原因是，sql里面日期最小值应该是：1900-1-1 00:00:00不可以比这个值再小了，再小的话就报错，  
解决办法:1.删除数据库中’0000-00-00 00:00:00’字段，这是因为数据错误  
解决办法:2.在url中添加zero[DateTime](https://so.csdn.net/so/search?q=DateTime)Behavior

```


jdbc.url=jdbc:mysql://127.0.0.1:3306/mysql?useUnicode=true&amp&characterEncoding=utf-8&amp&zeroDateTimeBehavior=convertToNull&amp&transformedBitIsBoolean=true&useSSL=true
```

### 数据库字段与java类对应关系

[java中日期类型和mysql中日期类型进行整合](http://www.cnblogs.com/haore147/p/3618045.html)

> DATETIME类型用在你需要同时包含日期和时间信息的值时。MySQL检索并且以’YYYY-MM-DD HH:MM:SS’格式显示DATETIME值，支持的范围是’1000-01-01 00:00:00’到’9999-12-31 23:59:59’。（“支持”意味着尽管更早的值可能工作，但不能保证他们可以。）
> 
> DATE类型用在你仅需要日期值时，没有时间部分。mysql检索并且以’YYYY-MM-DD’格式显示DATE值，支持的范围是’1000-01-01’到’9999-12-31’。
> 
> TIMESTAMP列类型提供一种类型，你可以使用它自动地用当前的日期和时间标记INSERT或UPDATE的操作。
> 
> TIME数据类型表示一天中的时间。MySQL检索并且以”HH:MM:SS”格式显示TIME值。支持的范围是’00:00:00’到’23:59:59’。
> 
> timestamp所能存储的时间范围为：’1970-01-01 00:00:01.000000’ 到 ‘2038-01-19 03:14:07.999999’。
> 
> datetime所能存储的时间范围为：’1000-01-01 00:00:00.000000’ 到 ‘9999-12-31 23:59:59.999999’。

实验：java >> mysql

```
===========java注入数据库==========

java类型   mysql类型        成功与否
date         date               yes
date         time               no
date         timestamp       no
date         datetime         no

time         date               no
time         time               yes
time         timestamp       no
time         datetime         no

timestamp date              yes
timestamp time              yes
timestamp timestamp     yes
timestamp datetime        yes
==========end java注入数据库========
总规律，如果A完全包含B，则A可以向B注入数据，否则报错
```

实验：mysql >> java

```
==========从数据库提取到java ==========

mysql类型    java类型     成与否
date             date         yes
date             time         yes 
date           timestamp   yes 

time           date           yes 
time           time           yes
time          timestamp    yes 

timestamp date           yes
timestamp time           yes
timestamp timestamp   yes

datetime      date         yes
datetime      time         yes
datetime    timestamp   yes
==========end 从数据库提取到java=======
不会出错，缺少的部分使用历元，而不是当前日期时间
```

总结：TIMESTAMP和DATETIME除了存储范围和存储方式不一样，没有太大区别。当然，对于跨时区的业务，TIMESTAMP更为合适。

编程路上风雨兼程，有一颗解决问题本源的心才会进步！
