---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/7812045693
author: 
---

# [重学安卓：你丢了 offer，只因拎不清 Activity 任务和返回栈 － 小专栏](https://xiaozhuanlan.com/topic/7812045693)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

## 前言

去年这个时候（2018.4）听坚守在本部的同事说，杭州这边，这段时间面了不下 30 个 因公司倒闭（某金融风波）而被迫离职的 Android 开发。

由于僧多肉少的缘故，同事便从进阶基础开始问，就比如 Activity 任务、返回栈、启动模式。没想到这 30 个人里面，仅有 1 个勉强过关。

是因为网上介绍 **任、返、启** 的文章不够多吗？不是的，恰恰相反，网上的文章多如牛毛，却没有一篇是摸着良心、实证检验过，都是人云亦云，书上说什么就信什么，未经过大脑的思考和动手的检验，便发布到网上。

考虑到接下来，还会有不少人会因为 如此基本的进阶知识都掌握不透 而面试遭拒，我便烦着自己就 任务、返回栈、启动模式 写一个专题，**并开源了配套的 15 个代码测试案例**（不要慌，文末链接给出）。所以 **即使不看别的文章，也请 务必 务必 务必 收藏好这一篇！**

## 提出了正确的问题，事情就解决了一半

在开始今天的话题之前，我们先提出 7 个基于深度思考得出的问题。

如果以下 7 个问题中，有超过 2 个你无法准确地回答，那么你需要注意了，以下介绍的每一小节内容，都是你在网上轻易搜不到的，因为这些都是我们通过 “深度思考” 和 “实证检验” 而得出，绝非以讹传讹、照本宣科之流。

正如爱因斯坦说的，“提出了正确的问题，事情就解决了一半”。因而毫不夸张地说，**光是看了以下提出的这 7 个问题，你就没白来。**

## Activity 任务、返回栈、启动模式的 7 个扪心自问

-   你知道任务的概念、职责和本质吗？
-   你知道返回栈的概念、职责和本质吗？
-   你知道任务和返回栈 并不是网文讹传的同一个概念吗？
-   你知道启动模式和任务的关系吗？（网文只答了一半，让人误以为 启动模式是指导 Activity 和任务内 Activity 的关系）
-   你知道 singleTask 和 standard 外加 singleTop 真正本质上的区别吗？
-   你知道清单声明和动态 FLAG 的区别吗？
-   你知道多个 App 之间 Activity 异常的连贯回退是怎么引起的吗？  
    ……

除了解答上述 7 个问题，本文还在文末额外赠送了：

-   通过一项实验，来证实返回栈的独立存在。
-   介绍 taskAffinity 与 standard、singleTop、singleTask 三者真实的关系。
-   介绍 singleTask 与任务的特殊关系。
-   介绍由于 singleTask 与任务的特殊关系而导致的异常状况及处理。

## 文章目录一览

-   前言
    -   提出了正确的问题，事情就解决了一半
    -   Activity 任务、返回栈、启动模式的 7 个扪心自问
-   任务和返回栈
    -   任务和返回栈 的概念分别为何？
    -   **为何分别存在任务和返回栈？**
-   启动模式
    -   **为何存在 启动模式 的设计？**
    -   4 种启动模式，表面上的特点分别为何？
    -   为何存在 多种启动模式 的设计？
    -   如何设置启动模式？
    -   SingleTask **如何指定与哪个任务关联？**
    -   **Note 2021.4.30 加餐：**
    -   DeepLink 和 allowTaskReparenting 实验的复现方式 和 注意事项
-   任务清空或保留 Activity 的几种方式？
-   **Note 2020.11.18 加餐**：
    -   对启动模式和 FLAG 区别的补充说明
-   综合案例**（证实了 返回栈 相对于 任务 的独立存在，全程动图和截图为证）**
    -   **Note 2020.6.17 加餐：**
    -   综合实验简化重制版
-   福利时间到！魔鬼就在细节中 —— 全网独家的、严密测试的 5 个结论：
    -   taskAffinity 真实的适用范围
    -   standard 和 singleTop 的真实本质
    -   singleTop 的真实本质
    -   singleTask 的真实本质及实验佐证
    -   singleTask 和 singleInstance 与最近访问列表的关系
-   **Note 2020.5.17 加餐**
    -   一个 app 能有多少个 ActivityStack
    -   **Note 2020.07.27 加餐：**
    -   如何在打印信息中正确区分 TaskRecord 和 ActivityStack
    -   **Note 2020.08.10 加餐：**
    -   通过 Android 9.0 的新 ADB 命令获取简洁的 Activity 堆栈信息
    -   最近访问列表中展示的卡片到底是什么
-   **Note 2020.5.28 加餐：**
    -   对 5.17 疑问的实验检验结果
-   **Note 2020.6.29 加餐：**
    -   SingleTask 和 SingleInstance 不新建 Task 的特殊情况
    -   为什么 SingleInstance Activity 回退直接回到桌面
-   **Note 2021.6.30 加餐：**
    -   系统为何不设计为直接通过 ActivityStack 来管理 ActivityRecord？

## 任务和返回栈 的概念分别为何？

`ActivityRecord`：

描述 Activity 的相关信息，对应着一个用户界面，是 Activity 管理的最小单位。

`TaskRecord`：

**是一个栈结构**，管理着多个 ActivityRecord，栈顶的 ActivityRecord 表示当前获得焦点的窗口。

`ActivityStack`：

是一个栈结构，管理着多个 TaskRecord，栈顶的 TaskRecord 表示当前获得焦点的任务。

> 对 焦点、前景、可见 等概念不熟悉的朋友，建议先阅读一下这篇短文 [《重学安卓：Activity 生命周期的 3 个辟谣》](https://xiaozhuanlan.com/topic/0213584967)，文中通过介绍 RemixOS 等安卓桌面操作系统，来方便你无障碍地理解 前景和可见 的区别，以及何谓“获得焦点”。

`ActivityStackSupervisior`：

管理着多个 ActivityStack。**当前只会有一个获得了焦点的 ActivityStack**。

> 划重点 👆 👆 👆 这一段全是重点。
> 
> 许多网文依赖于对官方文档 不假思索地照抄，且不幸的是，官方文档在 “任务与返回栈” 这篇的正文中 对概念的区分 **模棱两可**、对现象的描述 **含糊不清、与配图自相矛盾**，乃至网文误把 “任务” 和 “返回栈” 混为一谈，
> 
> 也正因为如此，它们无一能够解释 [配图](https://images.xiaozhuanlan.com/photo/2020/d5d0ce1dd12aca211054794ea8fb71f1.jpg) 中关于 “Back Stack 之外的 Background Task 进入 Back Stack 并置于原先 Foreground Activity 之前” 等现象。

在下文的实例中，我将亲手证实返回栈的存在，破解因 “人为因素” 造成的理解障碍。

![](https://images.xiaozhuanlan.com/photo/2021/8db0309dad6390013a1f89c80bbc87d5.png)

## 为何分别存在任务和返回栈？

我们通常说的任务和返回栈，分别指的是 TaskRecord 和 ActivityStack，**它们的存在 主要是为了维护“页面跳转的连贯性”体验**。

> 划重点 👆 👆 👆

比如用户在日记软件中添加照片，于是点击添加按钮，跳转到系统相册的选择器模式，选取照片后，又跳回日记软件。虽然日记软件和系统相册的页面来自两个 App，但这一系列的操作给用户的感觉，就好像是在同一个 App 中完成的。

换言之，即使是来自不同 App 的 Activity，也能存在于同一个任务中，完成连贯的跳转和回退操作。至于为何有了任务还要有返回栈，我们暂且按住不表、先往下阅读~

## 为何存在 启动模式 的设计？

**启动模式是用于定义 Activity 与任务的关联方式**。

> 划重点 👆 👆 👆

为适应不同场景的需要，总共设计了 4 种方式：  
`standard`、`singleTop`、`singleTask`、`singleInstance`。

## 4 种启动模式，表面上的特点分别为何？

> **standard - 标准模式**：

创建 Activity 的实例，并 **添加到启动它的源 Activity 所在的任务的栈顶**。不管栈内或栈顶是否已存在该 Activity 的实例。

> **singleTop - 栈顶复用模式**：

当 **启动它的源 Activity 所在的任务的栈顶已存在 该 Activity 的实例**，那么不创建该 Activity 的新实例 —— 而是走该 Activity 实例的 onNewIntent 回调，注入新的 intent，并执行 onResume（也就是不走 onCreate、onStart）。

否则就在栈顶创建一个新实例。

> **singleTask - 栈内复用模式**：

当 **该 Activity 所属的任务中已存在 该 Activity 的实例**，那么不创建该 Activity 的新实例 —— 而是 **首先将任务中该 Activity 实例之上的 Activity 全都出栈**，并且走该 Activity 实例的 onNewIntent 回调，注入新的 intent，并执行 onResume。

否则就在栈顶创建一个新实例。

（singleTask Activity 的所属任务，取决于清单中配置的 taskAffinity，**如果没有用 taskAffinity 指定任务名，默认是 Activity 所属 App 的默认任务**。下文会介绍 taskAffinity 的作用。）

> **singleInstance - 单例模式**：

会新建一个任务，并且 **独享这个任务**。也即 **整个系统 有且只有 这么一个 Activity 的实例**，多个 App 可共享该实例。

## 为何存在多种启动模式的设计？

standard，顾名思义，是默认的启动方式。

但为每个 Activity 都创建一个新的实例，开销太大了，因而有了 singleTop 和 singleTask 这两种复用模式，例如分别用于 **“Activity 在栈顶时基于新 Intent 完成纯刷新视图状态”**，以及 **“想要快速出栈多个 Activity 并返回到某个底层页面”** 的场景。

> 划重点 👆 👆 👆

又考虑到多个 App 可能需要共享同一个实例，因而又设计出 singleInstance 的复用模式。

上文我们提到了 4 种模式各自的特点，但模式之间究竟有何本质区别，这个我们在下文中会重点提到。

## 如何设置启动模式？

设置的方式通常有两种：

**在清单文件中，静态地为 Activity 指定启动模式**，例如：

```
<activity android:name=".TestActivity"
    android:launchMode="singleTask" />
```

**在代码中，通过为 Intent setFlag 的方式，动态地为 Activity 指定启动模式**，例如：

```
Intent i = new Intent(getApplicationContext(), TestActivity.class);
i.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
startActivity(i);
```

常用的 FLAG 有以下几种：

> 注：为了更好地显示 FLAG 本身的作用，以下对 FLAG 的谈论，都是以 “启动模式为 standard 的 Activity” 为例来赋予 FLAG，并与启动模式为 SingleTop 或 SingleTask 等的 Activity 在启动时的效果做对照说明。

FLAG\_ACTIVITY\_NEW\_TASK，近似于 singleTask 模式。当在清单中为 Activity 设置 taskAffinity 属性时，能跳转到指定任务（若先前不存在该任务，则先创建该任务）。该 FLAG 通常用于从非 Activity 的环境下启动 Activity（这么设计，是为了给 Activity 一个容身之处）。

FLAG\_ACTIVITY\_SINGLE\_TOP ，对应着 singleTop 模式。

FLAG\_ACTIVITY\_CLEAR\_TOP，近似于 singleTask 模式。当该 Activity 已存在于任务中，该 Activity 之上的 Activity 都会出栈，并且该 Activity 如为 standard，则会被重新创建，如为 singleTop，则是走 onNewIntent。

所以 **即使 FLAG\_ACTIVITY\_NEW\_TASK 与 FLAG\_ACTIVITY\_CLEAR\_TOP 共同使用，其效果也并不完全等同于 singleTask 模式**。

> **Note 2020.11.18 加餐：**
> 
> 感谢读者 [@Jackie](https://xiaozhuanlan.com/u/1300393854) 的反馈和补充，此处的意思即，**启动模式和 FLAG 的区别主要在于，前者是固定的，后者是可组合的**，例如 singleTask 是固定的、而 ClearTop 是可组合的：当你多数场景下不合适使用 SingleTask，但个别场景又需要它的这种清空栈中上方 Activity 的特性，那么此时你就选用 singleTop + clearTop FLAG 的组合，如此，在清单中你配置的是 singleTop，仅在你需要清空效果的场景下，在代码中动态附加 FLAG 来组合使用。
> 
> 划重点 👆 👆 👆

FLAG\_ACTIVITY\_NO\_HISTORY，被指定的 Activity 在跳转到其他 Activity 后，将从任务中移除。

FLAG\_ACTIVITY\_EXCLUDE\_FROM\_RECENTS，被指定的 Activity 不出现在 “最近应用” 列表中。

## 如何指定与哪个任务关联？

launchMode 属性可以指定以何种模式去与任务关联，那么如何指定具体与哪个任务关联呢？

可以通过在清单文件中为该 Activity 设置 taskAffinity 属性。

默认情况下 App 的任务名称为包名，因而我们这里为 taskAffinity 设置一个不一样的属性值，例如 “com.kunminx.task1”，来指定与该任务关联。如果返回栈中不存在该任务，则会新建一个该任务。**具体是在哪个返回栈新建，取决于启动该 Activity 的 Activity 所在任务所在的返回栈**。

> 划重点 👆 👆 👆

此外，清单文件中，Activity 的属性 allowTaskReparenting = true 意味着该 Activity 可以从一个任务迁移（回）到 taskAffinity 指定的任务。

**该属性的存在，主要是为了支持 DeepLink 的使用场景**：  
例如你通过 浏览器链接 启动了 知乎 的 allowTaskReparenting = true 的 Activity，那么此时该 Activity 处于 浏览器 的任务中；当通过 Home 键让 浏览器任务脱离焦点，并从桌面启动知乎时，由于该 Activity 被指定的 taskAffinity 值与 知乎 的任务一致，因而该 Activity 又会被迁移回 知乎 的任务中继续展示。

### Note 2021.4.30 加餐

#### DeepLink 和 allowTaskReparenting 实验的复现方式 和 注意事项

> 在 Deeplink 的场景下，我们通常不在 manifest 中为 Activity 定义 taskAffinity，也即默认使用 App 默认的 taskAffinity。
> 
> 该场景的 **复现过程 和 注意事项**，详见评论区 95 楼的解析。

## 任务清空或保留 Activity 的几种方式？

如果用户将任务切到后台，过了很长一段时间，系统会将这个任务中除了最底层的那个 Activity 之外的其它所有 Activity 全部清除。

当用户重新回到这个任务时，最底层的那个 Activity 将得到恢复。

为什么这么设计呢？

因为过了这么长时间，用户可能早忘了自己当初做了什么，所以一切从新。

> 这里的情况，和我在 [《Activity 生命周期 3 个辟谣》](https://xiaozhuanlan.com/topic/0213584967) 一文提到的“进程回收”不同，彼时“进程回收”是特指短暂地被回收，还是有恢复的可能，而此处的情况，是指长时间地处于空白进程，从而彻底被系统的其他机制给销毁，那么下一次进来的时候，给人感觉就像是重新打开 App 一样。

那么除了系统的这一默认设计，还有什么别的策略呢？

**alwaysRetainTaskState**

如果在清单文件中，将最底层的那个 Activity 的这个属性设置为 true，那么上面所描述的默认行为就将不会发生，任务中所有的Activity即使过了很长一段时间之后仍然会被继续保留。

**clearTaskOnLaunch**

如果将最底层的那个 Activity 的这个属性设置为 true，那么只要用户离开了当前任务，再次返回的时候就会将最底层 Activity 之上的所有其它Activity全部清除掉。简单来讲，就是一种和 alwaysRetainTaskState 完全相反的工作模式，它保证每次返回任务的时候都会是一种初始化状态，**即使用户仅仅离开了很短的一段时间**。

**finishOnTaskLaunch**

这个属性和 clearTaskOnLaunch 是比较类似的，不过它不是作用于整个任务上的，而是作用于单个 Activity 上。如果某个 Activity 将这个属性设置成 true，那么用户一旦离开了当前任务，再次返回时这个 Activity 就会被清除掉。

## 综合案例（证实了 返回栈 相对于 任务 的独立存在）

> 非常感谢读者 Fly\_with24🏀 于 [评论区 47 楼](https://xiaozhuanlan.com/topic/7812045693#reply47)，就 2019 年设计的本实验做了详尽的测试记录、发现了实验中存在的纰漏。后在读者孙致远的提议下，我决定重写和简化这一实验，以便新来的读者能无痛理解整个过程**（配套项目的代码已全面更新，可结合配套代码来体验）**。

### Note：2020.6.17 实验重制版：

默认情况下，一个返回栈可能只包含一个任务，但特殊情况下，可能引入多个任务。

大家可能都见过官网提供的这张图例，从 API 1 一直沿用到现在。那么以下我们借助一组实验，来方便你理解图例中所要表达的内容。

![官网提供的任务切换图例](https://images.xiaozhuanlan.com/photo/2020/d5d0ce1dd12aca211054794ea8fb71f1.jpg)

官网提供的任务切换图例

**首先我们配好实验环境：**创建 4 个 Activity A、B、C、D，在清单文件中将它们的 launchMode 配置为 SingleTask，并分别以 A、B 和 C、D 各为一组，配好单独的 taskAffinity（比如 A、B 的 taskAffinity 为 `com.kunminx.task.c`，C、D 的为 `com.kunminx.task.d`）

**然后我们根据动图中的步骤来实验：**

![](https://images.xiaozhuanlan.com/photo/2020/9929dd4010a39c35ca0f49988a1d7237.gif)

依次打开 C、D、A、B，然后点击 D，会发现此时在视觉上发生了 “横向切换”，随后再点击返回键时，依次出栈的是 D、C、B、A，

由此可知：

1.当从 Activity B 点击 D 时，视觉上的 “横向切换” 即发生了任务焦点的切换：

因为当后续点击返回键时，依次退出的是 D、C、B、A，而不是 D、B、A、C。也即**当 D 重获焦点，即导致 D 所在的整个任务也重获焦点、回到前景**。

2.**能支持任务与任务间的切换和连贯回退，说明任务之外还有一个独立于任务的栈，那就是返回栈**。

至此我们证实了，任务和返回栈是完全独立的两种存在，绝非网上人云亦云的 “同一个东西”。

![](https://images.xiaozhuanlan.com/photo/2020/a23dc0cc1980f4e696735ab3e8d9902c.png)

除此之外，**不要觉得 “任务切换” 这种设计很匪夷所思**。会感到困惑，主要是因为把自己的思维局限在手机的场景了。

事实上，除了我在[《Activity 生命周期的 3 个辟谣》](https://xiaozhuanlan.com/topic/0213584967)篇提到的 Remix OS 安卓桌面操作系统外，近几年的安卓旗舰手机比如华为、三星，基本都有 “Dex” 功能：接上专用的拓展坞即可化身为桌面操作系统，那么同一个 App 有多个 TaskRecord 实在太正常不过了：

你就可以想象成，对桌面系统来说，每个 TaskRecord 才是一个 “**窗口**”，而 ActivityRecord 不过是 “窗口” 中的 “**某一级页面**” —— 当我鼠标选中了某个页面，我实际上是让这个页面所在的整个窗口都重获焦点、回到前景。

> 划重点 👆 👆 👆

## 福利时间到。魔鬼就在细节中 —— 全网独家的、严密测试的 5 个结论：

1.清单中设置了 taskAffinity 的 Activity，能存在于 taskAffinity 指定名称的任务中：  
如果是 singleTask 和 singleInstance，它们能被直观地观察到，创建时即存在于被 taskAffinity 指定名称的任务中。  
如果是 standard 和 singleTop，它们在 allowTaskReparenting 为 true，且被其他应用以 DeepLink 的方式唤起时，才有机会观察到。

> 划重点 👆 👆 👆

2.**standard 和 singleTop 的 Activity 在哪个 TaskRecord 启动，全凭启动它的 Activity 在哪个 TaskRecord**（对此哪怕是启动另一个 App 的 Activity 仍成立）。

> 划重点 👆 👆 👆
> 
> Note 2020.11.10 感谢读者 @[霸者横栏](https://xiaozhuanlan.com/u/2352059227) 的反馈，此处的描述主要针对常规方式（当指定了目标 Activity，并直接 startActivity 的情况）。
> 
> `intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK)` 等方式启动 Activity 的情况例外（具体可回溯上文对 newTaskFlag 使用场景的解析）。

3.singleTop 确实是栈顶复用。

4.singleTask 确实是栈中复用，并且只是清空在它之上的 Activity。与 standard 和 singleTop 不同的是，**当被另一个 App 启动，singleTask 组件仍处于自己 App 的任务中，无论是复用还是新建的情况**。

> 划重点 👆 👆 👆

5.设置过 taskAffinity 的 singleTask 或 singleInstance 组件，只要是在一个全新的 task，该 task 就会在最近访问列表中以卡片的形式独立出现，卡片中显示的是 task 栈顶的页面。

> Note 2020.3.22：
> 
> 最后，上文在介绍启动模式之前，提到了 “为何有了任务还要有返回栈”，现在是不是也不言自明了呢？——

因为任务就是用来给 Activity 关联的，但除此之外，通过实验我们得知，部分场景下，还包含 “让某任务获得焦点” 以及 “维持回退操作的连贯性” 的需要，于是设计了返回栈，来负责任务之间层叠及出入栈的需要。

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

### Note 2020.5.27 加餐：

今天好几个刚订阅的小伙伴在群里讨论 task 和 stack，从他们的交流中就可发现，他们在短时间内已吸收了 文章循序渐进铺垫的 逻辑和认知，对此感到惊讶同时也很开心。

> 然后这边对交流中提出的疑问做个解析：

### 1.一个 app 到底有多少个 ActivityStack

> Note：感谢小伙伴 [@AE](https://xiaozhuanlan.com/u/AE).86 的补充，在 Android 系统版本 >= 9.0 的设备（模拟器）上，每新建一个 TaskRecord，都会同时新建一个 ActivityStack，因而《一个 app 到底有多少个 ActivityStack》加餐的内容目前仅限于提供 Android 9.0 之前设备的参考。有知晓设计如此变更 其背后的考虑的 小伙伴，欢迎随时在评论区补充。

（在 Android 9.0 之前的系统下）哪怕是 SingleInstance，也不过是独享了一个 TaskRecord，而不是新建和独享一个 ActivityStack。

这个借助小伙伴 @Fly\_with24🏀 在 47 楼提供的 adb shell 命令即可验证：  
（也即在手机页面测试流程走完一遍后，**在 terminal 中输入 adb shell 回车，然后输入 dumpsys activity activities 命令**。注意 **要使用原生或类原生 ROM 来测试**，比如 Android Studio 自带的模拟器，或 三星、一加 等系统。国产 ROM 魔改导致好多背景因素发生变化，也就失去了讨论的前提）：

### Note 2020.07.27 加餐：

#### 如何在打印信息中正确区分 TaskRecord 和 ActivityStack

![](https://images.xiaozhuanlan.com/photo/2020/95a073dab25f67cfa5b234f285929ad7.png)

> 注意看 👆 👆 👆 在打印出的结果中，搜索关键字 “Running activities”，你能看到这个 app 所有的 TaskRecord：

注意每当为 SingleTask 和 SingleInstance 的情况设置了一个新的 taskAffinity 时，都会单独起一个 TaskRecord（此处的 StackId 是指 TaskRecord 栈的 栈 Id）

![](https://images.xiaozhuanlan.com/photo/2020/b390855bcd60520b19d94b243ada3fc3.png)

> 然后我们搜索关键字 “ mFocusedStack”，如此能看到最终关于 ActivityStack 的情况：

得到的结果是，在同一个 app 内从一个 Activity 跳转到一个 singleInstance Activity 时，mFocusedStack 和 mLastFocusedStack 的 stackId 一致，也即并没有新建一个 stack。（注意，此处的 StackId 是指 ActivityStack 的 栈 Id，因为这个 stackId 是 ActivityStack 这个元素中的属性）

<- - Note 2020.07.27 加餐完毕 - ->

（注：mFocusedStack 和 mLastFocusedStack 分别指 “当前获得焦点的 stack” 和 “上一个获得焦点的 stack”。它们都是指针（或者说引用），可以指向系统中的任何 stack 对象。当我们在同一个 app 中从 standard Activity 跳转到 singleInstance Activity 发现 mFocusedStack 和 mLastFocusedStack 的 stackId 一致，我们就是通过这个来证明一个 app 确实只有一个 ActivityStack）

### Note 2020.08.10 加餐：

#### 通过 Android 9.0 的新 ADB 命令获取简洁的 Activity 堆栈信息

感谢小伙伴 @Fly\_with24🏀 的分享，

从 Android 9.0 开始，adb shell 开始支持新的堆栈命令：`dumpsys activity containers`

透过它，可以获取简洁的堆栈信息，从中我们可以轻松地看出，被赋予自定义 TaskAfficify 的 SingleTask Activity 前往了新建的 TaskRecord 和 ActivityStack，与 Android 9 之前的情况有所出入。

![](https://images.xiaozhuanlan.com/photo/2020/8ddaa6c935eccad8aa59babe0a708b7a.png)

<- - Note 2020.08.10 加餐完毕 - ->

### 2.最近访问列表中展示的最小单位是 设置过 taskAffinity 的 TaskRecord

![](https://images.xiaozhuanlan.com/photo/2020/a167b52ac48fde8a71e929c09445b87a.png)

一般 App 启动时新建的普通 TaskRecord，其不是没有 taskAffinity，而是其 taskAffinity 的值是 App 包名。

当 TaskRecord 中的 Activity 都出栈时，这个 TaskRecord 也会出栈。如果 ActivityStack 中包含多个 TaskRecord，那么当前焦点 TaskRecord 出栈（从最近访问列表中移出），并且底下的 TaskRecord 回到栈顶。

而如果 ActivityStack 中只有一个 TaskRecord（是 App 启动时的第一个 TaskRecord），并且该 TaskRecord 的 Activity 全都出栈了，那么这个 TaskRecord 不会出栈 —— **为什么这么设计呢？**—— TaskRecord 以卡片形式展示了 App 最后一次呈现的界面，为了让用户有机会从 “最近访问列表” 中看到此前打开过的卡片（以便重新访问），起到历史记录的作用。

(Note 2020.5.28 加餐：为此已安排了实验，实验结果显示：App 启动时新建的第一个 TaskRecord，在 ActivityRecord 全部出栈后，仍然存活于最近访问列表中。下一次从最近访问列表打开该 App，仍然是该 TaskRecord。具体可详见[《重学安卓》配套项目](https://github.com/KunMinX/Relearn-Android)的清单文件和 TestMainActivity 中安排的 log。）

### Note 2020.6.29 加餐：

### 1.SingleTask 和 SingleInstance 不新建 Task 的特殊情况

今天群里小伙伴 [@Hugo](https://xiaozhuanlan.com/u/2932978318) 在和 [@耳东君](https://xiaozhuanlan.com/u/8552580955) 讨论 SingleInstance 时分享了一个特殊状况：

Activity 在通过 startActivityForResult 的方式启动另一个 Activity 时，二者务必在同一个任务中，否则在 API 20 以下会在跳转时直接返回 `Activity.RESULT_CANCELED`，原因是源码设计师认为不该在两个任务之间 setResult（为什么呢？能想到反例场景的小伙伴可在评论区留言），

而最后官方的解决方案是，从 API 20 起，对于 startActivityForResult 的 requestCode >= 0 的情况（也即非 startActivity 跳转，因为 startActivity 是 startActivityForResult 的 requestCode 为 -1 的情况），SingleTask 和 SingleInstance 模式启动的 Activity 都不会新建一个任务。

### 2.为什么 SingleInstance Activity 回退直接回到桌面

读者耳东君给出的重现步骤是：

standard Activity 启动一个 SingleInstance Activity 后，按 最近访问列表 键，在最近访问列表中再点击 SingleInstance 所在任务卡片，再按返回键，此时直接回到了桌面，而不是之前 standard Activity 所在的任务，为什么呢？

对此读者 Flywith24 表示，Recent 界面也是个 Activity，属于 Launcher（具体我看是属于 Launcher 独立的 HomeStack 下的 task），

那么当从 最近访问列表 界面再次点击 SingleInstance 所在的任务卡片时，使得 RecentActivity 及其 task 成了始发站，SingleInstance Activity 及其 task 成了目的地，那么当从 SingleInstance Activity 返回时，自然就使 RecentActivity 的 task 重获焦点，并且由于是返回键，那么显示的是 桌面 Activity。

具体都可结合 “Note 2020.5.27 加餐” 的 “一个 app 只有一个 ActivityStack” 一节中提到的 Activity 命令 在[《重学安卓》配套项目](https://github.com/KunMinX/Relearn-Android)中测试，以加深印象。

### Note 2021.6.30 加餐：

### 系统为何不设计为直接通过 ActivityStack 来管理 ActivityRecord？

为何要在 ActivityStack 和 ActivityRecord 之间加个 TaskRecord 的设计？

—— 为了 “窗口多开” 成为可能：

其实联系生活实际不难理解这个问题，拿 PC 系统做类比的话，一个任务就相当于一个窗口（比如文件管理器窗口），一个窗口可以包含多个可 “前进、后退” 的页面，而通常为了文件相互拷贝的方便，用户是会打开多个文件管理器窗口（具体可参考我们在[《生命周期辟谣》](https://xiaozhuanlan.com/topic/0213584967)篇提供的 Android 桌面版操作系统文件管理器多开的场景），

—— 因而如果系统的设计中只支持 ActivityStack 来直接管理所有页面的话，就不能实现 “**窗口多开**” 了。

有了 “任务” 的设计，就能让窗口置于 “前景” 或 “可见” 的状态。移动端系统的存在晚于桌面端，因而我猜测 “任务” 的设计就是源自桌面操作系统 “窗口” 的设计。

> Note 2021.8.6：感谢小伙伴 [@Lιϙυιԃ](https://xiaozhuanlan.com/u/2067721379) 在评论区 106 楼的提醒，此处更准确的说法是，TaskRecord 的存在是 **为了让 “前进后退” 和 “窗口多开” 的同时成立 成为可能**。
> 
> 在没有 TaskRecord 的情况下，“前进后退” 或 “窗口多开” 理论上是可以分别单独成立的，但这种成立是互斥的，所以为了能同时成立，务必引入 TaskRecord 来管理 ActivityRecord。

## 版权声明

> Copyright © 2019-present KunMinX

**本文为专栏私有内容，谢绝转载**。

读者订阅了专栏，即享有了专栏文章的阅读权。

文章 **原创的引言、切入点、思路、结论** 凝聚了作者 KunMinX 本人的心血 及与参与者互动演化的结果，作者本人及作者在文中提名的有效贡献者 对相应的成果享有所有权。

**当您 借鉴或引用文中的引言、切入点、思路、结论 进行二次创作并打算发行时，须注明链接出处**，否则我们保留追责的权利。对此如有疑虑请及时沟通，或在发行前将作品交由本人核对。

当您看见有人 [洗稿](https://baike.baidu.com/item/%E6%B4%97%E7%A8%BF/20862574?fr=aladdin)、[剽窃](https://baike.baidu.com/item/%E5%89%BD%E7%AA%83/9398357?fr=aladdin)、未经授权转载本专栏的文章内容时，请及时向本人举报。

最后感谢您对专栏内容的阅读和喜欢。
