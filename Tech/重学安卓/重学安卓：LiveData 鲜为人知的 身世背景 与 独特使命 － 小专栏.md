---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/0168753249
author: 
---

# [重学安卓：LiveData 鲜为人知的 身世背景 与 独特使命 － 小专栏](https://xiaozhuanlan.com/topic/0168753249)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

·

> **Note 2021.8.18 新版本提示：**
> 
> 本文最初发行于 2019.7.19，在此期间，我们从未停下分享和交流的脚步，
> 
> 为此，一方面，我们不断为 LiveData 篇提供了 加餐和翻新，另一方面，不断萌生和沉淀了新的见解，
> 
> 时隔本文发行两周年零一个月，我们发行了本文的 “重制版” ——[《吃透 LiveData 本质，享用可靠的消息鉴权机制》](https://xiaozhuanlan.com/topic/6017825943)，通过 “循序渐进、言简意赅” 的叙述方式，重新为您阐述对 LiveData 的完整理解，
> 
> 感兴趣可以 “重制版” 内容为准，而打满细节补丁的本文 则可作为 “工具书” 来查阅和参考。

·

> Note 2021.5.25 **重要提示**：
> 
> 阅读本文的最佳时机是，**您已吃过** 阅读 “源码” 或 “源码分析文” 时 **找不到头绪的苦**。
> 
> 您还没吃过苦，那您先不要着急阅读本文。您得吃过苦，才会有体会。
> 
> 在您吃够这方面的苦后，您才有机会发现，本文正是专用于解决 “如何找到正确打开方式” 的困扰。
> 
> 我们绝不通篇贴源码，而是基于广泛的实践和反思，在累积过大量样本 乃至足以排除掉所有干扰信息后，**点到为止地揭露 LiveData 框架最为核心的本质**，方便您理解其真实的存在意义，乃至可以笃信地将其用在项目中。
> 
> 关于 LiveData 在项目中的实践，请另行查阅[《GitHub : Jetpack-MVVM-Best-Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)源码以及[《架构模式实践》篇](https://xiaozhuanlan.com/topic/8204519736) 的完整解析。

## 前言

上上上周，出于练手需要，经友人介绍 **接手并独立负责了 29 个页面、34 个 API、涉及 350 余个细节 的中大型电商软件的开发**（感谢友人在项目后期 主动提出帮忙调整布局和对接 JS）。

该软件有成熟的 iOS 实例，有专门适配 Android 的 360dp 设计稿，有 Restful API，所以尽管是第一次合作，总的来说沟通过程还是十分满意。

![](https://images.xiaozhuanlan.com/photo/2021/e9dc31190097edc3dfa4f95e9c08eb09.png)

在该项目中，我广泛地采用了 Jetpack 架构组件。

事实上，正因为 **依托于 Jetpack 面向标准化的、普适的 架构设计 和 理念规约**，我才 **得以在如此高强度的、紧凑的研发周期内，完成合作方原以为不太可能完成的任务**。

在有过这次研发经历后，我越发地想要向所有认识的、或者不认识的人推（qiang）荐（po）使用 Jetpack 架构组件。

并且，在架构组件中，如果说有哪个组件是我首推的，我一定会选择 LiveData。

## 被人云亦云者埋汰的 LiveData

在此之前，不少人对 LiveData 有着很深的误解，认为 LiveData 是个鸡肋的存在，并且老爱拿 RxJava 和 LiveData 比较。

> “RxJava 也能做到”，你一定听过不少这样的抬杠。

事实上，LiveData 和 RxJava 的关系，就像手与挖掘机的关系 —— 你可以直接用手来颠勺，也可以用挖掘机炒菜，但我们炒菜时 **需要能专注 “炒菜场景本身” 潜在的问题** —— 手足够灵活和应对这些问题，

而挖掘机就很难了，不仅 **难以专注和优化 “炒菜” 本身**，**还要额外先考虑** 怎么把 “硕大的” 挖掘机请到家里、怎么避免额外造成破坏 …

所以，今天这篇文章存在的缘由，就是为了帮助大家推开那些无聊的争论，来 **一睹真实的 LiveData，它究竟为超高效率、健壮而稳定的软件开发，带来了哪些不可思议的帮助**。

## 文章目录一览

-   前言
-   被人云亦云者埋汰的 LiveData
-   LiveData 的目标只有三个
-   LiveData 问世前的混沌世界
-   LiveData 为什么能解决这三个问题？
-   综上
-   **Note 2020.11.11 加餐：**
    -   举一个关于 “唯一可信源” 通俗易懂的生活案例
-   **Note 2020.2.17 加餐：**
    -   Fragment owner 最新设计的变更及缘由
-   **Note 2020.10.14 加餐：**
    -   对 “唯一可信源” 概念本质的补充说明
-   **Note 2020.4.11 加餐：**
    -   注意不要重复注册 LiveData
-   **Note 2021.8.16 加餐：**
    -   关于 “观察者模式” 和 “发布订阅模式” 的区别
-   **Note 2021.01.12 加餐：**
    -   匿名内部类有个 “重复订阅” 的坑需要注意
-   **Note 2021.04.12 加餐：**
    -   对 “Note 2021.01.12 加餐” 的进一步说明
-   **Note 2020.7.15 加餐：**
    -   LiveData 数据倒灌 背景缘由全貌 独家解析
-   **Note 2020.09.14 加餐：**
    -   对 “LiveData 不宜用在 Repository” 的解读
-   **Note 2020.11.06 加餐：**
    -   对 EventBus 适用场景的解析

## LiveData 的目标只有三个

这篇文章我反反复复地写了又推倒、推倒了又重写，因为立意特别困难 —— 我不能抛开 “架构设计” 和 “理念规约” 单独来介绍 LiveData —— 这样的 LiveData 是没有灵魂的。

**LiveData 是在 架构设计面向标准化 的背景下诞生的**，所以无论如何，我都要首先帮助读者了解状况：到底出于什么缘由，要存在 LiveData 这样的设计。

LiveData 的目标只有三个：

> 1.在 Lifecycle 的帮助下，规避订阅者回调的生命周期安全问题。
> 
> 2.遵循 “唯一可信源” 理念的约束，也即通过 “**决策逻辑内聚**” 和 “**读写分离**” 来规避 “跨域消息同步” 等场景下 **高频** 存在的可靠一致问题、使团队新手也能 不假思索写出低风险代码。
> 
> 3.就算不用 DataBinding，也能使 “单向依赖” 成为可能、规避潜在的内存泄漏等问题。

如果光是阅读了以上三点，你还是不太理解的话，那接下来我就分别介绍 99% 网文都不曾介绍的真实状况，来方便你迅速建立起感性的认识。

## LiveData 问世前的混沌世界

例如这一次我开发的音频播放软件，其中最核心的功能是播放控制，它总共牵涉到 57 个业务细节。

这里我只拎出最具代表性的 3 个细节：

> 1.当音乐被暂停、播放时，所有涉及播放元素的界面 都要切换到对应的状态。
> 
> 2.当切换歌曲时，所有涉及到当前歌曲信息的界面，都要正确展示信息。
> 
> 3.当切换歌曲时，所有涉及专辑列表的界面，都要正确展示每一项的播放状态。

首页

专辑列表

播放页

![](https://images.xiaozhuanlan.com/photo/2019/4f2900dd75695554adbbe42ef01cbab7.gif)

![](https://images.xiaozhuanlan.com/photo/2019/3e533b9246b2ba26a8210f672df4db33.gif)

![](https://images.xiaozhuanlan.com/photo/2019/2644ceb47db451ea69840a5848084943.gif)

> （截图来自本次开发的电商软件《FairyTales》，Achieved by KunMinX with doubled patience and Love）

第一条很好理解吧？如图所示，无论我是在 首页、列表页、播放页、还是在 通知栏 触发了播放按钮，一旦播放状态被改变，那么所有包含 展示播放状态 的页面，都要及时同步状态。

第二条的意思是，切歌后，在播放页面等 展示信息的地方，要正确展示当前专辑封面、歌曲的标题、作者等信息。

第三条细节，看似简单 —— 像前面 2 条一样，同步一下列表的 currentIndex （当前 item 的 index）就行？

实际上不是的，它背靠的是 “存在多种播放模式” 的背景，也即，当下可能处于 单曲循环、顺序播放、也可能处于 随机播放模式。

随机播放模式，实际使用的播放列表是一张打乱顺序的列表，是和给用户展示的专辑列表不同的列表。

> 那么如何处理 事件源 和 订阅者 的关系呢？

**首先**，既然要控制播放，自然是先封装一个播放控制的单例。

**然后**，为了解决状态同步的问题，可能首先想到的就是 EventBus —— 为每个页面都注册 EventBus，并且安排好监听方法。当任何一个页面发起通知，其他页面都跟着响应，做出对应的 UI 状态的变化。

看似就解决了 1、2 两个需求。

**再者**，对于第 3 个需求，因为如果要正确处理 在不同播放模式下，专辑列表和播放列表的正确关系，势必需要唯一可信的对象，以便我们分别在 专辑列表 和 播放列表 中拿到正确的 currentIndex，于是我们在当前页面通过单例拿到正确的 index，并通过 EventBus 来通知上一页（例如当前在播放页，上一页是专辑页）的列表做出变化。

看似第 3 个需求也解决了。

> 本以为这样需求就轻松解决了，没想到，这么做，出错的概率却是大大增加的，为什么呢？

首先，如果使用 EventBus，那么我们须在每个监听状态的页面、在特定的生命周期节点中 **手动地注册和解绑** EventBus，**这使得 因疏忽导致的编码不一致 而带来不可预期错误 的概率大大提升**（例如在页面步入 onDestroy 后，收到消息并调用了可能已为 null 的视图实例 … ）

> 注：对 “一致性” 的概念不熟悉的朋友可参见[《架构组件 “一致性” 概念》](https://xiaozhuanlan.com/topic/9340256871)篇的完整解析。

并且，**在缺乏理念约束的情况下，随意使用 EventBus，将难以追踪 “真正的事件源”** —— 事件源只有 1、2 个还好，如果是像今天这个 29 个页面的复杂项目，在调试时，光是找事件源，恐怕就得让人欲仙欲死。EventBus 追踪事件源的复杂度是 n \* (n - 1)，简言之就是 n²。

![](https://images.xiaozhuanlan.com/photo/2021/fb47b2d473cac71f592dd86bde22fc52.png)

再者，在缺乏理念约束的情况下，**通过 EventBus 从页面发送通知，难以总是确保消息的正确性 和 可靠性** —— 也许当前页面的信息是过时的呢？那么所有收到通知的页面，拿到的数据也就过时了，这非常影响用户体验。

## LiveData 为什么能解决这三个问题？

### 特性 1：遵循唯一可信源理念，解决消息分发的一致性问题

首先，我们需要再次明确的一点是：**LiveData 诞生的背景是，Google 希望确立一种标准化、规范化的开发模式，使得就算是团队新手，也能自然而然地遵循这样一种模式。**

为了达到这个目的，Google 可谓下了苦工，LiveData 就是在这样的背景下诞生的设计。

它被设计为仅限于负责 **数据在订阅者生命周期内的被分发**，因此除了 setValue / postValue，以及 observe，你再也看不到别的方法，是的，就是这么纯粹。

**正因为如此纯粹、甚至无法独当一面，乃至于你不得不借助 ViewModel 或单例，来完成单方向的数据分发** —— 只要你这样做了，Google 的愿望也就达到了。

为什么呢？

> 因为 **一旦这样做了，你不知不觉便达成了两个目标**：

一个是单向依赖：UI -> ViewModel -> Data。（单向依赖的好处，我们在下一篇 ViewModel 的专题中重点介绍）

另一个是 **从唯一可信源取材，完成数据的分发**。

![](https://images.xiaozhuanlan.com/photo/2021/359ef1cd05278437e315b122c58f0fa5.png)

唯一可信源，意味着**无论是哪个页面发起的对 “改变状态” 的请求，最终所有页面状态的改变，都是来自可信源统一的决策和分发**。

如此一来，复杂度从 n² 骤减至 1 —— 我们能够十分方便地确认 “只读数据” 的来源，并且 **数据一定总是最新的，而不至于取到的是由 ”某个信息滞后的页面“ 所发送的 “过时状态”**。

> 想想看，两周多一点的时间，横跨 29 个页面的协调交互，容不得半点错误 —— 排查不可预期的错误所耗费的时间，往往是惊人的。

—— LiveData 正是以自己的纯粹和克制，来客观上帮助开发者 不知不觉树立正确的编码习惯。

> **Note 2020.11.11：**
> 
> 有不少小伙伴表示 在工作中亲身经历后，隐约想起 原来这就是专栏中提到的 “**多页面状态同步不一致**” 的棘手问题，因而出于 **方便理解 和 加深印象** 的考虑，这里就 “唯一可信源” 的本质和案例 分别做了两次加餐，感兴趣的小伙伴可随时查阅：
> 
> 关于 “唯一可信源” 的本质特征和设计原理，详见下文 **Note 2020.10.14 加餐** 的解析。
> 
> 关于 “唯一可信源” 在生活中的案例 详见以下 **Note 2020.11.11 加餐** 的举例说明。
> 
> 👆👆👆 划重点

## Note 2020.11.11 加餐：

### 举一个关于 “唯一可信源” 通俗易懂的生活案例

群里有小伙伴表示对 “唯一可信源” 的概念有点懵逼，实际上 “唯一可信源” 的设计在生活中十分普遍，我举个公司群发邮件通知的案例：

> HR 群发邮件通知 “**这周三** 集体踏春，为期一天”。收到消息后大家议论纷纷，有抱怨 “为什么不是周五出游、这样刚好就和周末连在一起”，也有抱怨 “为什么不是 2 天，1 天多没劲”。总之大家你一句、我一句，消息一下传开了，但如果没有看过原始邮件的话，你都不知道 **哪个说法才是最初且可靠的**。
> 
> 然而昨天你和 HR 小丽吃饭的时候，她预先透露了，“公司 **这周五** 会踏春”，怎么今天就改为周三呢？如果是笔误，那么此时你有两个选择：
> 
> 一个是自己站在人群中央，大吼 “听我的，都听我的，这个肯定是笔误，昨天我得到的内部消息是周五”，
> 
> 另一个是私下与小丽沟通，如果是笔误，**还请 HR 统一再群发一次邮件** 来澄清此事。
> 
> 哪种做法可信度最高呢？当然是后者，因为前者的话，没有参与过公司高层的决策本身，**对事情的背景一无所知，可信度一般**；后者是公司代表，如果你想统一改变大家收到的通知，最有效的办法就是和这个公司代表沟通，由他来群发邮件。

于是关于这类事情，我们提炼了三个关键：

> 1.每个人都长着耳朵和嘴巴，好比每个人天生就具备发送和接收 EventBus 消息的能力，只不过明智的人懂得不 “道听途说”。
> 
> 2.人事部（唯一可信源）的关键在于，**他是独立于 “技术部” 的存在，拥有技术部员工（每个页面）所没有的权限**：他被允许编辑和群发邮件，而员工的权限仅限于咨询（发送请求）和接收邮件通知（**只读的、一手的信息**）。
> 
> 后续员工基于 “只读数据” 在脑海里再加工、形成他们自己的 “观点和立场”（“可变状态”）再拿出来相互传播，这些 “观点和立场” 的可信度都大大降低了。
> 
> 3.所以每当你想要获取 **最新可靠消息**，或 **统一影响** 其他同事（页面）的观点（“可变状态”），最稳妥的办法就是 当即与人事部（唯一可信源）沟通，由他来内部决策和 **统一群发一轮新的 “只读数据”**。

以下是代码示例。（具体可参见[《最佳实践》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)项目源码中 SharedViewModel 或 Request 的设计）。

![](https://images.xiaozhuanlan.com/photo/2020/14b3791fe4f502279b2dfb50fa896503.jpeg)

> 回到正轨，我们接着往下讲

### 特性 2：解决生命周期管理的一致性 和 生命周期安全

**再一个特点就是，在 Lifecycle 的帮助下，实现 “生命周期管理的一致性”，和 “生命周期安全”。**

生命周期安全，啥意思呢？注意观察 observe 方法，第一个参数就是 LifecycleOwner。

这么设计的目的是，让该 LifecycleOwner（例如当前 Fragment 实例）仅在 onResume 等生命周期节点内，能够接收来自该 LiveData 的数据分发，从而 **有效避免了当 当前页面已被 Destory，页面的视图仍在 observe 回调中被调用从而空指针异常。同时又不影响被多个 Fragment 共享的 ViewModel 所持有的该 LiveData 对其他 Fragment 的分发。**

同时，这里的 “生命周期安全”，实际上也依赖于 “生命周期管理的一致性” —— 自动地 注册或解绑 对事件源的监听。无需开发者手动编写，从而避免了因疏忽而造成的不必要的错误。

![](https://images.xiaozhuanlan.com/photo/2021/0df16ba25e726e739f3ad75a7c946655.png)

> Tip：Fragment owner 最新设计的变更及缘由，详见下文 **Note 2020.2.17 加餐**。

### 特性 3：实现单向依赖，规避内存泄漏

DataBinding 存在的目的是解决 “视图调用的一致性问题”，以及额外支持了 单向依赖、让 视图控制器 不被 ViewModel 所持有，从而规避了一系列的内存泄漏等隐患。

> 在 Jetpack 架构组件问世前的上一版 MVVM 架构中，ViewModel 并非 Jetpack ViewModel 这般，可以让生命周期跟随 Activity 级 Owner 而独立于 Fragment，所以这个时期的 单向依赖 几乎是靠 DataBinding 支持的。

而现在，就算不用 DataBinding，我们也能因为 ViewModel 和 LiveData 的配合，而达成 UI -> ViewModel -> Data。

这里我再次准备了一份直观的 UI -> ViewModel -> Data 关系图，**从请求，到响应，4 个步骤**，完成 **从唯一可信源到多个订阅者 —— 一对多的数据分发** —— 无论发起状态请求的是哪个页面。

![](https://images.xiaozhuanlan.com/photo/2021/beb402ea9b234ed7a3d43906d72ebbec.png)

## 综上

LiveData 的目标有且只有三个：

> 1.在 Lifecycle 的帮助下，规避订阅者回调的生命周期安全问题。
> 
> 2.遵循 “唯一可信源” 理念的约束，也即通过 “**决策逻辑内聚**” 和 “**读写分离**” 来规避 “跨域消息同步” 等场景下 **高频** 存在的可靠一致问题、使团队新手也能 不假思索写出低风险代码。
> 
> 3.就算不用 DataBinding，也能使 “单向依赖” 成为可能、规避潜在的内存泄漏等问题。

相信经过上述背景缘由的铺垫 ，你对 LiveData 的存在本质有了更深的理解，

原本让人找不到头绪的源码，相信也有了清晰的方向阅读。

## Note 2020.10.14 加餐：

### 对 “唯一可信源” 概念本质的补充说明

> **“唯一可信源” 的概念主要是针对 “页面间状态同步” 等场景**。
> 
> 在 Jetpack MVVM 中，唯一可信源主要指 **表现层或领域层中 LiveData 子类的直接持有者**，例如 ViewModel 等。
> 
> LiveData 的 setValue 和 postValue 默认被设计为 protected，也即对于 “状态同步请求” 等场景，允许开发者在开发过程中 “限制 ViewModel/Request 对 Activity/Fragment 暴露的是 **访问权限受限的父类 LiveData**，而不是作为子类的 MultableLiveData”，
> 
> 如此来做到，**数据对页面只读、且来源是统一的**，Activity/Fragment 只能基于拿到的数据去赋值给 ViewModel 中与视图绑定的可变状态，也即这些可变的状态，仅限于页面内部使用、**只能作为 “被通知方” 被改变**，对于源头的只读数据，它们无权篡改，如需要新的数据，只能 **单方面** 地向可信源发起请求，然后由可信源 **单方面** 地、统一地分发结果，如此通过实现 “读写分离” 来实现 “决策逻辑内聚” 从而实现多页面消息同步的可靠一致。
> 
> 具体可参考[《最佳实践》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)源码在 ViewModel 或 Request 中对 `getLiveData()` 的使用。

## Note 2020.2.17 加餐：

### Fragment owner 最新设计的变更及缘由

> 2020 年起，Fragment 中 LiveData 传入的 LifeCycleOwner 从 `fragment.this` 改进为 `getViewLifeCycleOwner`。
> 
> 如此设计 主要是为了解决 getView() 的生命长度 比 fragment 短（仅存活于 onCreateView 之后和 onDestroyView 之前），导致某些时候 fragment 其他成员还活着，但 getView() 为 null 的 生命周期安全问题，
> 
> 也即，**在 Fragment 的场景下，请使用 getViewLifeCycleOwner 来作为 liveData 的订阅者**。（并且注意，对 getViewLifeCycleOwner 的使用应在 onCreateView 之后和 onDestroyView 之前）
> 
> **Activity 则不用改变**。（关于 Fragment 和 Activity 生命周期的差别 及 **“为何设计出这些差别”**，详见[《我的碎片很听话，你的 Fragment 有自己的想法》](https://xiaozhuanlan.com/topic/0937256481)篇 “Fragment 生命周期为何如此设计” 一节的解析）

## Note 2020.4.11 加餐：

### 注意不要重复注册 LiveData

> 考虑到此前有多位小伙伴私下询问过 LiveData “重复回调”的问题，这里额外做个明示：
> 
> LiveData 内部维护了 Map 来管理 Observers，因而正常情况下，对于 **一个 LiveData 实例，在同一个页面中只该注册一次观察**、请勿在 RecyclerView Adapter 的 onBindViewHolder 等处注册，避免导致额外注册多个观察者，从而不可预期地在每次请求后 “收到多次推送”。
> 
> Tip：点击项目中 LiveData 的 observe 方法即可看到真相。  
> 观察者的添加主要发生在 `mObservers.putIfAbsent(observer, wrapper)` 这行；不是最底下用于解决 生命周期安全 的 `owner.getLifecycle().addObserver(wrapper)`。

## Note 2021.8.16 加餐：

### 关于 “观察者模式” 和 “发布订阅模式” 的区别

感谢小伙伴 [@Lιϙυιԃ](https://xiaozhuanlan.com/u/2067721379) 的私下讨论和确认，最终我们确认了，“观察者模式” 和 “发布订阅模式” 的本质区别 **并非** 之前我在 “2020.4.11 加餐” 中描述的「取决于 “发送者” 与 “观察者” 之间是 “一对一” 还是 “一对多” 的关系」，为此对 “2020.4.11 加餐” 做个更正和简化（你现在看到的已是更正且简化后的结果）

作为补充，此处我分享一下目前的理解，

所谓 “观察者模式”，即 “事件发送者” 和 “观察者” 之间直接衔接，没有任何鉴权或阻拦的缓冲设施，也即 “**上游发送通知，下游随即被迫营业**”，

而 “发布订阅模式” 与之的核心区别就在于 **引入了 “统一鉴权中心”**，也即 “上游发通知，下游营不营业、谁来营业、营什么业，都鉴权中心说了算，下游不会直接被迫营业”，

某种程度上，我们在正文中介绍的 “唯一可信源” 理念，就是 “发布订阅模式” —— SharedViewModel 是 “统一鉴权中心”，页面可以通过 SharedViewModel 的 request 方法来向其发送请求，由其内部决策结果并统一分发，而非页面自己直接向观察者推 “自认为对” 的结果。

## Note 2021.01.12 加餐：

### 匿名内部类有个 “重复订阅” 的坑需要注意

上述 **《加餐：注意不要重复注册 LiveData》** 中提到了 LiveData “多次订阅” 的情况，

这里的 “重复注册多个订阅者” 主要发生于 “使用匿名内部类而非 lambda” 的情况：当使用 lambda 时，基于 LiveData observe 方法内部的判断，在同一个页面或 adapter 内订阅不会发生重复订阅，但如果是匿名内部类，每次都会被认为是新的不同的实例，从而额外增加了一个新的订阅者。

> 关于此类现象的复现，可详见我在评论区 64 楼提供的示例代码。

## Note 2021.04.12 加餐：

### 对 “Note 2021.01.12 加餐” 的进一步说明

感谢 [@12ug3r](https://xiaozhuanlan.com/u/5558256758) [@永远年轻](https://xiaozhuanlan.com/u/%E6%B0%B8%E8%BF%9C%E5%B9%B4%E8%BD%BB) [@言和](https://xiaozhuanlan.com/u/7813603838) [@ZhaoGq](https://xiaozhuanlan.com/u/ZhaoGq) 等小伙伴 的讨论和不懈求证，具体而言，当在循环中入参 lambda，且 lambda 中未调用外部变量时，出于性能优化等因素的考虑，编译器会将闭包的内容统一抽出为同一个单例供多个方法入参用，

这也就导致了，当我们在 RecyclerView 等场景下（例如 RecyclerView 的 onLayoutChildren 环节在 fill 方法中存在循环体）为 LiveData.observe() 方法入参 lambda 且闭包内未调用外部变量时，会产生 “并不重复订阅” 的假象，但由于很难预测你的同事是否会使用 lambda，于是在后续的开发中，如使用入参 new Observer()，则会导致预期外的 “订阅了多个 Observe 实例” 的情况，

new Observer() 会重复订阅

lambda 在循环体等场景下可能被优化

![](https://images.xiaozhuanlan.com/photo/2021/e2df9e48ac2a99340eae4dad703a149d.jpg)

![](https://images.xiaozhuanlan.com/photo/2021/849079808e3b0da07aae1ebf11eb3552.jpg)

因而我们在此声明，需要特别注意 LiveData 在 “循环体” 场景下使用 liveData.observe() 可能存在的不符合业务目标的预期的坑。

（下图来自小伙伴 [@12ug3r](https://xiaozhuanlan.com/u/5558256758) 提供的求证截图，实验的背景是 Java 8 语境）

![](https://images.xiaozhuanlan.com/photo/2021/fb2010cfdbdb947f2660c21b91ff2aca.jpg)

![](https://images.xiaozhuanlan.com/photo/2021/7e980ddf309a19b08bb30a1724451b76.jpeg)

## Note 2020.7.15 加餐：

### LiveData 数据倒灌 背景缘由全貌 独家解析

> 注：本次加餐属于实战经验的补充，以[《LiveData》篇](https://xiaozhuanlan.com/topic/0168753249)、[《ViewModel》篇](https://xiaozhuanlan.com/topic/6257931840)、[《Jetpack MVVM 精讲》篇](https://xiaozhuanlan.com/topic/0129483567) 以及[《Jetpack MVVM Best Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice) 作为前置知识，假设小伙伴已完整查阅并吸收了上述内容。

实战中绕不开的高频问题：

> -   为什么 LiveData 默认被设计为粘性事件？
>     
> -   为什么 官方文档 推荐使用 SharedViewModel + LiveData（文档没明说，但事实上包含三个关键的背景缘由）？
>     
> -   乃至为什么存在 “数据倒灌” 的现象？
>     
> -   以及为什么在 “页面通信” 的场景下，不用静态单例、不用 LiveDataBus？
>     
> -   外网、美团、官方 demo 给出的 **Event 事件包装器、反射方式、SingleLiveEvent** 的解决方案，应用到实际项目中 分别存在怎样的缺陷？
>     
> -   如何以优雅的方式完美解决上述需求？
>     

对此可前往[《LiveData 数据倒灌 背景缘由全貌 独家解析》](https://xiaozhuanlan.com/topic/6719328450)查阅，此处不作累述。

## Note 2020.09.14 加餐：

### 对 “LiveData 不宜用在 Repository” 的解读

看到英文文章[《Don’t use LiveData in Repositories》](https://proandroiddev.com/dont-use-livedata-in-repositories-f3bebe502ed3)中 “LiveData 不应在 Repository 中使用” 的观点 “人传人”，这里简要分享下我的看法。

> 事实上，文中提及的核心问题在于：对 Transformation 等过度设计的类的使用，导致 LiveData 的 Observe 回调在接触 Activity/Fragment 之前被调用，从而在回调中因执行耗时逻辑而导致主线程阻塞。

因而，问题的关键在于 Transformation 这些过度设计的类，也即，**“不是 LiveData 本身应该出现在哪，而是 LiveData 的 Observe 回调应该出现在哪”**。

所以对此问题妥当的描述应是：“建议通过 Kotlin Flow 或 RxJava 来取代 Transformation 等过度设计的类”、“建议 LiveData 的 Observe 回调 **只用于最终的 Activity/Fragment**”，而不是 “LiveData 不应在 Repository 中使用”。

事实上如果你基于前文的深度思考 **认识并抓住技术的本质**，而不曾使用那些过度设计的类，那么在 Repository 中通过 ViewModel 注入的 LiveData 来回推数据完全没问题。

此外，[《最佳实践》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)项目又为何将 LiveData 移出 Repository 呢？主要是出于不同架构场景下的 “代码复用” 的考虑，具体缘由可详见[《如何让同事爱上架构模式、少写 bug 多注释？》](https://xiaozhuanlan.com/topic/8204519736)篇对 DataResult 的解析。

## Note 2020.11.08 加餐：

### 对 EventBus 适用场景的解析

抛开实际的背景谈论框架 “好不好用”，都是耍流氓。因而关于 EventBus 的使用，我们仍旧是结合实际背景来理解。

正如本文揭露的实际开发背景：多人协作的软件工程。在这样的背景下，团队成员的思维和能力参差不齐，不是谁都经历过、有理解有体会、有意琢磨且有能力做好 “订阅者管理”、**使跨域消息的同步性和一致性问题被彻底规避**。

唯一可信源开发理念的存在，是专用于订阅者消息同步一致性问题的解决方案，而 LiveData 正是在上述 “多人协作” 的背景下，基于唯一可信源理念的设计，透过 “先天的约束”（也即 通过访问控制权限 来控制消息只能单向发送且对订阅者只读，具体详见上文 **Note 2020.10.14 加餐**），使得就算对事情背景一无所知的新手，也能不假思索地写出低风险代码。

换言之，如果你本身已具备了 “唯一可信源” 的开发思想，那么在独立开发的个人项目中，完全可以根据实际场景（例如 “实体按键交互” 等非页面开发的场景）选用合适的框架，无论它是 LiveData、EventBus 还是 RxBus，

而如果是在团队项目中，为了规避 “订阅者管理” 这种 **高频且棘手的隐患**，应及时设立规范，杜绝 EventBus 的引入，以尽可能从源头上规避不可预期的错误。

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