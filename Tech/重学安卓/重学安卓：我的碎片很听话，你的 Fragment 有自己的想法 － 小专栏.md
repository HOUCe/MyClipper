---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/0937256481
author: 
---

# [重学安卓：我的碎片很听话，你的 Fragment 有自己的想法 － 小专栏](https://xiaozhuanlan.com/topic/0937256481)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

## 前言

很高兴见到你！

源于某天给组里的实习生安排了一项任务：开发一个云新闻客户端，推送行业内的消息，以及公司的活动近况。

该客户端由多个 Fragment 组成，Fragment 之间的关系有平级，也有跳转。

周五任务完成，拿到手一看，“有点东西啊”，同事感叹道。短短几天时间，实习生就能从零开始把一个新闻客户端搭好。没想到，当屏幕一旋转，问题就暴露了 —— Fragment 发生了重叠。

![](https://images.xiaozhuanlan.com/photo/2019/59d900f136315f9c7eba8c1e2321f77a.gif)

啥是重叠呢？为了便于理解，我录制了《重学安卓》项目的测试示例。  
如图可见，若 Activity 不做相关的处理，在 Activity 重建时，额外被创建的 Fragment 将叠加在被恢复的 Fragment 之上 —— 注意观察下方重叠的数字。

为什么会这样呢？

我们常常在网上见到《Fragment 重叠》相关的文章，可文章 **一上来就是怎么做、贴一长串代码，就是不告诉读者为什么** 会发生重叠、实际上究竟是什么状况引起的。

这使得多数人遇到了这样的文章，就只得先收藏着，然后过了一段时间又遇到这个题材的文章，觉得有用，又收藏了一篇，却都没来得及细看。

**这样问题就始终没有得到确认和解决。**

所以本文的目标，就是借着介绍 Fragment 的机会，来特别地给大家明确一下 Fragment 重叠究竟是什么状况。

**即使没有遭遇过重叠，也请务必阅读好这篇！**

## 文章目录一览

-   Fragment 的缘起和职责为何？
-   **Fragment 回退栈和 Activity 的区别？为何存在这样的区别？**
-   Fragment 通信与 Activity 的区别？为何存在这样的区别？
-   **Fragment 生命周期为何如此设计？**
-   Fragment 生命周期和 Activity 的关系？
-   Fragment 的重建和状态管理与 Activity 的区别？
-   为何会导致 Fragment 重叠？
-   为何推荐使用 DialogFragment？
-   为何视图控制器建议用 Fragment 而不是 Activity？
-   综上
-   **Note 2020.3.31 加餐：**
    -   Fragment 最新 API 支持 replace 回退后的状态恢复
-   后记

## Fragment 的缘起和职责为何？

Fragment 是在 Android 3.0 引入的，最初是专门针对平板视图而存在。

在 Android 4.0 中，Fragment 被升级为手机和平板都能使用。

![](https://images.xiaozhuanlan.com/photo/2021/018a3e4cf1af4b278358d8dece34ef5e.gif)

如此一来，我们便知道了，Fragment 的存在是为了：

-   适配横竖屏场景的不同需求。

例如在平板模式下显示手机布局，不美观，也浪费空间，Fragment 的出现就能解决这样的问题，让 List 和 Detail 同时显现。

-   实现代码的复用。

一个 Fragment 类可以被实例化多次，也可以在多个 Activity 中使用。反之，每新建一个 Activity，就要重新部署一套视图控制，就很麻烦。

因而，更纯粹地说，Fragment 的存在，是 **专注于承担视图控制器的责任**，以分担 Activity 的责任、让 Activity 更加专注于 “幕后协调者” 的工作。

## Fragment 回退栈和 Activity 的区别？为何存在这样的区别？

在 [《你丢了 offer，只因拎不清 Activity 任务和返回栈》](https://xiaozhuanlan.com/topic/7812045693) 一文中，我们正确区分了“任务”和“返回栈”实际是两个不同的概念，并且明白了“启动模式”是怎么影响 Activity 和任务之间的关系。

Fragment 也包含回退栈的设计。

> Note 2020.07.31
> 
> 注：即日起本文中使用 “回退栈” 来称谓 BackStack，区别于 Activity 的 “返回栈”，缘由是 Fragment 的 BackStack 是基于事务的操作，翻译成 “回退” 会更贴近 事务的背景下 RollBack（回滚）的含义。感谢读者 @[AE.86](https://xiaozhuanlan.com/u/5526812233) 的提醒。

在了解 Fragment 回退栈之前，我们先来了解一下 Fragment 生存的背景：

Fragment 是完全依附于 FragmentActivity 而存在，因为有且只有 FragmentActivity 直接或间接地包含了与 Fragment 管理相关的一系列成员，包括 FragmentController、FragmentManager 等等。

> 注：AppCompatActivity 是 FragmentActivity 的子类，所以使用 AppCompatActivity 即代表支持 Fragment。

也因此，和 Activity 的区别在于，Fragment 的管理没有任务栈，而只有回退栈。

—— 为什么这么设计呢？

如果通过上述的描述，你还是不免要如此发问、而不能逻辑连贯地秒懂的话，那还是推荐请你先阅读 [《你丢了 offer，只因拎不清 Activity 任务和返回栈》](https://xiaozhuanlan.com/topic/7812045693) 。

我换个方式再简单地解释一下：因为 Activity 是组件，组件被设计成是要考虑跨进程通信，因而 Activity 的管理不仅被设计出任务栈来管理 Activity，还设计出返回栈，用于管理来自多个 App 的 SingleTask 模式且 taskAffinity 指明过不同任务名的任务。而 Fragment 因为背靠单个 Activity，而不需要考虑 跨进程/跨App 的问题，因而它只需要回退栈。

> 划重点 👆👆👆

```
getSupportFragmentManager().beginTransaction()
     .add(R.id.fragment_container, fragment, fragmentName)
     .addToBackStack(null)
     .commit();
```

通常我们会以如上的方式将一个 Fragment 以事务的方式添加到回退栈。

事务我们在大学学习数据库时接触过，是指一组原子性的操作，也即这一波操作要么全完成，要么全不完成，绝不导致烂尾的现象，而且完成后还可以回滚到完成前的现场。

很显然 Fragment 的回退，就是事务的回滚。

那既然 Fragment 回退栈采用事务管理的方式，Activity 的管理为何不如此设计呢？

因为 Activity 与任务的关系、任务与返回栈的关系随时可能发生变化，在栈中的位置随时动态发生变化，而非 Fragment 这种单纯地进或出。

> 划重点 👆👆👆

被 Fragment 回退栈管理的一个个对象就叫 BackStackRecord —— 是对 FragmentTransaction 的实现 —— 当你提交 Fragment 事务时，实际上提交的就是一个个 BackStackRecord。

值得注意的是，上述提到的 FragmentController，它只是直接隶属于 FragmentActivity 的，用于批量管理 Fragment 的一个代理，实际真正在管理 Fragment 的是 FragmentManager …… 这些你们顺着源码去看就行，能不贴代码我尽量不贴代码，因为动不动就贴一长串代码是最影响阅读、且最没水平的 :D

## Fragment 通信与 Activity 的区别？为何存在这样的区别？

上一节我们讲到，Fragment 与 Activity 在返回栈设计上的差异，取决于 Fragment 和 Activity 在生存环境上的差异。

同样的，由于生存环境的差异，导致 Fragment 与 Activity、乃至 Fragment 之间的通信，不再是组件间的通信，而无需考虑跨进程通信、无需使用 Intent 通过 Binder 来通信，而是可以直接使用接口。

具体的 **代码示例请在《重学安卓》项目的 OneTestFragment 中查看**（不要慌，文末链接给出），此处不作累述。

## Fragment 生命周期为何如此设计？

在 [《Activity 生命周期的 3 个辟谣》](https://xiaozhuanlan.com/topic/0213584967) 一文中，我们结合“进程模式”的概念，介绍了 Activity 生命周期的设计依据。

Fragment 的生命周期同样有其设计依据。

Fragment 生命周期和 Activity 生命周期存在一定的差异，并且受事务添加方式的影响。

主要的差异表现在，Fragment 的生命周期节点相比 Activity 多了 onAttach、onCreateView、onViewCreated、onActivityCreated、onDestroyView、onDetech，而少了 onRestart。

为什么要多了这些节点呢？主要来自 3 方面的因素：

-   Fragment 的职责是管理视图。
-   Fragment 作为碎片而不是组件，应当占用更少资源。
-   为了状态管理的方便。

具体而言，由于 Fragment 的存在，是为了分摊 Activity 在视图控制方面的责任，因此，Fragment 的管理无非就是 View 的加载和销毁、View 状态及 Fragment 成员状态的保存和恢复。

此外，Android 为每个应用进程分配的内存极少，因而为了“能省即省”，而对 Fragment 生命周期做了如下的规划：

-   当 onAttach 时，就可以通过 setArgument 来注入初始化参数。

（注意：通过 setArgument 注入的参数，使得 Fragment 可以在状态重建时重新获取到这些参数。而通过常规方式从构造函数注入参数的，就没有那么幸运了 —— 为什么呢，因为 Activity 重建时你在 Activity 中判断不走手动创建新 Fragment 了，自然也就不会实例化和注入参数了，所以这也是 argument 这个设计存在的缘由）

> 划重点 👆👆👆

-   当 Fragment 被 replace 时，会走 onDestroyView 而不至于走 onDestroy 完全被销毁，为的是销毁视图的同时，使 Fragment 的状态（包括 View 状态及 Fragment 成员状态）得以保留，从而使当前内存空间得以被释放的同时、使下一次加载时又可直接走 onCreateView、比“冷加载”更快。

> 划重点 👆👆👆

## Fragment 生命周期和 Activity 的关系？

具体你可以跑一跑《重学安卓》项目中的 test01\_lifecycle\_test 包下的 OneActivity，**Log 我都尽数为你打好了**，你可以就着 LogCat 观察一下 Activity 和 Fragment 乃至 View 的生命周期的关系。

## Fragment 的重建和状态管理与 Activity 的区别？

简单来说，Fragment 和 Activity 在重建机制本身、和状态管理本身的唯一区别在于，Fragment 没有 onRestoreInstanceState 方法，因此如有需要，你只能在 onCreate 到 onActivityCreated 之间恢复手动保存的成员状态。

为何如此设计呢？因为 Fragment 光生命周期的方法就够多了，并且跑过 Log 你就会发现，Fragment 从 onCreate 到 onActivityCreated，是在 Activity 走 onCreate 之后才走的，也因此，恰是恢复状态的最佳时机，无须额外再搞个什么 onRestoreInstanceState。

> 总之，Activity 的重建和状态管理，和 Fragment 的重建和状态管理，大同小异：

Activity 状态 = Fragment 状态 + View 状态 + 成员状态。

Fragment 状态 = View 状态 + 成员状态。

其中

-   成员状态需要我们自己在 onSaveInstanceState 中手动保存。
-   并且 Activity 的 super.onSaveInstanceState 除了保存 View 状态，还通过 FragmentController 保存所有 Fragment 的状态。

关于 Activity 重建机制和状态管理，可以参考这一期 [《绝不丢失状态的 Activity 重建机制！》](https://xiaozhuanlan.com/topic/7692814530)

## 为何会导致 Fragment 重叠？

上述我们提到了，Activity 的 super.onSaveInstanceState 会保存所有 Fragment 的状态，并在重建时，将 Fragment 重建和恢复状态。（具体可参见 FragmentActivity 的 onSaveInstanceState 和 onCreate 源码）

```
protected void onCreate(@Nullable Bundle savedInstanceState) {
    ...
if (savedInstanceState != null) {
        Parcelable p = savedInstanceState.getParcelable(FRAGMENTS_TAG);
        mFragments.restoreSaveState(p);
    ...
}
```

而正因为我们的同志对这个状况没有事先的了解，而没有在自己的 Activity 中做出如下判断：

```
if (savedInstanceState != null) {
} else {
    getSupportFragmentManager().beginTransaction()
      .add(R.id.fragment_container, fragment, fragmentName)
      .addToBackStack(null)
      .commit();
}
```

还有别的状况会导致 Fragment 重叠吗？

曾经有，例如通过 hide 方法隐藏的 Fragment，visible 变量的值原先为 false，在重建后因没有得到赋值，而变为 true，也即在不该显示的状况下显示了。这是 Fragment 的一个 bug，早在 3 年前，在 support Library 24.0.0 中就已经修复了。

因而当下 Fragment 重叠只有一种可能，就是在 Activity 重建时，没有做出判断乃至重复创建和加载了 Fragment。

> 划重点 👆👆👆

## 为何推荐使用 DialogFragment？

最主要的原因是，相比于 Dialog，DialogFragment 可以跟随宿主完成重建和状态恢复。  
（比如 Dialog 正在加载进度条，结果旋转屏幕后，Dialog 没有被重建和恢复状态，而是直接被关闭了，这就影响用户体验）

就这么简单，具体如何使用，我就不做累述了，网文一搜一大把 :D

例如这里我为你搜到的两篇：

[https://blog.csdn.net/ys743276112/article/details/52962046](https://blog.csdn.net/ys743276112/article/details/52962046)

[https://www.jianshu.com/p/526fcf3e8db3](https://www.jianshu.com/p/526fcf3e8db3)

## 为何视图控制器建议用 Fragment 而不是 Activity？

总结起来，原因有 3：

-   正常来讲，Fragment 可以被混淆，Activity 不行，代码看光光。
-   Fragment 不是组件，是被专门设计为用于视图控制的，职责少、占资源小。
-   切换速度比 Activity 快太多。

后两个原因通过上述的分析，你差不多已经心中有数了。

关于第一个原因，我再仔细讲一讲：

是因为，ProGuard 是专门为 Java 设计的，它不能混淆如 manifest 这样的 xml，乃至于，当 Activity 类名被混淆，manifest 中便匹配不到对应的 Activity，就会因反射失败而导致崩溃。不仅 Activity 不能被混淆，View 也是一样的。

克服这个问题的办法倒不是没有，比如饿了么就开源了 Mess：[https://github.com/eleme/Mess](https://github.com/eleme/Mess) ，可以混淆 Activity 和 View。

## 综上

-   Fragment 的存在，是为了分担 Activity 在视图控制上的责任。
-   Fragment 因为生存环境是背靠单个 Activity、无须与跨进程组件发生交互，而只需单纯的回退栈设计。
-   Fragment 为了在重建时更节省资源、和在恢复时更快速，而新增了一系列区别于 Activity 的生命周期方法。
-   Fragment 的重叠不过是部分开发者未了解状况：在 Activity 重建时，会顺便重建 Fragment，因而需要开发者判断是否动态创建 Fragment。
-   使用 DialogFragment 是为了在重建时能恢复对话框内容的状态，并且 DialogFragment 相对来说方便支持更复杂的视图。
-   建议使用 Fragment 来作为视图控制器，而且最好还是单 Activity 应用，这么做的缘由是：代码安全、占资源小、切换快。

## Note 2020.3.31 加餐：

> 2020 年起，fragment 就算 replace，返回后仍然能恢复之前的状态，具体缘由详见评论区 20楼的补充。

## 后记

这一期打磨了将近 2 周时间，不停地想在 “深度思考” 和 “面面俱到” 之间达到一个平衡，

所以文章也是反复修改，直到现在，得到了令自己相对满意的结果。

我并不打算通过这篇文章，你能从 0 到 1 地学会怎么使用 Fragment，但我能够保证的是，**99% 的文章都未经思考的内容，你在这篇文章中全都能找到答案**。

这是我致力于提供的价值。

此外，关于 Fragment 从 0 到 1 的使用（甚至是最佳实践），你完全可以跑一下《重学安卓》项目，全网唯一良心、**经过严格检验、并且能跑起来的代码** :D

> **便捷访问：**
> 
> **[基本功：是随时随地可受用的 深度思考原则](https://xiaozhuanlan.com/topic/9837051426)**
> 
> [GitHub : 重学安卓 配套项目](https://github.com/KunMinX/Relearn-Android)
> 
> [GitHub : Jetpack-MVVM-Best-Practice](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)
> 
> **视图控制器系列：**
> 
> [重学安卓：Activity 生命周期的 3 个辟谣](https://xiaozhuanlan.com/topic/0213584967)
> 
> [重学安卓：绝不丢失状态的 Activity 重建机制！](https://xiaozhuanlan.com/topic/7692814530)
> 
> [重学安卓：你丢了 offer，只因拎不清 Activity 任务和返回栈](https://xiaozhuanlan.com/topic/7812045693)
> 
> [重学安卓：Intent 就是你的择偶标准啊！](https://xiaozhuanlan.com/topic/2869301475)
> 
> [重学安卓：我的碎片很听话，你的 Fragment 有自己的想法](https://xiaozhuanlan.com/topic/0937256481)

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

## 前言

很高兴见到你！

源于某天给组里的实习生安排了一项任务：开发一个云新闻客户端，推送行业内的消息，以及公司的活动近况。

该客户端由多个 Fragment 组成，Fragment 之间的关系有平级，也有跳转。

周五任务完成，拿到手一看，“有点东西啊”，同事感叹道。短短几天时间，实习生就能从零开始把一个新闻客户端搭好。没想到，当屏幕一旋转，问题就暴露了 —— Fragment 发生了重叠。

![](https://images.xiaozhuanlan.com/photo/2019/59d900f136315f9c7eba8c1e2321f77a.gif)

啥是重叠呢？为了便于理解，我录制了《重学安卓》项目的测试示例。  
如图可见，若 Activity 不做相关的处理，在 Activity 重建时，额外被创建的 Fragment 将叠加在被恢复的 Fragment 之上 —— 注意观察下方重叠的数字。

为什么会这样呢？

我们常常在网上见到《Fragment 重叠》相关的文章，可文章 **一上来就是怎么做、贴一长串代码，就是不告诉读者为什么** 会发生重叠、实际上究竟是什么状况引起的。

这使得多数人遇到了这样的文章，就只得先收藏着，然后过了一段时间又遇到这个题材的文章，觉得有用，又收藏了一篇，却都没来得及细看。

**这样问题就始终没有得到确认和解决。**

所以本文的目标，就是借着介绍 Fragment 的机会，来特别地给大家明确一下 Fragment 重叠究竟是什么状况。

**即使没有遭遇过重叠，也请务必阅读好这篇！**

## 文章目录一览

-   Fragment 的缘起和职责为何？
-   **Fragment 回退栈和 Activity 的区别？为何存在这样的区别？**
-   Fragment 通信与 Activity 的区别？为何存在这样的区别？
-   **Fragment 生命周期为何如此设计？**
-   Fragment 生命周期和 Activity 的关系？
-   Fragment 的重建和状态管理与 Activity 的区别？
-   为何会导致 Fragment 重叠？
-   为何推荐使用 DialogFragment？
-   为何视图控制器建议用 Fragment 而不是 Activity？
-   综上
-   **Note 2020.3.31 加餐：**
    -   Fragment 最新 API 支持 replace 回退后的状态恢复
-   后记

## Fragment 的缘起和职责为何？

Fragment 是在 Android 3.0 引入的，最初是专门针对平板视图而存在。

在 Android 4.0 中，Fragment 被升级为手机和平板都能使用。

![](https://images.xiaozhuanlan.com/photo/2021/018a3e4cf1af4b278358d8dece34ef5e.gif)

如此一来，我们便知道了，Fragment 的存在是为了：

-   适配横竖屏场景的不同需求。

例如在平板模式下显示手机布局，不美观，也浪费空间，Fragment 的出现就能解决这样的问题，让 List 和 Detail 同时显现。

-   实现代码的复用。

一个 Fragment 类可以被实例化多次，也可以在多个 Activity 中使用。反之，每新建一个 Activity，就要重新部署一套视图控制，就很麻烦。

因而，更纯粹地说，Fragment 的存在，是 **专注于承担视图控制器的责任**，以分担 Activity 的责任、让 Activity 更加专注于 “幕后协调者” 的工作。

## Fragment 回退栈和 Activity 的区别？为何存在这样的区别？

在 [《你丢了 offer，只因拎不清 Activity 任务和返回栈》](https://xiaozhuanlan.com/topic/7812045693) 一文中，我们正确区分了“任务”和“返回栈”实际是两个不同的概念，并且明白了“启动模式”是怎么影响 Activity 和任务之间的关系。

Fragment 也包含回退栈的设计。

> Note 2020.07.31
> 
> 注：即日起本文中使用 “回退栈” 来称谓 BackStack，区别于 Activity 的 “返回栈”，缘由是 Fragment 的 BackStack 是基于事务的操作，翻译成 “回退” 会更贴近 事务的背景下 RollBack（回滚）的含义。感谢读者 @[AE.86](https://xiaozhuanlan.com/u/5526812233) 的提醒。

在了解 Fragment 回退栈之前，我们先来了解一下 Fragment 生存的背景：

Fragment 是完全依附于 FragmentActivity 而存在，因为有且只有 FragmentActivity 直接或间接地包含了与 Fragment 管理相关的一系列成员，包括 FragmentController、FragmentManager 等等。

> 注：AppCompatActivity 是 FragmentActivity 的子类，所以使用 AppCompatActivity 即代表支持 Fragment。

也因此，和 Activity 的区别在于，Fragment 的管理没有任务栈，而只有回退栈。

—— 为什么这么设计呢？

如果通过上述的描述，你还是不免要如此发问、而不能逻辑连贯地秒懂的话，那还是推荐请你先阅读 [《你丢了 offer，只因拎不清 Activity 任务和返回栈》](https://xiaozhuanlan.com/topic/7812045693) 。

我换个方式再简单地解释一下：因为 Activity 是组件，组件被设计成是要考虑跨进程通信，因而 Activity 的管理不仅被设计出任务栈来管理 Activity，还设计出返回栈，用于管理来自多个 App 的 SingleTask 模式且 taskAffinity 指明过不同任务名的任务。而 Fragment 因为背靠单个 Activity，而不需要考虑 跨进程/跨App 的问题，因而它只需要回退栈。

> 划重点 👆👆👆

```
getSupportFragmentManager().beginTransaction()
     .add(R.id.fragment_container, fragment, fragmentName)
     .addToBackStack(null)
     .commit();
```

通常我们会以如上的方式将一个 Fragment 以事务的方式添加到回退栈。

事务我们在大学学习数据库时接触过，是指一组原子性的操作，也即这一波操作要么全完成，要么全不完成，绝不导致烂尾的现象，而且完成后还可以回滚到完成前的现场。

很显然 Fragment 的回退，就是事务的回滚。

那既然 Fragment 回退栈采用事务管理的方式，Activity 的管理为何不如此设计呢？

因为 Activity 与任务的关系、任务与返回栈的关系随时可能发生变化，在栈中的位置随时动态发生变化，而非 Fragment 这种单纯地进或出。

> 划重点 👆👆👆

被 Fragment 回退栈管理的一个个对象就叫 BackStackRecord —— 是对 FragmentTransaction 的实现 —— 当你提交 Fragment 事务时，实际上提交的就是一个个 BackStackRecord。

值得注意的是，上述提到的 FragmentController，它只是直接隶属于 FragmentActivity 的，用于批量管理 Fragment 的一个代理，实际真正在管理 Fragment 的是 FragmentManager …… 这些你们顺着源码去看就行，能不贴代码我尽量不贴代码，因为动不动就贴一长串代码是最影响阅读、且最没水平的 :D

## Fragment 通信与 Activity 的区别？为何存在这样的区别？

上一节我们讲到，Fragment 与 Activity 在返回栈设计上的差异，取决于 Fragment 和 Activity 在生存环境上的差异。

同样的，由于生存环境的差异，导致 Fragment 与 Activity、乃至 Fragment 之间的通信，不再是组件间的通信，而无需考虑跨进程通信、无需使用 Intent 通过 Binder 来通信，而是可以直接使用接口。

具体的 **代码示例请在《重学安卓》项目的 OneTestFragment 中查看**（不要慌，文末链接给出），此处不作累述。

## Fragment 生命周期为何如此设计？

在 [《Activity 生命周期的 3 个辟谣》](https://xiaozhuanlan.com/topic/0213584967) 一文中，我们结合“进程模式”的概念，介绍了 Activity 生命周期的设计依据。

Fragment 的生命周期同样有其设计依据。

Fragment 生命周期和 Activity 生命周期存在一定的差异，并且受事务添加方式的影响。

主要的差异表现在，Fragment 的生命周期节点相比 Activity 多了 onAttach、onCreateView、onViewCreated、onActivityCreated、onDestroyView、onDetech，而少了 onRestart。

为什么要多了这些节点呢？主要来自 3 方面的因素：

-   Fragment 的职责是管理视图。
-   Fragment 作为碎片而不是组件，应当占用更少资源。
-   为了状态管理的方便。

具体而言，由于 Fragment 的存在，是为了分摊 Activity 在视图控制方面的责任，因此，Fragment 的管理无非就是 View 的加载和销毁、View 状态及 Fragment 成员状态的保存和恢复。

此外，Android 为每个应用进程分配的内存极少，因而为了“能省即省”，而对 Fragment 生命周期做了如下的规划：

-   当 onAttach 时，就可以通过 setArgument 来注入初始化参数。

（注意：通过 setArgument 注入的参数，使得 Fragment 可以在状态重建时重新获取到这些参数。而通过常规方式从构造函数注入参数的，就没有那么幸运了 —— 为什么呢，因为 Activity 重建时你在 Activity 中判断不走手动创建新 Fragment 了，自然也就不会实例化和注入参数了，所以这也是 argument 这个设计存在的缘由）

> 划重点 👆👆👆

-   当 Fragment 被 replace 时，会走 onDestroyView 而不至于走 onDestroy 完全被销毁，为的是销毁视图的同时，使 Fragment 的状态（包括 View 状态及 Fragment 成员状态）得以保留，从而使当前内存空间得以被释放的同时、使下一次加载时又可直接走 onCreateView、比“冷加载”更快。

> 划重点 👆👆👆

## Fragment 生命周期和 Activity 的关系？

具体你可以跑一跑《重学安卓》项目中的 test01\_lifecycle\_test 包下的 OneActivity，**Log 我都尽数为你打好了**，你可以就着 LogCat 观察一下 Activity 和 Fragment 乃至 View 的生命周期的关系。

## Fragment 的重建和状态管理与 Activity 的区别？

简单来说，Fragment 和 Activity 在重建机制本身、和状态管理本身的唯一区别在于，Fragment 没有 onRestoreInstanceState 方法，因此如有需要，你只能在 onCreate 到 onActivityCreated 之间恢复手动保存的成员状态。

为何如此设计呢？因为 Fragment 光生命周期的方法就够多了，并且跑过 Log 你就会发现，Fragment 从 onCreate 到 onActivityCreated，是在 Activity 走 onCreate 之后才走的，也因此，恰是恢复状态的最佳时机，无须额外再搞个什么 onRestoreInstanceState。

> 总之，Activity 的重建和状态管理，和 Fragment 的重建和状态管理，大同小异：

Activity 状态 = Fragment 状态 + View 状态 + 成员状态。

Fragment 状态 = View 状态 + 成员状态。

其中

-   成员状态需要我们自己在 onSaveInstanceState 中手动保存。
-   并且 Activity 的 super.onSaveInstanceState 除了保存 View 状态，还通过 FragmentController 保存所有 Fragment 的状态。

关于 Activity 重建机制和状态管理，可以参考这一期 [《绝不丢失状态的 Activity 重建机制！》](https://xiaozhuanlan.com/topic/7692814530)

## 为何会导致 Fragment 重叠？

上述我们提到了，Activity 的 super.onSaveInstanceState 会保存所有 Fragment 的状态，并在重建时，将 Fragment 重建和恢复状态。（具体可参见 FragmentActivity 的 onSaveInstanceState 和 onCreate 源码）

```
protected void onCreate(@Nullable Bundle savedInstanceState) {
    ...
if (savedInstanceState != null) {
        Parcelable p = savedInstanceState.getParcelable(FRAGMENTS_TAG);
        mFragments.restoreSaveState(p);
    ...
}
```

而正因为我们的同志对这个状况没有事先的了解，而没有在自己的 Activity 中做出如下判断：

```
if (savedInstanceState != null) {
} else {
    getSupportFragmentManager().beginTransaction()
      .add(R.id.fragment_container, fragment, fragmentName)
      .addToBackStack(null)
      .commit();
}
```

还有别的状况会导致 Fragment 重叠吗？

曾经有，例如通过 hide 方法隐藏的 Fragment，visible 变量的值原先为 false，在重建后因没有得到赋值，而变为 true，也即在不该显示的状况下显示了。这是 Fragment 的一个 bug，早在 3 年前，在 support Library 24.0.0 中就已经修复了。

因而当下 Fragment 重叠只有一种可能，就是在 Activity 重建时，没有做出判断乃至重复创建和加载了 Fragment。

> 划重点 👆👆👆

## 为何推荐使用 DialogFragment？

最主要的原因是，相比于 Dialog，DialogFragment 可以跟随宿主完成重建和状态恢复。  
（比如 Dialog 正在加载进度条，结果旋转屏幕后，Dialog 没有被重建和恢复状态，而是直接被关闭了，这就影响用户体验）

就这么简单，具体如何使用，我就不做累述了，网文一搜一大把 :D

例如这里我为你搜到的两篇：

[https://blog.csdn.net/ys743276112/article/details/52962046](https://blog.csdn.net/ys743276112/article/details/52962046)

[https://www.jianshu.com/p/526fcf3e8db3](https://www.jianshu.com/p/526fcf3e8db3)

## 为何视图控制器建议用 Fragment 而不是 Activity？

总结起来，原因有 3：

-   正常来讲，Fragment 可以被混淆，Activity 不行，代码看光光。
-   Fragment 不是组件，是被专门设计为用于视图控制的，职责少、占资源小。
-   切换速度比 Activity 快太多。

后两个原因通过上述的分析，你差不多已经心中有数了。

关于第一个原因，我再仔细讲一讲：

是因为，ProGuard 是专门为 Java 设计的，它不能混淆如 manifest 这样的 xml，乃至于，当 Activity 类名被混淆，manifest 中便匹配不到对应的 Activity，就会因反射失败而导致崩溃。不仅 Activity 不能被混淆，View 也是一样的。

克服这个问题的办法倒不是没有，比如饿了么就开源了 Mess：[https://github.com/eleme/Mess](https://github.com/eleme/Mess) ，可以混淆 Activity 和 View。

## 综上

-   Fragment 的存在，是为了分担 Activity 在视图控制上的责任。
-   Fragment 因为生存环境是背靠单个 Activity、无须与跨进程组件发生交互，而只需单纯的回退栈设计。
-   Fragment 为了在重建时更节省资源、和在恢复时更快速，而新增了一系列区别于 Activity 的生命周期方法。
-   Fragment 的重叠不过是部分开发者未了解状况：在 Activity 重建时，会顺便重建 Fragment，因而需要开发者判断是否动态创建 Fragment。
-   使用 DialogFragment 是为了在重建时能恢复对话框内容的状态，并且 DialogFragment 相对来说方便支持更复杂的视图。
-   建议使用 Fragment 来作为视图控制器，而且最好还是单 Activity 应用，这么做的缘由是：代码安全、占资源小、切换快。

## Note 2020.3.31 加餐：

> 2020 年起，fragment 就算 replace，返回后仍然能恢复之前的状态，具体缘由详见评论区 20楼的补充。

## 后记

这一期打磨了将近 2 周时间，不停地想在 “深度思考” 和 “面面俱到” 之间达到一个平衡，

所以文章也是反复修改，直到现在，得到了令自己相对满意的结果。

我并不打算通过这篇文章，你能从 0 到 1 地学会怎么使用 Fragment，但我能够保证的是，**99% 的文章都未经思考的内容，你在这篇文章中全都能找到答案**。

这是我致力于提供的价值。

此外，关于 Fragment 从 0 到 1 的使用（甚至是最佳实践），你完全可以跑一下《重学安卓》项目，全网唯一良心、**经过严格检验、并且能跑起来的代码** :D

> **便捷访问：**
> 
> **[基本功：是随时随地可受用的 深度思考原则](https://xiaozhuanlan.com/topic/9837051426)**
> 
> [GitHub : 重学安卓 配套项目](https://github.com/KunMinX/Relearn-Android)
> 
> [GitHub : Jetpack-MVVM-Best-Practice](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)
> 
> **视图控制器系列：**
> 
> [重学安卓：Activity 生命周期的 3 个辟谣](https://xiaozhuanlan.com/topic/0213584967)
> 
> [重学安卓：绝不丢失状态的 Activity 重建机制！](https://xiaozhuanlan.com/topic/7692814530)
> 
> [重学安卓：你丢了 offer，只因拎不清 Activity 任务和返回栈](https://xiaozhuanlan.com/topic/7812045693)
> 
> [重学安卓：Intent 就是你的择偶标准啊！](https://xiaozhuanlan.com/topic/2869301475)
> 
> [重学安卓：我的碎片很听话，你的 Fragment 有自己的想法](https://xiaozhuanlan.com/topic/0937256481)

## 版权声明

> Copyright © 2019-present KunMinX

**本文为专栏私有内容，谢绝转载**。

读者订阅了专栏，即享有了专栏文章的阅读权。

文章 **原创的引言、切入点、思路、结论** 凝聚了作者 KunMinX 本人的心血 及与参与者互动演化的结果，作者本人及作者在文中提名的有效贡献者 对相应的成果享有所有权。

**当您 借鉴或引用文中的引言、切入点、思路、结论 进行二次创作并打算发行时，须注明链接出处**，否则我们保留追责的权利。对此如有疑虑请及时沟通，或在发行前将作品交由本人核对。

当您看见有人 [洗稿](https://baike.baidu.com/item/%E6%B4%97%E7%A8%BF/20862574?fr=aladdin)、[剽窃](https://baike.baidu.com/item/%E5%89%BD%E7%AA%83/9398357?fr=aladdin)、未经授权转载本专栏的文章内容时，请及时向本人举报。

最后感谢您对专栏内容的阅读和喜欢。

