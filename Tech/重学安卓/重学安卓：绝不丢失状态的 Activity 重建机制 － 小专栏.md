---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/7692814530
author: 
---

# [重学安卓：绝不丢失状态的 Activity 重建机制 － 小专栏](https://xiaozhuanlan.com/topic/7692814530)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

## 前言

很高兴见到你！

上一期我们在[《重学安卓：Activity 生命周期的 3 个辟谣》](https://xiaozhuanlan.com/topic/0213584967)中，通过介绍 99% 的生命周期文章都不曾解析过的 `“进程模式”`，来方便大家深入理解 Activity 生命周期 的`设计依据`、`存在意义` 及 `注意事项`。

并且，文末我还针对网文的 3 个讹传，来给大家辟了谣。

相信经过这样一次介绍，小伙伴们对生命周期有了更加深刻的认识。

那么这一期，我们来结合实际的案例，来讲解一下 Activity 的重建机制。

## 文章目录一览

-   前言
-   因为心里没底，而不敢用的状态恢复
-   什么是重建？引发重建的场景有哪些？
-   为何要设计出重建的机制？有何好处？
-   **状态保存和恢复的具体过程？**（99% 的网文遗漏的关键细节）
-   状态保存和恢复的的注意事项？
-   **Note 2020.3.15 加餐：**
    -   onSaveInstanceState 的执行时机在新 API 中已确定
-   如何避免 “配置发生变化” 导致的重建？
-   综上
-   **Note 2021.5.13 加餐：**
    -   更简便 且不易出错的 “状态保存和恢复” 方式
    -   App 切到后台，页面临时数据 “防丢” 的妙招

## 因为心里没底，而不敢用的状态恢复

源于一次阶段性的功能更新，同事向客户推送了 App 新版本，原以为万事大吉，毕竟经过一周的打磨，各项功能都已趋于流畅稳定，没想到，在客户近乎变态的数据操作下，有客户反映 “页面切换巨卡”、“速度巨慢”。

为何会产生这种情况呢？我找到编写该页面的同事询问了缘由。

同事说，问题在于每次 `onPause` 时，为了避免 App 切到后台被系统回收导致数据丢失，而做了大量的数据持久化操作。

**那为什么不直接通过 Activity 的状态管理机制 来管理临时数据呢？**

同事说，那东西“不靠谱”，用着感觉心里没底，

…… 是网上介绍 Activity 重建流程和状态管理的文章不够多吗？不是的，网文都有提到：当发生重建时，就会走 `onSaveInstanceState`（下文简称 “Save”） 和 `onRestoreInstanceState`（下文简称 “Restore”），

然而却没有一篇深入地探究过：当走 Save 和 Restore 时，过程中究竟经历了哪些细节、背后 **决定状态被成功保存和恢复的关键条件** 到底是怎样的。

这使得读者在使用这个机制时忐忑不安，无法 100% 确定不会出事。

于是在经过源码分析和再三的实测检验后，我将 “重建机制” 及 “状态管理机制” 的内容整理成这篇文章，并且开源了全套测试代码（不要慌，文末链接给出）。

如果阅读完本文后，你对二者的缘起、规律有了明确的认识，从而 100% 放心地在日后工作中使用，那我的这番功夫就没白费 ~

## 什么是重建？引发重建的场景有哪些？

正常的生命周期流程，是页面从正常创建到销毁全流程。

而重建流程，则是在特定条件下，引发页面被销毁，并再次被创建的流程。

**通常是 “系统资源的回收” 或 “配置发生变化” 导致的重建**。

> 划重点 👆 👆 👆

**系统资源回收是指：**

当 App 处于背景模式时，可能因系统内存不足而被回收。

例如按下 Home 键切回桌面，或是接电话跳转到电话程序，都有可能使此前的这个 App 处于背景模式。

**配置发生变化是指：**

当系统配置发生变化时，比如屏幕方向（旋转屏幕）、屏幕大小（折叠屏）、语言的改变等。

## 为何要设计出重建的机制？有何好处？

**一来：**对于资源回收的情况，保存状态并等到使用时再恢复，要比后台存留进程 **所占的资源要小得多**。

**二来：**对于配置变化的情况，比如当屏幕方向发生变化时，**唯有重建，才有机会加载不同的界面**，特别如果 横屏、竖屏、折叠屏 的布局不同的话。

> 划重点 👆 👆 👆

![](https://images.xiaozhuanlan.com/photo/2021/f219d2f5599604b932e47c60134ec84f.gif)

> 上述旋屏演示来自专栏配套项目[《Github：Jetpack MVVM Best Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)

## 那么重建时，状态保存和恢复的具体过程如何呢？

首先，“状态” 是指支撑 UI 界面内容展示的临时数据，比如 EditText 中的文本、CheckBox 的勾选与否。

有且只有引发重建时，Activity 会走 Save 和 Restore，来保存和恢复状态。

Save 执行在 `onStop` 之前，但不限于在 `onPause` 之前或之后。

> **Note 2020.3.15：**  
> 自 API 28 起（CompileSDKVersion、TargetSDKVersion >= 28 并且 Android 系统版本 >= 9.0 的环境下），onSaveInstanceState 执行时机已确定为在 `onStop` 之后。具体可见读者 Flywith24 在 [评论区 11 楼](https://xiaozhuanlan.com/topic/7692814530#reply11) 的补充。

而 Restore 确定是执行在 `onStart` 之后。

其次，Activity 有两个数据结构用于保存状态：

一个是 View 的 States，专门用于存储 View 的状态；

再一个是 Instance 的 States，用于存储 View 的 States 以及 开发者在 Save 中手动保存的 Activity 成员变量。

（数据结构的具体名称可自行顺着 onSaveInstanceState 方法到源码中查询，以具体源码为准，不同版本源码可能存在差异）

![](https://images.xiaozhuanlan.com/photo/2021/32ce3f72551832cc8c494afb5dc17cfa.gif)

在引发重建时，Activity 会自动为我们保存和恢复 View 的状态。**具体表现为：**

Activity 在 Save 时，会 **自动收集 View Hierachy 中每一个 “实现了状态保存和恢复方法” 的 View 的状态**，这些状态数据会在 Restore 时回传给每一个 View。**并且回传时是依据 View 的 id 来逐一匹配的。**

> 划重点 👆 👆 👆

其中，View 的状态数据会被收集到 View 的 States 中，View 的 States 也会随着 Activity 中其他被指定收集的成员变量，一同被收集到 Instance 的 States 中，等待恢复时逐个恢复。

## 状态保存和恢复的的注意事项？

简言之：

**1.为了成功保存状态，要求在 View 内部实现状态保存和恢复方法**。

原生的 View 都有做到；如果是自定义 View，务必记住这一点；如果第三方 View 没做到，可以通过继承其来实现保存和恢复方法。

**2.为了成功恢复状态，要求在布局中给 View 赋予相应的 id**。

> 划重点 👆 👆 👆

**3.如果是 Activity 的成员变量，需要额外在 Activity（子类）中重写 Save 和 Restore 方法**。

注意：

**重写仅仅是为了额外地保存成员变量**。重写方法时，记得要保留基类（super）的实现，**Activity 正是在基类中完成 View 状态的保存和恢复**。

> 划重点 👆 👆 👆

## 如何避免 “配置发生变化” 导致的重建？

在清单文件中为该 Activity 配置 `android:configChanges` 属性。

比如属性值 `orientation|screenSize` 对应着旋转屏幕，`locale` 对应着语言变化。

如此，在配置发生变化时，不会导致重建，而是走 `onConfigurationChanged` 回调。

## 综上

引发页面重建的场景通常有 “系统资源的回收” 或 “配置发生变化” 等。

注意仅仅是 “页面” 才有重建的概念（recreate），重建针对的是页面。

而之所以要有重建机制，是出于 **节省资源** 或 **重载不同布局** 的考虑。

并且 **只要遵循 《状态保存和恢复的的注意事项》一节所提到的关键细节，就能稳妥地保存和恢复状态。**

你再也不用为了临时数据而在 `onPause` 或 `onStop` 中执行涉及 I/O 的持久化存储操作啦！

## Note 2021.5.13 加餐：

### 更简便 且不易出错的 “状态保存和恢复” 的方式

页面成员的状态保存和恢复，可以通过 Jetpack ViewModel 来简化，

从前你需要在 定义常量、序列化实体对象、在 onSaveInstanceState 和 onRestoreInstanceState 中各写一遍状态保存恢复的代码（至少包含以上 4 个步骤，步骤越多，容易疏忽和出错的地方就越多，严重拖累和阻碍业务功能的编写）

现在你只需 “将成员托管给 Jetpack ViewModel” 这一步就可以享受 “可暂存和恢复的成员”，具体可自行查阅[《重学安卓 Jetpack ViewModel》](https://xiaozhuanlan.com/topic/6257931840)篇的解析。

### App 切到后台，页面临时数据 “防丢” 的妙招

Jetpack ViewModel 仅仅是使 “状态保存和恢复” 更简便和不易出错，但如果某些场景临时数据的恢复十分重要，那么可以考虑持久化存储，

与文中提到的 “转场时才去存储” 的策略不同的是，此处的策略是改为 “**间歇性提前自动将临时数据从内存 RAM 中备份到硬盘 ROM 中**”，

具体操作可回顾[《重学安卓：Activity 生命周期的 3 个辟谣》](https://xiaozhuanlan.com/topic/0213584967)篇 **Note 2020.9.26 加餐** 的分享。

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
> [重学安卓：绝不丢失状态的 Activity 重建机制](https://xiaozhuanlan.com/topic/7692814530)
> 
> [重学安卓：你丢了 offer，只因拎不清 Activity 任务和返回栈](https://xiaozhuanlan.com/topic/7812045693)
> 
> [重学安卓：Intent 好比你的择偶标准](https://xiaozhuanlan.com/topic/2869301475)
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