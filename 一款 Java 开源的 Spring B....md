# 一款 Java 开源的 Spring Boot 即时通讯 IM 聊天系统（附源码）

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzIyNTY4NjU0OQ==&mid=2247512110&idx=2&sn=4c02d0db4042da216d3bf15a2c1274e3&chksm=e8791554df0e9c421974fd1e2a4b6768a4ceb4ca3045c158e07e8d695d971cc2d68226e9a44f&mpshare=1&scene=1&srcid=1216MYbIyKwNgp2AvXyiNLC5&sharer_sharetime=1639613238845&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)秦子帅

> 来自：gitee

往期文章：[290家公司都在用的任务调度系统，还开源了（附源码）](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484212&idx=1&sn=6eed64a80ac91759db1a85de1f13e92a&chksm=c02b86a5f75c0fb3df43e4a040b79c1b8b9e712cca41c2337e3504ad15142913a1d103f35c68&scene=21#wechat_redirect)

**正文**

大家好，我是GitHub源码。今天，推荐一个即时通讯系统项目。

上次是谁要的即时通讯系统项目啊，猿哥帮你找到了。

这是我目前见过的最好的即时通讯系统项目。功能完整，代码结构清晰。值得推荐。

**开篇**

电商平台最不能缺的就是即时通讯，例如通知类下发，客服聊天等。今天，就来给大家分享一个开源的即时通讯系统。如对文章不感兴趣可直接跳至文章末尾，有获取源码链接的方法。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FWZoDoXZNb0truJdvOKMK64M4wB6E70DrgaXuszK8iaosNYbnDznXvGuGd1pMeSbrKkqReiaD8O9CibaRKIMnnDBbQ%2F640%3Fwx_fmt%3Dpng)

但文章内容是需要你简单的过一遍的，相信你能get到不少骚操作。

## **项目简介**

该项目是一套基于mina或netty框架下的推送系统,或许有一些企业有着自己一套即时通讯系统的需求，那么CIM为您提供了一个解决方案，目前CIM支持websocket，android，ios，桌面应用，系统应用等多端接入支持,可应用于移动应用，物联网，智能家居，嵌入式开发，桌面应用，WEB应用以及后台系统之间的即时消服务。

## **项目架构**

即时通讯聊天的架构都相对较简单，一般都是服务端+客户端，能实现用户A到用户B的聊天；含金量在于看看支不支持集群扩展。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2FR5ic1icyNBNd6I3dJicbNSAVN21OhVicvGr9Ml3vEu6x2d7vfy8ZP0y0X0W8zwBzH6EezfeqUGIeaiaJnryHgQx07jQ%2F640%3Fwx_fmt%3Djpeg)

## **项目主要模块**

项目分为，服务器端，和客户端，服务端是netty 整合websocket，客户端形式多种多样，都是调用服务端的，本篇就不重点介绍了。

**目录说明**

*   cim-use-examples是各个客户端使用示例
    
*   cim-client-sdk 是各个客户端的SDK源码-
    
*   cim-server-sdk 是服务端SDK源码,分为 mina和netty 两个版本，二者任选其一-
    
*   cim-boot-server是springboot服务端工程源码，使用Idea工具开发-
    

其中所有的sdk均为IntelliJ IDEA工程，Maven打包成jar导出引入到对应的客户端或服务端工程。搜索公众号GitHub猿回复“监控”，送你一份惊喜礼包。

## **功能预览**

1、控制台页面http://127.0.0.1:8080

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FWZoDoXZNb0truJdvOKMK64M4wB6E70Dr4CtBNOG70oUrDYF5l3ACCCBSzE8YyHGCt5MkJ2JKkabGibbn5fdGUEQ%2F640%3Fwx_fmt%3Dpng)

2、Android客户端

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FWZoDoXZNb0truJdvOKMK64M4wB6E70DrB2J0FcKF6ZR6goCMhfGwibXEmKaWd0iauRUj1GfaqLt67FKjLfv0Eqog%2F640%3Fwx_fmt%3Dpng)

3、Web客户端

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FWZoDoXZNb0truJdvOKMK64M4wB6E70Dr1uyJaVpYk6V2eKqTI6fKO5Zvzo9jVRrHFiaveVH6MK0o5MTlQfUcCNw%2F640%3Fwx_fmt%3Dpng)

## **结语**

此套开源的即时通讯系统，可以改成推送的，也可以改成聊天的，后端改改可以拿来直接使用，重点不在前端，但android 和ios还有web都支持，自己看代码中的例子吧，值不值得收藏，自己先看看文章，觉得可以收藏一下，慢慢看。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FR5ic1icyNBNd7gCjTYibduPsGTxuPTmbuBTIzPJgA0yYfg7ibiabBNY0jLqo1vEII4icCp8rCfmcIwMqicXXibd2FSOCyQ%2F640%3Fwx_fmt%3Dpng)**源码获取**![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FR5ic1icyNBNd7gCjTYibduPsGTxuPTmbuBTIzPJgA0yYfg7ibiabBNY0jLqo1vEII4icCp8rCfmcIwMqicXXibd2FSOCyQ%2F640%3Fwx_fmt%3Dpng)-

扫码下方二维码，后台回复【**聊天系统**】即可获取

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2FEa7dkfAlbibnyYRRu5ia5GFCQPq6s4XnhjesaLuKiafL412xuHC54Cg12jnGG0BS32OmnLQWTysLOiblnnWkt5Lb7g%2F640%3Fwx_fmt%3Djpeg)

##  

 

 往日文章： 

 

 [290家公司都在用的任务调度系统，还开源了（附源码）](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484212&idx=1&sn=6eed64a80ac91759db1a85de1f13e92a&chksm=c02b86a5f75c0fb3df43e4a040b79c1b8b9e712cca41c2337e3504ad15142913a1d103f35c68&scene=21#wechat_redirect)-
 

 

 [12 个非常适合做外包项目的开源后台管理系统](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484204&idx=1&sn=fddf3a241d537596d1b509dbd12a2757&chksm=c02b86bdf75c0fabf969fa0abb03d989ccc5bd8b0b65b6313b857c29fd860dca4dff452c5e75&scene=21#wechat_redirect)-
 

 

 [Java身份证号码识别系统（开源项目）](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484189&idx=1&sn=22b50a9cc996cf370312695d93eef137&chksm=c02b868cf75c0f9a3d19c85beb9da1e5ac72def0ac73711a78b04834d283bd18693e667beefc&scene=21#wechat_redirect)-
 

 

 [Java私活神器，分享一套SpringBoot+Vue通用的后台管理系统](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484180&idx=1&sn=1424e68603838bbbdbae77a5e1e8c56a&chksm=c02b8685f75c0f93a6b8e81956ae0511448e165a3cba573177916879a8a55ab5e667d29a09c9&scene=21#wechat_redirect)-
 

 

 [看看人家那电影系统，那叫一个优雅（附源码）](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484147&idx=1&sn=49a4db362ebbfd568b8d9b0bdeb4a5f4&chksm=c02b8762f75c0e74f096690e7d7857b4e163df3c579b5e0d46791dc8b5325ea9873e408fc4ee&scene=21#wechat_redirect)-
 

 

 [这后台管理系统，有逼格。（附源码）](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484132&idx=1&sn=f55c5d4d9c68b626b74e425a432a0f99&chksm=c02b8775f75c0e63338a70cf1de421d62f0e4d44e47a4a0d480c581eb6a868293c24b2f58540&scene=21#wechat_redirect)-
 

 

 [一个基于 SpringBoot2+redis+Vue 的商城管理项目，拼团、砍价、秒杀等都有，可二次开发接私活！](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484099&idx=1&sn=59d5a5819e5a717ebc66f579988ad4a0&chksm=c02b8752f75c0e4401821509db936d094380514a238aaf3bf94662c3308401c1c1fd4f0e61ae&scene=21#wechat_redirect)-
 

 

 [「开源」目前见过的最好的开源OA产品](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484091&idx=1&sn=fc00c64fa123ee4020cada74afc20eda&chksm=c02b872af75c0e3c7e9348815fe296c9d83942435ba1194fa7dabcb2357c358e03e288f00dd3&scene=21#wechat_redirect) -
 

 

 [牛逼，一套标星 11.2k 的公有云文件系统（附源码）](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484075&idx=1&sn=98227ef72775dd99197e2f280dcc8c03&chksm=c02b873af75c0e2cd454022930241820394998f58d792e9bfe3b0c3e184d926dc38b95f4560e&scene=21#wechat_redirect)-
 

 

 [推荐SpringBoot互联网企业级别的开源支付系统](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484052&idx=1&sn=57226d7ac81a85b801699fa7fed021a1&chksm=c02b8705f75c0e137659d397cd658123456b3d81edb40c9d027780518c091ace30f9c99e058c&scene=21#wechat_redirect)-
 

 

 [看看人家那文件传输神器，那叫一个优雅（附源码）](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484045&idx=1&sn=7d116238becdf5f0a950e2fc6ae11135&chksm=c02b871cf75c0e0aab4974c5ca0fe08147fc6ceecca6d707f0fee22ce48b23fe92e8122e8686&scene=21#wechat_redirect)-
 

 

 [GitHub 星标 17K，开源的在线转换智能表格项目（附源码）](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484028&idx=1&sn=86c5837f75bd1316cf413383b025ab0b&chksm=c02b87edf75c0efbf120d69be6accac646b16c4f666aca7ce0b20bada9f394df291153d31495&scene=21#wechat_redirect)-
[SpringBoot+Vue企业级支付系统！附源码！](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484020&idx=1&sn=d9d9672ab7842bdf876953610d007bd6&chksm=c02b87e5f75c0ef35450baff8b0e398a59c74482106d612c020cbe0dc55ca989fabab1bda2da&scene=21#wechat_redirect)-
 

 

 [分享一款基于 SpringBoot + ElementUi 的HC小区物联网平台](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484012&idx=1&sn=a4eb3c865062940f0db6edf5a048c30d&chksm=c02b87fdf75c0eeb24252764d5fdd935d72d6e7dab116dd745fe7d19a077df18e19a3255ac53&scene=21#wechat_redirect)-
 

 

 [这或许是最美的开源后台管理UI（附源码）！](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247484004&idx=1&sn=23172b40ec083470172e17e45e97bb5f&chksm=c02b87f5f75c0ee36a4f7b41e04d30d732a1b4bd467c18df0040b7a11901ec023fe171ffbc41&scene=21#wechat_redirect)-
 

 

 [Java学生宿舍管理系统，附上源码 ！](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247483990&idx=1&sn=c394083831f482fc9b0c11f45ced6ea9&chksm=c02b87c7f75c0ed18d25448c4c21cf7400ca8c40f86034469a5ea16235d1a333b3bc85885695&scene=21#wechat_redirect)-
 

 

 [看看人家那音乐网站系统，那叫一个优雅（附源码）！](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247483983&idx=1&sn=1dcbe9b3cf1a13d8500c714adcc4b5d4&chksm=c02b87def75c0ec85174f9d51da8f9cd6317a1b50f397cb54f5bb2017dafbde5d0f7051fae64&scene=21#wechat_redirect)-
 

 

 [强势开源一款完整的社交电商小程序！](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247483975&idx=1&sn=d9c242054cf4087fa3e8d4ab92e88390&chksm=c02b87d6f75c0ec00b33110bdc3bbf476e9ddce1573dbef10b48dd9d592ca52d8c1ac53571c6&scene=21#wechat_redirect)-
 

 

 [基于SpringBoot的仿豆瓣平台，附完整源码！](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247483963&idx=1&sn=076b318338c046d4cf4ca1d02e7be5e4&chksm=c02b87aaf75c0ebcda89fcfd8ff4df7c4531821fbb99b94cd5f7f6f2962b46ab8615e106f7c3&scene=21#wechat_redirect)-
 

 

 

 

 

 

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2FEa7dkfAlbibn9gUdryMiaCrTiaK7m5rLaz9S3YylFMWfqKItGrIUrgGlSkkRia57PWkTiajnIftSibsUYSs0qyXWtibSw%2F640%3Fwx_fmt%3Djpeg)

 

 

 **明天见(｡･ω･｡)ﾉ** 

 

-
版权申明：内容来源网络，版权归原创者所有。除非无法确认，我们都会标明作者及出处，如有侵权烦请告知，我们会立即删除并表示歉意。谢谢!

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzIyNTY4NjU0OQ==&mid=2247512110&idx=2&sn=4c02d0db4042da216d3bf15a2c1274e3&chksm=e8791554df0e9c421974fd1e2a4b6768a4ceb4ca3045c158e07e8d695d971cc2d68226e9a44f&mpshare=1&scene=1&srcid=1216MYbIyKwNgp2AvXyiNLC5&sharer_sharetime=1639613238845&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)