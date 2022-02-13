# 安卓开发用 Kotlin，有多香？

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzIxMTg5NjQyMA==&mid=2247505238&idx=1&sn=f833cc8fc17f756bd32b59aa1a755e77&chksm=974cc45da03b4d4b8a161877558f6b5ccdb7ef667cc100400c748d18ab5c002bcdc235f637e5&mpshare=1&scene=1&srcid=1230zeTUrKWcRfTHjKnZowaB&sharer_sharetime=1640838405501&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)技术最TOP

自 2017 年 Kotlin 被 Google 认证为 Android 开发官方编程语言后，最常提及的一个问题：**是否应该学习 Kotlin 进行 Android 开发？相比传统 Java 语言有什么优势？**-

如今答案十分清晰了 —— 这几年，Google 大力发展基于 Kotlin 的 Androidx 库、Jetpack 库、Compose 库，很多新特性都是为 Kotlin 优化的。可以说，不懂 kotlin，今后在 Android 开发领域标准库的发展上将很受阻碍，**Android 开发由 Java 转 Kotlin 早已势不可挡。**

相比起 Java 语言，Kotlin 的优势确实非常明显：

**第一，极高的生产效率。**Kotlin 是一种跨平台的静态类型语言，具有现代简洁的语法，**关键特性**包括 null 安全性、协程、数据类型、扩展函数等；这让开发者会用得很爽：前期开发效率更高，中期线上问题更少，后期代码更容易维护。而这正是 Java 做不到的。

**第二，强大的兼容性****。**Kotlin 可以与 Java 混合编程（说实话，这点影响很大），我们能够以渐进的方式将项目工程从 Java 迁移到 Kotlin，而不必担心是不是要一次性重写很多代码，从而产生新的问题。

**第三，用**** Kotlin 编写代码比 Java 更友好、更快捷**。Kotlin 吸收了众多编程语言的精髓，它的语法不像 Java 那么复杂，而且允许开发者在**不使用冗余类的情况下定义函数和静态对象**，这会让代码更容易阅读和调试。

为此，各个大厂的 Android 部门都在积极转型，目前市面上主流的 App 和库，大都是使用 Kotlin 语言开发的，在 Play Store 的前 1000 个应用程序中有 80% 以上使用 Kotlin。

随便打开一个招聘网站，看看大厂的 Android 招聘需求，基本都有“要求熟悉 Kotlin”或“**熟悉 Kotlin 语言者优先**”，而且**薪资总体上也略高一筹**（相比之下，Java 开发的用人成本在 Kotlin 的招聘方那里是可以接受的，毕竟市面上实在有太多 Java 程序员，可以根据项目需求在招聘中讨价还价）。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FXcGWeI8hE6Y4ibmR4oLibic07BoZmmGa3a5HkWev0H1rl3MzCyCftU1Qkv8lIIGpkPYAIVccqn9IDy41HbFw8CdIw%2F640%3Fwx_fmt%3Dpng)

当然，有的公司目前还是把 Kotlin 当做加分项。但不得不说，同等条件下，**会 Kotlin 的候选人胜率更大**。

**高效掌握 Kotlin 的方法**

Kotlin 是门典型的**易学难精**的语言：语法简洁，极容易入门，但又拥有许多的新特性，不容易掌；即使掌握了 Kotlin 的语法，想要写出优雅的代码，也不容易，更别提 Kotlin 特性的应用场景、底层实现原理了。

大部分的学习路径可能是这样的👇

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FXcGWeI8hE6Y4ibmR4oLibic07BoZmmGa3a5eujwIlSRCFeeISibA89ouGdwKW4bqhj2p31ZIMicQtP8FrpQFDLqNZDw%2F640%3Fwx_fmt%3Dpng)

当然，有 Java 基础可能会更容易些，但它本身是助力，也是阻力，毕竟两种语言在**不变性思维、空安全思维、表达式思维、函数思维、协程思维**等撰写代码的思维方式上，都不一样。

尤其是 **Kotlin 协程**，全是一堆新概念：协程、作用域、上下文、launch、async、Channel、Flow、异常处理...让人毫无头绪。

我当初啃协程时，也是一边研究协程源码、一边在工作中实践，踩着坑磕磕绊绊的学，找到靠谱的资料非常不容易（市面上太多花把势，能实打实讲透、提升学习者能力的少之又少）。

看过不少资料，从体系化层面，我推荐圈里的大佬**朱涛**，他最近出了个专栏**《朱涛 · Kotlin编程第一课》**。迫不及待地分享其中一张学习图谱👇

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2FXcGWeI8hE6Y4ibmR4oLibic07BoZmmGa3a5gKaFpyjadGmfqyaQjgpibXiccHJZaeOVPJLiaqpju9kcu6CKmj4icHmMlA%2F640%3Fwx_fmt%3Djpeg)

朱涛有多牛，一会下面详细介绍，但这个专栏，应该是你离顶尖技术人的思维过程最近的一次了，刚上线，看了更新的几篇，非常惊艳，不说教、不枯燥，配合动图展示，零基础也能拿下。一句话概括就是：**基础 + 实战 + 源码，手把手带你吃透 Kotlin 语法与协程。**

整个专栏对比 Kotlin 和 Java 语法的差异，结合案例详解 Kotlin 新特性的使用场景。顺便带你一起来用 Kotlin 写一个简单的 Android App。据说后期还有不定期的加餐，分享 Kotlin 在各个领域的最新实践，进一步扩展你的 Kotlin 知识面。

现在**仅需 ¥89**，立省 ¥40，购买后永久有效，推荐给你。若你是**新用户**，交个朋友，来**¥59** 拿去。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2FQFjUqsncFKk17Y7Xn3icdBrliaGIBl4NIA3pdoibIBrokkhBQYsVQTASezwFrI5T6b6O6kyKVTOIGCub5dZZsuKlA%2F640%3Fwx_fmt%3Djpeg)

👆扫码免费试读-

早鸟 + 口令「**kotlin666**」

到手仅 **¥89**，原价 ￥129

## 

**网上一抓一大把，为什么推荐这门课？**

建议你再看一眼作者，那可是朱涛啊。

**朱涛**是国内第一批探索 Kotlin 的 Android 开发者，博客《Kotlin Jetpack 实战》的作者，Google 认证的“谷歌开发者专家” (Android & Kotlin GDE)。此认证专家现**在全球有 27 位，但在中国只有 2 位**。

像朱涛这样的大佬能把自己多年经验毫无保留分享出来，让普通人可以接触并学习到，真的是多少钱都买不来的。

## 

**好学吗？会不会是“教做人”和“催眠课”？**

好学。

我很佩服朱涛的一点，就是能把“枯燥的内容讲得生动有趣”，用**动图**的形式，力求简单易懂，比如，为了让你理解 Kotlin 的扩展函数的使用场景，老朱精心制作了普通函数与扩展函数的转换动画：

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_gif%2FXcGWeI8hE6Y4ibmR4oLibic07BoZmmGa3a5UpZ0zicl6ibOPviaITy6txKOnfcYpfPmgNNSDxHxZmgZy5JAhok9uqQ1A%2F640%3Fwx_fmt%3Dgif)

### 

**一套独创的协程思维模型**

**协程一直都是 Kotlin 学习的难点，老朱独创的模型**展示了协程、线程与进程之间的关系，帮你在大脑里建立一个清晰、具体的协程模型。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_gif%2FXcGWeI8hE6Y4ibmR4oLibic07BoZmmGa3a5nicfa84gAz1LTdR0atiaPrd188W9pnTSRvemJVHF90zMAALQmNoa10TQ%2F640%3Fwx_fmt%3Dgif)

### 

**深入剖析协程挂起函数**

为了让你看到协程代码背后**挂起与恢复**的细节，精心制作了这个示意图。

视频加载失败，请刷新页面重试

 ![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACIAAAAqCAMAAADhynmdAAAAQlBMVEUAAACcnJycnJycnJyoqKicnJycnJycnJycnJycnJyfn5+cnJydnZ2enp6kpKSdnZ2cnJyenp6cnJycnJycnJybm5t8KrXMAAAAFXRSTlMAyeb3CNp3tJRvHIEtJhBgqztWRJ+p5TqGAAABCklEQVQ4y5WTi27DIAxFAUMhgTzX8/+/urB2pdKI0x0pSoRuruyLbf7gF3PBaDE6X44LyY0D1SJQsfd9PpMM/CJx60v8SmV1HMSi1lKyA1n0jnwWSO08l04uJbxpBmTrpDtbGB6fmxC6Tc4BHv9aZDJdJsHW9w43Jez9x8T5M4l31WZsJn2bsYY+nUum2lQkGIVANPZ4FCLWOJImSTgjZE2SkU9crmu57mj9JBc93Qzj9R1d3HSG5bN5MRsnUzcGKK8Ns02z+Da7rYQE4bUE2PG1C6kVnkCyf0pwX8/jwbyxCLhcHpKTFkvkwK3pRmXtRrVFoTGYLvN+t0EUl0qrRaF1pFBz0anp/ptvNB4SY1XDAVMAAAAASUVORK5CYII=) 刷新 

**个人认为学习一门新技术最快的方式：就是干**，直接撸项目比看多少篇文章都靠谱些。而这门课里设计了**大量实战项目**，且融合朱涛**独创的协程思维模型**，让你能直观地体验Kotlin的魅力，并**快速上手**。

我顺手把目录也贴在这了👇

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2FXcGWeI8hE6Y4ibmR4oLibic07BoZmmGa3a5FqIiaPMSp5kBfU0icuN6WsBEyrDdXWvbv670Zun6aLXaB9SghVvWUEvA%2F640%3Fwx_fmt%3Djpeg)

想入手**《朱涛 · Kotlin编程第一课》**注意了，再强调一遍优惠，手慢无

扫码免费试读

早鸟 + 口令「**kotlin666**」

到手仅 **¥89**，立省 ￥40

新同学**到手 ¥59**

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2FQFjUqsncFKk17Y7Xn3icdBrliaGIBl4NIAZX3TWWOvwWHUKn0jcrzjJUK4qwns9pawb6HIIko6570lEYzoL3mo6Q%2F640%3Fwx_fmt%3Djpeg)

-

当然，推荐 Kotlin 并不代表 Java 不好，编程语言之于开发者，就好比兵器之于武将。我们只是结合自己的实际需求，选择最适合自己的兵器，尽可能做到事半功倍。

协程等“思想金条”被朱涛时刻埋在专栏里的字里行间，你需要反复读、琢磨，才能获得经验值的增长。

点击**「阅读原文」****👇，****这次吃透 Kotlin 语言**。

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzIxMTg5NjQyMA==&mid=2247505238&idx=1&sn=f833cc8fc17f756bd32b59aa1a755e77&chksm=974cc45da03b4d4b8a161877558f6b5ccdb7ef667cc100400c748d18ab5c002bcdc235f637e5&mpshare=1&scene=1&srcid=1230zeTUrKWcRfTHjKnZowaB&sharer_sharetime=1640838405501&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)