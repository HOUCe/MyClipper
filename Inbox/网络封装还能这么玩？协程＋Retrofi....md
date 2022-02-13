# 网络封装还能这么玩？协程＋Retrofit带你秀一波

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650841066&idx=1&sn=7e0858240fdc16192520e4ed89e32f3c&chksm=80b74b74b7c0c2623e1a854b19ff00d1f1ede4856ff6aca659a12ccb411de62a6681a95e6467&mpshare=1&scene=1&srcid=1221ZTDusgV3xcRNrDtOGyqA&sharer_sharetime=1640047019015&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)鸿洋

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_gif%2F4JJPUsMZPgWIHnE1eR3qbBdvxQzoL2uuvRcEYibD4FL4wqWtANp1PRWLCdX8QaGztxkLmNf6VUDJvXFEqzrDbJQ%2F640%3Fwx_fmt%3Dgif)

前言

**自Google推出Kotlin之后很长一段时间都不温不火，现在为什么却成了当下热门的语言之一？**相信很多朋友都有这个疑惑。Kotlin被Goodle公司选为开发Android App的一级语言，由于是全新开发和设计的语言，在各方面都有着其先进性，作为通用语言，Kotlin可以运行在Java虚拟机、浏览器、Android上的静态语言，与Java完全兼容。**总而言之，Kotlin可以做后台、前端、Android等等。**Kotlin不仅简洁、安全、工具友好，关键是利用Kotlin协程搭建网络框架才是真的香！-

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_gif%2F4JJPUsMZPgWIHnE1eR3qbBdvxQzoL2uuvRcEYibD4FL4wqWtANp1PRWLCdX8QaGztxkLmNf6VUDJvXFEqzrDbJQ%2F640%3Fwx_fmt%3Dgif)

Kotlin时代下的Retrofit

说到网络框架大家应该都不陌生，但凡尝试过协程＋Retrofit的人，都会果断抛弃之前使用的rxjava+Retrofit，因为**协程＋Retrofit会让代码更加清晰简洁，它可以做到以同步的方式写出异步的代码。**

**优点：**

*   代码更加清晰、优雅；
    
*   封装调用简单，代码复用更好；
    
*   代码不额外依赖其他三方库；-
    

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fsz_mmbiz_jpg%2FpRO2ZMJPAqJMlpNyzt7ibjMouKkas1DpGqibsPYQ80L3WZgELU9slB6h9ZUtehUKfpAKj0RXqXD5BHT3HNeNEcng%2F640%3Fwx_fmt%3Djpeg)

（图片来源网络）

使用协程+Retrofit最大的好处就是**可以避免回调，使异步逻辑更加简洁、清晰。运用同步代码处理异步网络央求，减去Retrofit中网络回调。**现在越来越多的App都转用Kotlin去开发了，**谁掌握住了协程+Retrofit谁就可以在面试中取得先机。** 但是接触过网络模块的朋友应该都遇到过：**Json数据的适配问题、API传输安全、中间人攻击、API被窃听等等一系列的问题。**一般来说Json被反序列化的时候会引发：接口请求失败，可它还是要全部反序列化的情况，最终导致接口的返回数据形式不一致。**那要怎么才能避免以上问题的发生呢？**

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_gif%2F4JJPUsMZPgWIHnE1eR3qbBdvxQzoL2uuvRcEYibD4FL4wqWtANp1PRWLCdX8QaGztxkLmNf6VUDJvXFEqzrDbJQ%2F640%3Fwx_fmt%3Dgif)

怎么让协程完美适配Retrofit？

Kotlin协程的开发模式让网络请求的过程变得更加精简，但想要更加完善的在Kotlin语言下搭建好网络模块，我们还是需要钻研它的底层原理。为了帮助大家早日进阶成为高级安卓工程师，特邀**首批Android开发者【Allen老师】**为大家带来**《适配多态数据的协程网络模块搭建及安全优化》**系列直播课程，让大家更好地拥抱**协程+Retrofit+OkHttp！**

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2F4JJPUsMZPgWIHnE1eR3qbBdvxQzoL2uuZwuSPpkLxkf308fRX41CLEPwggSqLnjCeVIcO0fuaCIPHIcvdInQrA%2F640%3Fwx_fmt%3Dpng)

12月21日 - 22日，每晚20:00-

**首批Android开发者\[Allen\]**

倾心打造

原价 ¥199，限时 **免费** 立刻学习！

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fsz_mmbiz_jpg%2FpRO2ZMJPAqLzXkviacn92PEpniaYIic1crWPA8ePaDOiaXRO4DQZydyXjH0BALs41WGocHicS4TcWa6OibQm8EByk1Zw%2F640%3Fwx_fmt%3Djpeg)

长按扫码添加客服，锁定 **免费** 名额

**【直播+录播】【笔记课件】+【源码】**

仅前 300 人有效，先到先得！

**大厂名师手把手教学**

![](https://image.cubox.pro/article/2021102717120158424/68180.jpg)

听课后，还能获取互联网环境中，Andorid核心技术路线图，里面的内容和方向，让你学习起来更明确，更体系。

![](https://image.cubox.pro/article/2021102717120131813/66674.jpg)

还为大家准备了超级干货内部教材～-

免费报名直播就可以获得**《Android架构开发手册**》：报名就送！报名就送！报名就送！

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2F71ibgGpZLr2VSEd77Ofic71ibR3ujgGJULOQS2GibVZaxBycvfNPYJRcEXjop6U12h59AdkApOa5iakpB0zMbc5iaeDA%2F640%3Fwx_fmt%3Dpng)

**独创实战特训营服务**

![](https://image.cubox.pro/article/2021102717120171829/66326.jpg)

## **课程福利**

**1.免费学习协程+Retrofit系列课程**

**2.** **提供学习直播+预习资料+源码+老师课后答疑**

**3.** **赠送课程学习资料，****课程录播课程（可永久观看）**

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fsz_mmbiz_jpg%2FpRO2ZMJPAqLzXkviacn92PEpniaYIic1crWPA8ePaDOiaXRO4DQZydyXjH0BALs41WGocHicS4TcWa6OibQm8EByk1Zw%2F640%3Fwx_fmt%3Djpeg)

**最后直播中加赠《2022中高级Android面试必知百题》**

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2F4JJPUsMZPgWIHnE1eR3qbBdvxQzoL2uuGiaypaJovzx42quSJJow2icLsRZLAgEicYjViaf2uIpMribibPs1Wypibhdibg%2F640%3Fwx_fmt%3Djpeg)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fsz_mmbiz_jpg%2FpRO2ZMJPAqLzXkviacn92PEpniaYIic1crWPA8ePaDOiaXRO4DQZydyXjH0BALs41WGocHicS4TcWa6OibQm8EByk1Zw%2F640%3Fwx_fmt%3Djpeg)

扫码即可领取

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650841066&idx=1&sn=7e0858240fdc16192520e4ed89e32f3c&chksm=80b74b74b7c0c2623e1a854b19ff00d1f1ede4856ff6aca659a12ccb411de62a6681a95e6467&mpshare=1&scene=1&srcid=1221ZTDusgV3xcRNrDtOGyqA&sharer_sharetime=1640047019015&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)