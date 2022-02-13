---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在前面的内容中，我们特意说过在现实情况中，数据并不会被轻易获取，通常数据都在业务数据库中，需要将其抽取，进行下一步的转换操作。这一步在真实环境中会花费大量时间，尤其是数据量特别大的情况，因为通常会涉及巨量的读写性能消耗。

在不同的业务场景下，业务数据库通常会有不同的选择，主要分为两类：关系型数据库和NoSQL 数据库。

-   **关系数据库**：是创建在关系模型基础上的数据库，借助集合代数等数学概念和方法来处理数据库中的数据。现实世界中的各种实体以及实体之间的各种联系均用关系模型来表示。关系型数据库主要使用 SQL 作为自己的查询语言。
    
-   **NOSQL（Not Only SQL）**：是对不同于传统关系数据库的数据库管理系统的统称。该系统允许部分数据使用 SQL 系统存储，允许其他数据使用 NOSQL 系统存储。其数据存储可以不需要固定的表格模式以及元数据，也经常会避免使用 SQL 的连接操作，一般有分布式、高可用、高可扩展的特征。
    

本课时的主要内容有：

-   项目架构
    
-   关系型数据库的数据导出
    
-   NoSQL 数据库的数据导出
    

### 项目架构

在开始本课时的主要内容前，我们先来看看整个项目的架构，也让你可以更好地了解这个项目的设计与规划。

下图从下至上是系统的分层设计，最下面是数据源，也就是我们的业务数据库。它通过数据导入层，导入到我们的大数据平台中，这个大数据平台通常由 Hadoop 生态构建，通过数据转换层进行转换与处理，处理后生成的数据集合可以看成数据集市，最后由 BI 应用访问数据集市层进行报表生成。可以看到数据从下往上地单向流动。

![1.png](https://s0.lgstatic.com/i/image/M00/47/57/Ciqc1F9HkmiACjkbAABW85xL6RU920.png)

### 关系型数据库的数据导出

关系型数据库的导出过程比较简单，Spark 也有现成的方案，在之前的课程中，我们学习过 read 读取器 API，可以很方便地从支持 JDBC 的数据库中拉取数据，如下面的代码所示：

```
import org.apache.spark.sql.SparkSession
object BICubing {
def main(args: Array[String]): Unit = {
val spark = SparkSession.builder().appName("BI-CUBING")
    .master("local")
    .getOrCreate()  
val busniess = spark.read.format("jdbc")
    .option("driver","com.mysql.jdbc.Driver")
    .option("url", "jdbc:mysql://master:3306/ttable")
    .option("dbtable", "busniess")
    .option("user", "root")
    .option("password", "123456")
    .load()
val user = spark.read.format("jdbc")
    .option("driver","com.mysql.jdbc.Driver")
    .option("url", "jdbc:mysql://master:3306/ttable")
    .option("dbtable", "user")
    .option("user", "root")
    .option("password", "123456")
    .load()
val checkin = spark.read.format("jdbc")
    .option("driver","com.mysql.jdbc.Driver")
    .option("url", "jdbc:mysql://master:3306/ttable")
    .option("dbtable", "chenckin")
    .option("user", "root")
    .option("password", "123456")
    .load()
val tip = spark.read.format("jdbc")
    .option("driver","com.mysql.jdbc.Driver")
    .option("url", "jdbc:mysql://master:3306/ttable")
    .option("dbtable", "tip")
    .option("user", "root")
    .option("password", "123456")
    .load()
val review = spark.read.format("jdbc")
    .option("driver","com.mysql.jdbc.Driver")
    .option("url", "jdbc:mysql://master:3306/ttable")
    .option("dbtable", "review")
    .option("user", "root")
    .option("password", "123456")
    .load()
    busniess.createOrReplaceTempView("busniess")
    user.createOrReplaceTempView("user")
    tip.createOrReplaceTempView("tip")
    checkin.createOrReplaceTempView("checkin")
    review.createOrReplaceTempView("review")    
  }
}
```

从代码中可以看到，读取出来的数据可直接进行转换与处理，这个过程需要比较长的时间，主要取决于数据量的大小与数据库的读性能。由于数据库不是分布式的，它的读性能至关重要。

### NoSQL 数据库的数据导出

NoSQL 数据库主要以开源软件为主，产品丰富，使用广泛。在很多场景下，它已经取代了关系型数据库，其中比较有代表性的就是 MongoDB。

MongoDB 是一个文档数据库，它将数据存储在类似 JSON 的文档中。这是认识数据的最自然方法，比传统的行/列模型更具表现力和功能。它的主要特点有：

**丰富的 JSON 文档**

-   是处理数据最自然、有效的方式。
    
-   支持数组和嵌套对象作为值。
    
-   允许灵活和动态的模式。
    

**强大的查询语言**

-   丰富且富有表现力的查询语言，无论你的文档中有多少个嵌套，都可以按任何字段进行过滤和排序。
    
-   支持聚合和其他现代用例，例如基于地理的搜索、图形搜索和文本搜索。
    
-   查询本身就是 JSON，因此很容易组合，不再需要连接字符串来动态生成 SQL 查询。
    

**类似关系型数据库的一些特性**

-   具有快照隔离的分布式多文档 ACID 事务。
    
-   支持查询连接。
    

从上面可以看出，MongoDB 的底层数据结构和 JSON 非常类似，所以 MongoDB 也提供直接将数据导出为 JSON 格式的工具，我们可以使用 mongoexport 命令将文档集合导出为 json 文件。导出命令如下面的代码所示：

```
mongoexport -d dbtable -c busniess --json -o /yourpath/busniess.json
mongoexport -d dbtable -c review --json -o /yourpath/review.json 
mongoexport -d dbtable -c tip --json -o /yourpath/tip.json
mongoexport -d dbtable -c user --json -o /yourpath/user.json
mongoexport -d dbtable -c checkin --json -o /yourpath/checkin.json
```

导出完成后，大多数情况下，还需要将其上传到 Hadoop 的文件系统 HDFS 中，HDFS 提供了 put 命令，非常方便，命令如以下代码所示：

```
hadoop dfs -put /yourpath/busniess.json /dw/bi/busniess.json
hadoop dfs -put /yourpath/review.json /dw/bi
hadoop dfs -put /yourpath/tip.json /dw/bi/
hadoop dfs -put /yourpath/user.json /dw/bi/
hadoop dfs -put /yourpath/checkin.json /dw/bi/
```

第一个路径地址是本地文件地址，也就是 MongoDB 导出的地址，第二个路径地址为上传到 HDFS 的文件夹地址。

上传完成后，还需要用 Spark 读取器 API 进行读取，以便后续的转换与处理，代码如下：

```
import org.apache.spark.sql.SparkSession
object BICubing {
def main(args: Array[String]): Unit = {
val spark = SparkSession.builder().appName("BI-CUBING")
    .master("local")
    .getOrCreate()  
val busniess = spark.read.format("json").load("/dw/bi/busniess.json")
val user = spark.read.format("json").load("/dw/bi/user.json")
val checkin = spark.read.format("json").load("/dw/bi/checkin.json")
val review = spark.read.format("json").load("/dw/bi/review.json")
val tip = spark.read.format("json").load("/dw/bi/tip.json")
    busniess.createOrReplaceTempView("busniess")
    user.createOrReplaceTempView("user")
    tip.createOrReplaceTempView("tip")
    checkin.createOrReplaceTempView("checkin")
    review.createOrReplaceTempView("review")
  }
}
```

### 总结

数据导出过程非常耗时且重要，是 ETL 的重要组成部分，也是数据分析的基础。通常来说，在生产环境中，你的业务数据库不止有一种，所以这个过程有可能非常复杂。本课时完成的是分层设计中的数据导入层，使用的是数据源层的数据。

另外，大家可以看到，在 NoSQL 数据库的数据导出的过程中，既有命令行，也有 Spark 代码，两者交替进行，所以你还需要将这些过程整合到一个过程中去，这部分我们将在下个课时介绍。

本节课的内容看似简单，但要运用到实际情况中还需反复练习，所以我们暂不介绍更多，希望你在课后努力消化，有问题欢迎留言。
