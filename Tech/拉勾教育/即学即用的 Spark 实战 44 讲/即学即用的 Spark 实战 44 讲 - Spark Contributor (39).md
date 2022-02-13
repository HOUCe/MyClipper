---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971
author: 
---

# [即学即用的 Spark 实战 44 讲 - Spark Contributor - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=71#/detail/pc?id=1971)


在本模块中，我为你设计了一个基于真实数据的项目，也就是前面提到的商业智能系统，可以让你对 Spark 有更形象的认识，并对前面学习的知识进行巩固。为了便于理解，该模块简化了业务复杂度和技术复杂度，作为你的第一个 Spark 项目是比较合适的。

本课时的主要内容是：

-   美国的大众点评网 Yelp 和 Yelp 2016 Dataset Challenge 数据集介绍。
    
-   作为 Yelp 运营负责人，你希望知道什么？
    

### Yelp 与数据集介绍

Yelp 由前 PayPal 员工罗素·西蒙斯和杰里米·斯托普尔曼于 2004 年在美国旧金山成立，公司发展迅速，得到几轮融资支持，2010 年营业额达到 3 千万美元，以及 450 万评论量。2009 年到 2012 年，其在欧洲和亚洲的业务也蓬勃发展。

这是一个著名的商户点评网站，与我们的大众点评网类似，美国各地用户只要注册就能在 Yelp 网站上给商户打分、评论，还能和其他用户交流感想体验。Yelp 上的商户包括各地餐馆、购物中心、酒店、旅游等。

就像 Yelp 的 slogan 一样，这家公司看重的是真实客户的真实评价，尤其注重把一小部分热衷点评的用户吸引过来，同时给予优质客户奖励。这种形式提高了 Yelp 网站上各种信息的可信度，也把有深入体验的优质用户和那些走马观花的用户区分开了。

Yelp 在 2016 年公开了其内部业务数据集（Yelp 2016 Dataset Challenge），供数据科学竞赛爱好者使用，Kaggle 也收录了该数据集（下载地址：[https://www.kaggle.com/yelp-dataset/yelp-dataset](https://www.kaggle.com/yelp-dataset/yelp-dataset)）。由于是真实数据集，其中有不少有趣的内容，我们这次的项目也是基于该数据集。

下面我们对数据集进行一个介绍，它包括以下几个表：

-   业务表 (businesss)
    
-   评价表 (reviews)
    
-   小贴士表 (tips)
    
-   用户信息表 (user information)
    
-   签到表 (check-ins)
    

数据集共 10G 左右，包含 668 万条评论。它来自 19 万个商业机构，涵盖了 10 个都市区域，有大概 100 多万个属性标签。此外，Yelp 还提供了一些图像数据，比如商户的图片数据，由于在本项目中用不到，故不对其进行介绍。

#### business 表

以下代码是以 Json 格式表示的 business 表中的一条数据，代表了一个商业组织的相关数据（例如某个餐馆）。

```
{
"business_id": "tnhfDv5Il8EaGSXZGiuQGg",
"name": "Garaje",
"address": "475 3rd St",
"city": "San Francisco",
"state": "CA",
"postal code": "94107",
"latitude": 37.7817529521,
"longitude": -122.39612197,
"stars": 4.5,
"review_count": 1198,
"is_open": 1,
"attributes": {
"RestaurantsTakeOut": true,
"BusinessParking": {
"garage": false,
"street": true,
"validated": false,
"lot": false,
"valet": false
        },
    },
"categories": [
"Mexican",
"Burgers",
"Gastropubs"
    ],
"hours": {
"Monday": "10:00-21:00",
"Tuesday": "10:00-21:00",
"Friday": "10:00-21:00",
"Wednesday": "10:00-21:00",
"Thursday": "10:00-21:00",
"Sunday": "11:00-18:00",
"Saturday": "10:00-21:00"
    }
}
```

为了让你对这些字段有更直观的认识，我截取了 Yelp 网站商家主页的部分内容，如下图所示，网页内容可以很容易地和上述数据对应。

![Drawing 0.png](https://s0.lgstatic.com/i/image/M00/46/50/Ciqc1F9E02aAJy1jAAOsbGl7FgA043.png)  
![Drawing 1.png](https://s0.lgstatic.com/i/image/M00/46/50/Ciqc1F9E02-AYCSuAACY_JFFhNA737.png)

结合网页内容，我们可以更形象地观察上面的代码，它包含商家的开业时间、评论条数、地理位置，以及属性值与类目信息等。

#### review 表

以下代码是以 Json 格式表示的 review 表中的一条数据，代表了一个用户（user）对一个商业机构（busniess）的一条评论。

```
{
"review_id": "zdSx_SD6obEhz9VrW9uAWA",
"user_id": "Ha3iJu77CxlrFm-vQRs_8g",
"business_id": "tnhfDv5Il8EaGSXZGiuQGg",
"stars": 4,
"date": "2016-03-09",
"text": "Great place to hang out after work: the prices are decent, and the ambience is fun. It's a bit loud, but very lively. The staff is friendly, and the food is good. They have a good selection of drinks.",
"useful": 0,
"funny": 0,
"cool": 0
}
```

结合网页中与评论相关的内容进行观察，如下图所示：

![Drawing 2.png](https://s0.lgstatic.com/i/image/M00/46/50/Ciqc1F9E03uAV6oEAAVp7j4PaHc367.png)

评论表的一条数据包含完整的评论文本数据，包括撰写评论的 user\_id 和撰写评论的对象 business\_id。结合网页，同样会让你有更形象的感知。

#### user 表

以下代码是以 Json 格式表示的 user 表中的一条数据，代表了一个用户的基本情况。

```
{
"user_id": "Ha3iJu77CxlrFm-vQRs_8g",
"name": "Sebastien",
"review_count": 56,
"yelping_since": "2011-01-01",
"friends": [
"wqoXYLWmpkEH0YvTmHBsJQ",
"KUXLLiJGrjtSsapmxmpvTA",
"6e9rJKQC3n0RSKyHLViL-Q"
    ],
"useful": 21,
"funny": 88,
"cool": 15,
"fans": 1032,
"elite": [
2012,
2013
    ],
"average_stars": 4.31,
"compliment_hot": 339,
"compliment_more": 668,
"compliment_profile": 42,
"compliment_cute": 62,
"compliment_list": 37,
"compliment_note": 356,
"compliment_plain": 68,
"compliment_cool": 91,
"compliment_funny": 99,
"compliment_writer": 95,
"compliment_photos": 50
}
```

而与数据对应的网站用户主页部分如下图所示：

![Drawing 3.png](https://s0.lgstatic.com/i/image/M00/46/5B/CgqCHl9E04eAUECAAAK5xvD2DYI866.png)

一条用户数据包括该用户的朋友以及与该用户关联的所有元数据。

#### tip 表

以下代码是以 Json 格式表示的 tip 表中的一条数据，代表了一条简短的评论。

```
{
"text": "Secret menu - fried chicken sando is da bombbbbbb Their zapatos are good too.",
"date": "2013-09-20",
"compliment_count": 172,
"business_id": "tnhfDv5Il8EaGSXZGiuQGg",
"user_id": "49JhAJh8vSQ-vM4Aourl0g"
}
```

与数据对应的网站小贴士页如下图所示：

![Drawing 4.png](https://s0.lgstatic.com/i/image/M00/46/50/Ciqc1F9E05KACWWQAAF_7xDrN9M074.png)

这种小贴士的对象同样是商家，但小贴士比评论更简短，并且倾向于传达快速的建议。

#### checkin 表

以下代码是以 Json 格式表示的 checkin 表中的一条数据，代表了一个商户的签到情况，包括商户的 business\_id 及顾客的签到时间。

```
{
"business_id": "tnhfDv5Il8EaGSXZGiuQGg"
"date": "2016-04-26 19:49:16, 2016-08-30 18:36:57, 2016-10-15 02:45:18, 2016-11-18 01:54:50, 2017-04-20 18:39:06, 2017-05-03 17:58:02"
}
```

### 从 Yelp 运营负责人的视角分析

高质量的评论是 Yelp 的灵魂，作为 Yelp 的运营负责人，你希望能从评论中发现些有趣的东西，从而更好地帮助商家，活跃用户。

从数据方体的角度来看，review 表中 stars、useful、funny、cool 字段就是度量，而 busniess 表中的 city、state 字段以及 review 表中的 date 字段都可以作为时间与空间维度。通过数据方体与多维分析，能够很好地对数据进行分析。

Yelp 的商业智能系统希望构建一个关于评论的数据方体，并进行相应的分析与可视化。通过这种方法，将得到以商业机构为主体的相关“业务知识”，以辅助运营人员更好地决策。例如评分更高的商业主体有哪些特征，以及有哪些特征是我们平时所忽略的，等等。详细方法在之后的课程会展开。

这里注意，为了简化，本项目只会构建一个数据方体，但是在一般规模的生产环境中，商业智能系统需要构建的数据方体的数量非常多，基础数据也远远不止我们这个项目中的 5 张表。

**需要特别说明的一点是**，通常在数据仓库或者商业智能系统项目开始时，你并不会直接获取到上面下载的 Yelp 的数据集，我们需要的数据往往在业务数据库中，所以下个课时，我们将模拟将数据从业务数据库中导出的过程。

### 总结

作为项目开始的第一个课时，我主要带你熟悉数据集、数据结构，了解商业智能系统的分析需求。为了便于理解，无论是对数据集还是分析需求都做了大量简化。如果是真实情况，每个环节都会增加上百倍的复杂性。所以近期课程都需要你反复巩固，以便更好地运用到工作中。
