---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


在互联网行业中，要说哪一个领域是数据分析最发光发热的地方，那就是电子商务领域。绝大多数电商网站，都强依赖于数据分析帮助其挖掘新的用户与订单增长机会。

典型的应用场景如通过数据分析发现不同类型用户对于商品、店铺的偏好，从而进行针对性的推荐，又或者通过数据分析发现用户未来潜在的购买概率，以决定是否要给用户发放红包或者优惠券来引导用户下单。

无论是哪个方向，数据分析都能显著提升业务的效果，是电商业务不可或缺的一环。

今天，我就带你利用本章节学到的 pandas 知识，来对一个真实的电商数据集进行分析。一方面，我们可以系统性地复习 pandas 知识，同时你也可以亲自感受下电商数据分析实战项目的分析流程。

### 任务背景

阿普尔星球最大的电商网站阿普闪购计划策划一场推广活动，通过发短信的形式，向潜在的用户发送广告和优惠信息，吸引他们来阿普闪购注册并购物。

由于预算以及短信服务商的限制，没有办法对大范围的用户投放，这就需要缩小人群的范围，找出最有可能产生转化的人群。此外，人们的下单行为往往也和时间呈现一定的相关性，什么时候推送营销短信也很重要。

现阶段你作为阿普闪购的数据分析师，这个任务自然就落到了你的肩上，你的目标是：

-   通过数据分析，找到最有可能转化的人群特征（年龄、性别、地域）等；
    
-   通过数据分析，决定出最有利于转化的营销短信投放时间。
    

通过和多个部门的沟通，最终你从阿普闪购的数据部门申请到了如下数据权限。

-   用户行为表：最近 6 个月的用户行为记录。
    
-   VIP 会员表：用户 VIP 会员开通情况。
    
-   用户信息表：用户的相关信息。
    

### 数据集获取与解读

你可以从这个链接，下载本次案例所用到的数据集。

> [https://pan.baidu.com/s/1b2nlYEJ0UwKeqXx7yJclDw](https://pan.baidu.com/s/1b2nlYEJ0UwKeqXx7yJclDw?fileGuid=xxQTRXtVcqtHK6j8)提取码：k2yy

解压之后可以看到三个文件：

-   `user_behavior_time_resampled.csv` 代表用户行为表；
    
-   `vip_users.csv` 代表 VIP 会员表；
    
-   `user_info.csv` 代表用户信息表。
    

csv 文件是 Excel 兼容的，你可以通过 Excel 打开，看到每个表的信息如图所示。

![image (3).png](https://s0.lgstatic.com/i/image6/M01/42/AE/CioPOWC0suOAPOUKAAGnszceDAo726.png)

![image (4).png](https://s0.lgstatic.com/i/image6/M01/42/A5/Cgp9HWC0suuANGIUAAFQkVoH6-k485.png)

![image (5).png](https://s0.lgstatic.com/i/image6/M01/42/A5/Cgp9HWC0svSAXPYiAAEcDRxzy2w920.png)

每一个表的第一行是表头，剩下的就是具体数据。每个表头的含义“字如其名”，很好理解。

首先是用户行为表， `user_behavior_time_resampled.csv` 。

![image (6).png](https://s0.lgstatic.com/i/image6/M00/42/AE/CioPOWC0svuAZawlAACBNIh74W8138.png)

然后是 VIP 会员表，`vip_user.csv`

![image (7).png](https://s0.lgstatic.com/i/image6/M00/42/AE/CioPOWC0swGASJ-CAABJBW_w35Q625.png)

最后，再看用户信息表， `user_info.csv`

![image (8).png](https://s0.lgstatic.com/i/image6/M00/42/AE/CioPOWC0swmAAh2eAACMgba2Xoc999.png)

> 看完了上述数据集的描述，细心的你恐怕已经发现用户行为（ user\_log ）表中存在的小问题：有两个时间相关的字段，一个叫 time\_stamp，一个叫 timestamp。 这两个字段的关系是什么？哪个能代表正确的时间？目前没有更多的资料。我们将在后续从数据本身尝试对这两个字段进行分析与解读。

接下来我们就通过 pandas 加载这些数据，来进行初步分析。

### 加载数据

首先在你的工作目录中，新建一个 chapter17 目录，用来保存本讲实战的相关内容。之后在该目录中新建目录：data\_set，用于保存下载到的数据集。然后，我们把上面下载的三个 csv 文件拷贝到 chapter17/dataset 目录。

接下来，我们开始对这些数据文件进行分析。

打开 VS Code，按【CTRL+P】 调出命令面板，输入 >Jupyter: ，在列表中选择 【Create New Blank Jupyter Notebook】来新建一个 Notebook，之后按【CTRL + S】保存为 chapter17.ipynb。

结合我们这一章的内容，对于数据集的加载和分析，最核心就是要使用 pandas 工具包，所以首先新建一个 Cell，导入必要的工具：pandas。

输入之后，运行 Cell（Shift + Enter）。运行之后，pd 这个变量就代表了 pandas 。这样我们后续就可以使用 `pd` 调用 pandas 的函数了。

现在我们需要加载 csv 文件。之前我们的课程有介绍过，加载 csv 文件可以使用 pandas 的 read\_csv 函数，该方法可以接收一个文件路径，返回 pandas 的 DataFrame。

在这里，我们用三个 DataFrame 去加载这三个 csv 文件。新建 Cell，输入如下代码，并执行。

```
df_user_log = pd.read_csv("EComm/user_behavior_time_resampled.csv")
df_vip_user = pd.read_csv("EComm/vip_user.csv")
df_user_info = pd.read_csv("EComm/user_info.csv")
```

这几行代码的执行时间一般取决于 csv 文件本身的大小，由于用户行为表的数据量较多，所以会执行一段时间。当 Cell 的序号部分由 \[\*\] 变为 \[4\] 的时候，说明 Cell 已经执行完毕。

执行中：

![Drawing 3.png](https://s0.lgstatic.com/i/image6/M00/40/B5/CioPOWCmOB-AF1lGAABuYf4QRDM166.png)

执行完毕：

![Drawing 4.png](https://s0.lgstatic.com/i/image6/M00/40/AD/Cgp9HWCmOCSAOgOuAABsw5d6aHU172.png)

接下来我们需要看一看加载之后的 DataFrame 的大概情况。前面的内容里介绍过，查看 DataFrame 的方法非常简单，只需要把 DataFrame 的变量放在 Cell 的最后一行并运行 Cell， Notebook 会自动打印出该 DataFrame 的概述。

现在，我们来分别查看一下这三个 DataFrame。新建 Cell，输入以下代码并执行。

Notebook 会有如下输出：

![Drawing 5.png](https://s0.lgstatic.com/i/image6/M00/40/B5/CioPOWCmOCqANxl3AACZJLJHi84499.png)

可以看到，df\_user\_log 一共包含 10,985,066 条记录。

同样的方法分别查看 df\_vip\_user 与 df\_user\_info， 代码与结果如下图所示。

输出结果：

![Drawing 6.png](https://s0.lgstatic.com/i/image6/M00/40/AD/Cgp9HWCmODWADV7KAABHziz0n7s845.png)

输出结果：

![Drawing 7.png](https://s0.lgstatic.com/i/image6/M00/40/AD/Cgp9HWCmOD6AWbWwAABCpU_ORoo569.png)

从上面 3 个 notebook 输出的表格摘要，我们对于三个表的数据量级有了一个大概的认识。

首先是量级上，user\_log 表的记录是最多的，有 1098 万行，user\_info 表则有 26 万行，vip\_user 表则有 42 万行。

其次，三个表都包含 user\_id 这一列，代表后续我们可以基于 user\_id 来做一些跨表联动分析。

还记得之前讲到，数据集中往往有缺失的数据，在 Notebook 中打印出来的时候一般显示 NaN。上面 vip\_user 表的概述也出现了 NaN，代表存在缺失的字段。所以在后面的步骤里我们还会针对每个表都进行缺失字段的分析与处理。

### timestamp 疑云

前面的内容，我们提到目前 user\_log 表中存在两个时间戳字段，比较迷惑。这两个字段究竟代表什么呢？

我们还是再看一次 user\_log 表的概要，如下图所示。

![Drawing 8.png](https://s0.lgstatic.com/i/image6/M01/40/AD/Cgp9HWCmOEmAXap6AACZJLJHi84201.png)

可以看出，两个时间戳字段 time\_stamp 和 timestamp 的值是不一样的。 time\_stamp 看起来是个整数，值比较小，而 timestamp 看起来是个浮点数，值比较大，普遍在五位数，其他暂时看不出什么有用的信息。

这个时候，对于不明确含义的字段，通常的做法是先看一下它的边界（即最大值和最小值）。

新建 Cell，输入以下代码并执行。下面的代码分别通过 pandas 的 max 函数和 min 函数，获取了 time\_stamp 与 timestamp 的最大值与最小值，并将通过在 print 函数中打印出来，并且因为打印的时候要和提示字符串进行拼接，所以 max 和 min 函数返回的结果都使用 str 函数进行了类型转换，将整数转换为了字符串。

```
time_stamp_max = str(df_user_log['time_stamp'].max())
time_stamp_min = str(df_user_log['time_stamp'].min())
print("time_stamp max: " + time_stamp_max, "time_stamp min: " + time_stamp_min)
timestamp_max = str(df_user_log['timestamp'].max())
timestamp_min = str(df_user_log['timestamp'].min())
print("timestamp max: " + timestamp_max, "timestamp min: " + timestamp_min)
```

输出以下结果：

```
time_stamp max: 1112, time_stamp min: 511
timestamp max: 86399.99327792758, timestamp min: 0.10787397733480476
```

可以看到，time\_stamp 的最大值为 1112，最小值为 511，而 timestamp 的最大值为 86399.99 最小值为 0.1。

从数据集的描述中，用户行为表是用户 6 个月的行为，那 time\_stamp 最大 1112，最小 511 看起来就特别像日期。代表最小日期是 5 月 11 日，最大日期是 11 月 12 日。

那既然 time\_stamp 是日期，那 timestamp 会不会是具体的时间呢？timestamp 的最大值为 86399 ，而一天最大的秒数为 24\*3600 = 86400。两个数字非常接近，那基本可以认定 timestamp 代表的是一天中的第几秒发生了这个行为。

破解了两个时间字段的问题，为了避免后面有歧义，我们将 time\_stamp 列重命名为 date。

新建 Cell，输入以下代码并执行。

```
df_user_log.rename(columns={'time_stamp':'date'}, inplace = True)
df_user_log
```

重命名 DataFrame 的某一列的名字，可以使用 rename 函数，第一个参数代表要重命名的列的映射关系，这里表示把 time\_stamp 重命名为 date，第二个参数 inplace = True 代表在当前 DataFrame 中生效。  
输出结果为：

![Drawing 9.png](https://s0.lgstatic.com/i/image6/M00/40/AD/Cgp9HWCmOFSAGIcpAAD3scaJFVY738.png)

可以看到，原来的 time\_stamp 列已经被改为 date。

### 清洗数据

在现实世界中，除了字段的含义可能有不对之外，数据集本身也会有缺失的情况，比如某些条记录缺少字段之类的。所以在正式分析之前，我们都需要对这些缺失值进行处理。

在前面的章节中，我们已经学过使用 isna 函数来获得缺失值，pandas 同时还提供了另一个类似的函数 isnull，这次我们来使用这个函数完成任务。接下来我们就使用这个工具来分析我们的三个表。

#### （1）清洗 user\_log 表

首先从 user\_log 表开始，新建 Cell， 输入以下代码并执行。

```
df_user_log.isnull().sum()
```

结果输出如下所示。

```
user_id            0
item_id            0
cat_id             0
seller_id          0
brand_id       18132
date               0
action_type        0
timestamp          0
dtype: int64
```

从上述结果中可以看出，user\_log 表中大概有 1.8 w 条数据缺少品牌 id 的字段，缺失率为0.16%（1.8w/1098w)，一般这个数据量级不会影响到数据分布的分析，暂时不处理。

#### （2）清洗 user\_info 表

接下来用同样的方法分析 user\_info 表。

```
df_user_info.isnull().sum()
```

结果输出如下所示。

```
user_id         0
age_range    2217
gender       6436
dtype: int64
```

从上述结果可以看出， user\_info 表中有 2217 条记录缺失了年龄字段，有 6436 条记录中缺失了性别字段。  
在后续的分析中，我们会尝试分析目标用户群性别和年龄的分布，所以这里需要对这些缺失的值进行处理。由于缺失的数据量较少，所以我们选择直接删掉这些有缺失的记录。

在章节的前面部分，我们讲过缺失值的常见处理方式，对于删除缺失值，可以使用 dropna 函数，所以这里我们可以轻松完成这个任务。如下所示。

```
df_user_info = df_user_info.dropna()
df_user_info
```

结果输出为：  
![Drawing 10.png](https://s0.lgstatic.com/i/image6/M01/40/AD/Cgp9HWCmOGaAbYzDAABCwfU5Sl4840.png)

可以看到 user\_info 的表记录数变为了 417708（之前是 424170，减少了 6462 条记录）。现在，我们通过上述方法排除了字段值为 NULL 的记录。

但你还记得我们对 user\_info 表中年龄字段的描述吗？age\_range 为 0.0 或者 gender 为 0 同样代表数据缺失。我们来看一下这部分的数据有多少。

```
print(df_user_info.loc[df_user_info["age_range"] == 0.0, ["user_id"]].shape)
print(df_user_info.loc[df_user_info["gender"] == 0.0, ["user_id"]].shape)
```

输出：

第一行代表年龄为空的记录数，第二行代表性别为空的记录数。整体的比例并不低，所以这里我们选择暂时保留这些数据，在后续分析环节再进行过滤。（因为当某条记录性别为空年龄不为空时，对于分析年龄分布仍然有价值，反之亦然）。

#### （3）清洗 vip\_user 表

接下来，我们同样使用 isnull 函数来分析 vip\_user 表。

```
df_vip_user.isnull().sum()
```

输出：

```
user_id        0
merchant_id    0
label          0
dtype: int64
```

从输出结果来看， vip\_user 这个表没有数据缺失，不用清理。

### 数据分布分析

接下来，就进入了我们本次分析的核心环节，数据分布分析。

我们希望从数据分布中分析出最有可能转化的用户的特征。直白地说，我们希望分析出目前阿普闪购的用户中，什么年龄段的用户最多，什么性别的用户最多。进一步，希望分析出下单的用户里，年龄和性别的分布，这样才能知道给哪个年龄段和哪个性别的用户发营销短信是最有用的。

这里我们会用到一个非常强大的 pandas 函数：value\_counts，value\_counts 一般是针对 Series 对象，用于统计 Series 中有多少个不同的值，以及每个值出现的次数。比如：

```
ser1 = pd.Series([1,1,3,3,3,3,4,5])
ser1.value_counts()
```

输出为：

```
3    4
1    2
5    1
4    1
dtype: int64
```

左边是值，右边是出现的次数，并且按照出现的次数从高到低排序。

通过 value\_counts 函数可以非常便捷地实现某个字段的分布分析。

#### （1）用户年龄分布分析

第一步，我们先分析用户表中用户的年龄段分布。如上文所说，数据分布可以直接使用 DataFrame 的 value\_counts 函数。

```
df_user_info.age_range.value_counts()
```

输出

```
3.0    110952
0.0     90638
4.0     79649
2.0     52420
5.0     40601
6.0     35257
7.0      6924
8.0      1243
1.0        24
Name: age_range, dtype: int64
```

除开未知的数据不看（假设未知的部分的分布符合整体的分布），可以发现，除去取值为 0 （代表未知）之外，取值为 3.0 和 4.0 的占到绝大多数，回忆上面数据集的定义，取值为 3 代表 25～30 岁，取值为 4 代表 30～34 岁。所以年龄在可以得到， 25～34 岁之间的用户占绝大多数。（age\_range 取值 3.0、4.0）。

然后，我们可以通过代码计算出 25～34 岁用户的比例。首先通过 loc 函数筛选出所有不等于 0 的记录，然后计算年龄段等于 3.0 与年龄段等于 4.0 的总数除以所有非 0 记录的总数，计算的代码如下：

```
user_ages = df_user_info.loc[df_user_info["age_range"] != 0, "age_range"]
user_ages.loc[(user_ages == 3) | (user_ages == 4) ].shape[0] / user_ages.shape[0]
```

输出

可以看到，25～34 岁用户占到了 58% 的比例。

#### （2）用户性别分布分析

接下来，我们使用 value\_counts 来计算用户信息中性别的分布信息。

```
df_user_info.gender.value_counts()
```

输出

```
0.0    285634
1.0    121655
2.0     10419
Name: gender, dtype: int64
```

结合之前的数据集定义，0 代表女性，1 代表男性，2 代表未知。可以看到，阿普闪购的核心用户群是女性，是男性数量的 2.35 倍。

从用户群体的分析上，我们大概已经勾勒出我们的目标用户画像，25~34 岁之间的女性群体。但目前分析的只是注册用户的信息，会不会存在男性虽然注册用户少但是购买力却更强呢？为了验证这个假象，我们就需要结合订单数据来分析。

#### （3）user\_log 与 user\_info 表合并

分析完了独立的用户信息表后，我们希望分析不同用户群的下单行为特征。但是下单量在 user\_log 表中，而用户信息在 user\_info 表中，这样的话就不能简单地用 value\_counts 来统计了，通过观察两个表的表结构不难发现，它们有共同的字段：user\_id。

像这样的场景，我们可以通过 user\_id 这样的共同字段来把两个表合并在一起。

```
df_user_log = df_user_log.join(df_user_info.set_index('user_id'), on = 'user_id')
df_user_log
```

上述代码将 user\_info 表合并到 user\_log 表，通过 user\_id 的字段进行关联，之后第二行我们查看最新的 user\_log 表，显示如下。  
![Drawing 11.png](https://s0.lgstatic.com/i/image6/M01/40/B6/CioPOWCmOIeAUg-8AAJ8F2VTW80886.png)

可以看到，user\_info 表的 age\_range、gender 字段已经被合并到了 user\_log 表中。接下来，我们就可以分析下单用户的性别与年龄特征了。

#### （4）不同年龄段用户下单行为分析

首先我们需要过滤出下单用户，在第 13 课时介绍过，我们可以使用 loc 函数来过滤出符合某个条件的记录。要过滤下单用户，集合数据集的定义，就是 action\_type = order。 我们搭配 loc 函数与 value\_counts 函数，即可实现针对下单用户的年龄段分析。

```
df_user_log.loc[df_user_log["action_type"] == "order", ["age_range"]].age_range.value_counts()
```

输出：

```
3.0    172525
4.0    153795
0.0    114908
5.0     79298
6.0     61534
2.0     59072
7.0     10785
8.0      1924
1.0        21
Name: age_range, dtype: int64
```

虽然 user\_log 表中有很多行为，但这里我们核心关注的还是最终的转化：下单，所以我们筛选了行为为 “order”的记录，查看其年龄段分布。可以看到，下单的年龄段分布和用户信息的分布基本一致，25-34 岁的人占到 59.9%。

#### （5）不同性别用户下单行为分析

接下来，我们进行下单用户的性别分析。

```
df_user_log.loc[df_user_log["action_type"] == "order", ["gender"]].gender.value_counts()
```

输出

```
0.0    467381
1.0    161999
2.0     24482
Name: gender, dtype: int64
```

可以看到，女性用户不仅注册用户远超过男性，购买力同样惊人，是男性下单量的 2.9 倍。基本可以确定，我们发送营销信息要聚焦的用户群应该是 25～34 岁的女性。

用户群确定了之后，下一步要确定发送的时间。接下来，我们需要分析用户下单的时间特征。

#### （6）不同日期的下单行为分析

虽然我们可以直接对 date 字段进行 value\_count，但我们更希望是找到一个适合投放的日期范围，而不是具体某一天。所以这里我们使用分组的 value\_count, 下面的代码我们将所有日期分成六组，统计每组的订单量的分布。

对 value\_counts 的结果进行分组，我们之前介绍过，可以通过给 value\_counts 函数增加 bins 参数，bins 的值代表要分成几组，之后的数据分布就会按组输出。在这里，因为数据集是半年的，所以我们分六组（看每个月的分布）。

```
df_user_log.loc[df_user_log["action_type"] == "order", ["date"]].date.value_counts(bins = 6)
```

输出

```
(1011.0, 1111.0]    333721
(811.0, 911.0]       70699
(911.0, 1011.0]      69427
(510.399, 611.0]     68776
(611.0, 711.0]       62901
(711.0, 811.0]       54053
Name: date, dtype: int64
```

可以看到，用户在 10月中下旬一直到 11 月上旬这个时间段，下单量较为集中。分析完了日期分布后，接下来我们分析一下一天中的时间段的分布。

#### （7）不同时间段的下单行为分析

timestamp 字段存储了每条记录下单的时间，从当天零点开始累积的秒数。并不是很直观，我们更希望可以基于小时级的数据去分析。所以我们考虑基于 timestamp 这一列，新创建一列时间，来表示小时。

根据高级索引一讲我们学习的内容，新建列只需要直接给对应的列名赋值即可，值需要是一个合法的 Series。在这里，我们就是以 timestamp 列为基础，将其值除 3600， 然后用这个值创建一个新的列：time\_hours\_view.

```
df_user_log.loc["time_hours_view"] = df_user_log["timestamp"]/3600
df_user_log
```

输出：

![Drawing 12.png](https://s0.lgstatic.com/i/image6/M01/40/B6/CioPOWCmOLSATUfpAALItLUe5Q0881.png)

从图中可以看到，我们的 time\_hours\_view 已经被添加到了表格的最后一列。

接下来的事情就比较简单了，我们直接用 value\_count 来统计新增的 time\_hours\_view 字段，就可以实现对一天中的小时级分布进行分布统计。我们以两个小时为尺度，来查看分布，所以分为 12 组。

```
df_user_log.loc[df_user_log["action_type"] == "order", ["time_hours_view"]].time_hours_view.value_counts(bins = 12)
```

输出

```
(20.0, 22.0]     94209
(22.0, 24.0]     91529
(18.0, 20.0]     91330
(16.0, 18.0]     85681
(14.0, 16.0]     75372
(12.0, 14.0]     63580
(10.0, 12.0]     50909
(8.0, 10.0]      38938
(6.0, 8.0]       27962
(4.0, 6.0]       19428
(2.0, 4.0]       12639
(-0.025, 2.0]     8000
Name: time_hours_view, dtype: int64
```

从上述结果可以看到，晚上 8 点到 10 点之间是下单最为密集的，订单量为 94209 单。

### 小结

至此，我们本次短信营销的目标人群和时间就已经基本分析完毕了。我们应该针对 **25～34 岁的女性，在 10 月中下旬到 11 月中旬的晚上 8 点到 10 点进行短信的批量发送，这样应该可以收获最好的转化效率。**

让我们一起来回顾一下这次任务的方法步骤：

-   认真阅读数据源描述；
    
-   结合数据源描述和观察数据集的字段，以及抽样查看一些记录，查看是否有错误的地方；
    
-   筛选数据表中的缺失值以及结合缺失值的量级决定保留或者抛弃；
    
-   针对原始表进行数据分布分析（user\_info 表）；
    
-   多个表联合查询数据分布（user\_info 联合 user\_log 表）；
    
-   对于连续值， 通过 bins 参数实现分段统计。
    

再回顾一下，我们这个案例中用到的关键技术：

-   解决 timestamp 问题时，我们通过 max 函数和 min 函数，分析了部分列的边界；
    
-   清洗数据时，我们使用 isnull 函数来帮助我们发现缺失值，使用 dropna 函数来删除包含缺失值的记录；
    
-   在分布分析时，我们使用 value\_counts 来计算某个字段的数据分布，并且针对部分数据范围很大的字段，使用 bins 参数来对分布结果进行分组；
    
-   当我们需要将具备共同字段的多张表拼接到一起时，使用 join 函数，并且在参数中通过 set\_index 指定共同字段；
    
-   在部分字段不方便直接分析时，比如一天中的秒数，我们可以通过 df.apply 来将其转化为一个易于分析的新字段，比如我们将其转换为小时级的数值；
    
-   在需要数据筛选时，使用 loc 函数配合条件，可以筛选出 DataFrame 中符合条件的一个子集，比如 user\_log 表中，我们仅关注下单的记录，所以就可以通过 loc 函数配合 action\_type=="order" 筛选出相应的记录。
    

这次，我用了最基础的基于数据分布的分析法来进行分析，其实还有其他的办法，你知道哪些更好的方法与思路？可以写在留言区与大家分享。
