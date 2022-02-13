---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/2073915486
author: 
---

# [重学安卓：过目难忘 Android GUI 关系梳理 － 小专栏](https://xiaozhuanlan.com/topic/2073915486)


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
> 我们绝不通篇贴源码，而是基于广泛的实践和反思，在累积过大量样本 乃至足以排除掉所有干扰信息后，**点到为止地揭露 各 GUI 组件 最为核心的本质**，方便您理解其真实的存在意义、捋清楚相互间 “层层递进” 的关系，乃至可以笃信地做出选择 和在项目中使用。

## 前言

从事 Android 开发 3~5 年，从 “纯业务开发” 进阶到 “细节定制工作”，难免高频接触 “自定义 View”、“GUI 性能优化” 等领域的概念和问题，

> 诸如 Canvas、Paint、Path、View、Drawable、Layout、Inflater、include、merge、ViewStub、PhoneWindow、ViewRootImpl …

网上介绍这些概念的文章数不胜数，甚至还有像上一期[《百闻不如一见的 视图系统 架构全貌》](https://xiaozhuanlan.com/topic/6420935178) 咱们在文末推荐的 “图文并茂” 自定义 View 教程，

那为什么还是让无数 刚刚想要进阶的小伙伴 感到困扰呢？

## Android GUI 系统仍在困扰着 80% 的进阶者

因素有很多，除了当事人 **缺乏 “从 0 到 1 完整踩坑” 的经历** 外，我们在互联网上所接触的内容也是一大原因 —— 多数网文 **只关心 “是什么、怎么做”，却对 “为什么” 绝口不提**，

这使得读者们首先就 **很难有机会搞明白**：为什么要阅读这些文章、为什么要学习这些技术点、每个技术的 “作用边界” 到底是从哪到哪、技术之间的 “关系和顺序” 又该如何串起来 … 乃至网上的教程 再好再多，也无从下手、无从看起。

> 与此同时，过往的文章中 我们多次提到，凡事唯有 **首先通过 “深度思考” 为自己打造一把 “知根知底” 的钥匙**，才有机会打开 新世界的大门，从而借助前人已铺设好的 "是什么、怎么做"，来玩转那个领域的技术，

因而这一期，我们继续以 “深度思考” 的方式，来 “**有层次感**” 地铺垫关于 Android GUI 系统的 **感性认知**。

考虑到这样 精心打磨的摆渡文 是看一篇少一篇，因而 **就算不去 hold 住面试官，也请务必跟随本文的脚步**，将 Android GUI 系统的来龙去脉 **无痛地过一遍**。

## 文章目录一览

-   前言
-   Android GUI 系统仍在困扰着 80% 的进阶者
-   是一分为二的理解方式
    -   谁才是可视化排版的根基？
    -   View 和 Drawable 不过是排版的模板
    -   Layout 和 Inflater 不过是后来者
    -   include，merge，ViewStub 是解药的解药
-   作为承上启下的小结
    -   上文的 "Window" 为何一直加个双引号？
    -   ViewRootImpl 是怎么帮 Canvas 与窗口对应上的？
    -   是对症下药的 排版渲染 性能优化指南
    -   PhoneWindow 本质 及 事件分发的内幕
-   综上

## 是一分为二的理解方式

阅读过 [上一期](https://xiaozhuanlan.com/topic/6420935178) 的小伙伴已确知：

> 视图系统主要包括 **排版 渲染** 和 **事件 分发** 这两大类工作。
> 
> 其中 客户端 运行在 “用户进程” 中，负责 **可视化内容的排版 和 向服务端输出**，以及 **接收来自服务端的事件 并在视图树中分发**；
> 
> 视图服务 则运行在 “系统进程” 中，负责 **对排版内容的渲染**，以及 **传递触控事件给客户端**。

考虑到 日常工作 和 进阶的困扰 主要集中在 **客户端 排版 API** 这一块，因而本文主要分为两个部分来讲解：

第一部分主要是 **单刀直入 “推理和解析” 排版的根基之所在**，以及 **层层递进的 “客户端 排版 API” 关系网**；

第二部分 我们再以 “特定视角” 来解析整个 Android GUI 系统。

## 谁才是可视化排版的根基？

早在[《重学安卓：Activity 的快乐你不懂》](https://juejin.im/post/5ce651d4f265da1bb13f0a5b)一文中，我们就对 Android 的可视化系统 做了一次简要的推理，

其实光是截取 “这篇文章的中间部分” 加以放大，就能 “单刀直入” 解决 80% 小伙伴 关于排版 的困扰 ——

我们不如从这篇文章的 “Window” 一节开始深入：

> “Window” 的存在，主要是为了管理 UI 内容的排版。

那么有没有读者 细思过这句话 —— 它作为管理者，其背后，究竟是 **哪位 “绘制者” 在被管理呢**？

"那时候" 并没有所谓的 View …… 那么究竟是谁在绘制？…

一路追根溯源，噢！原来是 Canvas ——

> Canvas 才是可视化排版的根基！**排版依赖于绘制** —— 在排版这件事上，Canvas 可以没有 View，但 View 不能没有 Canvas。

![图片来自 Unsplash，如图用户通过 Canvas 手绘了不规则图形](https://images.xiaozhuanlan.com/photo/2021/8b6903ebed027db8b696aeec84ff3f5d.png)

图片来自 Unsplash，如图用户通过 Canvas 手绘了不规则图形

Paint 和 Path 都是 Canvas 的小弟，真正 **与排版输出 存在直接关系的 最源头 API**，就是 Canvas 本尊。

## View 和 Drawable 不过是排版的模板

然而 既然是视图系统，那就不仅仅是原始、无序的 “绘制” 本身。

因而在有了 “Window” 和 Canvas 的基础上，还需确立 View / ViewGroup 这样规则的 **排版标准**，从而我们得以 在这套 **可拓展的标准** 的基础上，构建 **可复用的具体模板**。

> 例如 Button、TextView、ImageView，这些，都是 "可复用的模板"，**开发者日常只需与 上层的这些 View 打交道，而无需直接与 Canvas 打交道**、无需做什么都得先手动基于 Canvas 来写个几百行的排版代码。
> 
> 并且，如果对 现成的模板感到不满意，也可以自己再封装一个 新的模板，也即人们通常所说的 "自定义控件"。

与此同时，View 只是大致描述了 视图构建的模板，如果要更改 具体 View 的可视化细节，那岂不是又要接触 Canvas？

因此，同样是出于 **灵活性和复用** 的考虑，而衍生出 Drawable 的设计，它的存在是用于负责更具体的 可视化细节的模板：

例如通过 ShapeDrawable 来描述 **视图的轮廓和背景**、通过 StateListDrawable 来描述 **视图的点击效果** 等等，

使得我们绝大部分情况下 都能使用成熟的 Drawable 模板 来描述排版细节，避免良莠不齐的开发人员 因直接与 Canvas 打交道 而埋下的 不可预知的隐患。

> 划重点 👆 👆 👆

16、20、32dp 圆角

16、20dp 圆角

16dp 圆角

![](https://images.xiaozhuanlan.com/photo/2021/df4444edd0f2fb73e644149691331223.jpg)

![](https://images.xiaozhuanlan.com/photo/2021/a39c244b434a44a663067feb4378cf83.jpg)

![](https://images.xiaozhuanlan.com/photo/2021/d9aa8c6306b0ae6e2bb1c7f09ab6bb32.jpg)

> 图片截自 “小米天气” 客户端：是无处不在的、各种规格的圆角。

## Layout 和 Inflater 不过是后来者

所以 Layout 和 LayoutInflater 不过是在有了 View 和 Drawable 的基础上，才有的后来者。

说来你可能不信，可这也许真是 Google 宠 Android 开发者的证明 ~

接触过的 iOS 开发者多有抱怨，在 iOS 上写布局 实在酸爽，没有预览，全靠想象 …

> 是的，没错，Layout 对应的是 视图树；shape、selector 等 对应的是 Drawable —— Google 模仿微软 WPF 的设计，巧妙地通过 XML 的方式，来实现 **一目了然的 声明式编程**、**可实时预览的布局**，以及 向 MVC 模式致敬 的 **视图 和 视图控制器分离** —— 这些都极大地方便了开发者 创建和修改可视化内容。

然而，XML 声明式编程 在解决上述问题的同时，引入了新的问题：

1.**XML 排版资源的复用率极低**。例如，当项目中的控件 对圆角 shape 有新的规格要求时，哪怕不同规格之间仅仅是 圆角 dp 存在细微差异，也不能像动态代码那样 通过修改参数即可完成，而是需要 重新创建一个新的 shape Drawable 文件。

2.XML 的解析由 LayoutInflater 负责，LayoutInflater 是通过 **深度优先遍历** 算法解析 XML 来 **构建视图树**，从而当布局嵌套层次加深等原因导致的 View 个数过多时，XML 的解析就会愈加耗时。

## include，merge，ViewStub 是解药的解药

所以 XML 布局 其实只是一种 **充分非必要** 的 构建视图树的方式。

我们看到许多 东拼西凑 的性能优化网文，不分场合地兜售 include，merge，ViewStub，事实上 它们仅仅是用于 XML 布局的情况 —— **通过减少层级来 解决 inflate 耗时问题**。

像 [Telegram 源码](https://github.com/DrKLO/Telegram) 中，这种 全动态编码 的布局构建方式，就完全用不上 从 LayoutInflater 到 ViewStub 的这些技术。

> 划重点 👆 👆 👆

## 作为承上启下的小结

所以到此为止，对 "Window" & Canvas；View & Drawable；Layout & Inflater；merge & ViewStub 等 技术点 **层层递进的关系**，是不是也就做到 **心中有数** 了呢？

是的，无论 Button、TextView、ImageView，还是 MaterialButton、TextInputLayout 等等，无论其表面如何千变万化，都不过是 **为了在特定场景下 符合用户直觉 而衍生出的 新的、具体的 排版模板**，本质上都是 **基于 Canvas 去绘制 特定样式的 可视化内容**。

而 Inflate 也不过是发生在 当开发者选择通过 “声明式编程” 而非 “动态编码” 的方式 去构建视图树。所以若非使用 LayoutInflater，开发者甚至无须知道 include，merge，ViewStub 的存在。

![](https://images.xiaozhuanlan.com/photo/2021/123c9ba004c9e9e83eac64db1619c6ff.png)

好，经过上文这样分析和梳理一遍，相信大家已对 “View 体系” 的结构耳清目明，

因而下文我们继续专注于 让一些原本令人觉得 “复杂”、“避之不及” 的混沌系统 变得 “清晰”、“可控”，以便 **读者们 “有灵感、有方向” 觉得可以深入**、得以自行将 “网上现成的优质资源” 物尽其用。

所以，

## 上文的 “Window” 为何一直加个双引号？

上文我们打一开始就是从[《重学安卓：Activity 的快乐你不懂》](https://juejin.im/post/5ce651d4f265da1bb13f0a5b)一文的 "Window" 开始向上演绎，那么下文我们将反过来 向下追溯 Android GUI 的底层实现。

还是在该文中，我们介绍到，"是因为有了 窗口 乃至多窗口的需要，所以才有了 ‘Window’"；并且我们知道，一个应用程序通常可以有多个窗口，

于是基于 **设计模式之 最小知道原则**，我们可以推测，**通往底层的 窗口级 排版输出 API 有且只有一个**。

一路追溯 —— 原来是 addView

![](https://images.xiaozhuanlan.com/photo/2021/230b8d35a999d6386c58cf50169b483a.png)

这个 rootView，通常就是上文我们提到的 —— 构建的视图树。（当然它也可以不是层层嵌套的视图树，单个视图实例也是可以的）

而作为多窗口应用程序，我们是 **以窗口为单位来排版**，所以第二个参数就是 窗口本身的一些状态描述，包括窗口的 **宽高、方位、层级、特性** 等等。

与此同时，

> 前文我们已经提到，视图系统主要分为 **运行于客户端进程中的 视图接口**，和 **运行于 系统进程中的 视图服务** (关于为什么要分别运行在两种进程中，具体缘由的分析可回顾[《百闻不如一见的 视图系统 架构全貌》](https://xiaozhuanlan.com/topic/6420935178))，

所以可知，这里的 WindowManager 不过是 **客户端暴露给开发者的 窗口级排版输出接口**，从而，如果我们想知道 具体的排版环节 以及与服务端通信的内幕，我们可以从 addView 点进去一探究竟。

> 在[《百闻不如一见的 视图系统 架构全貌》](https://xiaozhuanlan.com/topic/6420935178)中我们介绍到，在 C/S 架构的背景下，客户端若要协调 **窗口级的 排版输出 和 事件输入**，需要有个 **窗口代理** 来承担这个职责。

于是从 addView 点进去，我们最终发现，在 Android GUI 系统中，ViewRootImpl 正是扮演着这样的角色，也即上文我们提到的带双引号的 "Window"。

## ViewRootImpl 是怎么帮 Canvas 与窗口对应上的？

在介绍 ViewRootImpl 之前，先简要介绍下服务端组件。

为了实现多窗口渲染，后台存在 SurfaceFlinger 这样的组件，来将多个窗口合并和渲染，这才有了 你能够同时看到的 Activity 和 浮在其上的 Dialog。

那么这里你一定有这样的疑问：

**客户端 onDraw 中的 Canvas，究竟是怎么与 服务端渲染的 surface 窗口 对应上的？**

从 addView 点进去我们可以发现，ViewRootImpl 在实例化时，会与服务端建立连接。随后，ViewRootImpl 会通过 setView 方法，来开始它的工作：

> 首先，在执行 performTraversals 方法的最初，ViewRootImpl 会通过 requestWindow 方法，向后台申请一块 Surface（具体的代码逻辑，大家可通过网上的 [源码分析文](https://blog.csdn.net/freekiteyu/article/details/79483406) 了解），
> 
> 随后，在遍历视图树 乃至执行 measure、layout、draw 三大排版流程的 draw 时，**视图树 "代代相传" 的 Canvas，正是来自这块 Surface**，而这块 Surface 终将因添加到 BufferQuene 而为 SurfaceFlinger 所渲染。

## 是对症下药的 排版渲染 性能优化指南

所以，抛开上文提到的 专用于 **inflate 方式构建视图树** 的优化方案 include + merge + ViewStub，

针对 排版渲染，有它自己的背景因素（VSync）和优化方案。

简单来说，**目标是让 每一次完整的 排版渲染 耗时小于 16ms**（以 60Hz 刷新率为例）。

最能体现这 "每一次" 的，就是滑动的场景：

> 基于[《不如我们 从零开始设计一套 视图系统》](https://xiaozhuanlan.com/topic/0162375948)一文我们已确知，**每一段看似连贯的滑动，其本质都是 无数次的 输入和输出 的交替运作**，也即，本质上是 视图树旗下各元素的 坐标参数 在不断发生变化，从而整个视图树的 排版渲染工作 在不断地 执行、执行、再执行。

也许你的手指只是轻轻往上一划，可这一划却 触发了 Scroller 或 Animator 等组件 基于自身计数的套路 来调用 视图的 invalidate 或 requestLayout 方法 —— 带动了长达 1 秒的 "**惯性滑动**"，从而这期间 人脑感觉的滑动流畅，其本质是 **60 次 排版渲染 的完整执行**。

划动

GPU 排版渲染呈现

GPU 过度绘制调试

![](https://images.xiaozhuanlan.com/photo/2020/56441dc77ddced332e72423fb5bc7d70.gif)

![](https://images.xiaozhuanlan.com/photo/2020/f33a0ca69ca82518619afcef69975b24.gif)

![](https://images.xiaozhuanlan.com/photo/2020/6862d3cd7847e1030731fa6022f91224.gif)

> 图片截自 2020 年初的 “酷安” 客户端。
> 
> 如图，可以发现，在划动的场景下，部分页面因没有 100% 满足 **每秒完整的 60 次渲染**，而在列表滑动的过程中 存在 **“一跳一跳” 的顿感**。

所以 include + merge + ViewStub 能救场吗？未必能。因为对 单视图树的窗口 来说，**构建视图树 是一开始就完成的、一次性的 初始化工作**，后续 一再地排版和渲染，都没 inflater 的事儿了。

所以此处就别再提这仨了，而是对症下药 ——

早在 2015 年，Google 在 Udacity 的课程 就有分享 排版渲染 **性能优化的思路**，这里贴一份 [博客版链接](http://hukai.me/android-performance-memory/)，不再累述。

## PhoneWindow 的本质 及 事件分发的内幕

作为窗口代理，ViewRootImpl 除了 为排版输出 "代言"，还扮演着传递事件的角色 —— 服务端传回的事件，ViewRootImpl 会首先传递给 "setView 时期注入到自己内部的那个” rootView。

那 Activity 啥情况呢？其实简单，Activity 的 rootView 是 DecorView，

而 DecorView 拿到 ViewRootImpl 发给它的事件，**若直接向下分发，那就没 Activity 啥事儿了呀** —— 事实上，Activity 自己也想作为事件分发的一环，毕竟如果下面的视图都没有消费的，那可不就回归给 Activity 这个视图控制器来消费么。

那怎么办呢？那就创建一个 “Window”，**专门用于约定一套窗口事件分发的标准**，这套标准由 PhoneWindow 来代理，由 Activity 来传承，从而 DecorView 作为 PhoneWindow 的内部类，能在自己内部去 将事件分发给 被注入到 PhoneWindow 内的 Activity，而 Activity 作为 Window 的传承，又能继续走自己实现的分发标准，从而导致了，事件从 DecorView 开始 上 Activity 绕了一圈，然后才从 DecorView 继续往下执行事件分发。

![](https://images.xiaozhuanlan.com/photo/2021/6c0c33007525aeb3c8dea6b19c974269.gif)

所以这里所谓的 PhoneWindow，它同样只是个代理，只不过 ViewRootImpl 是最源头的代理，而它是最末端的代理。

> 划重点 👆 👆 👆

至此，Canvas 与 Surface；ViewRootImpl 与 PhoneWindow 的关系，相信小伙伴们已梳理清楚。

## 综上

视图系统主要负责 **作为输出的 排版渲染** 和 **作为输入的 事件分发** 这两大类工作。

其中，客户端主要负责 **排版 和 分发事件**，服务端主要负责 **渲染 和 传递事件**。

**排版的根基是绘制**，绘制主要由 Canvas 以及 Paint、Path 来负责。

Canvas 之所以能绘制到对应的窗口，是因为它本身就来自对应的 Surface。

Surface 由窗口的源头代理 ViewRootImpl 申请得到，并通过添加到 BufferQuene 而得以被后台的 SurfaceFlinger 合成和渲染。

View 和 Drawable 是 Android 可视化系统的 排版标准/模板，**Canvas 表现力的上限决定了 View 和 Drawable 表现力的上限**。

XML 和 LayoutInflater 是 **充分非必要的** 用于支持 声明式编程 的 **视图树构建工具**，它们在 解决实时预览需求的同时 引入了性能的问题。

include、merge 和 ViewStub 是优化 inflate 性能问题的工具，**是工具的工具**。

抛开构建视图树，排版过程主要包括 measure、layout、draw，而在 VSync 定时刷新的背景下，**要求 每一次完整的排版渲染流程，要在 16ms 内完成，以确保流畅的滑动等体验**。因此需要结合 Google 给出的 排版渲染 性能优化建议，来 有的放矢地解决 卡顿 等问题。

最后，ViewRootImpl 和 PhoneWindow，二者都是窗口的代理，只不过 **ViewRootImpl 是最前线、最源头的窗口代理**，负责与视图服务通信、兼顾输出和输入；**PhoneWindow 是末端的窗口代理**，主要负责 为窗口的事件分发确立标准。

> **便捷链接：**
> 
> **[基本功：是随时随地可受用的 深度思考原则](https://xiaozhuanlan.com/topic/9837051426)**
> 
> [GitHub : 重学安卓 配套项目](https://github.com/KunMinX/Relearn-Android)
> 
> [GitHub : Jetpack-MVVM-Best-Practice](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)
> 
> **视图系列文章：**
> 
> [重学安卓：从 0 到 1 “自定义视图” 完整爬坑顺序](https://xiaozhuanlan.com/topic/9361075842)
> 
> [重学安卓：滑动冲突 的快乐你不懂！](https://xiaozhuanlan.com/topic/8796215034)
> 
> [重学安卓：不如我们 从零开始设计一套 视图系统](https://xiaozhuanlan.com/topic/0162375948)
> 
> [重学安卓：百闻不如一见的 视图系统 架构全貌](https://xiaozhuanlan.com/topic/6420935178)
> 
> [重学安卓：过目难忘 Android GUI 关系梳理](https://xiaozhuanlan.com/topic/2073915486)
> 
> [重学安卓：一通百通 “声明式 UI” 扫盲干货](https://xiaozhuanlan.com/topic/2356748910)

## 版权声明

> Copyright © 2019-present KunMinX

**本文为专栏私有内容，谢绝转载**。

读者订阅了专栏，即享有了专栏文章的阅读权。

文章 **原创的引言、切入点、思路、结论** 凝聚了作者 KunMinX 本人的心血 及与参与者互动演化的结果，作者本人及作者在文中提名的有效贡献者 对相应的成果享有所有权。

**当您 借鉴或引用文中的引言、切入点、思路、结论 进行二次创作并打算发行时，须注明链接出处**，否则我们保留追责的权利。对此如有疑虑请及时沟通，或在发行前将作品交由本人核对。

当您看见有人 [洗稿](https://baike.baidu.com/item/%E6%B4%97%E7%A8%BF/20862574?fr=aladdin)、[剽窃](https://baike.baidu.com/item/%E5%89%BD%E7%AA%83/9398357?fr=aladdin)、未经授权转载本专栏的文章内容时，请及时向本人举报。

最后感谢您对专栏内容的阅读和喜欢。