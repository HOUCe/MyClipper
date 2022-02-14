---
created: 2021-11-10
source: https://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247484951&idx=1&sn=b238bc917f1cfaa584191345cfc0389a&chksm=e9212476de56ad60a5b2b2767c1339fb6b310cd4cd18db67e7aeec9ff2bdcd7b0329a6fc8d88#rd
author: 
---

# [flomo weekly vol.034 - flomo to Notion](https://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247484951&idx=1&sn=b238bc917f1cfaa584191345cfc0389a&chksm=e9212476de56ad60a5b2b2767c1339fb6b310cd4cd18db67e7aeec9ff2bdcd7b0329a6fc8d88#rd)


我对 Notion 有很深厚的感情，在设计 flomo 之前，我一直在 Notion 中经营「产品沉思录」这个数字花园。

由于 Notion 的数据库及强大的排版、协作功能，让我这个不会编程的人得以高效地完成这座数字花园的建设和维护，并使其成为一份重要的个人数字资产 —— 四年来，里面存储了上千篇关于互联网产品、人生哲学的内容积累，以及手工整理的九个大专题。同时还有几千位读者在其中阅读、互动。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wDNLH7zcd1ON9iac3LHp6ibhtpV430zbWK7d2D9G5iblofEvpkPkHSxJPNy8HqNgDCkxbTbiacrFXsSPG1QM2o0SIA/640?wx_fmt=png&tp=png&wxfrom=5&wx_lazy=1&wx_co=1)

但一个花园的耕耘，并非朝夕之力，更需要源源不断的信息溪流，来对花园进行灌溉。而我们今日处在一个碎片化的时代，如果不能有意识地对信息进行捕捉和思考，就会被洪流吞噬。

**收藏稍后阅读？基本上不会再读；把口味交给推荐引擎？那便是自己钻进信息茧房。**

既然碎片化时代已经到来，那么就尝试将碎片的信息转化为知识卡片，然后用来灌溉自己的数字花园，建设知识宫殿。

flomo 便是在这个情境下诞生的。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

所以随着 Notion 的 API 发布，我们第一时间酝酿支持。

这或许是中文领域首个支持 Notion API 的笔记工具吧。这样之前介绍的 [**用 flomo + notion 打造流动的知识体系**](http://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247484316&idx=1&sn=b855c3c94e78be0e70c7638a5961bda4&chksm=e92121fdde56a8ebfca2f1b7dbb9ea642ecbce2c43e3204be61fccf97c78397db6382c3a6a01&scene=21#wechat_redirect) ，就可以更简单地操作了。

从竞争的视角来看，我们将最核心的数据共享给了「竞争对手」，丧失了一条重要的护城河。但如 [**之前的文章**](http://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247484695&idx=1&sn=4d684ce42099c3fd4708be72c8cc37e5&chksm=e9212776de56ae60b852d0e8e39cbae81863504bb8324d42e714d1905a2cc965b0a798854770&scene=21#wechat_redirect) 所说，flomo 的竞争壁垒从来不是产品功能设计，也不是贩卖用户隐私，**而是共建者的支持与认可。** 从这个角度来看，让共建者们的数据流动更自由，也是我们愿意为之努力的目标。

由于 Notion 数据库很强大，**所以我们选择了实时全量导出到 Notion。**这样相当于你在 flomo 中的所有记录，都能第一时间导入到 Notion 的数据库。同时，基于 Notion 数据库强大的表现力，你可以进行新的展示和加工。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们不会选择 All in one，我们甘愿在幕后做些不起眼的小事 —— 帮你更方便地记录，建立连接、回顾。我们愿意做一些小事，能陪伴一些人，足够久。

流水不争先，争的是滔滔不绝。

关注「Notion中文社区」公众号，学习更多 Notion 用法

___

**Notion 同步能达到以下效果：** 

-   全量推送 flomo 内容至 Notion 的指定 database。（这里不做定制输出，是因为 Notion 里 database 的筛选和排序足够强，可以自由决定如何处理） 
    
-   你在 flomo 内创建新 memo，以及编辑 memo，均会自动同步至 Notion。在 Notion 中删除不会影响 flomo 的内容。
    
-   你在 flomo 删除 memo，不会删除 Notion 的对应内容。
    
-   同步内容包含 内容、创建日期、 标签、附件四个属性（其中附件正在等待 Notion 支持），标题部分会取 memo 的正文前几个字。 
    

**由于目前 Notion 的 API 还处于 beta（测试版）状态，有一些不如意的地方：**

-   同步速度会有一定延迟（尤其是首次），因为限制了 API 请求速度
    
-   暂时不支持同步图片 （等待 Notion 开放）
    
-   格式会有一定的丢失（比如加粗） 
    

后续随着其 API 进一步完善，我们也会不断地跟进。 **如需更多帮助，请点击阅读全文，或加入微信群交流。**

flomo to Notion 是 flomo 会员特有功能，成为 flomo 的 `PRO` 会员后，请用电脑访问 flomoapp.com\[1\] ，在设置 - 账号详情（最底部）找到同步按钮，根据提示操作即可。如有问题，欢迎扫描文章底部二维码入群，或访问：

https://help.flomoapp.com/advance/notion-sync.html

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

没有购买会员的用户，只需邀请你的好友加入 flomo，即可获得 flomo 会员。前往【菜单-设置-有些福利】，可以复制你的邀请链接。😉   

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

___

我们为大家准备了一些 flomo + Notion 联动的使用场景。如果你有一些更有趣的用法，欢迎在文末加入我们的社群，一起分享共建。

## 1\. 筛选某类 flomo 内容作为素材库

1.在 flomo database 中设置过滤器（或者直接搜索），选择你想要找到的某些标签和素材（注意由于 Notion 不支持多级标签，所以如果是多级标签内容，请注意多选）

2.切换为合适的展示方式（比如 Gallery view）

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3.找到合适的素材内容，将其挪至你在 Notion 中其他的素材库备选（当然也可以复制一份再同步过去）

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

4.也可以在 page 的右侧新建一列，这样就能够边写边看准备的资料咯

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2\. 做一个 flomo 内容的共享主页

1.设置 flomo database 的过滤器，选择你想公开的标签内容（注意由于 Notion 不支持多级标签，所以如果是多级标签内容，请注意多选）

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2.选择对应的展示样式，然后锁定当前的 database

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3.将此页面设置为共享，即可供大家访问啦。这样后续你在这个标签下记录的 flomo 内容，就可以自动同步到这里。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

你可以复制链接打开，查看效果

_https://www.notion.so/43dda59002864a2da8beb92b887c6975_

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 3\. 通过 Group by 功能，查看自己每周的记录  

Group 是 Notion 新增的重要功能，可以方便的将其中的数据按照某些条件分组。这里只是一个简单的应用。

1.设置 flomo database 的 group by 的类型如下图  

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2.根据需要选择 view 视图，就可以很方便的看到每月/每周/每年写的 memo 数量，以及根据需要单独展开访问其内容。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 欢迎加入 flomo 共建者群探讨交流  

### ![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 如需更多操作帮助，请点击「阅读原文」

### References  

`[1]` flomoapp.com: _http://flomoapp.com_
