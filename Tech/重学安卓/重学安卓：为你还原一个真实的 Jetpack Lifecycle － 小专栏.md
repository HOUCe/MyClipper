---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/3684721950
author: 
---

# [重学安卓：为你还原一个真实的 Jetpack Lifecycle － 小专栏](https://xiaozhuanlan.com/topic/3684721950)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

·

> Note 2021.5.25 **重要提示**：
> 
> 阅读本文的最佳时机是，**您已吃过** 阅读 “源码” 或 “源码分析文” 时 **找不到头绪的苦**。
> 
> 您还没吃过苦，那您先不要着急阅读本文。您得吃过苦，才会有体会。
> 
> 在您吃够这方面的苦后，您才有机会发现，本文正是专用于解决 “如何找到正确打开方式” 的困扰。
> 
> 我们绝不通篇贴源码，而是基于广泛的实践和反思，在累积过大量样本 乃至足以排除掉所有干扰信息后，**点到为止地揭露 Lifecycle 框架最为核心的本质**，方便您理解其真实的存在意义，乃至可以笃信地将其用在项目中。
> 
> 关于 Lifecycle 在项目中的实践，请另行查阅[《GitHub : Jetpack-MVVM-Best-Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)源码（可自行在 Android Studio 中通过全局查找关键字 “DefaultLifecycleObserver” 来定位）。

## 前言

前几期，我们在 全网独家 深度思考 的引导下，通过

> [重学安卓：Activity 生命周期的 3 个辟谣](https://xiaozhuanlan.com/topic/0213584967)
> 
> [重学安卓：绝不丢失状态的 Activity 重建机制](https://xiaozhuanlan.com/topic/7692814530)
> 
> [重学安卓：你丢了 offer，只因拎不清 Activity 任务和返回栈](https://xiaozhuanlan.com/topic/7812045693)
> 
> [重学安卓：Intent 好比你的择偶标准](https://xiaozhuanlan.com/topic/2869301475)
> 
> [重学安卓：我的碎片很听话，你的 Fragment 有自己的想法](https://xiaozhuanlan.com/topic/0937256481)

分别正确理解了 视图控制器 Activity 和 Fragment 的 **生命周期、重建机制、状态管理、路由跳转、页面通信** 的 **存在缘由、设计依据、职责边界 和 相互间关系**。

相信阅读完这几篇的朋友，对视图控制器的脉络已形成深刻的印象。

## 被提前提上日程的 Jetpack

所以，本来我是想，紧接着出几期 “视图系统” 相关的解读，而 Jetpack 放在最后一期软件工程实战时统一介绍。

没想到在后台有读者留言说，期待能早日看到我对 Jetpack 的解说。

—— 是网上关于 Jetpack 的文章太少了吗？不是的，恰恰相反，网上的 Jetpack 文章一抓一大把，却基本上是 **未经思考 就发布到网上** 的 大段摘抄、全篇都在贴源码的、人云亦云、含糊其辞 的搬运文。😡😠

这不仅不能 **帮助读者迅速了解状况 —— 究竟是面临了什么问题，乃至于问世了这样的框架**。反而还让人看了觉得云里雾里、白费大量时间。（例如，生命周期 “感知” 是一个十分抽象的术语，一味地用 “感知” 来自嗨，并不能让更多的人 “感知” 真实的状况）

更有甚者，完全脱离事实 去 **意淫和鼓吹一些本不存在的职责和作用**，给读者造成误导。

此外，考虑到前几期刚好是 完整地将 视图控制器的知识点 给过了一遍。

于是我打算趁热打铁地将 **生命周期管理、状态管理、页面通信、路由跳转** 的 Jetpack 最佳实践给大家过一遍。

也即接下来几期，我会分别解说 **Lifecycle、ViewModel、LiveData、Navigation**，来方便大家迅速理解真实的状况。

## 文章目录一览

-   前言
-   被提前提上日程的 Jetpack
-   Lifecycle 的目标只有三个
-   Lifecycle 问世前的混沌世界
-   Lifecycle 为什么能解决这三个问题？
-   那 Lifecycle 具体是依赖什么机制运作的呢？
-   引入 Lifecycle 后的世界
-   综上
-   附注
-   **Note 2020.2.27 加餐：**
    -   造成人们误认为 “页面 onPause 时不会收到 LiveData 通知” 的原因
-   **Note 2021.2.21 加餐：**
    -   对 “一致性” 及 “一致性问题” 本质的解析

## Lifecycle 的目标只有三个

Lifecycle 最早是在 support 26.1.0 时被引入的。它的设计是如此地成熟和应景，乃至于已经成为源码的一部分，而几乎无需使用者在 Gradle 额外地配置依赖。 \[[注1](https://xiaozhuanlan.com/topic/3684721950#tip1)\]

**Lifecycle 的目标只有三个：**

> 1.实现生命周期 **管理的一致性**，做到 “一处修改、处处生效”。
> 
> 2.让第三方组件能够 **随时在自己内部拿到生命周期状态**，以便执行 **及时叫停 错过时机的 异步业务** 等操作。
> 
> 3.让第三方组件在调试时 能够 **更方便和安全地追踪到 事故所在的生命周期源**。

此外不存在任何 个别人士讹传的其他作用！

> 划重点 👆👆👆

如果光是阅读了以上三点，你还是不太理解的话，那接下来我就分别介绍 99% 网文都不曾介绍的真实状况，来方便你迅速建立起感性的认识。

## Lifecycle 问世前的混沌世界

例如我们开发了一个需要及时关闭资源的第三方组件 GpsManager。那么为了做到跟随生命周期及时启用或释放资源，我们分别需要在每个依赖 GpsManager 的 Activity 中调用 onResume、onPause 等方法。

并且为了能够 **及时叫停 错过时机 的异步业务，来避免内存泄漏等情况发生**，我们额外安排了 isActive 变量，以便能够及时从 Activity 拿到 “异步业务是否适合继续” 的指标，并在异步的业务中定期做出判断。

![](https://images.xiaozhuanlan.com/photo/2021/d05bc888b3fc90495c070584826888ba.png)

那这样带来一个什么问题呢？—— 复杂度激增导致的容易出错。

为什么呢？

因为我们需要为每个依赖 GpsManager 的 Activity 都安排好 生命周期调用 和 状态通知。

一旦 Activity 变多，就 **难以保证修改的一致性**，比如日后我在 ActivityA 修改了 GpsManager 生命周期的管理方式，而同事的 ActivityB 忘了被修改，这就造成了问题。

> 如果你对 “一致性” 的概念不太理解的话，那么请回顾一下大学时期 数据库课程中提到的 “存储过程”，它就是典型的 “一致性” 封装：将一套连贯的 SQL 序列封装在一个 “API” 中，外部人员只需调用这个 “API”，而无需知道内部具体发生了什么。
> 
> 这避免了程序员 A 修改了某 SQL 语句，而程序员 B 没跟上。并且，“API” 的管理人只需一处修改，就能做到处处生效。

此外，对接收状态通知的需求，导致了我们可能需要在每个 Activity 中的每个生命周期回调额外安排上 setActive，这无疑又增加了复杂度。

再者，如果调试时，我想确知当前的事件源是哪个 Activity 呢？传统的方式只能额外地注入 Activity 到 GpsManager 中了。虽然如今倡导单 Activity App，使得 Activity 的生命周期基本和 Application 持平，而规避了一系列内存泄漏的问题，但如果事件源是 Fragment 呢？

**Lifecycle 就是为解决上述三个问题而存在。**

## Lifecycle 为什么能解决这三个问题？

在 [《Activity 的快乐你不懂》](https://xiaozhuanlan.com/topic/4568971203) 中我们介绍到，Activity 是一种模版方法模式的实现，也即将窗口管理、多任务管理等开发者不需要关注的细节，放在基类中完成，而对子类只暴露生命周期等回调。

这样开发者就能通过继承 Activity，来拿到简洁干净的子类模板 Activity。

同理，Lifecycle 也包含着模版方法模式的封装思想，将生命周期的管理封装在基类中，这样在子类模板中只需统一地调用一行 “API”：`getLifecycle().addObserver(mGpsManager)` ，来实现生命周期管理的一致性。

## 那 Lifecycle 具体是依赖什么机制运作的呢？

与此同时，正是基于观察者模式，使得将需要被 生命周期调用 和 状态通知 的第三方组件，作为观察者添加到 List / Map 中，并在 生命周期持有者（LifecycleOwner） Activity / Fragment 等发生生命周期节点变化时，通过事件分发代理者（LifecycleRegister）以遍历的方式通知所有观察者执行相应的逻辑。

这就要求能够提供一个统一的、标准化的接口或抽象类（例如 DefaultLifecycleObserver），来方便第三方组件实现生命周期方法。

并且，这些第三方组件通过这些方法，应能够及时得到当前的事件源和生命周期状态，因而上述接口方法传入的参数还应包括事件源 LifecycleOwner，且事件源应持有能够反映生命周期状态的实体（Lifecycle）。\[[注2](https://xiaozhuanlan.com/topic/3684721950#tip2)\]

## 引入 Lifecycle 后的世界

在 **模板方法模式 和 观察者模式** 的共同作用下：

生命周期的管理在基类 Activity 中自动完成，开发者只需在子类模板 Activity 中手动添加一行 “API” 完成调用，而无需操心解绑的操作。

同时，在 GpsManager 中，因实现了 LifecycleObserver 的接口，而能够在自己内部观察和及时拿到 LifecycleOwner 和 Lifecycle，以便 调试 或 执行 强生命周期 的操作。

![](https://images.xiaozhuanlan.com/photo/2021/41a228f7002ec0e95b458208aff2ee5c.png)

至此，Lifecycle 背靠的真实状况已介绍完。相信大家对于 Lifecycle 的大体构造已初步形成了感性的认识。

具体的源码我便不做累述了，大家可以很容易地带着感性认识去阅读和探究。

比如大家可以思考一下，LifecycleRegistry 中的 getStateAfter 方法 为什么要设计为 `ON_CREATE` 和 `ON_STOP` 对应着 `CREATED`，而 `ON_START` 和 `ON_PAUSE` 对应着 `STARTED` 呢？

答案在明晚之前公布，有想法的话欢迎提前在评论区留言。

## 综上

Lifecycle 的存在，是为了：

1.实现生命周期 **管理的一致性**，做到 “一处修改、处处生效”。

2.让第三方组件能够 **随时在自己内部拿到生命周期状态**，以便执行 **及时叫停 错过时机的 异步业务** 等 强生命周期 操作。

3.让第三方组件在调试时 能够 **更方便和安全地追踪到 事故所在的生命周期源**。

因此，Lifecycle 绝不是 个别人士刻意渲染的 **“有它你就能活过今晚，没有就不行”**，而是有它你能 “过得更好”，有它你能 更简便、更不容易出错 地完成生命周期的管理。

![](https://images.xiaozhuanlan.com/photo/2021/1545d183478d84c6867ce62a17259c9b.png)

> 全文完

## 附注

**注1：**在 API 26.1.0 以后，Lifecycle 几乎无需使用者在 Gradle 额外地配置依赖，但一些特殊环境下的拓展类，如 Java8 支持的 DefaultLifecycleObserver 除外。

```
implementation "androidx.lifecycle:lifecycle-runtime:2.0.0"
implementation "androidx.lifecycle:lifecycle-common-java8:2.0.0"
```

**注2：**正因为是通过回调方法来传入 LifecycleOwner，使得其作用域仅限于方法内部，而不至于扩散为成员变量。这已经可以满足在回调方法中调试时 对追踪事件源的需要了。

所以请不要额外地在观察者内部声明 LifecycleOwner 成员变量、并将方法参数中的 owner 对象赋值给该成员变量，不然就有内存泄漏的风险了。

## Note 2020.2.27 加餐：

### 造成人们误认为 “页面 onPause 时不会收到 LiveData 通知” 的原因

看到网上有不少 以讹传讹的网文 传播 “页面 onPause 时 不会收到 LiveData 通知” 等不实观点，给读者们徒添困扰、耽误大量时间，特此辟谣：

事实恰恰相反：onPause 可以收到，而 onStart 不能收到（截至 2020.2.27 AndroidX Fragment 的实现） ——

造成谣传现象的原因是，这些作者误把 LifeCycleOwner actived 对应的状态 STARTED、RESUMED 与页面生命周期 onStart、onResume 的混为一谈，乃至得出 “onPause 时不会收到消息” 的谬论。

![](https://images.xiaozhuanlan.com/photo/2021/9066b0f4d772c8c5dd7ff0185d71ed5d.png)

其实通过 官网的流程示意图（官网原图的事件全部大写，容易造成混淆，上图为我特别定制的版本，将事件改为驼峰命名）便可清晰直观地 Get 到：只有 onResume 和 onPause 是介于 STARTED、RESUMED 状态之间，也即 **只有这两个生命周期节点 100% 确定能收到 LiveData 的推送**（FragmentActivity 额外支持 onStart 期间的接收）。

（此外，你知道为什么这里 OnCreate ~ OnDestroy 被称为 “Events”（事件）吗？详见我在[《Activity 生命周期的 3 个辟谣》](https://xiaozhuanlan.com/topic/0213584967)中的解析。

## Note 2021.2.21 加餐：

### 关于 “一致性” 概念的完整解析

阅读过[《让人耳目一新的 Jetpack MVVM 精讲》](https://xiaozhuanlan.com/topic/0129483567)的小伙伴或许对 “一致性” 这个概念有特别的印象，因为类似这种 “跳出源码、直接从源头出发来解析本质” 的内容 在网上极为罕见，而 “解决一致性问题” 又几乎是 “抓住每个架构组件的本质” 的关键之所在。

与此同时又是考虑到 由于 “一致性” 的概念并非 Android 专属，而是软件工程通用，所以我们唯有去深入理解它的本质，才有机会在 Android 乃至各平台下做到举一反三、随机应变。

所以最终我们将 **关于 “一致性”、“一致性问题” 以及 “实现一致性的手段” 的解析**，集中整合到[《架构组件 “一致性问题” 解析》](https://xiaozhuanlan.com/topic/9340256871)篇来统一维护，对 “一致性” 乃至 “架构组件本质” 感兴趣的小伙伴，可自行前往查阅。

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