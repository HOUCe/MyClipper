---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/6719328450
author: 
---

# [重学安卓：LiveData 数据倒灌 背景缘由全貌 独家解析 － 小专栏](https://xiaozhuanlan.com/topic/6719328450)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

·

> 注：本文以
> 
> [《重学安卓：为你还原一个真实的 Jetpack Lifecycle》](https://xiaozhuanlan.com/topic/3684721950)
> 
> [《重学安卓：LiveData 鲜为人知的 身世背景 和 独特使命》](https://xiaozhuanlan.com/topic/0168753249)
> 
> [《重学安卓：有了 Jetpack ViewModel 真的可以为所欲为》](https://xiaozhuanlan.com/topic/6257931840)
> 
> [《重学安卓：是让人耳目一新的 Jetpack MVVM 精讲》](https://xiaozhuanlan.com/topic/0129483567) 以及
> 
> [《GitHub：Jetpack MVVM Best Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice) 作为前置知识，假设小伙伴已完整查阅并吸收了上述内容。

## 前言

上周有位在字节工作的小伙伴私下感叹：“文章相见恨晚，恨晚呐”。

据该小伙伴描述，他早在 2017 年就开始在项目中尝试 AAC（Android Architecture Components），原以为好日子即将开始，**没想到被 LiveData 的 “数据倒灌” 问题折腾得怀疑人生**。

由于了解 Jetpack 乃至爬坑 LiveData 的开发者并不多，网上关于这方面的资料略少，且多数开发者自学一门新技术 都是首选官方文档，

然而这一次问题的根源就在于，[官方文档](https://developer.android.google.cn/topic/libraries/architecture/viewmodel#sharing) 亲自推荐通过 SharedViewModel + LiveData 的组合来解决页面通信的问题，却在相当一段长的时间里 都没有亲自验证过并解决 **该方案在场景下存在的致命缺陷**。

> —— 架构组件的存在，本是为了规避不可预期的错误，没想到却因为这个 徒添了不可预期的错误。

## 姗姗来迟的笼统描述

与此类现象相关的解决方案，最早是在 2018 年中旬才在网上陆续刊登。

比如国外的：[LiveData with SnackBar, Navigation and other events](https://medium.com/androiddevelopers/livedata-with-snackbar-navigation-and-other-events-the-singleliveevent-case-ac2622673150)

以及国内的：[Android消息总线的演进之路：用LiveDataBus替代RxBus、EventBus](https://tech.meituan.com/2018/07/26/android-livedatabus.html)

但无论哪一种，都没有就此现象的背景做过 “因地制宜” 的概括，而只是笼统地用 “粘性事件” 来表达，也因此多数新上手 Jetpack 架构组件的小伙伴 **根本无从理解这个 “粘性事件” 到底意味着什么，更不曾料想到 当下正困扰着自己的，就是这个**。

因此，本文的目标即是 提供对 “数据倒灌” 现象的 **独家概括、缘由分析，以及趋近完美的解决方案**。

看完后，如你对 “数据倒灌” 这个概念有了印象、未来在遇到类似状况时能想起、并重新翻出和回溯这篇文章，那我的愿望也就达到了。

## 文章目录一览

-   前言
-   姗姗来迟的笼统描述
-   “数据倒灌” 的由来
-   为什么会有数据倒灌
    -   **Note：2020.07.17 加餐：**
    -   图文解析数据倒灌发生的场景
-   **为何非要通过 ViewModel + LiveData 来处理页面通信？**
-   为什么通过全局 ViewModel 而不是 静态单例？
-   为什么不用 LiveDataBus？
-   最新的 Result API 如何？
-   **Event 事件包装器、反射方式、SingleLiveEvent 分别存在什么问题？**
-   **UnPeekLiveData 是如何解决这些问题的？**
-   GitHub & Maven 依赖
-   综上
-   **Note 2020.07.21 加餐：**
    -   UnPeekLiveData 增加一层 ProtectedUnPeekLiveData 的设计缘由解析
-   **Note 2020.10.15 加餐：**
    -   升级到 UnPeekLiveData v4.0 享用 “更快更稳” 的防倒灌机制
-   **Note 2021.04.22 加餐：**
    -   升级到 UnPeekLiveData v5.0 获得更好的内存性能和更友好的 API 访问
-   **Note 2021.06.18 加餐：**
    -   是代码复杂度更小的 UnPeekLiveData v6.0 设计
-   **Note 2021.07.19 加餐：**
    -   是代码复杂度进一步减小的 UnPeekLiveData v6.1
-   **Note 2021.08.10 加餐：**
    -   分享一份 v6.1 源码简化版，方便对核心 “防倒灌” 机制的理解
-   **Note 2021.08.12 加餐：**
    -   是内存管理效率进一步优化的 UnPeekLiveData v7.0
-   **Note 2021.08.21 加餐：**
    -   分享一份 v7.2 源码完整版，是简练完整的防倒灌设计

## “数据倒灌” 的由来

今天提到的 “数据倒灌” 一词，缘于我为了方便理解和记忆 **“页面在 ‘二进宫’ 时收到旧数据推送” 的情况**，而在 2019 年 **自创并在网上传播的 对此类现象的概括**。

> 遗憾的是，[官方文档](https://developer.android.google.cn/topic/libraries/architecture/viewmodel#sharing) 至今 (截至 2020.7.15) 都没有意识到 并就该致命问题做出警告。

## 为什么会有数据倒灌

前言中已经提到，新上手的小伙伴们依赖官方文档，而官方文档给出的 “页面间通信” 场景的解决方案 存在致命缺陷 ——

一方面，用于通信的 LiveData 是被托管在 Activity / Application 级作用域的 SharedViewModel 中，于是 **LiveData 的生存期长于任何一个 Fragment**（假设通信双方是 Fragment）**：当二级 Fragment 出栈时，LiveData 实例仍存在**。

另一方面，LiveData 本身是被设计为粘性事件的，也即，一旦 LiveData 中持有数据，那么在观察者订阅该 LiveData 时，会被推送最后一次数据。

> 这样的设定本身也符合 LiveData 的常用场景（State） —— 比如 **在页面重建时，自动推送最后一次数据，而不必重新去向后台请求**。

只不过，这个特性在 “页面间通信” 场景（Event）下，就是致命的灾难 ——

新上手的小伙伴完全意料不到，还会有这样 “预期外” 的状况在等着自己，于是成就了读者群 “高频提问和解答” TOP 2。

> 不知道我这样说，有没说明白？——

我举一个存在 “数据倒灌” 隐患的例子：

> **Note 2021.8.16：重要提示，有言在先：**
> 
> 如想更好地理解和加深印象，可自行基于原生的 LiveData 来编写 demo 和复原灾难现场。
> 
> （最简单的办法是 **修改 [UnPeekLiveData](https://github.com/KunMinX/UnPeek-LiveData) 开源项目**，在 sample module 下的 SharedViewModel 中，将 ProtectedUnPeekLiveData 改为 LiveData、将 UnPeekLiveData 改为 MutableLiveData，然后照着下面的例子跑一遍）
> 
> 教程可以明确告诉你哪里有坑，但 **凡事唯有从 0 到 1 完整踩过一遍坑，才会真的有体会**。
> 
> 👆 👆 👆 划重点

假设 此处我们有 **列表、详情、编辑** 三个页面，返回栈中已经到达了三级页面 “编辑页”，编辑页的作用是编辑内容 并保存和返回到二级页面。

为了让 **编辑的结果在详情页中体现**，所以在保存的同时，我们会从编辑页通过 SharedViewModel 中的 MutableLiveData 去给详情页发消息，通知其刷新界面。

![](https://images.xiaozhuanlan.com/photo/2021/38ebe02731124b74ff80834bc8778e3d.png)

此时用户回到详情页，觉得内容 ok，便进一步回退到一级页面，即列表页，

![](https://images.xiaozhuanlan.com/photo/2021/002e41b2f5c7bd62f957418ab249be01.png)

那么接下来，用户在列表页中选择 **另一个 item** 点击，并再次跳到详情页时，就会因为**观察了 “已实例化过、并携带有旧数据” 的 MutableLiveData，而收到它的 “不符合预期” 的推送**。

![](https://images.xiaozhuanlan.com/photo/2021/74f3c71a3e0b5438d92b50ba28cd4274.png)

这种 “粘性设计” 的存在意义 刚刚上文已经提到了，很显然，它在当下这种场景下会带来致命的灾难。

## 为何非要通过 ViewModel + LiveData 来处理页面通信？

关于 页面通信的场景，我们能拼凑出的背景主要有以下几块：

-   事件的观察者是页面，但页面可能非激活状态、甚至处于被销毁状态，那么此时在消息回调中接触页面成员时，**存在生命周期安全的隐患**。
-   事件的观察者必须是页面，对页面的消息推送，**不能让其他地方 能够监听和收到 不可预期的推送**。
-   事件的发送者必须来自唯一可信源，如此在 “多页面状态同步” 等场景下，能够因 “数据总是来自唯一可信源的统一决策和分发”，而 **确保了状态同步的一致性和可靠性**。

而 ViewModel + LiveData 的组合，无疑是这种现状下 超低成本 解决上述 3 大问题的最优解，因而 **问题也就进一步地转化为 解决 LiveData 的 “数据倒灌” 问题**。

## 为什么通过全局 ViewModel 而不是 静态单例？

这个问题是过去 1 年读者群里问到最多的问题，被排在 “高频提问和解答” 的 TOP 1。

原因很简单，因为：

1.ViewModel 是可以声明为页面的私有成员来使用，如此可以 **将 “页面通信” 的消息限制在 “仅在视图控制器之间传播”，避免污染到外部**。

2.同时也可 **避免 ViewModel 持有的 LiveData 被外部组件拿到，造成不可预期的推送**。

## 为什么不用 LiveDataBus？

这个问题在 “高频提问和解答” 的排位是 TOP 3。

不在 “页面通信” 的场景下使用 LiveDataBus 是因为，我们是以 “**多人协作、页面繁杂的软件工程**” 为背景来谈论架构设计。在这样的背景下，任何微不足道的隐患，都可能被无限放大。

Bus 自身 **缺乏唯一可信源的理念约束**、仅仅只是消息的中转站、**无法做到 “决策逻辑内聚”**，乃至难以确保 “消息同步的可靠一致”，应彻底从项目中移除，以免团队新手的误用乃至滥用。

具体缘由可参考[《关于 LiveData 本质，你看到了第几层》](https://xiaozhuanlan.com/topic/6017825943) 篇的解析。

## 最新的 Result API 如何？

事实上，Result API 和 Bus 没有本质区别，应尽可能避免使用。

## Event 事件包装器、反射方式、SingleLiveEvent 分别存在什么问题？

在[《Jetpack MVVM 精讲》](https://xiaozhuanlan.com/topic/0129483567)中我分别提到了 **Event 事件包装器、反射方式、SingleLiveEvent** 这三种方式来解决 “数据倒灌” 的问题。它们分别来自上文我们提到的[外网](https://medium.com/androiddevelopers/livedata-with-snackbar-navigation-and-other-events-the-singleliveevent-case-ac2622673150)、[美团](https://tech.meituan.com/2018/07/26/android-livedatabus.html)的文章，和[官方最新 demo](https://github.com/android/architecture-samples/blob/dev-todo-mvvm-live/todoapp/app/src/main/java/com/example/android/architecture/blueprints/todoapp/SingleLiveEvent.java)。

然而它们分别存在如下问题：

> **Event 事件包装器：**
> 
> -   对于多观察者的情况，只允许第一个观察者消费，这不符合现实需求；
> -   而且手写 Event 事件包装器，在 Java 中存在 null 安全的一致性问题。
> 
> **反射干预 Version 的方式：**
> 
> -   存在延迟，无法用于对实时性有要求的场景；
> 
> **官方最新 demo 中的 SingleLiveEvent：**
> 
> -   是对 Event 事件包装器 一致性问题的改进，但未解决多观察者消费的问题；
> 
> **三者的共同点：**
> 
> -   LiveData 的数据会随着 SharedViewModel 长久滞留在内存中得不到释放。

## UnPeekLiveData 是如何解决这些问题的？

作为[《Jetpack MVVM Best Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)项目的中流砥柱，UnPeekLiveData 在过去一年收到了许多反馈。

事实上，对 UnPeekLiveData 的更新和维护是一波三折。为了解决上述 6 个问题、使能够被完美地应用在实际开发中，试过许多改进的方法。

并且非常感谢近期 [hegaojian](https://github.com/hegaojian)、Angki、Flynn、[Joker\_Wan](https://juejin.im/user/5829b958d20309005403f4d6)、小暑知秋、[大棋](https://juejin.im/user/1785262613208376/posts)、空白、qh、lvrenzhao、半节树人 等小伙伴积极的试用和反馈，使得未被觉察的问题 被及时发现和纳入考虑。

> 关于 UnPeekLiveData 解决这些问题 的思路，详见下文对每个版本源码设计思路的解析，同时我们在库中保留了每个版本的设计，方便小伙伴对 UnPeekLiveData 演化历程的回溯。

## GitHub & Maven 依赖

[https://github.com/KunMinX/UnPeekLiveData](https://github.com/KunMinX/UnPeekLiveData)

## 综上

为了避免生命周期安全的问题、为了保证多页面状态同步的一致性和消息的可靠性、为了避免页面通信对外部造成污染，因而**推荐 SharedViewModel + LiveData 的方式来作为处理页面通信的最优解**。

然而被 SharedViewModel 持有的 LiveData，在页面二进宫的场景下会造成 ”数据倒灌“ 的情况。因而 “各路神仙” 分别祭出了 **Event 事件包装器、反射方式、SingleLiveEvent** 等方式来解决 “数据倒灌”。

遗憾的是，这些解决方案都存在缺陷，无法完美地用在实际项目中，

因而我们结合实际场景的需要，设计和不断改进 UnPeekLiveData 来同时满足：  
**非入侵设计、支持多观察者消费、防止数据倒灌、支持消息手动从内存中释放** 等等，

如此一来，再也不用担心 “数据倒灌” 和 “内存溢出” 等问题了。

## Note 2020.07.21 加餐：

### UnPeekLiveData 增加一层 ProtectedUnPeekLiveData 的设计缘由解析

此举的出发点与 LiveData 之于 MutableLiveData 的设计一致，也即增加一层 ProtectedUnPeekLiveData，使得 UnPeekLiveData 同 MutableLiveData 一样，可以 **通过“向上转型” 的方式来在 Activity/Fragment 等场景下使 LiveData 只读，以此来实现数据的 “读写分离”，避免（来自数据层的） “源数据” 中途遭到不可预期的篡改**。

> 注：MutableLiveData 的 “Mutable”，人如其名，意思是 “多变的”，而我们这里要的是只读的。

如果这样说还不理解，详见[《关于 LiveData 本质，你看到了第几层》](https://xiaozhuanlan.com/topic/6017825943)篇对此的解析。

## Note 2020.10.15 加餐：

### 升级到 UnPeekLiveData v4.0 享用 “更快更稳” 的防倒灌机制

我们在 UnPeekLiveData v3.0 的基础上，参考了小伙伴 Flywith24 [WrapperLiveData](https://github.com/Flywith24/WrapperLiveData) 遍历 ViewModelStore 的思路，以此提升 “防止倒灌时机” 的精准度。

> 注：出于在现有 AndroidX 源码的背景下实现 “防倒灌机制” 的需要，**v4.0 对 Observe 方法的使用做了微调**，改为分别针对 Activity/Fragment 提供 ObserveInActivity 和 ObserveInFragment 方法，具体缘由详见源码注释的说明。

目前为止，UnPeekLiveData **实现和保留的特点** 如下：

> 1.一条消息能被多个观察者消费（since v1.0）
> 
> 2.消息被所有观察者消费完毕后才开始阻止倒灌（since v4.0）
> 
> 3.可以通过 clear 方法手动将消息从内存中移除（since v4.0）
> 
> 4.让非入侵设计成为可能，遵循开闭原则（since v3.0）
> 
> 5.基于 “访问权限控制” 支持 "读写分离”，遵循唯一可信源的消息分发理念（since v2.0，也即上文提到的 “ProtectedUnPeekLiveData” 设计）

并且我们仍保留了 **构造器模式** 的设计，后续如有定制功能拓展，我们会逐步在 Builder 中添加配置。

零入侵设计

防倒灌机制

Builder 构造器

![](https://images.xiaozhuanlan.com/photo/2020/2e43ef26ceb2df1f1ba22377986b3484.jpeg)

![](https://images.xiaozhuanlan.com/photo/2020/558e5daceb110711bc1718403790f5d6.jpeg)

![](https://images.xiaozhuanlan.com/photo/2020/64057b94b9965ffb8002a79a83b6eb52.jpeg)

## Note 2021.04.22 加餐：

### 升级到 UnPeekLiveData v5.0 获得更好的内存性能和更友好的 API 访问

感谢就职于 “腾讯音乐部门” 的小伙伴 @[张健](https://github.com/zhangjianlaoda) 应邀对 UnPeekLiveData 做的优化和升级。

**该版本保留了 UnPeekLiveData v4 的上述几大特点**，并在适当时机基于反射等机制，来彻底解决 UnPeekLiveData v4 下 Observers 无法释放、重复创建，以及 foreverObserver、removeObserver 被禁用等问题，将 UnPeekLiveData 的内存性能再往上提升了一个阶梯。

同时，该版本使 Observe 等方法的方法名和形参列表与官方 API 保持一致，尽可能减少新上手小伙伴的学习成本。

> 具体可参见 UnPeekLiveData 最新源码注释的说明。

## Note 2021.06.18 加餐：

### 是代码复杂度更小的 UnPeekLiveData v6.0 设计

感谢小伙伴 @[wl0073921](https://github.com/wl0073921) 对 UnPeekLiveData 源码的演化做出的贡献，

V6 版源码翻译和完善自小伙伴 wl0073921 在 [issue](https://github.com/KunMinX/UnPeek-LiveData/issues/11) 中的分享，

V6 版源码相比于 V5 版的改进之处在于，引入 Observer 代理类的设计，这使得在旋屏重建时，无需通过反射方式跟踪和复用基类 Map 中的 Observer，转而通过 removeObserver 的方式来自动移除和在页面重建后重建新的 Observer，

因而复杂度由原先的分散于基类数据结构，到集中在 proxy 对象这一处，进一步方便了源码逻辑的阅读和后续的修改。

> 具体可参见 UnPeekLiveData 最新源码及注释的说明。

## Note 2021.07.19 加餐：

### 是代码复杂度进一步减小的 UnPeekLiveData v6.1

感谢小伙伴 @[liweiGe](https://github.com/liweiGe) 分享的 “Pair 类设计” 的启发，

最终我们在 v6 版 UnPeekLiveData 的基础上进一步将 state 字段迁移至 ObserverProxy 类中，使 Map 得以合二为一、逻辑更加简练，方便源码阅读和后续的修改。

感兴趣可自行源码查阅。

## Note 2021.08.10 加餐：

### 分享一份 v6.1 源码简化版，方便对核心 “防倒灌” 机制的理解

看到 issue 区有小伙伴从头开始追溯 UnPeekLiveData 的演化历程，此处贴一份最新源码的简化版，

该简版仅保留最核心的 “event 防倒灌”，剔除了 内存清理、允许空值、允许 “state 倒灌” 等设计，感兴趣可自行对照查阅。

[v6.1 源码简化版](https://images.xiaozhuanlan.com/photo/2021/aba3be8c78f2c7cb5299d2d018a6f3f0.png)

## Note 2021.08.12 加餐：

### 是内存管理效率进一步优化的 UnPeekLiveData v7.0

感谢小伙伴 @[RebornWolfman](https://github.com/RebornWolfman) 在 [issue](https://github.com/KunMinX/UnPeek-LiveData/issues/17) 中的分享，

相较于上一版，我们在 V7 版源码中，通过在 "代理类/包装类" 中自行维护一个版本号，在 UnPeekLiveData 中维护一个当前版本号，分别来在 setValue 和 Observe 的时机来改变和对齐版本号，如此使得无需另外管理一个 Observer map，从而进一步规避了内存管理的问题，

同时这也是继 V6 版源码以来，最简的源码设计，方便阅读理解和后续修改。

> 具体可参见 UnPeekLiveData 最新源码及注释的说明。

## Note 2021.08.21 加餐：

### 分享一份 v7.2 源码完整版，是简练完整的防倒灌设计

再次感谢小伙伴 @[RebornWolfman](https://github.com/RebornWolfman) 在 [issue](https://github.com/KunMinX/UnPeek-LiveData/issues/21) 中的分享，v7.2 提供了更为简练的方式来解决 v7.0 removeObserver 时潜在的内存泄漏问题，以下 v7.2 版源码设计：

[v7.2 源码完整版](https://images.xiaozhuanlan.com/photo/2021/cab7dae5c6c3348bf5b35ad8f8e71f91.png)

## 谁在使用

感谢小伙伴们对 “开源库使用情况” 匿名调查问卷的参与，截至 2021年4月25日，我们了解到

包括 “腾讯音乐、BMW、TCL” 在内的诸多知名厂商的软件，都参考过我们开源的 [Jetpack MVVM Scaffold](https://github.com/KunMinX/Jetpack-MVVM-Scaffold) 架构模式，以及正在使用我们维护的 [UnPeek-LiveData](https://github.com/KunMinX/UnPeek-LiveData) 等框架。

目前我们已将具体的统计数据更新到 相关的开源库 ReadMe 中，错过本次问卷调查的小伙伴也不用担心，我们继续对此保持开放，不定期将小伙伴们登记的公司和产品更新到表格，

以便吸引到更多的小伙伴 参与到对这些架构组件的 使用、反馈，集众人之所长，让架构组件得以不断演化和升级。

[https://wj.qq.com/s2/8362688/124a/](https://wj.qq.com/s2/8362688/124a/)

集团 / 公司

产品

腾讯音乐

即将更新，暂时保密

TCL

内置应用，暂时保密

左医科技

诊室听译机器人

BMW

Speech

上海互教信息有限公司

知心慧学教师

美术宝

弹唱宝

网安

## 版权声明

> Copyright © 2019-present KunMinX

文中提到的 对 “**数据倒灌**” 一词及其现象的概括、**倒灌场景的复现**、对 “为什么推荐使用 SharedViewModel + LiveData 方案” 的**三个背景缘由**的理解、对 LiveData 粘性设计**在页面重建场景下的意义**的理解、对 Event 事件包装器、反射方式、SingleLiveEvent **各自存在的缺陷**的理解，以及对 UnPeekLiveData 的 “**延迟自动清理消息**” 的设计，**均属于本人独立原创的成果**，本人对此享有最终解释权。

**本文为专栏私有内容，谢绝转载**。

读者订阅了专栏，即享有了专栏文章的阅读权。

文章 **原创的引言、切入点、思路、结论** 凝聚了作者 KunMinX 本人的心血 及与参与者互动演化的结果，作者本人及作者在文中提名的有效贡献者 对相应的成果享有所有权。

**当您 借鉴或引用文中的引言、切入点、思路、结论 进行二次创作并打算发行时，须注明链接出处**，否则我们保留追责的权利。对此如有疑虑请及时沟通，或在发行前将作品交由本人核对。

当您看见有人 [洗稿](https://baike.baidu.com/item/%E6%B4%97%E7%A8%BF/20862574?fr=aladdin)、[剽窃](https://baike.baidu.com/item/%E5%89%BD%E7%AA%83/9398357?fr=aladdin)、未经授权转载本专栏的文章内容时，请及时向本人举报。

最后感谢您对专栏内容的阅读和喜欢。

> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

·

> 注：本文以
> 
> [《重学安卓：为你还原一个真实的 Jetpack Lifecycle》](https://xiaozhuanlan.com/topic/3684721950)
> 
> [《重学安卓：LiveData 鲜为人知的 身世背景 和 独特使命》](https://xiaozhuanlan.com/topic/0168753249)
> 
> [《重学安卓：有了 Jetpack ViewModel 真的可以为所欲为》](https://xiaozhuanlan.com/topic/6257931840)
> 
> [《重学安卓：是让人耳目一新的 Jetpack MVVM 精讲》](https://xiaozhuanlan.com/topic/0129483567) 以及
> 
> [《GitHub：Jetpack MVVM Best Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice) 作为前置知识，假设小伙伴已完整查阅并吸收了上述内容。

## 前言

上周有位在字节工作的小伙伴私下感叹：“文章相见恨晚，恨晚呐”。

据该小伙伴描述，他早在 2017 年就开始在项目中尝试 AAC（Android Architecture Components），原以为好日子即将开始，**没想到被 LiveData 的 “数据倒灌” 问题折腾得怀疑人生**。

由于了解 Jetpack 乃至爬坑 LiveData 的开发者并不多，网上关于这方面的资料略少，且多数开发者自学一门新技术 都是首选官方文档，

然而这一次问题的根源就在于，[官方文档](https://developer.android.google.cn/topic/libraries/architecture/viewmodel#sharing) 亲自推荐通过 SharedViewModel + LiveData 的组合来解决页面通信的问题，却在相当一段长的时间里 都没有亲自验证过并解决 **该方案在场景下存在的致命缺陷**。

> —— 架构组件的存在，本是为了规避不可预期的错误，没想到却因为这个 徒添了不可预期的错误。

## 姗姗来迟的笼统描述

与此类现象相关的解决方案，最早是在 2018 年中旬才在网上陆续刊登。

比如国外的：[LiveData with SnackBar, Navigation and other events](https://medium.com/androiddevelopers/livedata-with-snackbar-navigation-and-other-events-the-singleliveevent-case-ac2622673150)

以及国内的：[Android消息总线的演进之路：用LiveDataBus替代RxBus、EventBus](https://tech.meituan.com/2018/07/26/android-livedatabus.html)

但无论哪一种，都没有就此现象的背景做过 “因地制宜” 的概括，而只是笼统地用 “粘性事件” 来表达，也因此多数新上手 Jetpack 架构组件的小伙伴 **根本无从理解这个 “粘性事件” 到底意味着什么，更不曾料想到 当下正困扰着自己的，就是这个**。

因此，本文的目标即是 提供对 “数据倒灌” 现象的 **独家概括、缘由分析，以及趋近完美的解决方案**。

看完后，如你对 “数据倒灌” 这个概念有了印象、未来在遇到类似状况时能想起、并重新翻出和回溯这篇文章，那我的愿望也就达到了。

## 文章目录一览

-   前言
-   姗姗来迟的笼统描述
-   “数据倒灌” 的由来
-   为什么会有数据倒灌
    -   **Note：2020.07.17 加餐：**
    -   图文解析数据倒灌发生的场景
-   **为何非要通过 ViewModel + LiveData 来处理页面通信？**
-   为什么通过全局 ViewModel 而不是 静态单例？
-   为什么不用 LiveDataBus？
-   最新的 Result API 如何？
-   **Event 事件包装器、反射方式、SingleLiveEvent 分别存在什么问题？**
-   **UnPeekLiveData 是如何解决这些问题的？**
-   GitHub & Maven 依赖
-   综上
-   **Note 2020.07.21 加餐：**
    -   UnPeekLiveData 增加一层 ProtectedUnPeekLiveData 的设计缘由解析
-   **Note 2020.10.15 加餐：**
    -   升级到 UnPeekLiveData v4.0 享用 “更快更稳” 的防倒灌机制
-   **Note 2021.04.22 加餐：**
    -   升级到 UnPeekLiveData v5.0 获得更好的内存性能和更友好的 API 访问
-   **Note 2021.06.18 加餐：**
    -   是代码复杂度更小的 UnPeekLiveData v6.0 设计
-   **Note 2021.07.19 加餐：**
    -   是代码复杂度进一步减小的 UnPeekLiveData v6.1
-   **Note 2021.08.10 加餐：**
    -   分享一份 v6.1 源码简化版，方便对核心 “防倒灌” 机制的理解
-   **Note 2021.08.12 加餐：**
    -   是内存管理效率进一步优化的 UnPeekLiveData v7.0
-   **Note 2021.08.21 加餐：**
    -   分享一份 v7.2 源码完整版，是简练完整的防倒灌设计

## “数据倒灌” 的由来

今天提到的 “数据倒灌” 一词，缘于我为了方便理解和记忆 **“页面在 ‘二进宫’ 时收到旧数据推送” 的情况**，而在 2019 年 **自创并在网上传播的 对此类现象的概括**。

> 遗憾的是，[官方文档](https://developer.android.google.cn/topic/libraries/architecture/viewmodel#sharing) 至今 (截至 2020.7.15) 都没有意识到 并就该致命问题做出警告。

## 为什么会有数据倒灌

前言中已经提到，新上手的小伙伴们依赖官方文档，而官方文档给出的 “页面间通信” 场景的解决方案 存在致命缺陷 ——

一方面，用于通信的 LiveData 是被托管在 Activity / Application 级作用域的 SharedViewModel 中，于是 **LiveData 的生存期长于任何一个 Fragment**（假设通信双方是 Fragment）**：当二级 Fragment 出栈时，LiveData 实例仍存在**。

另一方面，LiveData 本身是被设计为粘性事件的，也即，一旦 LiveData 中持有数据，那么在观察者订阅该 LiveData 时，会被推送最后一次数据。

> 这样的设定本身也符合 LiveData 的常用场景（State） —— 比如 **在页面重建时，自动推送最后一次数据，而不必重新去向后台请求**。

只不过，这个特性在 “页面间通信” 场景（Event）下，就是致命的灾难 ——

新上手的小伙伴完全意料不到，还会有这样 “预期外” 的状况在等着自己，于是成就了读者群 “高频提问和解答” TOP 2。

> 不知道我这样说，有没说明白？——

我举一个存在 “数据倒灌” 隐患的例子：

> **Note 2021.8.16：重要提示，有言在先：**
> 
> 如想更好地理解和加深印象，可自行基于原生的 LiveData 来编写 demo 和复原灾难现场。
> 
> （最简单的办法是 **修改 [UnPeekLiveData](https://github.com/KunMinX/UnPeek-LiveData) 开源项目**，在 sample module 下的 SharedViewModel 中，将 ProtectedUnPeekLiveData 改为 LiveData、将 UnPeekLiveData 改为 MutableLiveData，然后照着下面的例子跑一遍）
> 
> 教程可以明确告诉你哪里有坑，但 **凡事唯有从 0 到 1 完整踩过一遍坑，才会真的有体会**。
> 
> 👆 👆 👆 划重点

假设 此处我们有 **列表、详情、编辑** 三个页面，返回栈中已经到达了三级页面 “编辑页”，编辑页的作用是编辑内容 并保存和返回到二级页面。

为了让 **编辑的结果在详情页中体现**，所以在保存的同时，我们会从编辑页通过 SharedViewModel 中的 MutableLiveData 去给详情页发消息，通知其刷新界面。

![](https://images.xiaozhuanlan.com/photo/2021/38ebe02731124b74ff80834bc8778e3d.png)

此时用户回到详情页，觉得内容 ok，便进一步回退到一级页面，即列表页，

![](https://images.xiaozhuanlan.com/photo/2021/002e41b2f5c7bd62f957418ab249be01.png)

那么接下来，用户在列表页中选择 **另一个 item** 点击，并再次跳到详情页时，就会因为**观察了 “已实例化过、并携带有旧数据” 的 MutableLiveData，而收到它的 “不符合预期” 的推送**。

![](https://images.xiaozhuanlan.com/photo/2021/74f3c71a3e0b5438d92b50ba28cd4274.png)

这种 “粘性设计” 的存在意义 刚刚上文已经提到了，很显然，它在当下这种场景下会带来致命的灾难。

## 为何非要通过 ViewModel + LiveData 来处理页面通信？

关于 页面通信的场景，我们能拼凑出的背景主要有以下几块：

-   事件的观察者是页面，但页面可能非激活状态、甚至处于被销毁状态，那么此时在消息回调中接触页面成员时，**存在生命周期安全的隐患**。
-   事件的观察者必须是页面，对页面的消息推送，**不能让其他地方 能够监听和收到 不可预期的推送**。
-   事件的发送者必须来自唯一可信源，如此在 “多页面状态同步” 等场景下，能够因 “数据总是来自唯一可信源的统一决策和分发”，而 **确保了状态同步的一致性和可靠性**。

而 ViewModel + LiveData 的组合，无疑是这种现状下 超低成本 解决上述 3 大问题的最优解，因而 **问题也就进一步地转化为 解决 LiveData 的 “数据倒灌” 问题**。

## 为什么通过全局 ViewModel 而不是 静态单例？

这个问题是过去 1 年读者群里问到最多的问题，被排在 “高频提问和解答” 的 TOP 1。

原因很简单，因为：

1.ViewModel 是可以声明为页面的私有成员来使用，如此可以 **将 “页面通信” 的消息限制在 “仅在视图控制器之间传播”，避免污染到外部**。

2.同时也可 **避免 ViewModel 持有的 LiveData 被外部组件拿到，造成不可预期的推送**。

## 为什么不用 LiveDataBus？

这个问题在 “高频提问和解答” 的排位是 TOP 3。

不在 “页面通信” 的场景下使用 LiveDataBus 是因为，我们是以 “**多人协作、页面繁杂的软件工程**” 为背景来谈论架构设计。在这样的背景下，任何微不足道的隐患，都可能被无限放大。

Bus 自身 **缺乏唯一可信源的理念约束**、仅仅只是消息的中转站、**无法做到 “决策逻辑内聚”**，乃至难以确保 “消息同步的可靠一致”，应彻底从项目中移除，以免团队新手的误用乃至滥用。

具体缘由可参考[《关于 LiveData 本质，你看到了第几层》](https://xiaozhuanlan.com/topic/6017825943) 篇的解析。

## 最新的 Result API 如何？

事实上，Result API 和 Bus 没有本质区别，应尽可能避免使用。

## Event 事件包装器、反射方式、SingleLiveEvent 分别存在什么问题？

在[《Jetpack MVVM 精讲》](https://xiaozhuanlan.com/topic/0129483567)中我分别提到了 **Event 事件包装器、反射方式、SingleLiveEvent** 这三种方式来解决 “数据倒灌” 的问题。它们分别来自上文我们提到的[外网](https://medium.com/androiddevelopers/livedata-with-snackbar-navigation-and-other-events-the-singleliveevent-case-ac2622673150)、[美团](https://tech.meituan.com/2018/07/26/android-livedatabus.html)的文章，和[官方最新 demo](https://github.com/android/architecture-samples/blob/dev-todo-mvvm-live/todoapp/app/src/main/java/com/example/android/architecture/blueprints/todoapp/SingleLiveEvent.java)。

然而它们分别存在如下问题：

> **Event 事件包装器：**
> 
> -   对于多观察者的情况，只允许第一个观察者消费，这不符合现实需求；
> -   而且手写 Event 事件包装器，在 Java 中存在 null 安全的一致性问题。
> 
> **反射干预 Version 的方式：**
> 
> -   存在延迟，无法用于对实时性有要求的场景；
> 
> **官方最新 demo 中的 SingleLiveEvent：**
> 
> -   是对 Event 事件包装器 一致性问题的改进，但未解决多观察者消费的问题；
> 
> **三者的共同点：**
> 
> -   LiveData 的数据会随着 SharedViewModel 长久滞留在内存中得不到释放。

## UnPeekLiveData 是如何解决这些问题的？

作为[《Jetpack MVVM Best Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)项目的中流砥柱，UnPeekLiveData 在过去一年收到了许多反馈。

事实上，对 UnPeekLiveData 的更新和维护是一波三折。为了解决上述 6 个问题、使能够被完美地应用在实际开发中，试过许多改进的方法。

并且非常感谢近期 [hegaojian](https://github.com/hegaojian)、Angki、Flynn、[Joker\_Wan](https://juejin.im/user/5829b958d20309005403f4d6)、小暑知秋、[大棋](https://juejin.im/user/1785262613208376/posts)、空白、qh、lvrenzhao、半节树人 等小伙伴积极的试用和反馈，使得未被觉察的问题 被及时发现和纳入考虑。

> 关于 UnPeekLiveData 解决这些问题 的思路，详见下文对每个版本源码设计思路的解析，同时我们在库中保留了每个版本的设计，方便小伙伴对 UnPeekLiveData 演化历程的回溯。

## GitHub & Maven 依赖

[https://github.com/KunMinX/UnPeekLiveData](https://github.com/KunMinX/UnPeekLiveData)

## 综上

为了避免生命周期安全的问题、为了保证多页面状态同步的一致性和消息的可靠性、为了避免页面通信对外部造成污染，因而**推荐 SharedViewModel + LiveData 的方式来作为处理页面通信的最优解**。

然而被 SharedViewModel 持有的 LiveData，在页面二进宫的场景下会造成 ”数据倒灌“ 的情况。因而 “各路神仙” 分别祭出了 **Event 事件包装器、反射方式、SingleLiveEvent** 等方式来解决 “数据倒灌”。

遗憾的是，这些解决方案都存在缺陷，无法完美地用在实际项目中，

因而我们结合实际场景的需要，设计和不断改进 UnPeekLiveData 来同时满足：  
**非入侵设计、支持多观察者消费、防止数据倒灌、支持消息手动从内存中释放** 等等，

如此一来，再也不用担心 “数据倒灌” 和 “内存溢出” 等问题了。

## Note 2020.07.21 加餐：

### UnPeekLiveData 增加一层 ProtectedUnPeekLiveData 的设计缘由解析

此举的出发点与 LiveData 之于 MutableLiveData 的设计一致，也即增加一层 ProtectedUnPeekLiveData，使得 UnPeekLiveData 同 MutableLiveData 一样，可以 **通过“向上转型” 的方式来在 Activity/Fragment 等场景下使 LiveData 只读，以此来实现数据的 “读写分离”，避免（来自数据层的） “源数据” 中途遭到不可预期的篡改**。

> 注：MutableLiveData 的 “Mutable”，人如其名，意思是 “多变的”，而我们这里要的是只读的。

如果这样说还不理解，详见[《关于 LiveData 本质，你看到了第几层》](https://xiaozhuanlan.com/topic/6017825943)篇对此的解析。

## Note 2020.10.15 加餐：

### 升级到 UnPeekLiveData v4.0 享用 “更快更稳” 的防倒灌机制

我们在 UnPeekLiveData v3.0 的基础上，参考了小伙伴 Flywith24 [WrapperLiveData](https://github.com/Flywith24/WrapperLiveData) 遍历 ViewModelStore 的思路，以此提升 “防止倒灌时机” 的精准度。

> 注：出于在现有 AndroidX 源码的背景下实现 “防倒灌机制” 的需要，**v4.0 对 Observe 方法的使用做了微调**，改为分别针对 Activity/Fragment 提供 ObserveInActivity 和 ObserveInFragment 方法，具体缘由详见源码注释的说明。

目前为止，UnPeekLiveData **实现和保留的特点** 如下：

> 1.一条消息能被多个观察者消费（since v1.0）
> 
> 2.消息被所有观察者消费完毕后才开始阻止倒灌（since v4.0）
> 
> 3.可以通过 clear 方法手动将消息从内存中移除（since v4.0）
> 
> 4.让非入侵设计成为可能，遵循开闭原则（since v3.0）
> 
> 5.基于 “访问权限控制” 支持 "读写分离”，遵循唯一可信源的消息分发理念（since v2.0，也即上文提到的 “ProtectedUnPeekLiveData” 设计）

并且我们仍保留了 **构造器模式** 的设计，后续如有定制功能拓展，我们会逐步在 Builder 中添加配置。

零入侵设计

防倒灌机制

Builder 构造器

![](https://images.xiaozhuanlan.com/photo/2020/2e43ef26ceb2df1f1ba22377986b3484.jpeg)

![](https://images.xiaozhuanlan.com/photo/2020/558e5daceb110711bc1718403790f5d6.jpeg)

![](https://images.xiaozhuanlan.com/photo/2020/64057b94b9965ffb8002a79a83b6eb52.jpeg)

## Note 2021.04.22 加餐：

### 升级到 UnPeekLiveData v5.0 获得更好的内存性能和更友好的 API 访问

感谢就职于 “腾讯音乐部门” 的小伙伴 @[张健](https://github.com/zhangjianlaoda) 应邀对 UnPeekLiveData 做的优化和升级。

**该版本保留了 UnPeekLiveData v4 的上述几大特点**，并在适当时机基于反射等机制，来彻底解决 UnPeekLiveData v4 下 Observers 无法释放、重复创建，以及 foreverObserver、removeObserver 被禁用等问题，将 UnPeekLiveData 的内存性能再往上提升了一个阶梯。

同时，该版本使 Observe 等方法的方法名和形参列表与官方 API 保持一致，尽可能减少新上手小伙伴的学习成本。

> 具体可参见 UnPeekLiveData 最新源码注释的说明。

## Note 2021.06.18 加餐：

### 是代码复杂度更小的 UnPeekLiveData v6.0 设计

感谢小伙伴 @[wl0073921](https://github.com/wl0073921) 对 UnPeekLiveData 源码的演化做出的贡献，

V6 版源码翻译和完善自小伙伴 wl0073921 在 [issue](https://github.com/KunMinX/UnPeek-LiveData/issues/11) 中的分享，

V6 版源码相比于 V5 版的改进之处在于，引入 Observer 代理类的设计，这使得在旋屏重建时，无需通过反射方式跟踪和复用基类 Map 中的 Observer，转而通过 removeObserver 的方式来自动移除和在页面重建后重建新的 Observer，

因而复杂度由原先的分散于基类数据结构，到集中在 proxy 对象这一处，进一步方便了源码逻辑的阅读和后续的修改。

> 具体可参见 UnPeekLiveData 最新源码及注释的说明。

## Note 2021.07.19 加餐：

### 是代码复杂度进一步减小的 UnPeekLiveData v6.1

感谢小伙伴 @[liweiGe](https://github.com/liweiGe) 分享的 “Pair 类设计” 的启发，

最终我们在 v6 版 UnPeekLiveData 的基础上进一步将 state 字段迁移至 ObserverProxy 类中，使 Map 得以合二为一、逻辑更加简练，方便源码阅读和后续的修改。

感兴趣可自行源码查阅。

## Note 2021.08.10 加餐：

### 分享一份 v6.1 源码简化版，方便对核心 “防倒灌” 机制的理解

看到 issue 区有小伙伴从头开始追溯 UnPeekLiveData 的演化历程，此处贴一份最新源码的简化版，

该简版仅保留最核心的 “event 防倒灌”，剔除了 内存清理、允许空值、允许 “state 倒灌” 等设计，感兴趣可自行对照查阅。

[v6.1 源码简化版](https://images.xiaozhuanlan.com/photo/2021/aba3be8c78f2c7cb5299d2d018a6f3f0.png)

## Note 2021.08.12 加餐：

### 是内存管理效率进一步优化的 UnPeekLiveData v7.0

感谢小伙伴 @[RebornWolfman](https://github.com/RebornWolfman) 在 [issue](https://github.com/KunMinX/UnPeek-LiveData/issues/17) 中的分享，

相较于上一版，我们在 V7 版源码中，通过在 "代理类/包装类" 中自行维护一个版本号，在 UnPeekLiveData 中维护一个当前版本号，分别来在 setValue 和 Observe 的时机来改变和对齐版本号，如此使得无需另外管理一个 Observer map，从而进一步规避了内存管理的问题，

同时这也是继 V6 版源码以来，最简的源码设计，方便阅读理解和后续修改。

> 具体可参见 UnPeekLiveData 最新源码及注释的说明。

## Note 2021.08.21 加餐：

### 分享一份 v7.2 源码完整版，是简练完整的防倒灌设计

再次感谢小伙伴 @[RebornWolfman](https://github.com/RebornWolfman) 在 [issue](https://github.com/KunMinX/UnPeek-LiveData/issues/21) 中的分享，v7.2 提供了更为简练的方式来解决 v7.0 removeObserver 时潜在的内存泄漏问题，以下 v7.2 版源码设计：

[v7.2 源码完整版](https://images.xiaozhuanlan.com/photo/2021/cab7dae5c6c3348bf5b35ad8f8e71f91.png)

## 谁在使用

感谢小伙伴们对 “开源库使用情况” 匿名调查问卷的参与，截至 2021年4月25日，我们了解到

包括 “腾讯音乐、BMW、TCL” 在内的诸多知名厂商的软件，都参考过我们开源的 [Jetpack MVVM Scaffold](https://github.com/KunMinX/Jetpack-MVVM-Scaffold) 架构模式，以及正在使用我们维护的 [UnPeek-LiveData](https://github.com/KunMinX/UnPeek-LiveData) 等框架。

目前我们已将具体的统计数据更新到 相关的开源库 ReadMe 中，错过本次问卷调查的小伙伴也不用担心，我们继续对此保持开放，不定期将小伙伴们登记的公司和产品更新到表格，

以便吸引到更多的小伙伴 参与到对这些架构组件的 使用、反馈，集众人之所长，让架构组件得以不断演化和升级。

[https://wj.qq.com/s2/8362688/124a/](https://wj.qq.com/s2/8362688/124a/)

集团 / 公司

产品

腾讯音乐

即将更新，暂时保密

TCL

内置应用，暂时保密

左医科技

诊室听译机器人

BMW

Speech

上海互教信息有限公司

知心慧学教师

美术宝

弹唱宝

网安

## 版权声明

> Copyright © 2019-present KunMinX

文中提到的 对 “**数据倒灌**” 一词及其现象的概括、**倒灌场景的复现**、对 “为什么推荐使用 SharedViewModel + LiveData 方案” 的**三个背景缘由**的理解、对 LiveData 粘性设计**在页面重建场景下的意义**的理解、对 Event 事件包装器、反射方式、SingleLiveEvent **各自存在的缺陷**的理解，以及对 UnPeekLiveData 的 “**延迟自动清理消息**” 的设计，**均属于本人独立原创的成果**，本人对此享有最终解释权。

**本文为专栏私有内容，谢绝转载**。

读者订阅了专栏，即享有了专栏文章的阅读权。

文章 **原创的引言、切入点、思路、结论** 凝聚了作者 KunMinX 本人的心血 及与参与者互动演化的结果，作者本人及作者在文中提名的有效贡献者 对相应的成果享有所有权。

**当您 借鉴或引用文中的引言、切入点、思路、结论 进行二次创作并打算发行时，须注明链接出处**，否则我们保留追责的权利。对此如有疑虑请及时沟通，或在发行前将作品交由本人核对。

当您看见有人 [洗稿](https://baike.baidu.com/item/%E6%B4%97%E7%A8%BF/20862574?fr=aladdin)、[剽窃](https://baike.baidu.com/item/%E5%89%BD%E7%AA%83/9398357?fr=aladdin)、未经授权转载本专栏的文章内容时，请及时向本人举报。

最后感谢您对专栏内容的阅读和喜欢。