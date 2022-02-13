# 「开源」目前见过的最好的开源OA产品

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzIyNTY4NjU0OQ==&mid=2247511718&idx=2&sn=d9f3cfb7ea939c08975f68707870dbbd&chksm=e8790bdcdf0e82ca5b663b579d5569205aa41e3475927ba6b136350aa398859e9811f30bcc01&mpshare=1&scene=1&srcid=1203a9tpFUh5pei00PLNI8Fc&sharer_sharetime=1638486791723&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)秦子帅

上次是谁要OA的项目啊，Java项目分享帮找到了-

这是我目前见过的最好的开源OA产品。功能完整，代码结构清晰。值得推荐。

## 1.项目介绍

oasys是一个OA办公自动化系统，使用Maven进行项目管理，基于springboot框架开发的项目，mysql底层数据库，前端采用freemarker模板引擎，Bootstrap作为前端UI框架，集成了jpa、mybatis等框架。作为初学springboot的同学是一个很不错的项目，如果想在此基础上面进行OA的增强，也是一个不错的方案。关注 Java项目分享

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FdCtZK7DicnAhI5jxrO075VlnACBkCZZQN3gCJuadL6tM65HtRcSNsuGZVsyGVDA4haVibM6vTQFZHzEkRvAKVVfw%2F640%3Fwx_fmt%3Dpng)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FdCtZK7DicnAhI5jxrO075VlnACBkCZZQNI1agOmoqX1bDEVZN7tzlJAbTyBb9DHLLib69yRsKCtqZTPHkS7DP4cA%2F640%3Fwx_fmt%3Dpng)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FdCtZK7DicnAhI5jxrO075VlnACBkCZZQNv3tCaibjdTVvkAVdibB1qvsj51MFiama9Y4E5xYiaCStTXqGnMicWSwsjdg%2F640%3Fwx_fmt%3Dpng)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FdCtZK7DicnAhI5jxrO075VlnACBkCZZQN2p13kiaZ7P4RueaoMBCSdSH8x2JCfDk0ha6JH0eIpB5lfLq78HQbpMw%2F640%3Fwx_fmt%3Dpng)

### 2.框架介绍

#### 项目结构

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FdCtZK7DicnAhI5jxrO075VlnACBkCZZQNuWiaTpyxR32oPXoibvX2VtqHxlFzMYyX3Hl5Dg8dcVMXekfx5HATRTpA%2F640%3Fwx_fmt%3Dpng "项目结构目录.png")

#### 前端

技术

名称

版本

官网

freemarker

模板引擎

springboot1.5.6.RELEASE集成版本

https://freemarker.apache.org/

Bootstrap

前端UI框架

3.3.7

http://www.bootcss.com/

Jquery

快速的JavaScript框架

1.11.3

https://jquery.com/

kindeditor

HTML可视化编辑器

4.1.10

http://kindeditor.net

My97 DatePicker

时间选择器

4.8 Beta4

http://www.my97.net/

#### 后端

技术

名称

版本

官网

SpringBoot

SpringBoot框架

1.5.6.RELEASE

https://spring.io/projects/spring-boot

JPA

spring-data-jpa

1.5.6.RELEASE

https://projects.spring.io/spring-data-jpa

Mybatis

Mybatis框架

1.3.0

http://www.mybatis.org/mybatis-3

fastjson

json解析包

1.2.36

https://github.com/alibaba/fastjson

pagehelper

Mybatis分页插件

1.0.0

https://pagehelper.github.io

### 3.部署流程

 1.下载项目、把oasys.sql导入本地数据库-
 2. 修改application.properties，-
 3. 修改数据源，oasys——>自己本地的库名，用户名和密码修改成自己的-
 4. 修改相关路径，配置图片路径、文件路径、附件路径。(static/image/oasys.jpg 拷贝到配置的图片路径下，不然会报 FileNotFoundException )-
 5. OasysApplication.java中的main方法运行，控制台没有报错信息，数据启动时间多久即运行成功-
 6. 在浏览器中输入localhost:8088/logins

 

 **源码获取** 

 

 ![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FR5ic1icyNBNd7gCjTYibduPsGTxuPTmbuBTIzPJgA0yYfg7ibiabBNY0jLqo1vEII4icCp8rCfmcIwMqicXXibd2FSOCyQ%2F640%3Fwx_fmt%3Dpng)**源码获取**![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FR5ic1icyNBNd7gCjTYibduPsGTxuPTmbuBTIzPJgA0yYfg7ibiabBNY0jLqo1vEII4icCp8rCfmcIwMqicXXibd2FSOCyQ%2F640%3Fwx_fmt%3Dpng) -
 

扫码下方二维码，后台回复【**OA**】即可获取

 

 ![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fsz_mmbiz_jpg%2FGb5lsYBRibtdhIqEMPbPEvjibP4ZZ1oRGvHiaQBcKDuphfJagrYy7mdswIwKkQWSLuyCvafO50OEeBeh9QNShAn9w%2F640%3Fwx_fmt%3Djpeg) -
 

##  

 

 往日文章： 

 

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
 

 

 [分享一个基于 Vue3.x 的数据可视化大屏项目（附源码）](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247483951&idx=1&sn=e44b0c5ca9bf77dd8147710e97c88ef4&chksm=c02b87bef75c0ea8f2f53b8d5c39e4d2c089f2501e9be7d65ece7c547f83b61df6a86244fc4f&scene=21#wechat_redirect)-
 

 

 [GitHub校招黑名单，火爆全网！](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247483947&idx=1&sn=54805f33e71575396f142996078b13ed&chksm=c02b87baf75c0eacecff666301e9c3a968d2c911fd13245dc705d279ba63822e1c37650f5ac1&scene=21#wechat_redirect) 

 

 [SpringBoot+Dubbo+Vue企业级社区项目！（附源码）](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247483943&idx=1&sn=4f3f13be333e616fe8d283821e666442&chksm=c02b87b6f75c0ea079ecb90f1c143bb2465c30358fb911d9da3895186f1ec7023413c2e8d410&scene=21#wechat_redirect) -
 

 

 [强力推荐一个完善的物流（WMS）管理项目（附代码）](http://mp.weixin.qq.com/s?__biz=Mzg5MzcwMzQyMg==&mid=2247483858&idx=1&sn=e2d10498a2902f380189a23f448e0bec&chksm=c02b8443f75c0d55d7552b8f1231321f11ed5ccad40fdad659aa8aacacfcc7e7c7e28c5efdd7&scene=21#wechat_redirect) -
 

 

 

 

 

 

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2FEa7dkfAlbibn9gUdryMiaCrTiaK7m5rLaz9S3YylFMWfqKItGrIUrgGlSkkRia57PWkTiajnIftSibsUYSs0qyXWtibSw%2F640%3Fwx_fmt%3Djpeg)

 

 

 **明天见(｡･ω･｡)ﾉ** 

 

 

 版权申明：内容来源网络，版权归原创者所有。除非无法确认，我们都会标明作者及出处，如有侵权烦请告知，我们会立即删除并表示歉意。谢谢! 

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzIyNTY4NjU0OQ==&mid=2247511718&idx=2&sn=d9f3cfb7ea939c08975f68707870dbbd&chksm=e8790bdcdf0e82ca5b663b579d5569205aa41e3475927ba6b136350aa398859e9811f30bcc01&mpshare=1&scene=1&srcid=1203a9tpFUh5pei00PLNI8Fc&sharer_sharetime=1638486791723&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)