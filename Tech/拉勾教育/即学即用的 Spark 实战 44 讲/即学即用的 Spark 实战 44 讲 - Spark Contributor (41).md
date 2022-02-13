---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在前面的课时中，我们已经将数据从业务数据库中导出到大数据平台里，可以简单地将导出后的数据集合视为数据仓库。当然在实际情况中，数据也不可能这么合适，这个时候就需要进行数据清洗与转换，在这之后，数据集合才能被称其为数据仓库，这个过程就是我们前面提到的 ETL。

从数据源到数据集市的整个过程如下图所示。而我们今天要学习的是下一个步骤：为数据仓库中的数据构建数据立方体，也就是图中的 Cubing 操作。生成的数据立方体就是实际意义上的数据集市。

![1.png](https://s0.lgstatic.com/i/image/M00/48/F8/Ciqc1F9OCnKAMkGWAABa83_NNug190.png)

本课时的主要内容有：

-   构建数据立方体
    
-   整合脚本
    
-   调度平台
    

### 构建数据立方体

这节课，我们继续用 Yelp 内部业务数据集（Yelp 2016 Dataset Challenge）中的数据进行演示。

我们用其中的 busniess 表与 review 表构建一个数据立方体， 用来分析有关评论的信息，代码如下：

```
val sqlStr = "SELECT  r.cool, r.funny, r.stars, r.userful, l.city, l.state, r.date FROM  business l JOIN review r ON l.business_id = r.business_id"
    spark.sql(sqlStr).write.orc("/yourpath/dm")
```

用连接操作，将 busniess 表和 review 表连接在一起，并过滤 掉了无关字段，例如 review 表中的 text 字段，该字段是一大段文本，占用空间非常大，过滤掉这个字段可以显著提高查询性能。不过这里要**特别说明的是**，我们可以用一些人工智能技术将这个字段处理为结构化的数据，从而便于后续处理，这也是 ETL 的一种。其中要强调的是，最后保存到文件系统时，我们采用了 ORC 格式（Optimized Row Columnar，是一种 Hadoop 生态圈中的列式存储格式），这对于数据立方体来说非常友好，ORC 格式可以显著降低连接后的数据集大小。总之，在生成数据立方体的同时，需要考虑如何优化查询性能。

同样，在一个较大型的组织中，一般会同时构建成百上千个数据立方体，于是选择 ORC 格式储存就显得尤为重要。因为在一个正常的商业智能系统的项目中，一般来说 ETL 和构建数据立方体会占整个开发时间的 70%-80%，而真正的分析只需要很少的时间。

根据“第 39 课时|作为 Yelp 运营负责人，如何根据数据进行决策”的内容我们已经知道，该数据立方体的维度主要分为地点（city、state）和时间（date），度量分别为 cool、funny、stars、useful 字段。

### 整合脚本

数据立方体构建完成后，需要将数据导出与构建数据立方体的过程整合，以便能够一次性完成整个过程。

以业务数据库 MongoDB 为例，我们先把 MongoDB 数据导出并上传到 HDFS 的过程拆开，分别写成两个脚本。

第一个脚本如以下代码所示，是负责将 MongoDB 中的数据导出到本地文件系统的 mongoexport.sh：

```
#!/bin/bash
mongoexport -d dbtable -c busniess --json -o /yourpath/busniess.json
mongoexport -d dbtable -c review --json -o /yourpath/review.json 
mongoexport -d dbtable -c tip --json -o /yourpath/tip.json
mongoexport -d dbtable -c user --json -o /yourpath/user.json
mongoexport -d dbtable -c checkin --json -o /yourpath/checkin.json
```

第二个脚本是将导出的文件上传到 HDFS 的脚本 upload.sh，如以下代码所示：

```
#!/bin/bash
hadoop dfs -put /yourpath/busniess.json /dw/bi/busniess.json
hadoop dfs -put /yourpath/review.json /dw/bi
hadoop dfs -put /yourpath/tip.json /dw/bi/
hadoop dfs -put /yourpath/user.json /dw/bi/
hadoop dfs -put /yourpath/checkin.json /dw/bi/
```

然后我们需要将 Spark 代码打包为一个 jar 包，并编写一条提交 Spark 作业的命令，如下面的代码所示：

```
spark-submit --name Cubing --class com.yelp.BICubing --master yarn-cluster --executor-memory 2G --num-executors 10 /yourpath/cubing.jar
```

经过上述操作，所有的过程就都脚本化了。现在我们需要调度这三个脚本，使它们在一定的时间以一定的顺序执行，这就需要用到调度平台。

### 调度平台

前面提到过，在一个较大型的组织中，一般会同时构建成百上千个数据立方体，每天定时运行的作业多达数千个，作业与作业之间的依赖错综复杂，并非像我们演示的项目一样，会按照顺序执行。如果没有一个统一的作业调度工具的话，难以管理如此复杂的工作流。

在生产环境的集群中，一般都会配备一个调度器，也称为调度中心，负责集群所有的作业调度工作。Hadoop 生态圈有 Java 编写的 Apache Oozie，但由于其功能的局限性，尤其不能表现有向无环图的作业依赖关系，已经逐渐退出历史舞台。

目前**比较成熟的调度器**有 Airbnb 的 Airflow 和 LinkedIn 的 Azkaban。本节课采用 Airflow 作为系统的调度中心。

Airflow 是用 Python 实现任务管理、调度、监控工作流的平台。与 Ozzie 的 XML 配置文件相比，Airflow 的理念是“配置即代码”，对于描述工作流、判断触发条件等过程，全部采用 Python 脚本，编写工作流就像在写脚本一样，能更快捷地在线上做功能扩展。Airflow 充分利用 Python 的灵巧轻便，是一款非常好用的调度器。

下面的图片是 Airflow 的主界面，DAG 选项列出了所有工作流的配置，可以看到每份配置都是一份 Python 代码文件。

![Drawing 1.png](https://s0.lgstatic.com/i/image/M00/49/02/CgqCHl9OCZ2AJi7GAACdD_Xssl4740.png)

下图是以可视化的形式展现数据处理的工作流：

![Drawing 2.png](https://s0.lgstatic.com/i/image/M00/49/02/CgqCHl9OCaOAADVVAAEpHLgVDeI153.png)

我们可以在线编辑配置代码文件，下图是一份代码文件：

![Drawing 3.png](https://s0.lgstatic.com/i/image/M00/49/02/CgqCHl9OCamAQPHwAAEHfsxdsc0520.png)

Airflow 的作业调度配置文件就是一个 Python 脚本。在脚本中，可以用 Python 灵活地定义计算作业工作流。编写完成后，需要将该脚本放置在位于 Airflow 安装目录下、airflow.cfg 文件中配置项 dags\_folder 指定的目录下，放置完成即可生效。根据本课时的内容，一共需要配置 3 个作业：MongoExport、Upload 和 BICubing，配置文件如下：

```
"""
Code that goes along with the Airflow tutorial located at:
https://github.com/airbnb/airflow/blob/master/airflow/example_dags/tutorial.py
"""
from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from datetime import datetime, timedelta
default_args = {
'owner': 'yourname',
'depends_on_past': False,
'start_date': datetime(2018, 9, 28),
'email': ['airflow@example.com'],
'email_on_failure': False,
'email_on_retry': False,
'retries': 1,
'retry_delay': timedelta(minutes=5),
}
dag = DAG('dag-datalake', default_args=default_args)
task1 = BashOperator(
    task_id='MongoExport',
    bash_command='./mongoexport.sh',
    dag=dag
)
task2 = BashOperator(
    task_id='Upload',
    bash_command='./upload.sh',
    dag=dag
)
task3 = BashOperator(
    task_id='Cubing',
    bash_command='spark-submit --name Cubing --class com.yelp.BICubing --master yarn-cluster --executor-memory 2G --num-executors 10 /yourpath/cubing.jar',
    dag=dag
)
task2.set_upstream(task1)
task3.set_upstream(task2)
```

在脚本里，我们定义了 3 个计算作业及其执行命令，最后定义了作业之间的依赖关系，这会决定作业的执行顺序。定义的方法非常简单，每个作业只需要指定自己的上游作业即可：

```
task2.set_upstream(task1)
task3.set_upstream(task2)
```

这样定义的计算作业的工作流如下图所示：

![Drawing 4.png](https://s0.lgstatic.com/i/image/M00/49/03/CgqCHl9OCbSAb-DSAACEIf6UbfQ342.png)

我们还可以用如下代码的方式，构造一个作业依赖多个作业的情况：

```
task1.set_upstream(task2,task3)
```

定义的结果如下图所示：

![Drawing 5.png](https://s0.lgstatic.com/i/image/M00/49/03/CgqCHl9OCbuAWLIIAACA8WF-KYY898.png)

定义完成后，用户可以配置执行时间，作业将会自动运行。

### 总结

在本课时中，我们学习了如何构建数据立方体，在生成数据立方体的同时，也就成功构建了数据集市。另外，我们还引入了开源调度平台 Airflow，完成了整个过程的调度与定时执行，在生产环境中，调度平台的使用非常必要。从这个课时开始，我们引入了开源软件来满足我们的需求，这在大数据开发中非常常见。在下个课时中，我们将使用 Airbnb 开源的另一个软件来满足分析需求与可视化需求。

本节课仍然不留额外的思考题，希望你反复巩固内容，以便应对更复杂的项目。
