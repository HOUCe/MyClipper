---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


本课时我们进入实战课程的演练：探索葡萄牙银行电话调查的结果。本课时主要用真实数据构建了一个数据探索场景，这既不是一个项目也不是一个应用，只是一个探索的过程。

这个过程在实际应用中是非常常见的，无论是对分析师还是工程师来说，对数据的探索都是必要的，为此，Spark 也创新地开发了 Spark Shell 应用，让编译型语言 Scala 用起来像脚本语言一样，提高了实践效率。

数据（下载链接为：[https://pan.baidu.com/s/1up25t-HQF16Sx4-naC0rJw](https://pan.baidu.com/s/1up25t-HQF16Sx4-naC0rJw)，密码为 jzke）来源于葡萄牙银行电话调查的结果，本课时会通过一些分析手段逐步对数据展开探索，直到用户得到想要的信息。本课时的内容完全是实践，没有理论，希望你动手一起做。由于案例中用到了 Dataset API，故采用 Scala 版本。

下面这段代码读取了葡萄牙银行通过电话访问进行市场调查得到的数据集，并统计了数据条数：

```
import org.apache.spark.sql.types._
import org.apache.spark.sql.{SparkSession}
import org.apache.spark.sql.functions._
case class Call(age: Double, job: String, marital: String, edu: String, 
credit_default: String, housing: String, loan: String, 
contact: String, month: String, day: String, 
dur: Double, campaign: Double, pdays: Double, 
prev: Double,pout: String, emp_var_rate: Double, 
cons_price_idx: Double, cons_conf_idx: Double, euribor3m: Double, 
nr_employed: Double, deposit: String)
val age = StructField("age", DataTypes.IntegerType)
val job = StructField("job", DataTypes.StringType)
val marital = StructField("marital", DataTypes.StringType)
val edu = StructField("edu", DataTypes.StringType)
val credit_default = StructField("credit_default", DataTypes.StringType)
val housing = StructField("housing", DataTypes.StringType)
val loan = StructField("loan", DataTypes.StringType)
val contact = StructField("contact", DataTypes.StringType)
val month = StructField("month", DataTypes.StringType)
val day = StructField("day", DataTypes.StringType)
val dur = StructField("dur", DataTypes.DoubleType)
val campaign = StructField("campaign", DataTypes.DoubleType)
val pdays = StructField("pdays", DataTypes.DoubleType)
val prev = StructField("prev", DataTypes.DoubleType)
val pout = StructField("pout", DataTypes.StringType)
val emp_var_rate = StructField("emp_var_rate", DataTypes.DoubleType)
val cons_price_idx = StructField("cons_price_idx", DataTypes.DoubleType)
val cons_conf_idx = StructField("cons_conf_idx", DataTypes.DoubleType)
val euribor3m = StructField("euribor3m", DataTypes.DoubleType)
val nr_employed = StructField("nr_employed", DataTypes.DoubleType)
val deposit = StructField("deposit", DataTypes.StringType)
val fields = Array(age, job, marital, 
edu, credit_default, housing, 
loan, contact, month, 
day, dur, campaign, 
pdays, prev, pout, 
emp_var_rate, cons_price_idx, cons_conf_idx, 
euribor3m, nr_employed, deposit)
val schema = StructType(fields)
val spark = SparkSession
.builder()
.appName("data exploration")
.master("local")
.getOrCreate()
import spark.implicits._
val df = spark
.read
.schema(schema)
.option("sep", ";")
.option("header", true)
.csv("./bank/bank-additional-full.csv")
println(df.count())
```

运行之后的结果为：41188。接下来我们再来根据婚姻情况统计各类人群的数量和缺失值的数量。

```
val dm = spark
.read
.schema(schema)
.option("sep", ";")
.option("header", true)
.csv("./bank/bank-additional-full-missing.csv")
dm.groupBy("marital").count().show()
```

结果为：

![Drawing 0.png](https://s0.lgstatic.com/i/image/M00/1B/C9/CgqCHl7fP0eACZ5pAABAb_pyhCc901.png)

现在我们再根据职业统计各类人群的数量和缺失值的数量：

```
dm.groupBy("job").count().show()
```

结果为：

![Drawing 1.png](https://s0.lgstatic.com/i/image/M00/1B/BE/Ciqc1F7fP1GADeUyAACYePuacpI978.png)

接下来根据教育情况统计各类人群的数量和缺失值的数量：

```
dm.groupBy("edu").count().show()
```

结果为：

![Drawing 2.png](https://s0.lgstatic.com/i/image/M00/1B/BE/Ciqc1F7fP1mAS4AeAAB14vZ5jyI780.png)

下面我们选取数值类字段作为数据子集，进行描述性统计：

```
val dsSubset=dm.select("age","dur","campaign","prev","deposit").cache()
dsSubset.describe().show()
```

结果为：

![Drawing 3.png](https://s0.lgstatic.com/i/image/M00/1B/BE/Ciqc1F7fP2KAQ13MAACaLdyyyCM693.png)

下面这段代码判断了变量间的相关性：

```
println(dsSubset.stat.cov("age","dur"))
```

结果为：-2.3391469421265874。

接下来计算相关系数：

```
println(dsSubset.stat.corr("age","dur"))
```

结果为：8.657050101409117E-4。

下面计算每个年龄段的婚姻状态分布：

```
 ds.stat.crosstab("age","marital").orderBy("age_marital").show(20)
```

结果为：

![Drawing 4.png](https://s0.lgstatic.com/i/image/M00/1B/C9/CgqCHl7fP2uADq6rAADP-qyhVGc851.png)

下面这段代码展示了所有受访人的学历背景出现频率超过 0.3 的学历：

```
println(ds.stat.freqItems(Seq("edu"),0.3).collect()(0))
```

结果为：  
\[WrappedArray(high.school, university.degree, professional.course)\]。

下面计算受访用户年龄的分位数：

```
df.stat.approxQuantile("age",Array(0.25,0.5,0.75),0.0)
.foreach(println)
```

结果为：

![Drawing 5.png](https://s0.lgstatic.com/i/image/M00/1B/C9/CgqCHl7fP3OALu9-AAAHMrnBXYU267.png)

接下来则需要根据定期存款意愿将客户分组，并进一步进行分析：

```
dsSubset
.groupBy("deposit")
.agg(count("age").name("Total customers"),
round(avg("campaign"),2).name("Avgcalls(curr)"),
round(avg("dur"),2).name("Avg dur"),round(avg("prev"),2).name("AvgCalls(prev)")).withColumnRenamed("value","TDSubscribed?")
.show()
```

结果为：

![Drawing 6.png](https://s0.lgstatic.com/i/image/M00/1B/CA/CgqCHl7fP3uAAtbMAAB1XvA5LG4689.png)

这段代码根据年龄将客户分组，并进一步进行分析：

```
dsSubset
.groupBy("age")
.agg(count("age").name("Total customers"),
round(avg("campaign"),2).name("Avgcalls(curr)"),
round(avg("dur"),2).name("Avg dur"),
round(avg("prev"),2).name("AvgCalls(prev)")).orderBy("age")
.show()
```

结果为：

![Drawing 7.png](https://s0.lgstatic.com/i/image/M00/1B/BE/Ciqc1F7fP5KAJxseAAGtpeiBSns139.png)

### 小结

数据分析师通过上面这个过程，就可以对数据大致的质量、分布、基本统计信息有了一个基本的印象，这样探索的目的也就达到了。在很多情况下，一些分析师在数据量不大的情况下也喜欢用 Spark 来分析数据，这得益于 DataFrame API 与 Datasets API 的简洁与功能完善。这份数据中，有趣的地方还有很多，你不妨用 Python DataFrame API 与 Spark SQL 尽情探索吧。
