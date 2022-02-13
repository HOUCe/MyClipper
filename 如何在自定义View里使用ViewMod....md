# 如何在自定义View里使用ViewModel

[www.zhihu.com](https://www.zhihu.com/column/c_1324407681373732864?utm_source=wechat_session&utm_medium=social&utm_oi=38400975437824)一叶知秋

前言 ViewModel只能在Activty和Fragment里使用吗，能不能在View里使用呢？ 假如我要提供一个 View ，它包含一堆数据和状态，比如一个新闻列表、时刻表等。我是否可以再这个这个自定义 View 里使用 ViewModel 去管理数据呢？ 在View中使用ViewModel 答案是肯定的！那么我们说干就干，看看到底怎…

## [月薪10K码农，跳槽到40K架构师，Android技术学习路线图汇总](https://zhuanlan.zhihu.com/p/450518513)

一、介绍 身为一个Android开发，如果你不清楚自己要从哪开始，下个阶段要学什么，到哪里算是结束，可以参考下面资深架构师整理的 Android 开发者 2021 版最新的学习路线图。 这里包括你从接触互联网的基础内容开始，了解一部分如Java基础，Android基础的语言，之后学习其他底层，NDK、跨平台相关知识。当一…

## [还在用kapt吗？ 试试ksp吧 | 项目复盘](https://zhuanlan.zhihu.com/p/450113775)

作者：究极逮虾户 链接：[https://juejin.cn/post/6939472660573192206](https://link.zhihu.com/?target=https%3A//juejin.cn/post/6939472660573192206) 大家退后，今天我要开始表演一下装逼的艺术。这次我们尝试性的使用谷歌前一阵子公布的[ksp(Kotlin Symbol Processing)](https://link.zhihu.com/?target=https%3A//link.juejin.cn/%3Ftarget%3D)，一款专门拿来给 Kotlin 项目提升注解生成速度的。 在 ksp 出来以前，对于这种注解解释器，我们使用的都是java…

## [看完字节跳动实时音视频通讯学习笔记，“60W”年薪信手拈来](https://zhuanlan.zhihu.com/p/449584822)

什么是实时音视频 实时音视频（RTC）即基于IP技术实现的实时交互的音视频通信技术。 RTC 与 直播常用协议的区别 而这里我们要使用的RTC技术就厉害了~ 它是基于IP技术的，它的延迟低于400ms，RTC传输的内容是音视频数据。 实时音视频应用场景 音视频通话 产品功能 1V1，多人音视频通话 可以美颜、使用道具等等。…

本篇文章的所有知识点是亲身经历十余家一二线互联网企业面试后总结产出，包含应聘Android开发岗位的各个方面的高频知识点，主要针对但不局限于Android应届面试。以下所有知识点都整理发布在Github/Gitbook，方便大家整理学习，文末附有链接。 Java **Java基础** Java集合框架 Java集合——Array…

## [Flutter 的 2021 年终总结](https://zhuanlan.zhihu.com/p/449081175)

作者：Marno 链接：[https://juejin.cn/post/704479485310376348](http://link.zhihu.com/?target=https%3A//juejin.cn/post/704479485310376348) 一、新年寄语 又到年底了，不知道你们有没有觉得，自从过了某个年龄以后，时间好像就开始过的越来越快了。 不知不觉，新冠疫情发生已经有 2 年多了，从疫情最开始的人心惶惶，再到我们国人万众一心抗疫，这场苦难好像无形中也增加了民族的凝聚力。 如果可以许下一个愿望…

## [RecyclerView的一些优化点](https://zhuanlan.zhihu.com/p/448712709)

1.RecycledPool的重用 **场景以及使用：** 多个RecyclerView出现，并且他们的item布局结构一致，这时候可以进行重用。 在进行RecyclerView的初始化设置时候进行RecycledPool的设置。 //每个单元的视频列表的RecycledPool private var mRecycledV…

## [终于整完了，2021年大厂Android岗中高级面经分享](https://zhuanlan.zhihu.com/p/448697650)

一眨眼又到年底了，每到这个时候，我们都会慢慢反思，这一年都做了什么？有什么进步？年初的计划都实现了吗？明年年初有跳槽的底气了吗？ 况且近年我们经历了新冠疫情的洗礼，很多程序员都经历了失业，找工作的恐慌。导致今年的互联网环境太差，需要自己有足够的知识储备，才能够应对这凌冽的寒风。 本文主要是整理了中高级Android需要会的…

## [相比 XML , Compose 性能到底怎么样？](https://zhuanlan.zhihu.com/p/448259492)

作者：RicardoMJiang 链接：[https://juejin.cn/post/7008522702835154980](https://link.zhihu.com/?target=https%3A//juejin.cn/post/7008522702835154980) 前言 最近 Compose 已经正式发布了 1.0 版本，这说明谷歌认为 Compose 已经可以用于正式生产环境了 那么相比传统的 XML , Compose 的性能到底怎么样呢？ 本文主要从构建性能与运行时两个方面来分析 Compose 的性能，数据主要来源于…

## [ViewModel中传入Context的方法](https://zhuanlan.zhihu.com/p/447811392)

原文链接：[ViewModel中传入Context的方法 - 掘金](https://link.zhihu.com/?target=https%3A//juejin.cn/post/6909736545729642504) ViewModel 使用的越来越多了，严格来说，官方并不建议你在 ViewModel 中添加 Context 的引用。同时， ViewModel 的构造方法是没有任何参数的，有的时候会很不灵活。以下记录两种方法。 1.通过kotlin的拓展函数 fun <T : Vi…

[查看原网页: www.zhihu.com](https://www.zhihu.com/column/c_1324407681373732864?utm_source=wechat_session&utm_medium=social&utm_oi=38400975437824)