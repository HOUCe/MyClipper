---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/5032186794
author: 
---

# [重学安卓：是难得一见的 Jetpack MVVM 最佳实践 － 小专栏](https://xiaozhuanlan.com/topic/5032186794)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

·

> 注：本文为[《GitHub：Jetpack-MVVM-Best-Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)项目简介的冗余备份，**如对项目结构和设计依据感兴趣，可前往**[《如何让同事爱上架构模式、少写 bug 多注释？》](https://xiaozhuanlan.com/topic/8204519736) 篇查阅。

## 前言

很高兴见到你！

上周我在 各大技术社区 发表了一篇 [《Jetpack MVVM 精讲》](https://juejin.im/post/5dafc49b6fb9a04e17209922)，原以为在 知识网红 唱衰安卓 的 2019 会无人问津，没想到文章一经发布，从 国内知名公司 的架构师、技术经理，到 世界级公司 的 Android 开发 都在看。

![](https://images.xiaozhuanlan.com/photo/2019/45d2770d3d944536da2d18b1fb518f33.png)

并且从读者的反馈来看，近期大部分安卓开发 已跳出舒适圈，开始尝试认识和应用 Jetpack MVVM 到实际的项目开发中。

只可惜，关于 Jetpack MVVM，网上多是 **东拼西凑、人云亦云、通篇贴代码** 的文章，这不仅不能 提供完整的视角 来帮助读者 首先明确背景状况，更是给还没入门 Jetpack 的读者 **徒添困扰**、起到 **劝退** 的作用。

好消息是，这一期，我们带着 **精心打磨的 Jetpack MVVM 最佳实践案例** 来了！

是 爱不释手 的 交互设计！

是 连贯 的 用户体验

唯一可信源 的 统一分发

![](https://images.xiaozhuanlan.com/photo/2019/07db39293c2e147dec82ee8d6fae3cd9.gif)

![](https://images.xiaozhuanlan.com/photo/2019/695c8b146d8df1d92ca28c207a9afc92.gif)

![](https://images.xiaozhuanlan.com/photo/2019/cf8274cb454187e80264739b9849a34b.gif)

## 文章目录一览

-   前言
-   横竖屏布局 的 无缝切换
-   项目简介
-   The One More Thing is
-   Who is using
-   **Note 2020.09.10 加餐：**
    -   Jetpack MVVM 或借鉴了 MVI 开发模式

## 横竖屏布局 的 无缝切换

![](https://images.xiaozhuanlan.com/photo/2019/284d73006a325d7cb2e0c763d35a0e40.gif)

## 项目简介

本人拥有 3 年的 移动端架构 践行和设计经验，领导团队重构的 中大型项目 多达十数个，对 Jetpack MVVM 架构在 确立规范化、标准化 开发模式 以 **减少不可预期的错误** 所作的努力，有着深入的理解。

在这个案例中，我将为你展示，Jetpack MVVM 是如何 **以简驭繁** 地 将原本十分容易出错、一出错就会耽搁半天时间的开发工作，通过 寥寥的几行代码 轻而易举地完成。

> 👆👆👆 划重点

在这个项目中，

> 我们为 **横、竖屏** 的情况 分别安排了两套 **截然不同的布局**，并且在 [生命周期](https://xiaozhuanlan.com/topic/0213584967)、[重建机制](https://xiaozhuanlan.com/topic/7692814530)、[状态管理](https://xiaozhuanlan.com/topic/7692814530)、[DataBinding](https://xiaozhuanlan.com/topic/9816742350)、[ViewModel](https://xiaozhuanlan.com/topic/6257931840)、[LiveData](https://xiaozhuanlan.com/topic/0168753249) 、[Navigation](https://xiaozhuanlan.com/topic/5860149732) 等知识点的帮助下，通过寥寥几行代码，轻松做到 **在横竖屏两种布局间 无缝地切换，并且不产生任何 预期外的错误**。  
> ·  
> 我们在多个 Fragment 页面 分别安排了 **播放状态 指示器**（包括 播放暂停按钮状态、播放列表当前索引指示 等），并向你展示了 如何 以及为何 通过 [LiveData](https://xiaozhuanlan.com/topic/0168753249) **配合** 作为唯一可信源 的 [ViewModel](https://xiaozhuanlan.com/topic/6257931840) 或单例，来实现 **全应用范围内 可追溯事件 的统一分发**。  
> ·  
> 我们在 Fragment 和 Activity 之间分别安排了 跨页面通信，从而向你展示 如何基于 **迪米特原则**（也称 最少知道原则）、通过 UnPeekLiveData 和 应用级 SharedViewModel 来实现 **生命周期安全的、事件源可追溯的 页面通信**（事件回调）。  
> ·  
> 我们在 `ui.page` 、`data.repository`、`domain.request` 等目录下，分别安排了 视图控制器、[ViewModel](https://xiaozhuanlan.com/topic/6257931840) 、DataRepository 等 内容，从而向你展示，**单向依赖** 的架构设计，是如何通过分层的 数据请求和响应，来 **规避 内存泄漏** 等问题。  
> ·  
> 本项目的代码一律采用 经过 ISO 认证的 标准化工业级语言 Java 来编写。并且，在上述目录 所包含的 类中，我们大都 **提供了丰富的注释**，来帮助你理解 骨架代码 为何要如此设计、如此设计能够 **在软件工程的背景下** 避免哪些不可预期的错误。

除了 **在 蕴繁于简 的代码中 掌握 MVVM 最佳实践**，你还可以 从这个开源项目中 获得的内容 包括：

1.  整洁的代码风格 和 标准的资源命名规范。
2.  对 视图控制器 知识点的 深入理解 和 正确使用。
3.  AndroidX 和 Material Design 2 的全面使用。
4.  ConstraintLayout 约束布局的最佳实践。
5.  **优秀的 用户体验 和 交互设计**。
6.  绝不使用 Dagger，绝不使用奇技淫巧、编写艰深晦涩的代码。
7.  One More Thing：

## The One More Thing is：

详见 GitHub 仓库 [Jetpack-MVVM-Best-Practice](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)

感谢小伙伴们对 “开源库使用情况” 匿名调查问卷的参与，截至 2021年4月25日，我们了解到

包括 “腾讯音乐、BMW、TCL” 在内的诸多知名厂商的软件，都参考过我们开源的此架构模式，以及正在使用我们维护的 [UnPeek-LiveData](https://github.com/KunMinX/UnPeek-LiveData) 等框架。

目前我们已将具体的统计数据更新到 相关的开源库 ReadMe 中，错过本次问卷调查的小伙伴也不用担心，我们继续对此保持开放，不定期将小伙伴们登记的公司和产品更新到表格，

以便吸引到更多的小伙伴 参与到对这些架构组件的 使用、反馈，集众人之所长，让架构组件得以不断演化和升级。

[https://wj.qq.com/s2/8362688/124a/](https://wj.qq.com/s2/8362688/124a/)

集团 / 公司 / 品牌 / 团队

产品

腾讯音乐

QQ 音乐

TCL

内置应用，暂时保密

贵州广电网络

乐播播

福建树叶网络科技有限公司  
福建天奖网络科技有限公司

天奖谱林

小辣椒

BMW

Speech

上海互教信息有限公司

知心慧学教师

美术宝

弹唱宝

网安

## Note 2020.09.10 加餐：

### Jetpack MVVM 或借鉴了 MVI 开发模式

刚刚有小伙伴私下分享了他对 MVI 开发模式 的理解，这里顺带对 Jetpack MVVM 的结构做个简要的补充。

[国外网友绘制的 MVVM + MVI 开发模式架构图](https://images.xiaozhuanlan.com/photo/2020/f283f9bbd68dddaba0014f4f533f0c8b.png)

正如我们在[《是让人提神醒脑的 MVP、MVVM 关系精讲》](https://xiaozhuanlan.com/topic/6105792348) 中谈到的，**MVVM 开发模式的本质是 “解决视图调用的一致性问题”**，也即 **透过 DataBinding 等技术手段做到 100% 规避 “视图实例的 null 安全问题”**，因而 MVVM 开发模式从技术实现的角度来说 可以简单归结为 DataBinding + ViewModel。

与此同时，Jetpack MVVM 还包含了 LiveData 这个用于 “在表现层接收请求到的数据 并将数据分发给订阅它的页面”，也即 LiveData **透过 MVI 响应式编程的思想实现了 页面对 ViewModel 的 “单向依赖”**，以期规避不可预期的 **内存泄漏** 等问题。

然 LiveData 的本质主要集中在 **“解决数据分发的一致性问题”** 和 **“解决生命周期安全的一致性问题”**，也即 **透过 “唯一可信源” 的理念来规避不可预期的推送**，并且能自动解注册、始终保证 仅在视图控制器处于激活等状态下 透过回调接触视图控制器的成员。因而严格来说，LiveData 对 MVI 开发模式只是有一定的借鉴，是作为对 Jetpack MVVM 架构组件在 MVVM 开发模式下 对表现层数据分发的补充。

> **快捷访问：**
> 
> **[基本功：是随时随地可受用的 深度思考原则](https://xiaozhuanlan.com/topic/9837051426)**
> 
> [GitHub : 重学安卓 配套项目](https://github.com/KunMinX/Relearn-Android)
> 
> [GitHub : Jetpack-MVVM-Best-Practice](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)
> 
> **Tip：**  
> · 考虑到移动端的阅读体验，已将代码改为图片展示。网页/小程序点击图片可放大查阅。  
> · Jetpack MVVM 系列文章会不定期增量更新，更新的内容会提供 Note 标记，可通过 ctrl + F（Windows）或 command + F（MacOS）搜索 “Note” 快速检索和查阅。

## 版权声明

> Copyright © 2019-present KunMinX

**本文为专栏私有内容，谢绝转载**。

读者订阅了专栏，即享有了专栏文章的阅读权。

文章 **原创的引言、切入点、思路、结论** 凝聚了作者 KunMinX 本人的心血 及与参与者互动演化的结果，作者本人及作者在文中提名的有效贡献者 对相应的成果享有所有权。

**当您 借鉴或引用文中的引言、切入点、思路、结论 进行二次创作并打算发行时，须注明链接出处**，否则我们保留追责的权利。对此如有疑虑请及时沟通，或在发行前将作品交由本人核对。

当您看见有人 [洗稿](https://baike.baidu.com/item/%E6%B4%97%E7%A8%BF/20862574?fr=aladdin)、[剽窃](https://baike.baidu.com/item/%E5%89%BD%E7%AA%83/9398357?fr=aladdin)、未经授权转载本专栏的文章内容时，请及时向本人举报。

最后感谢您对专栏内容的阅读和喜欢。