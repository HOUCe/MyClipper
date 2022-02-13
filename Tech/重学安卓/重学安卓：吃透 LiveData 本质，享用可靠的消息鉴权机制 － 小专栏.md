---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/6017825943
author: 
---

# [重学安卓：吃透 LiveData 本质，享用可靠的消息鉴权机制 － 小专栏](https://xiaozhuanlan.com/topic/6017825943)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

·

> **重要提示**：
> 
> 阅读本文的最佳时机是，**您已吃过** 阅读 “源码” 或 “源码分析文” 时 **找不到头绪的苦**。
> 
> 您还没吃过苦，那您先不要着急阅读本文。您得吃过苦，才会有体会。
> 
> 在您吃够这方面的苦后，您才有机会发现，本文正是专用于解决 “如何找到正确打开方式” 的困扰。
> 
> 我们绝不通篇贴源码，而是基于广泛的实践和反思，在累积过大量样本 乃至足以排除掉所有干扰信息后，**点到为止地揭露 LiveData 框架最为核心的本质**，方便您理解其真实的存在意义，乃至可以笃信地将其用在项目中。
> 
> 关于 LiveData 在项目中的实践，请另行查阅[《GitHub : Jetpack-MVVM-Best-Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)源码，以及[《架构模式实践》篇](https://xiaozhuanlan.com/topic/8204519736) 的完整解析。

## 前言

经过 2 年的修订和交流，关于 LiveData 的各角度理解已尘埃落定，所以这一期，我们以简练的话语，循序渐进将 “LiveData 的本质” 铺陈开来。

如果你已爬过阅读源码的坑，并且是第一次阅读本专栏关于 LiveData 的内容，那么通过本文可以了解到，关于 LiveData，最多可以了解到什么程度。

如果过去 2 年你已反复通读本专栏 “架构模式” 乃至 LiveData 相关的内容，那么本文可以作为参考，检验自己对 LiveData 的理解 是否和我们所介绍的一致。

## 文章目录一览

-   前言
-   **第一层**：LiveData 是与 UI 打交道的 “最后一环”
-   **第二层**：LiveData 可以推状态，也可以推事件
    -   广播：封闭式 的 发布订阅模式
-   **第三层**：LiveData 支持 “唯一可信源” 的鉴权设计
    -   状态是私域的，事件是公域的
    -   公证处：开放式 的 发布订阅模式
-   **第四层**：LiveData 存在 “数据倒灌” 的问题
    -   使用 UnPeek-LiveData 解决 “数据倒灌” 问题
-   综上

## 第一层：LiveData 是与 UI 打交道的 “最后一环”

LiveData 内部是 “观察者模式” 的设计，通过 Map 来维护观察者序列，上游通过 setValue 来推送最新消息，下游的观察者们便能收到消息，在 Activity/Fragment 等 “视图控制器” 中处理剩余的 UI 逻辑。

但如果仅此而已，还用 LiveData 干啥呢？EventBus 不香吗？或者直接写个 interface 回调不就完了么？

所以为什么要使用 LiveData，为什么考虑将其作为 “最后一环” …

因为 **LiveData 可缓存值，且包含 “生命周期安全” 的设计**，

LiveData 没观察者时不发送 **且保留值**，有观察者注册时就发送，并且由于 LiveData 内部有 Lifecycle 组件的加持，使得进一步约束为 “**观察者活跃时才发送，观察者不活跃时不发送**”，

—— 如此减少了不必要的性能浪费，且确保了 “生命周期安全”，避免在 onStop 等不稳定的环节被推送消息、被迫执行回调逻辑而发生 crash 等现象（因为 onStop 有可能是 “回到桌面” 或 “切换 App” 引发的，而这两者在 “非原生系统” 中又是可能存在不可预期的问题）

![](https://images.xiaozhuanlan.com/photo/2021/ba8a175cdedb1a14d72fefb449853f34.png)

## 第二层：LiveData 可以推状态，也可以推事件

基于第一层的认识，人们明白 LiveData 相比 EventBus 或 interface 的好处是，有 Lifecycle 的加持乃至能够规避 “不必要的性能浪费” 和确保 “生命周期安全”，尽可能避免由此引发的 null 安全等问题。

因而基于这一层理解，有人试图基于 “发布订阅模式”，将 LiveData 封装为 LiveDataBus，这样就可以通过 LiveDataBus 在页面之间发送生命周期安全的事件了。

所谓 “发布订阅模式”，就是在普通 “观察者模式” 中引入 “中间人/代理人”，发送者的消息不是直接推送到观察者，而是经由 “中间人/代理人” 来推送，这样做的好处是，中间人可以 **实施鉴权**、可以 **筛选消息** 并推送给指定分组的观察者，而不像 “观察者模式” 那样 **观察者们无差别地承受所有推送**。

![](https://images.xiaozhuanlan.com/photo/2021/4b1cb3cb9055c15db19c4e23f27db63c.png)

### 广播：封闭式 的 发布订阅模式

但这里值得注意的是，无论 LiveDataBus 还是 EventBus，它们本质上都是 “**广播**” —— 一种 “封闭式” 的 “发布订阅模式”，

也即它们的 “中间人/代理人” 逻辑是封闭的、写死的，仅仅起到代码 “解耦” 的作用 —— 解耦了 “观察者” 和 “发送者” 页面，因此还有个事实上更为重要的问题没有解决。

![](https://images.xiaozhuanlan.com/photo/2021/7dc202d9521609b6f7a5ec82bbf789dd.png)

## 第三层：LiveData 支持 “唯一可信源” 的鉴权设计

最开始接触 LiveData 和 MutableLiveData 的时候，多数人估计一脸懵逼 —— 为什么要将 LiveData 的 setValue 和 postValue 设置为 protected，然后在 MutableLiveData 中设置为 public —— 就是官方文档也没有说出个所以然。

然而在长足的爬坑过程中，你若经历过多次 为 “**消息推送难以溯源**”、“**消息同步不可靠不一致**” 等问题加班排查到半夜十二点，便有机会追溯造成此类现象的最根本因素，顿悟 “状态”、“事件” 的区别，乃至萌生 “唯一可信源” 的鉴权设计理念。

### 状态是私域的，事件是公域的

每个页面都包含状态，这些状态是私有的，并且托管在 State-ViewModel，例如 RecyclerView Adapter 的 ArrayList、播放按钮的 Boolean（Play/Pause）等等，

这些状态是可以随意改变的，页面在自己内部修改和使用，没有任何问题，但如果你希望将状态同步到其他页面，这时候你若通过 LiveDataBus 等 “广播” 直接推，就会造成上述 “难溯源”、“不可靠不一致” 等问题，

> —— 因为一来，**每个页面的状态 完全是由页面自己内部的 UI 逻辑推导出**，这就好比两个人就同一件事（例如 播放状态）持不同立场，公说公有理（觉得此时应该是 Play），婆说婆有理（觉得此时该是 Pause），但无论谁觉得自己有理，都不足以反映完整的客观事实，
> 
> 二来消息一旦发出去了，就很难追溯到底是哪个页面发来的，也即如果出了事（推出来的消息有误），很难找到为此负责的页面。

![](https://images.xiaozhuanlan.com/photo/2021/040fce447d79f04f921f5f98cc71dd9a.gif)

### 公证处：开放式 的 发布订阅模式

所以这时候，就需要设立一个第三方机构，“用事实说话” —— **将最终决定某状态为何值的逻辑，统一安排在第三方机构这一处**，

> —— 也即我们在[《架构组件 “一致性问题” 全面解析》](https://xiaozhuanlan.com/topic/9340256871)篇提到的 “使逻辑**内聚**”。

因而每当有页面想让其他页面状态同步，不能随意听信 —— **不允许页面直接向其他页面发号施令，而是仅能向公证处发起请求，由公证处来审核、决策和下发最终的通知给其他页面**，如此即可确保消息的 “溯源” 和 “可靠一致” 问题，

因为就算逻辑出错了，我不用去找是哪个页面的馊主意（想找其实也找不到），我直接找 “公证处” 这一处查阅和修改就行。

所以这个 “公证处”，就是我们所谓的 “唯一可信源”，而 LiveData setValue、postValue 的 protected —— 或者说 “读写分离” 设计，就是为了支持这种 “唯一可信源” 结构的成立 —— 让页面只能 observe，不能 setValue，只有公证处自己有权在内部决策和 setValue。

![](https://images.xiaozhuanlan.com/photo/2021/c2711796c3fced1095ecb35c1537dc4d.png)

## 第四层：LiveData 存在 “数据倒灌” 的问题

基于上述分析，我们得知，在消息发送场景，“唯一可信源” 是目前唯一可胜任的最优解，

然而在使用 SharedViewModel（Event-ViewModel）+ LiveData 的过程中，我们发现 LiveData 竟存在 “数据倒灌” 的问题 …

事实上，“数据倒灌”，或者说 “粘性设计”，适用于将 LiveData 用作 “状态” 的场景，例如旋屏后自动回推最后一次数据来渲染视图，然而既然 LiveData 存在 mutable 的设计，那就暗示了它本来也有意支持 “事件” 的推送，

而事件是不适合倒灌的，因为此时的 LiveData 是托管在作用域和生命周期大于页面的 Event-ViewModel 上，那么 LiveData 推送给上一个页面的数据，会残留，下一个页面进来时，就会 “不请自来” 地被倒灌 “脏数据”。

> 所以 AndroidX 包下的 LiveData，在我看来只是个半成品，并且也正是这种 “亦支持事件 亦不支持到位” 的 “自相矛盾” 的设计，导致小伙伴们纷纷迷惑和中招。
> 
> 关于 “数据倒灌” 场景的复现，可参见我们在[《LiveData 数据倒灌 背景缘由全貌 独家解析》](https://xiaozhuanlan.com/topic/6719328450)中的解析。

![](https://images.xiaozhuanlan.com/photo/2021/090c660e5fce4f5ce4cb2f93d8b5835b.png)

### 使用 UnPeek-LiveData 解决 “数据倒灌” 问题

我们于 2019 年 7 月 20 日开源了 UnPeek-LiveData，经过 2 年多的持续维护，得益于小伙伴们的积极参与，UnPeek-LiveData 现已经历了 7 次大版本升级（每每 “设计思路发生剧变”，升一个大版本），

并且不断有 来自国内或国际大厂工作的小伙伴 私下反馈说，他们正在重构的项目中使用。

在最新的 V7.1 版本中，我们通过在包装类中比对 version 等方式规避了 Map 的引入，从而使内存性能与原生 LiveData 齐平、源码逻辑更加简练和方便阅读修改，

感兴趣可自行测试和在项目中使用。

[GitHub：UnPeek-LiveData](https://github.com/KunMinX/UnPeek-LiveData)

## 综上

LiveData 是与 UI 打交道的 “最后一环”，LiveData 可缓存值，且包含 “生命周期安全” 的设计，在观察者活跃时才发送，观察者不活跃时不发送，

LiveData 可以推状态，也可以推事件，于是有人将其封装为 “广播”，例如 LiveDataBus，

然而广播并不能解决 “消息溯源” 和 “消息同步可靠一致” 的问题，因而我们利用 LiveData 实现 “**决策逻辑内聚 / 公证处 / 唯一可信源 读写分离**” 的设计来解决，

然而至此我们发现 LiveData 存在 “数据倒灌/粘性设计” 的问题，因而我们设计和维护了 UnPeek-LiveData 开源库来专门解决这个问题。

## 版权声明

> Copyright © 2019-present KunMinX

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

> **重要提示**：
> 
> 阅读本文的最佳时机是，**您已吃过** 阅读 “源码” 或 “源码分析文” 时 **找不到头绪的苦**。
> 
> 您还没吃过苦，那您先不要着急阅读本文。您得吃过苦，才会有体会。
> 
> 在您吃够这方面的苦后，您才有机会发现，本文正是专用于解决 “如何找到正确打开方式” 的困扰。
> 
> 我们绝不通篇贴源码，而是基于广泛的实践和反思，在累积过大量样本 乃至足以排除掉所有干扰信息后，**点到为止地揭露 LiveData 框架最为核心的本质**，方便您理解其真实的存在意义，乃至可以笃信地将其用在项目中。
> 
> 关于 LiveData 在项目中的实践，请另行查阅[《GitHub : Jetpack-MVVM-Best-Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)源码，以及[《架构模式实践》篇](https://xiaozhuanlan.com/topic/8204519736) 的完整解析。

## 前言

经过 2 年的修订和交流，关于 LiveData 的各角度理解已尘埃落定，所以这一期，我们以简练的话语，循序渐进将 “LiveData 的本质” 铺陈开来。

如果你已爬过阅读源码的坑，并且是第一次阅读本专栏关于 LiveData 的内容，那么通过本文可以了解到，关于 LiveData，最多可以了解到什么程度。

如果过去 2 年你已反复通读本专栏 “架构模式” 乃至 LiveData 相关的内容，那么本文可以作为参考，检验自己对 LiveData 的理解 是否和我们所介绍的一致。

## 文章目录一览

-   前言
-   **第一层**：LiveData 是与 UI 打交道的 “最后一环”
-   **第二层**：LiveData 可以推状态，也可以推事件
    -   广播：封闭式 的 发布订阅模式
-   **第三层**：LiveData 支持 “唯一可信源” 的鉴权设计
    -   状态是私域的，事件是公域的
    -   公证处：开放式 的 发布订阅模式
-   **第四层**：LiveData 存在 “数据倒灌” 的问题
    -   使用 UnPeek-LiveData 解决 “数据倒灌” 问题
-   综上

## 第一层：LiveData 是与 UI 打交道的 “最后一环”

LiveData 内部是 “观察者模式” 的设计，通过 Map 来维护观察者序列，上游通过 setValue 来推送最新消息，下游的观察者们便能收到消息，在 Activity/Fragment 等 “视图控制器” 中处理剩余的 UI 逻辑。

但如果仅此而已，还用 LiveData 干啥呢？EventBus 不香吗？或者直接写个 interface 回调不就完了么？

所以为什么要使用 LiveData，为什么考虑将其作为 “最后一环” …

因为 **LiveData 可缓存值，且包含 “生命周期安全” 的设计**，

LiveData 没观察者时不发送 **且保留值**，有观察者注册时就发送，并且由于 LiveData 内部有 Lifecycle 组件的加持，使得进一步约束为 “**观察者活跃时才发送，观察者不活跃时不发送**”，

—— 如此减少了不必要的性能浪费，且确保了 “生命周期安全”，避免在 onStop 等不稳定的环节被推送消息、被迫执行回调逻辑而发生 crash 等现象（因为 onStop 有可能是 “回到桌面” 或 “切换 App” 引发的，而这两者在 “非原生系统” 中又是可能存在不可预期的问题）

![](https://images.xiaozhuanlan.com/photo/2021/ba8a175cdedb1a14d72fefb449853f34.png)

## 第二层：LiveData 可以推状态，也可以推事件

基于第一层的认识，人们明白 LiveData 相比 EventBus 或 interface 的好处是，有 Lifecycle 的加持乃至能够规避 “不必要的性能浪费” 和确保 “生命周期安全”，尽可能避免由此引发的 null 安全等问题。

因而基于这一层理解，有人试图基于 “发布订阅模式”，将 LiveData 封装为 LiveDataBus，这样就可以通过 LiveDataBus 在页面之间发送生命周期安全的事件了。

所谓 “发布订阅模式”，就是在普通 “观察者模式” 中引入 “中间人/代理人”，发送者的消息不是直接推送到观察者，而是经由 “中间人/代理人” 来推送，这样做的好处是，中间人可以 **实施鉴权**、可以 **筛选消息** 并推送给指定分组的观察者，而不像 “观察者模式” 那样 **观察者们无差别地承受所有推送**。

![](https://images.xiaozhuanlan.com/photo/2021/4b1cb3cb9055c15db19c4e23f27db63c.png)

### 广播：封闭式 的 发布订阅模式

但这里值得注意的是，无论 LiveDataBus 还是 EventBus，它们本质上都是 “**广播**” —— 一种 “封闭式” 的 “发布订阅模式”，

也即它们的 “中间人/代理人” 逻辑是封闭的、写死的，仅仅起到代码 “解耦” 的作用 —— 解耦了 “观察者” 和 “发送者” 页面，因此还有个事实上更为重要的问题没有解决。

![](https://images.xiaozhuanlan.com/photo/2021/7dc202d9521609b6f7a5ec82bbf789dd.png)

## 第三层：LiveData 支持 “唯一可信源” 的鉴权设计

最开始接触 LiveData 和 MutableLiveData 的时候，多数人估计一脸懵逼 —— 为什么要将 LiveData 的 setValue 和 postValue 设置为 protected，然后在 MutableLiveData 中设置为 public —— 就是官方文档也没有说出个所以然。

然而在长足的爬坑过程中，你若经历过多次 为 “**消息推送难以溯源**”、“**消息同步不可靠不一致**” 等问题加班排查到半夜十二点，便有机会追溯造成此类现象的最根本因素，顿悟 “状态”、“事件” 的区别，乃至萌生 “唯一可信源” 的鉴权设计理念。

### 状态是私域的，事件是公域的

每个页面都包含状态，这些状态是私有的，并且托管在 State-ViewModel，例如 RecyclerView Adapter 的 ArrayList、播放按钮的 Boolean（Play/Pause）等等，

这些状态是可以随意改变的，页面在自己内部修改和使用，没有任何问题，但如果你希望将状态同步到其他页面，这时候你若通过 LiveDataBus 等 “广播” 直接推，就会造成上述 “难溯源”、“不可靠不一致” 等问题，

> —— 因为一来，**每个页面的状态 完全是由页面自己内部的 UI 逻辑推导出**，这就好比两个人就同一件事（例如 播放状态）持不同立场，公说公有理（觉得此时应该是 Play），婆说婆有理（觉得此时该是 Pause），但无论谁觉得自己有理，都不足以反映完整的客观事实，
> 
> 二来消息一旦发出去了，就很难追溯到底是哪个页面发来的，也即如果出了事（推出来的消息有误），很难找到为此负责的页面。

![](https://images.xiaozhuanlan.com/photo/2021/040fce447d79f04f921f5f98cc71dd9a.gif)

### 公证处：开放式 的 发布订阅模式

所以这时候，就需要设立一个第三方机构，“用事实说话” —— **将最终决定某状态为何值的逻辑，统一安排在第三方机构这一处**，

> —— 也即我们在[《架构组件 “一致性问题” 全面解析》](https://xiaozhuanlan.com/topic/9340256871)篇提到的 “使逻辑**内聚**”。

因而每当有页面想让其他页面状态同步，不能随意听信 —— **不允许页面直接向其他页面发号施令，而是仅能向公证处发起请求，由公证处来审核、决策和下发最终的通知给其他页面**，如此即可确保消息的 “溯源” 和 “可靠一致” 问题，

因为就算逻辑出错了，我不用去找是哪个页面的馊主意（想找其实也找不到），我直接找 “公证处” 这一处查阅和修改就行。

所以这个 “公证处”，就是我们所谓的 “唯一可信源”，而 LiveData setValue、postValue 的 protected —— 或者说 “读写分离” 设计，就是为了支持这种 “唯一可信源” 结构的成立 —— 让页面只能 observe，不能 setValue，只有公证处自己有权在内部决策和 setValue。

![](https://images.xiaozhuanlan.com/photo/2021/c2711796c3fced1095ecb35c1537dc4d.png)

## 第四层：LiveData 存在 “数据倒灌” 的问题

基于上述分析，我们得知，在消息发送场景，“唯一可信源” 是目前唯一可胜任的最优解，

然而在使用 SharedViewModel（Event-ViewModel）+ LiveData 的过程中，我们发现 LiveData 竟存在 “数据倒灌” 的问题 …

事实上，“数据倒灌”，或者说 “粘性设计”，适用于将 LiveData 用作 “状态” 的场景，例如旋屏后自动回推最后一次数据来渲染视图，然而既然 LiveData 存在 mutable 的设计，那就暗示了它本来也有意支持 “事件” 的推送，

而事件是不适合倒灌的，因为此时的 LiveData 是托管在作用域和生命周期大于页面的 Event-ViewModel 上，那么 LiveData 推送给上一个页面的数据，会残留，下一个页面进来时，就会 “不请自来” 地被倒灌 “脏数据”。

> 所以 AndroidX 包下的 LiveData，在我看来只是个半成品，并且也正是这种 “亦支持事件 亦不支持到位” 的 “自相矛盾” 的设计，导致小伙伴们纷纷迷惑和中招。
> 
> 关于 “数据倒灌” 场景的复现，可参见我们在[《LiveData 数据倒灌 背景缘由全貌 独家解析》](https://xiaozhuanlan.com/topic/6719328450)中的解析。

![](https://images.xiaozhuanlan.com/photo/2021/090c660e5fce4f5ce4cb2f93d8b5835b.png)

### 使用 UnPeek-LiveData 解决 “数据倒灌” 问题

我们于 2019 年 7 月 20 日开源了 UnPeek-LiveData，经过 2 年多的持续维护，得益于小伙伴们的积极参与，UnPeek-LiveData 现已经历了 7 次大版本升级（每每 “设计思路发生剧变”，升一个大版本），

并且不断有 来自国内或国际大厂工作的小伙伴 私下反馈说，他们正在重构的项目中使用。

在最新的 V7.1 版本中，我们通过在包装类中比对 version 等方式规避了 Map 的引入，从而使内存性能与原生 LiveData 齐平、源码逻辑更加简练和方便阅读修改，

感兴趣可自行测试和在项目中使用。

[GitHub：UnPeek-LiveData](https://github.com/KunMinX/UnPeek-LiveData)

## 综上

LiveData 是与 UI 打交道的 “最后一环”，LiveData 可缓存值，且包含 “生命周期安全” 的设计，在观察者活跃时才发送，观察者不活跃时不发送，

LiveData 可以推状态，也可以推事件，于是有人将其封装为 “广播”，例如 LiveDataBus，

然而广播并不能解决 “消息溯源” 和 “消息同步可靠一致” 的问题，因而我们利用 LiveData 实现 “**决策逻辑内聚 / 公证处 / 唯一可信源 读写分离**” 的设计来解决，

然而至此我们发现 LiveData 存在 “数据倒灌/粘性设计” 的问题，因而我们设计和维护了 UnPeek-LiveData 开源库来专门解决这个问题。

## 版权声明

> Copyright © 2019-present KunMinX

**本文为专栏私有内容，谢绝转载**。

读者订阅了专栏，即享有了专栏文章的阅读权。

文章 **原创的引言、切入点、思路、结论** 凝聚了作者 KunMinX 本人的心血 及与参与者互动演化的结果，作者本人及作者在文中提名的有效贡献者 对相应的成果享有所有权。

**当您 借鉴或引用文中的引言、切入点、思路、结论 进行二次创作并打算发行时，须注明链接出处**，否则我们保留追责的权利。对此如有疑虑请及时沟通，或在发行前将作品交由本人核对。

当您看见有人 [洗稿](https://baike.baidu.com/item/%E6%B4%97%E7%A8%BF/20862574?fr=aladdin)、[剽窃](https://baike.baidu.com/item/%E5%89%BD%E7%AA%83/9398357?fr=aladdin)、未经授权转载本专栏的文章内容时，请及时向本人举报。

最后感谢您对专栏内容的阅读和喜欢。