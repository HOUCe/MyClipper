---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/6257931840
author: 
---

# [重学安卓：在页面开发中 左右逢源的 Jetpack ViewModel － 小专栏](https://xiaozhuanlan.com/topic/6257931840)


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
> 我们绝不通篇贴源码，而是基于广泛的实践和反思，在累积过大量样本 乃至足以排除掉所有干扰信息后，**点到为止地揭露 ViewModel 框架最为核心的本质**，方便您理解其真实的存在意义，乃至可以笃信地将其用在项目中。
> 
> 关于 ViewModel 在项目中的实践，请另行查阅[《GitHub : Jetpack-MVVM-Best-Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)源码以及[《架构模式实践》篇](https://xiaozhuanlan.com/topic/8204519736) 的完整解析。

## 前言

上一期，我们借着现实中对音乐播放器的开发，来帮助大家体会为何存在 LiveData 这样的设计 —— 它究竟是在怎样的背景下、为了解决什么样的问题而诞生。

相信阅读过上一期的朋友，对 LiveData 的存在缘由、**目标使命**、职责边界 已形成深刻的印象。

并且，就算将来不用 LiveData，也能因 LiveData 树立起的 **通过 “唯一可信源” 完成状态分发** 的理念，而减少 90% 不可预期的错误，从而让开发工作顺风顺水、畅快无比。

## 被严重低估的 Jetpack ViewModel

提到 ViewModel，大部分人都会觉得 “没啥可说的”。或许他们对 ViewModel 的认知，还停留在 MVVM - Clean 的时代，或者干脆误以为是 Presenter 般的存在。

这使得 ViewModel 的价值 就没有真正地 被重视和利用起来。

Jetpack ViewModel 果真是某些人所说的，是 MVP Presenter 的阉割版，或作为 Clean ViewModel 的存在吗？

答案是否定的。

作为在 **面向成熟的、标准化、规范化的架构设计** 的背景下诞生的 Jetpack ViewModel，绝不可与 Presenter 或是 Clean ViewModel 同日而语。

Jetpack ViewModel 在实现状态管理框架的 **单向依赖** 理念中，有着不可替代的作用。

所以长话短说，让我们来一起领略一下，在 Jetpack ViewModel（下文皆以 “ViewModel” 来指代）问世前和问世后，软件开发发生了怎样的剧变。

## 文章目录一览

-   前言
-   被严重低估的 Jetpack ViewModel
-   ViewModel 的目标只有三个
-   ViewModel 问世前的混沌世界
-   ViewModel 为何能解决这三个问题？
    -   **Note 2020.8.9 加餐：**
    -   对作用域机制本质描述的简化重制
-   引入 ViewModel 后的世界
-   **Note 2021.07.18 加餐：**
    -   对 “**状态管理一致性问题**” 的解析及示例补充
-   **Note 2020.10.28 加餐：**
    -   解析 ViewModel 在实际开发中 “两大高频场景” 下的使用
-   需要明确的 3 个小细节
    -   **Note 2020.2.10 加餐：**
    -   对 SavedStated 被单独抽取维护的缘由的解析
-   **Note 2021.2.4 加餐：**
    -   对 ViewModel 潜在的内存泄漏的解析
-   **Note 2021.07.20 加餐：**
    -   ViewModel 业务能力外的场景及应对方案
-   综上

## ViewModel 的目标只有三个

ViewModel 最早是在 2017 年的 Google I/O 大会上被提出，同时间问世的还有 Lifecycle 和 LiveData。

在谈论 ViewModel 时常常避不开 LiveData。LiveData 就像 ViewModel 的左臂右膀，支撑 ViewModel 作为中间层 对状态管理所尽的“承上启下”的职责。

所以尽管本文的目标是介绍 ViewModel 存在缘由、设计依据、职责边界，本着介绍相互间关系时的需要，还是会顺带展示 LiveData 的身影。

**ViewModel 的目标只有三个：**

> 1.让状态管理独立于视图控制器，从而实现 “**状态管理的分治**”、实现 “**状态管理的一致性**” 和 “**状态共享**”（跨页面通信）。
> 
> 2.为状态设置作用域，使状态的共享做到作用域可控。
> 
> 3.实现单向依赖，避免内存泄漏。

如果光是阅读了以上三点，你还是不太理解的话，那接下来我就分别介绍 99% 网文都不曾介绍的真实状况，来方便你迅速建立起感性的认识。

## ViewModel 问世前的混沌世界

例如我们开发了一款单 Activity 的地图应用。应用的需求是：

> 1.地图作为背景供全局查看和操作。
> 
> 2.当前页面皆以 Fragment 的形式展示和管理。
> 
> 　　2.1.其中 ListFragment（列表页）持有一级图形工具的单例 ListGraphicManager，可以 对地图上的图斑进行选中 和 跳转到详情页。
> 
> 　　2.2.List 中的每个 item 可跳转到 DetailFragment（详情页），详情页持有二级图形工具的单例 DetailGraphicManager，可以对地图上的图斑进行选中和编辑轮廓。并且在退出详情页时重置二级图形工具中的状态数据。

值得注意的一点是，对图斑的操作导致的图斑的变化，需要及时反映在 Fragment 的展示中。因而此前不仅是 Fragment 去引用 GraphicManager，GraphicManager 也反过来持有了 Fragment 的引用。

这造成了什么问题呢？

**首先**，由于图形工具的生命周期比 Fragment 长，当 Fragment destroy 时，因被图形工具持有，而可能造成内存泄漏。

**并且**，列表页和详情页之间如果要通信，只能另外编写接口，通过 Activity 来中转，这样项目中便产生了大量的接口。

如果不用接口，而是用 EventBus 的话，就容易导致上一节我们提到的，因 **缺失 唯一可信源理念的 约束** 造成状态分发的滥用，而指数级地增长不可预期的错误。

**再者**，如果页面彼此间不能共享数据，那么就需要编写大量的重复代码，例如 ListPresenter 和 DetailPresenter 包含某些同样的请求和逻辑，却因隶属于不同的页面，而不得不将同样的参数注入两份，并额外增加了状态管理的负担（例如页面退出时，你需要手动地一个个将状态对象置空，不然有内存泄漏的风险）。

**最后**，在屏幕旋转并发生重建后，Presenter 因直接隶属于 Activity，而同样被销毁，起不到存储大数据供视图控制器恢复状态的作用，状态恢复完全靠视图控制器的重建机制支撑着。

而该重建机制的设计目标本是支持轻量级的状态的存储与恢复，重量级的势必影响效率，而可能发生主线程阻塞 从而造成旋屏时的卡顿。

## ViewModel 为何能解决这三大问题？

在 ViewModel 问世前，我自主设计过 现如今在我 GitHub 主页展示的 VIABUS 架构的雏形。

VIABUS 的设计目标之一，就是让 页面 和 业务 完全分离，使得 业务 能够完全独立于特定页面的生命周期，单独地存活和工作。并且业务能够为多个页面共享，无需每个页面都单独创建一个业务实例。

VIABUS 业务的职责更像是 Jetpack 中的 DataRepository，负责后端逻辑，因而 VIABUS 的缺陷是少了作为中间层的状态管理。

![](https://images.xiaozhuanlan.com/photo/2019/ea5565aa5d37afedcdf09ca076eafb76.png)

与 VIABUS 表面看上去有点类似，实际上又有所不同的是，ViewModel 的职责是专用于状态管理。

例如在我们开发的单 Activity 软件中，Activity 的生命周期是视图控制器中最长的，所有的 Fragment 都不如它长命。

如果我们将 Activity 作为页面的容器，将 Fragment 作为实际的页面，那么对状态的需求存在以下两种情况：

> 1.有些状态是跨页面共享的，也即退出某个页面时，状态不会跟随该页面的 destory 而被清空。
> 
> 2.另有些状态是私有的，那么当退出该页面时，状态跟随页面的 destory 而被清空。

—— 这使得 ViewModel 实例的管理，绝非 VIABUS 那么简单粗暴 —— 直接在 BUS 单例中通过 Map 来管理多个业务实例来搞定。它 **有作用域可控的需求**。

那怎么实现在状态可共享的情况下，还做到作用域可控呢？

Google 是这么设计的：

**首先，ViewModel 的职责是专门管理状态**，尤其是那些 “重新获取的代价比较大” 的状态（例如通过网络请求到的 List 等数据，抛开 “费流量” 不说，https 请求本身涉及大量的加解密运算，耗费 CPU 资源，耗电显著）。

**并且，ViewModel 归根结底是被对应的 Activity/Fragment 持有**。只不过 ViewModel 和 Lifecycle 一样，通过模版方法模式封装得太好了，乃至于开发者感觉不到 它实际上与视图控制器的关系十分紧密，并不是表面看上去的 “那么独立” 和 “逍遥法外”。

> 👆👆👆 划重点

其中，Activity 和 Fragment，是作为 ViewModelStoreOwner 的实现，每个 ViewModelStoreOwner 持有着唯一的一个 ViewModelStore，也即 **每个视图控制器都持有自己的一个 ViewModelStore**。

**ViewModelStore 的作用是维护一个 Map，来管理视图控制器所持有的所有 ViewModel**。

所以三者的关系是，ViewModelStoreOwner 持有 ViewModelStore 持有 ViewModel。

并且 **当作为 ViewModelStoreOwner 的视图控制器被 destory 时（重建的情况除外），ViewModelStore 会被通知去完成清除状态操作**，从而将 Map 中管理着的 ViewModel 全部走一遍 clear 方法，并且清空 Map。

（clear 方法是 final 级，不可修改，clear 方法中包含 onClear 钩子，开发者可重写 onClear 方法来自定义数据的清空）

> Note 2020.8.9 加餐：**对作用域机制本质描述的简化重制**
> 
> 简化前的原文 见评论区 58 楼的备份，以下替换为简化后的内容 ——

那么，在有了上述 ViewModelStoreOwner - ViewModelStore - Map - ViewModel 设计的 **兜底** 后，如何做到 “状态共享以及作用域可控” 呢？

很简单 —— **通过 “工厂模式” 来搞定**：

在 Fragment 页面中创建 ViewModelProvider 实例，并在入参中注入 ViewModelStoreOwner，参照上述的兜底设计，我们可知，此处即 **通过 ViewModelStoreOwner 来反映作用域**：

譬如上述新建的 ViewModelProvider 传入的 ViewModelStoreOwner 的是 getActivity() ，那么它实际上是透过 “该 Activity 持有的 ViewModelStore 持有的 Map” 来 put 或 get ViewModel —— 如果 Map 中已包含目标 ViewModel 实例，那么当页面获取到该 ViewModel 实例，便达成了与该 Activity 或隶属于该 Activity 的其他 Fragment 共享 ViewModel 实例的目的。

同理，如果传入的 ViewModelStoreOwner 的是 Fragment.this，那么它的作用域仅限于本 Fragment，并且当 Fragment 离场时，该 ViewModel 实例也会连同 Fragment 一起被销毁。

换言之，**同一个 ViewModel 子类，基于不同的作用域，获取到的实例并非同一个**。根据传入的 ViewModelStoreOwner 我们便能拿到符合实际场景所需的 ViewModel 实例。

**这就是 ViewModelProvider 这样设计的精妙之所在！**

> 👆👆👆 这一段全是重点，ViewModel 设计的精华之所在。
> 
> 在实战中，对于状态托管的场景，我们通常将作用域设定为 “仅限本页面”，而对于 “页面返场回调” 等 “页面间通信” 的场景，则使用作用域大于本页面的方式（如 Activity、Application 级作用域），具体可详见[《最佳实践》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)源码中 SharedViewModel 的使用、DataBindingFragment 的封装，以及[《LiveData 数据倒灌 背景缘由全貌 独家解析》](https://xiaozhuanlan.com/topic/6719328450) 的解析。

## 引入 ViewModel 后的世界

回到我们最初的地图软件开发需求：

> 1.在列表页中操作地图时，点击地图图斑是跳转到相应的详情页。
> 
> 2.在详情页中操作地图时，点击地图图斑：
> 
> 　　2.1.如果当前为查看模式，就关闭此详情页，并继续跳转到图斑对应的详情页。
> 
> 　　2.2.如果当前为编辑模式，则将选中图斑的轮廓绘成虚线，并进行编辑。
> 
> 　　2.3.在编辑的过程中，图形状态要实时反映到详情页。
> 
> 3.在详情页中保存编辑状态后，列表页要及时更新结果。
> 
> 4.退出详情页时清除临时状态。

因而，我们可以通过 ViewModel 而不是纯粹的单例，来实现作用域可控的状态的管理。

**首先**，我们可以定义一个 GraphicViewModel，它的作用域是全局共享，可供列表页和详情页的查看模式使用。因此我们在页面中引用时，为 ViewModelProvider 传入 Activity。

![](https://images.xiaozhuanlan.com/photo/2020/e81b9ab770afd931fe3538eea19ebcdf.png)

如此一来，就算详情页 destory，该 ViewModel 的状态仍保留，继续为列表页等其他页面服务。

**然后**，我们定义一个 EditViewModel，它的作用域是详情页，仅供详情页编辑模式使用。因此我们在页面中引用时，为 ViewModelProviders 传入 DetailFragment.this。

![](https://images.xiaozhuanlan.com/photo/2020/8dfba94b49ea040c80238667e5c47429.png)

如此，该 ViewModel 生命周期完全跟随详情页，当详情页被 destory 时，该 ViewModel 会被 clear。

—— 是的，原本复杂的、脆弱的、需要人为控制的载入和清理操作，因为在统一在后台被模版方法模式封装好了，乃至于事情变得如此简单。

**再者**，详情页在编辑时，结果要实时反映到详情页，且编辑若是被保存了，那么结果也要及时反映到列表页上。

注意：这里我们不再通过（滥用）EventBus 直接从图形工具的单例中通知详情页，或从详情页通知列表页做出变化，取而代之的是，用 ViewModel 来管理图形状态，且遵循唯一可信源 —— 让 ViewModel 通过 LiveData 这个唯一可信源来完成状态分发。

于是，当图形状态变化时，

![](https://images.xiaozhuanlan.com/photo/2020/81f99cb284e39d57eb9df45a6a05d925.png)

![](https://images.xiaozhuanlan.com/photo/2020/ea72e31598569cf699ee175ef82a2474.png)

并且，当详情页保存编辑时，

![](https://images.xiaozhuanlan.com/photo/2020/c0483c493ce81745be85cb4c91023d30.png)

![](https://images.xiaozhuanlan.com/photo/2020/d4b78663be549c995c0f676510ed9f49.png)

**此外**，由于状态都存放在 LiveData 中，交给 ViewModel 来托管，那么在下一次视图控制器被重建时，可以直接从 ViewModel 中 get LiveData 的最后一次的 value，而不用像我们在《[绝不丢失状态的 Activity 重建机制](https://xiaozhuanlan.com/topic/7692814530)》中介绍的那样，手动保存成员变量，然后又手动恢复，从而做到 **状态管理的分治**，尽可能避免了线程阻塞造成的卡顿。

> 所谓分治，就是分开处理：让重建机制自动处理 View 轻量状态的保存和恢复，让 ViewModel 托管像 List 这样的重量状态，或通过网络请求得到的数据，从而无需像以前一样，自己在 onSaveInstanceState 和 onRestoreInstanceState 中手动保存和恢复成员变量（需要序列化，耗费一定性能的），更无须重新网络请求、耗费更多的电量。

## Note 2021.07.18 加餐：

### 对 “状态管理一致性问题” 的解析及示例补充

上文我们提到了，ViewModel 能托管页面的状态，实现 “状态管理的分治”，实际上这里面还有个细节，就是旧时候我们保存恢复状态，往往需要额外为状态定义一个常量、并且分别在 onSaveInstanceState 和 onRestoreInstanceState 方法中手动书写保存和恢复的语句，

如此一来就埋下了一致性隐患，毕竟页面往往有数十个，每个页面需要保存恢复的状态也有数个到数十个不等，总会有疏忽。

![](https://images.xiaozhuanlan.com/photo/2021/a1d5bab506d15607e4daf58ce09d420c.png)

## Note 2020.10.28 加餐：

### 解析 ViewModel 在实际开发中 “两大高频场景” 下的使用

在《最佳实践》项目中，我们分别展示了 state-ViewModel 和 event-ViewModel 在两大高频场景（状态托管 和 跨页面通信）下的使用。

现如今我们已在项目中全面确立了 这两种使用方式和对实例的命名，以方便语义上的理解和区分。

![](https://images.xiaozhuanlan.com/photo/2021/9f36f25836da65508e086c6141772f9e.jpeg)

关于这两种设计的存在缘由 和 在实战中的使用，可详见[《如何让同事爱上架构模式、少写 bug 多注释》](https://xiaozhuanlan.com/topic/8204519736)篇的解析，此处不作累述。

## 需要明确的 3 个小细节

**1.传入不同的作用域，拿到的是毫不相干的两个 ViewModel 实例。**

在 “为何能解决这三大问题” 一节中，我们已经解析了 ViewModel 的作用域实际上就是取决于 在 ViewModelProvider 传入的、持有该 ViewModel 所在的 ViewModelStore 的 ViewModelStoreOwner。

换言之，如果被引用的 ViewModel 来自不同的 ViewModelStoreOwner，它俩实际上是两个 ViewModel 实例，而不是同一个，所以所持有的 LiveData 也不是同一个。

因而在页面中，通过 ViewModel 实例拿到 LiveData 来 observe 时，要注意区分该 ViewModel 是不是你预期的那个 ViewModel，否则该 observe 可能收不到你从别处分发来的状态。

**2.在状态重建时，Activity 持有的 ViewModelStore 不会被销毁。**

细心的朋友可能会有疑问，ViewModel 既然是被视图控制器持有，那在视图控制器被重建时，它为何不会被一同销毁呢？

因为视图控制器中存在某个叫**“当环境变化时保留状态”**的机制（onRetainNonConfigurationInstance，详见 ComponentActivity 源码），使得当重建时，ViewModelStore 被保留，而在重建后，恢复了 Activity 对该 ViewModelStore 的持有。

并且，对于重建的情况，还会走 isChangingConfigurations 的判断，如果为 true，就不走 ViewModelStore 的 clear。

相当于是对 ViewModel 实例进行了 **双重保护**。

> 对于 Fragment 的私有域 ViewModel，这里按住不表，因为我写这篇文章时（2019.8.15），和我 2 个月前看到的源码 **有很大的出入**。目前 Fragment 的私有域 ViewModel 似乎被改为不受保护了，暂未想明白这么设计的缘由是为何、将来是否依然如此。
> 
> 这里我提供一下相关的线索：
> 
> Fragment 类中的 setRetainInstance 方法，
> 
> FragmentManagerImpl 类中 moveToState 方法中的 nonConfig.clearNonConfigState 方法，

大家可以自行去感受一下（AndroidX 源码）

### Note：2020.2.10 加餐：真相大白：

#### 对 SavedStated 被单独抽取维护的缘由的解析

> AndroidX 的 Fragment、AppCompatActivity、ViewModel 源码已发生变化。
> 
> Fragment 的 Retain 已废弃，现如今 AndroidX 已单独引入 SaveState 包来解决 重建时 状态恢复的问题。
> 
> 此举是为了让 Fragment 和 Activity 的 public 生命周期设计趋近（例如未来 attach、detach 方法可能被 private 或 hide），以减少开发者的学习成本 和 源码设计师的维护成本。

**3.只要不作，就能避免因中间层持有页面而导致的内存泄漏**

最后，正由于 ViewModel 是个被精妙设计的 “单例”，使得视图控制器依赖它，它却不依赖于视图控制器，从而你只要不往 ViewModel 中塞视图控制器的 Context 等引用，就能避免一系列内存泄漏的隐患（例如当 Fragment 引用了 Activity 持有的 ViewModel 实例时）。

## 综上

**ViewModel 的目标有三个：**

> 1.让状态管理独立于视图控制器，从而实现 “**状态管理的分治**”、实现 “**状态管理的一致性**” 和 “**状态共享**”（跨页面通信）。
> 
> 2.为状态设置作用域，使状态的共享做到作用域可控。
> 
> 3.实现单向依赖，避免内存泄漏。

## Note 2021.2.4 加餐：

### 对 ViewModel 潜在的内存泄漏的解析

Jetpack MVVM 的 “单向依赖” 设计，目的就是通过让生命周期越短的依赖于越长的，来规避内存泄漏的问题，Activity -> viewModel -> Repository。

然而尽管如此，在目前的 AndroidX 系统源码中还是存在内存泄漏的设计（截至 2021.2.4，感谢小伙伴 @[hanshengjian](https://github.com/hanshengjian) 的分享）：

目前所知的关于 ViewModel 潜在的内存泄漏的情况，主要发生于 “**环境发生变化**” 等场景，例如 “旋转屏幕导致的重建”，这个过程中会产生新的 Activity（通过 Log 可发现重建后 Activity 的 hashCode 已发生改变），但上一任所持有的 ViewModelStore 对象会通过 “状态保存恢复机制” 一直延用到下一任身上，而导致上一任 Activity 回收不了。

这个问题不知道 Google 未来会不会解决，同时这也意味着 **天底下没有完美的工具，只有合适的工具，在引入一个工具来解决某问题时，通常伴随着引入别的副作用，因此可结合各方面因素来权衡利弊**（至少在软工安全的层面来说是利大于弊，因为配合 LiveData 规避了一系列生命周期安全和 null 安全问题，以及实现 更快、更省电、省流量的状态保存恢复）

## Note 2021.07.20 加餐：

### ViewModel 业务能力外的场景及应对方案

Jetpack ViewModel 的效用仅限于 “此刻”，也即当 App 还在前台时，为环境重建等问题提供状态的托管和恢复。

而对于 App 退居后台的情况，临时数据通常是会随着 App 进程被回收而一去不复返，

> 比如填表的场景：填表过程中你切换到系统相机 App，拍完照再切回来，发现 App 已被 “杀后台”，

因而对于这种情况的临时数据恢复，不要寄希望于 ViewModel，也不要寄希望于 onSaveInstanceState（以防国产 ROM 存在魔改），而是填表过程中便 **间歇性地从内存中将临时数据持久化存储**（通过 SharedPreference 等），那么下次启动 App 时便有机会检测和恢复。

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
> 我们绝不通篇贴源码，而是基于广泛的实践和反思，在累积过大量样本 乃至足以排除掉所有干扰信息后，**点到为止地揭露 ViewModel 框架最为核心的本质**，方便您理解其真实的存在意义，乃至可以笃信地将其用在项目中。
> 
> 关于 ViewModel 在项目中的实践，请另行查阅[《GitHub : Jetpack-MVVM-Best-Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)源码以及[《架构模式实践》篇](https://xiaozhuanlan.com/topic/8204519736) 的完整解析。

## 前言

上一期，我们借着现实中对音乐播放器的开发，来帮助大家体会为何存在 LiveData 这样的设计 —— 它究竟是在怎样的背景下、为了解决什么样的问题而诞生。

相信阅读过上一期的朋友，对 LiveData 的存在缘由、**目标使命**、职责边界 已形成深刻的印象。

并且，就算将来不用 LiveData，也能因 LiveData 树立起的 **通过 “唯一可信源” 完成状态分发** 的理念，而减少 90% 不可预期的错误，从而让开发工作顺风顺水、畅快无比。

## 被严重低估的 Jetpack ViewModel

提到 ViewModel，大部分人都会觉得 “没啥可说的”。或许他们对 ViewModel 的认知，还停留在 MVVM - Clean 的时代，或者干脆误以为是 Presenter 般的存在。

这使得 ViewModel 的价值 就没有真正地 被重视和利用起来。

Jetpack ViewModel 果真是某些人所说的，是 MVP Presenter 的阉割版，或作为 Clean ViewModel 的存在吗？

答案是否定的。

作为在 **面向成熟的、标准化、规范化的架构设计** 的背景下诞生的 Jetpack ViewModel，绝不可与 Presenter 或是 Clean ViewModel 同日而语。

Jetpack ViewModel 在实现状态管理框架的 **单向依赖** 理念中，有着不可替代的作用。

所以长话短说，让我们来一起领略一下，在 Jetpack ViewModel（下文皆以 “ViewModel” 来指代）问世前和问世后，软件开发发生了怎样的剧变。

## 文章目录一览

-   前言
-   被严重低估的 Jetpack ViewModel
-   ViewModel 的目标只有三个
-   ViewModel 问世前的混沌世界
-   ViewModel 为何能解决这三个问题？
    -   **Note 2020.8.9 加餐：**
    -   对作用域机制本质描述的简化重制
-   引入 ViewModel 后的世界
-   **Note 2021.07.18 加餐：**
    -   对 “**状态管理一致性问题**” 的解析及示例补充
-   **Note 2020.10.28 加餐：**
    -   解析 ViewModel 在实际开发中 “两大高频场景” 下的使用
-   需要明确的 3 个小细节
    -   **Note 2020.2.10 加餐：**
    -   对 SavedStated 被单独抽取维护的缘由的解析
-   **Note 2021.2.4 加餐：**
    -   对 ViewModel 潜在的内存泄漏的解析
-   **Note 2021.07.20 加餐：**
    -   ViewModel 业务能力外的场景及应对方案
-   综上

## ViewModel 的目标只有三个

ViewModel 最早是在 2017 年的 Google I/O 大会上被提出，同时间问世的还有 Lifecycle 和 LiveData。

在谈论 ViewModel 时常常避不开 LiveData。LiveData 就像 ViewModel 的左臂右膀，支撑 ViewModel 作为中间层 对状态管理所尽的“承上启下”的职责。

所以尽管本文的目标是介绍 ViewModel 存在缘由、设计依据、职责边界，本着介绍相互间关系时的需要，还是会顺带展示 LiveData 的身影。

**ViewModel 的目标只有三个：**

> 1.让状态管理独立于视图控制器，从而实现 “**状态管理的分治**”、实现 “**状态管理的一致性**” 和 “**状态共享**”（跨页面通信）。
> 
> 2.为状态设置作用域，使状态的共享做到作用域可控。
> 
> 3.实现单向依赖，避免内存泄漏。

如果光是阅读了以上三点，你还是不太理解的话，那接下来我就分别介绍 99% 网文都不曾介绍的真实状况，来方便你迅速建立起感性的认识。

## ViewModel 问世前的混沌世界

例如我们开发了一款单 Activity 的地图应用。应用的需求是：

> 1.地图作为背景供全局查看和操作。
> 
> 2.当前页面皆以 Fragment 的形式展示和管理。
> 
> 　　2.1.其中 ListFragment（列表页）持有一级图形工具的单例 ListGraphicManager，可以 对地图上的图斑进行选中 和 跳转到详情页。
> 
> 　　2.2.List 中的每个 item 可跳转到 DetailFragment（详情页），详情页持有二级图形工具的单例 DetailGraphicManager，可以对地图上的图斑进行选中和编辑轮廓。并且在退出详情页时重置二级图形工具中的状态数据。

值得注意的一点是，对图斑的操作导致的图斑的变化，需要及时反映在 Fragment 的展示中。因而此前不仅是 Fragment 去引用 GraphicManager，GraphicManager 也反过来持有了 Fragment 的引用。

这造成了什么问题呢？

**首先**，由于图形工具的生命周期比 Fragment 长，当 Fragment destroy 时，因被图形工具持有，而可能造成内存泄漏。

**并且**，列表页和详情页之间如果要通信，只能另外编写接口，通过 Activity 来中转，这样项目中便产生了大量的接口。

如果不用接口，而是用 EventBus 的话，就容易导致上一节我们提到的，因 **缺失 唯一可信源理念的 约束** 造成状态分发的滥用，而指数级地增长不可预期的错误。

**再者**，如果页面彼此间不能共享数据，那么就需要编写大量的重复代码，例如 ListPresenter 和 DetailPresenter 包含某些同样的请求和逻辑，却因隶属于不同的页面，而不得不将同样的参数注入两份，并额外增加了状态管理的负担（例如页面退出时，你需要手动地一个个将状态对象置空，不然有内存泄漏的风险）。

**最后**，在屏幕旋转并发生重建后，Presenter 因直接隶属于 Activity，而同样被销毁，起不到存储大数据供视图控制器恢复状态的作用，状态恢复完全靠视图控制器的重建机制支撑着。

而该重建机制的设计目标本是支持轻量级的状态的存储与恢复，重量级的势必影响效率，而可能发生主线程阻塞 从而造成旋屏时的卡顿。

## ViewModel 为何能解决这三大问题？

在 ViewModel 问世前，我自主设计过 现如今在我 GitHub 主页展示的 VIABUS 架构的雏形。

VIABUS 的设计目标之一，就是让 页面 和 业务 完全分离，使得 业务 能够完全独立于特定页面的生命周期，单独地存活和工作。并且业务能够为多个页面共享，无需每个页面都单独创建一个业务实例。

VIABUS 业务的职责更像是 Jetpack 中的 DataRepository，负责后端逻辑，因而 VIABUS 的缺陷是少了作为中间层的状态管理。

![](https://images.xiaozhuanlan.com/photo/2019/ea5565aa5d37afedcdf09ca076eafb76.png)

与 VIABUS 表面看上去有点类似，实际上又有所不同的是，ViewModel 的职责是专用于状态管理。

例如在我们开发的单 Activity 软件中，Activity 的生命周期是视图控制器中最长的，所有的 Fragment 都不如它长命。

如果我们将 Activity 作为页面的容器，将 Fragment 作为实际的页面，那么对状态的需求存在以下两种情况：

> 1.有些状态是跨页面共享的，也即退出某个页面时，状态不会跟随该页面的 destory 而被清空。
> 
> 2.另有些状态是私有的，那么当退出该页面时，状态跟随页面的 destory 而被清空。

—— 这使得 ViewModel 实例的管理，绝非 VIABUS 那么简单粗暴 —— 直接在 BUS 单例中通过 Map 来管理多个业务实例来搞定。它 **有作用域可控的需求**。

那怎么实现在状态可共享的情况下，还做到作用域可控呢？

Google 是这么设计的：

**首先，ViewModel 的职责是专门管理状态**，尤其是那些 “重新获取的代价比较大” 的状态（例如通过网络请求到的 List 等数据，抛开 “费流量” 不说，https 请求本身涉及大量的加解密运算，耗费 CPU 资源，耗电显著）。

**并且，ViewModel 归根结底是被对应的 Activity/Fragment 持有**。只不过 ViewModel 和 Lifecycle 一样，通过模版方法模式封装得太好了，乃至于开发者感觉不到 它实际上与视图控制器的关系十分紧密，并不是表面看上去的 “那么独立” 和 “逍遥法外”。

> 👆👆👆 划重点

其中，Activity 和 Fragment，是作为 ViewModelStoreOwner 的实现，每个 ViewModelStoreOwner 持有着唯一的一个 ViewModelStore，也即 **每个视图控制器都持有自己的一个 ViewModelStore**。

**ViewModelStore 的作用是维护一个 Map，来管理视图控制器所持有的所有 ViewModel**。

所以三者的关系是，ViewModelStoreOwner 持有 ViewModelStore 持有 ViewModel。

并且 **当作为 ViewModelStoreOwner 的视图控制器被 destory 时（重建的情况除外），ViewModelStore 会被通知去完成清除状态操作**，从而将 Map 中管理着的 ViewModel 全部走一遍 clear 方法，并且清空 Map。

（clear 方法是 final 级，不可修改，clear 方法中包含 onClear 钩子，开发者可重写 onClear 方法来自定义数据的清空）

> Note 2020.8.9 加餐：**对作用域机制本质描述的简化重制**
> 
> 简化前的原文 见评论区 58 楼的备份，以下替换为简化后的内容 ——

那么，在有了上述 ViewModelStoreOwner - ViewModelStore - Map - ViewModel 设计的 **兜底** 后，如何做到 “状态共享以及作用域可控” 呢？

很简单 —— **通过 “工厂模式” 来搞定**：

在 Fragment 页面中创建 ViewModelProvider 实例，并在入参中注入 ViewModelStoreOwner，参照上述的兜底设计，我们可知，此处即 **通过 ViewModelStoreOwner 来反映作用域**：

譬如上述新建的 ViewModelProvider 传入的 ViewModelStoreOwner 的是 getActivity() ，那么它实际上是透过 “该 Activity 持有的 ViewModelStore 持有的 Map” 来 put 或 get ViewModel —— 如果 Map 中已包含目标 ViewModel 实例，那么当页面获取到该 ViewModel 实例，便达成了与该 Activity 或隶属于该 Activity 的其他 Fragment 共享 ViewModel 实例的目的。

同理，如果传入的 ViewModelStoreOwner 的是 Fragment.this，那么它的作用域仅限于本 Fragment，并且当 Fragment 离场时，该 ViewModel 实例也会连同 Fragment 一起被销毁。

换言之，**同一个 ViewModel 子类，基于不同的作用域，获取到的实例并非同一个**。根据传入的 ViewModelStoreOwner 我们便能拿到符合实际场景所需的 ViewModel 实例。

**这就是 ViewModelProvider 这样设计的精妙之所在！**

> 👆👆👆 这一段全是重点，ViewModel 设计的精华之所在。
> 
> 在实战中，对于状态托管的场景，我们通常将作用域设定为 “仅限本页面”，而对于 “页面返场回调” 等 “页面间通信” 的场景，则使用作用域大于本页面的方式（如 Activity、Application 级作用域），具体可详见[《最佳实践》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)源码中 SharedViewModel 的使用、DataBindingFragment 的封装，以及[《LiveData 数据倒灌 背景缘由全貌 独家解析》](https://xiaozhuanlan.com/topic/6719328450) 的解析。

## 引入 ViewModel 后的世界

回到我们最初的地图软件开发需求：

> 1.在列表页中操作地图时，点击地图图斑是跳转到相应的详情页。
> 
> 2.在详情页中操作地图时，点击地图图斑：
> 
> 　　2.1.如果当前为查看模式，就关闭此详情页，并继续跳转到图斑对应的详情页。
> 
> 　　2.2.如果当前为编辑模式，则将选中图斑的轮廓绘成虚线，并进行编辑。
> 
> 　　2.3.在编辑的过程中，图形状态要实时反映到详情页。
> 
> 3.在详情页中保存编辑状态后，列表页要及时更新结果。
> 
> 4.退出详情页时清除临时状态。

因而，我们可以通过 ViewModel 而不是纯粹的单例，来实现作用域可控的状态的管理。

**首先**，我们可以定义一个 GraphicViewModel，它的作用域是全局共享，可供列表页和详情页的查看模式使用。因此我们在页面中引用时，为 ViewModelProvider 传入 Activity。

![](https://images.xiaozhuanlan.com/photo/2020/e81b9ab770afd931fe3538eea19ebcdf.png)

如此一来，就算详情页 destory，该 ViewModel 的状态仍保留，继续为列表页等其他页面服务。

**然后**，我们定义一个 EditViewModel，它的作用域是详情页，仅供详情页编辑模式使用。因此我们在页面中引用时，为 ViewModelProviders 传入 DetailFragment.this。

![](https://images.xiaozhuanlan.com/photo/2020/8dfba94b49ea040c80238667e5c47429.png)

如此，该 ViewModel 生命周期完全跟随详情页，当详情页被 destory 时，该 ViewModel 会被 clear。

—— 是的，原本复杂的、脆弱的、需要人为控制的载入和清理操作，因为在统一在后台被模版方法模式封装好了，乃至于事情变得如此简单。

**再者**，详情页在编辑时，结果要实时反映到详情页，且编辑若是被保存了，那么结果也要及时反映到列表页上。

注意：这里我们不再通过（滥用）EventBus 直接从图形工具的单例中通知详情页，或从详情页通知列表页做出变化，取而代之的是，用 ViewModel 来管理图形状态，且遵循唯一可信源 —— 让 ViewModel 通过 LiveData 这个唯一可信源来完成状态分发。

于是，当图形状态变化时，

![](https://images.xiaozhuanlan.com/photo/2020/81f99cb284e39d57eb9df45a6a05d925.png)

![](https://images.xiaozhuanlan.com/photo/2020/ea72e31598569cf699ee175ef82a2474.png)

并且，当详情页保存编辑时，

![](https://images.xiaozhuanlan.com/photo/2020/c0483c493ce81745be85cb4c91023d30.png)

![](https://images.xiaozhuanlan.com/photo/2020/d4b78663be549c995c0f676510ed9f49.png)

**此外**，由于状态都存放在 LiveData 中，交给 ViewModel 来托管，那么在下一次视图控制器被重建时，可以直接从 ViewModel 中 get LiveData 的最后一次的 value，而不用像我们在《[绝不丢失状态的 Activity 重建机制](https://xiaozhuanlan.com/topic/7692814530)》中介绍的那样，手动保存成员变量，然后又手动恢复，从而做到 **状态管理的分治**，尽可能避免了线程阻塞造成的卡顿。

> 所谓分治，就是分开处理：让重建机制自动处理 View 轻量状态的保存和恢复，让 ViewModel 托管像 List 这样的重量状态，或通过网络请求得到的数据，从而无需像以前一样，自己在 onSaveInstanceState 和 onRestoreInstanceState 中手动保存和恢复成员变量（需要序列化，耗费一定性能的），更无须重新网络请求、耗费更多的电量。

## Note 2021.07.18 加餐：

### 对 “状态管理一致性问题” 的解析及示例补充

上文我们提到了，ViewModel 能托管页面的状态，实现 “状态管理的分治”，实际上这里面还有个细节，就是旧时候我们保存恢复状态，往往需要额外为状态定义一个常量、并且分别在 onSaveInstanceState 和 onRestoreInstanceState 方法中手动书写保存和恢复的语句，

如此一来就埋下了一致性隐患，毕竟页面往往有数十个，每个页面需要保存恢复的状态也有数个到数十个不等，总会有疏忽。

![](https://images.xiaozhuanlan.com/photo/2021/a1d5bab506d15607e4daf58ce09d420c.png)

## Note 2020.10.28 加餐：

### 解析 ViewModel 在实际开发中 “两大高频场景” 下的使用

在《最佳实践》项目中，我们分别展示了 state-ViewModel 和 event-ViewModel 在两大高频场景（状态托管 和 跨页面通信）下的使用。

现如今我们已在项目中全面确立了 这两种使用方式和对实例的命名，以方便语义上的理解和区分。

![](https://images.xiaozhuanlan.com/photo/2021/9f36f25836da65508e086c6141772f9e.jpeg)

关于这两种设计的存在缘由 和 在实战中的使用，可详见[《如何让同事爱上架构模式、少写 bug 多注释》](https://xiaozhuanlan.com/topic/8204519736)篇的解析，此处不作累述。

## 需要明确的 3 个小细节

**1.传入不同的作用域，拿到的是毫不相干的两个 ViewModel 实例。**

在 “为何能解决这三大问题” 一节中，我们已经解析了 ViewModel 的作用域实际上就是取决于 在 ViewModelProvider 传入的、持有该 ViewModel 所在的 ViewModelStore 的 ViewModelStoreOwner。

换言之，如果被引用的 ViewModel 来自不同的 ViewModelStoreOwner，它俩实际上是两个 ViewModel 实例，而不是同一个，所以所持有的 LiveData 也不是同一个。

因而在页面中，通过 ViewModel 实例拿到 LiveData 来 observe 时，要注意区分该 ViewModel 是不是你预期的那个 ViewModel，否则该 observe 可能收不到你从别处分发来的状态。

**2.在状态重建时，Activity 持有的 ViewModelStore 不会被销毁。**

细心的朋友可能会有疑问，ViewModel 既然是被视图控制器持有，那在视图控制器被重建时，它为何不会被一同销毁呢？

因为视图控制器中存在某个叫**“当环境变化时保留状态”**的机制（onRetainNonConfigurationInstance，详见 ComponentActivity 源码），使得当重建时，ViewModelStore 被保留，而在重建后，恢复了 Activity 对该 ViewModelStore 的持有。

并且，对于重建的情况，还会走 isChangingConfigurations 的判断，如果为 true，就不走 ViewModelStore 的 clear。

相当于是对 ViewModel 实例进行了 **双重保护**。

> 对于 Fragment 的私有域 ViewModel，这里按住不表，因为我写这篇文章时（2019.8.15），和我 2 个月前看到的源码 **有很大的出入**。目前 Fragment 的私有域 ViewModel 似乎被改为不受保护了，暂未想明白这么设计的缘由是为何、将来是否依然如此。
> 
> 这里我提供一下相关的线索：
> 
> Fragment 类中的 setRetainInstance 方法，
> 
> FragmentManagerImpl 类中 moveToState 方法中的 nonConfig.clearNonConfigState 方法，

大家可以自行去感受一下（AndroidX 源码）

### Note：2020.2.10 加餐：真相大白：

#### 对 SavedStated 被单独抽取维护的缘由的解析

> AndroidX 的 Fragment、AppCompatActivity、ViewModel 源码已发生变化。
> 
> Fragment 的 Retain 已废弃，现如今 AndroidX 已单独引入 SaveState 包来解决 重建时 状态恢复的问题。
> 
> 此举是为了让 Fragment 和 Activity 的 public 生命周期设计趋近（例如未来 attach、detach 方法可能被 private 或 hide），以减少开发者的学习成本 和 源码设计师的维护成本。

**3.只要不作，就能避免因中间层持有页面而导致的内存泄漏**

最后，正由于 ViewModel 是个被精妙设计的 “单例”，使得视图控制器依赖它，它却不依赖于视图控制器，从而你只要不往 ViewModel 中塞视图控制器的 Context 等引用，就能避免一系列内存泄漏的隐患（例如当 Fragment 引用了 Activity 持有的 ViewModel 实例时）。

## 综上

**ViewModel 的目标有三个：**

> 1.让状态管理独立于视图控制器，从而实现 “**状态管理的分治**”、实现 “**状态管理的一致性**” 和 “**状态共享**”（跨页面通信）。
> 
> 2.为状态设置作用域，使状态的共享做到作用域可控。
> 
> 3.实现单向依赖，避免内存泄漏。

## Note 2021.2.4 加餐：

### 对 ViewModel 潜在的内存泄漏的解析

Jetpack MVVM 的 “单向依赖” 设计，目的就是通过让生命周期越短的依赖于越长的，来规避内存泄漏的问题，Activity -> viewModel -> Repository。

然而尽管如此，在目前的 AndroidX 系统源码中还是存在内存泄漏的设计（截至 2021.2.4，感谢小伙伴 @[hanshengjian](https://github.com/hanshengjian) 的分享）：

目前所知的关于 ViewModel 潜在的内存泄漏的情况，主要发生于 “**环境发生变化**” 等场景，例如 “旋转屏幕导致的重建”，这个过程中会产生新的 Activity（通过 Log 可发现重建后 Activity 的 hashCode 已发生改变），但上一任所持有的 ViewModelStore 对象会通过 “状态保存恢复机制” 一直延用到下一任身上，而导致上一任 Activity 回收不了。

这个问题不知道 Google 未来会不会解决，同时这也意味着 **天底下没有完美的工具，只有合适的工具，在引入一个工具来解决某问题时，通常伴随着引入别的副作用，因此可结合各方面因素来权衡利弊**（至少在软工安全的层面来说是利大于弊，因为配合 LiveData 规避了一系列生命周期安全和 null 安全问题，以及实现 更快、更省电、省流量的状态保存恢复）

## Note 2021.07.20 加餐：

### ViewModel 业务能力外的场景及应对方案

Jetpack ViewModel 的效用仅限于 “此刻”，也即当 App 还在前台时，为环境重建等问题提供状态的托管和恢复。

而对于 App 退居后台的情况，临时数据通常是会随着 App 进程被回收而一去不复返，

> 比如填表的场景：填表过程中你切换到系统相机 App，拍完照再切回来，发现 App 已被 “杀后台”，

因而对于这种情况的临时数据恢复，不要寄希望于 ViewModel，也不要寄希望于 onSaveInstanceState（以防国产 ROM 存在魔改），而是填表过程中便 **间歇性地从内存中将临时数据持久化存储**（通过 SharedPreference 等），那么下次启动 App 时便有机会检测和恢复。

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