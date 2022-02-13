---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/5860149732
author: 
---

# [重学安卓：柳暗花明 Jetpack Navigation 打开方式与缺陷分析 － 小专栏](https://xiaozhuanlan.com/topic/5860149732)


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
> 我们绝不通篇贴源码，而是基于广泛的实践和反思，在累积过大量样本 乃至足以排除掉所有干扰信息后，**点到为止地揭露 Navigation 框架最为核心的本质**，方便您理解其真实的存在意义，乃至可以笃信地将其用在项目中。
> 
> 关于 Navigation 在项目中的实践，请另行查阅[《GitHub : Jetpack-MVVM-Best-Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)源码以及[《架构模式实践》篇](https://xiaozhuanlan.com/topic/8204519736) 的完整解析。

## 前言

上周我在新项目中用上了 Jetpack Navigation，对于它的 **声明式设计** 十分喜欢。

没想到，就在用得正爽的时候，发现它在 navigate 方法中的实现，居然是通过 replace 完成页面跳转的，除此之外，就没有 add hide 的方式。这导致了 **页面返回时，每次都要重绘，大概率造成转场时的卡顿**。

在 google sample 的 issue 中发现一年前有人提过这个问题，但官方并没有解释这么设计的缘由。

于是我转而去咨询今天要介绍的一位嘉宾，看看他是怎么看的这个设计，以及如果想通过 Navigation add hide，除了重写代码，还有没有别的招。

没想到，**当天下午提的问题，当天晚上就收到了详尽的回复**。（不要慌，关于此嘉宾以及解决方案，文末给出）

> 考虑到我在公开场合发文一贯秉持的原则是：为读者 **首先明确状况、减少困扰、建立感性认识**。
> 
> 因而，我绝不会一上来就解释针对 replace 的问题我们该怎么做，而是从 Navigation 的 **存在缘由、设计依据、职责边界** 开始介绍，这样才能帮助更多的人 **首先理解状况**：

为什么要存在 Navigation？

Navigation 的存在是为了解决什么问题？

之前究竟是遇到了什么问题？

Navigation 引入后又发现了什么新问题？

……

![图例来自官网](https://images.xiaozhuanlan.com/photo/2019/6824fbc766a6304da144bf60cb01dd3c.png)

图例来自官网

## 文章目录一览

-   前言
    
-   Navigation 的目标主要有 3 个
    
-   Navigation 问世前的混沌世界
    
-   Navigation 为什么能解决这三个问题？
    
-   那 Navigation 具体是依赖什么机制运作的呢？
    
    -   1.定义声明式编程协议
    -   2.抽象和封装控制器代码
    -   3.对作用域的补充说明
-   引入 Navigation 后的世界
    
-   综上
    
-   作为压轴分享的痛点解决方案
    
-   **Note 2019.8.7 加餐：**
    
    -   作为额外附赠的使用说明
-   **Note 2019.11.4 加餐：**
    
    -   对官方使用 replace 的缘由的理解 和 专属解决方案
-   **Note 2020.3.31 加餐：**
    
    -   不建议通过复用 View 的方式优化 onCreateView 的缘由
-   **Note 2020.07.16 加餐：**
    
    -   GitHub 上的 Navigation Add Hide 修改版的致命通病 和 独家解决方案
-   \*\*Note 2021.07.10 加餐：\*\*
    -   Navigation 有什么难以替代的好处？

## Navigation 的目标主要有 3 个

在《重学安卓》的前几期中，我们分别在

> [《Activity 任务和返回栈》篇](https://xiaozhuanlan.com/topic/7812045693)、[《Intent》篇](https://xiaozhuanlan.com/topic/2869301475)、[《Fragment 本质》篇](https://xiaozhuanlan.com/topic/0937256481)

中深入介绍了 Activity 和 Fragment **由于生存环境、沟通目标的差异，而在** “页面管理” 和 “路由跳转机制” 的 “设计依据、职责边界、乃至相互间关系” 上 **存在的区别**。

相信阅读完这几篇的朋友，对 Activity 和 Fragment 在 “页面管理、路由跳转” 等方面存在的差异已有深刻印象，

而我们今天要介绍的 Navigation，正是基于上述分裂的环境，为解决 “应用内导航” 的问题而存在。

它的目标主要有 3 个：

> 1.通过声明式编程，来确保 “应用内导航” 的一致性。
> 
> 2.通过可视化编程，来直观地反映页面的路由关系。
> 
> 3.通过抽象，来整合 Activity 和 Fragment 的路由跳转代码。

如果光是阅读了以上三点，你还是不太理解的话，那接下来我就分别介绍 99% 网文都不曾介绍的真实状况，来方便你迅速建立起感性的认识。

## Navigation 问世前的混沌世界

在 Navigation 出现之前，想必大家用得最多的就是 YoKey 大神的 Fragmentation。

鉴于早期 Fragment 的坑防不胜防，而大家平时工作又没时间去确认状况，因而多是急忙地用上 Fragmentation “保平安”。

考虑到现如今 Fragment 的 bug 早已被修复，因而从适配 AndroidX 开始，我便转而采用原生的办法实现路由管理。例如：

![](https://images.xiaozhuanlan.com/photo/2021/723e892ddd516c3ddf9e78c2b2f67cb2.png)

那这样造成什么问题呢？

例如开发一款音乐播放器，在首页和专辑列表页 等多个源 Fragment 中 需要跳转到该页面，那么在每个源页面中 我都需要动态注入同样的 转场动画 等 样板代码，**那么日后随着源页面的激增，当源页面 A 中改了转场动画，源页面 B 中忘了改转场动画，就会造成不一致的问题**。

![](https://images.xiaozhuanlan.com/photo/2021/d61a47bb1922e595d8117a1e75583321.png)

> 如果你对 “一致性” 的概念不太理解的话，那么请回顾一下大学时期 数据库课程中提到的 “存储过程”，它就是 **典型的 “一致性” 封装**：将一套连贯的 SQL 序列封装在一个 “API” 中，外部人员只需调用这个 “API”，而无需知道内部具体发生了什么。
> 
> 这避免了程序员 A 修改了某 SQL 语句，而程序员 B 没跟上。并且，“API” 的管理人只需一处修改，就能做到处处生效。

此外，一个项目的 Fragment 可能会很多（例如在上周的新项目中，我新建了多达 25 个页面），日后接手项目的同事，在缺乏文档的情况下，很难第一时间掌握项目状况、并定位到问题所在页面：

顺藤摸瓜、通过 manifest 和 布局文件找到程序入口和对应的 Fragment，并不总是最优的办法。

再者，如上一节提到的，Activity 和 Fragment 的生存环境不同，**Activity 因为是组件，可被允许和其他 App 的组件通信，而在设计之初就考虑到 是面向跨进程通信的组件化管理**，那么无论是任务栈、返回栈管理，还是路由跳转的设计，都和 Fragment 有着天壤之别。

因而，就应用内的导航来说，如果你要允许 Activity 也来专职视图控制器，那么它同样也会遇到上述 “样板代码” 的问题。并且，将 “应用内导航” 的工作托管给两套 API 显然是不方便的。

**Navigation 正是为解决上述三个问题而存在。**

## Navigation 为什么能解决这三个问题？

在 [《你用不惯 RxJava，只因缺了这把钥匙》](https://juejin.im/post/5cb82a42e51d456e62545ac6) 中我们介绍到，RxJava 操作符同 SQL 一样，本质上是声明式编程，也即你 **只需告诉后台要做什么，而无需告诉后台怎么做**。

**“怎么做” 的逻辑已经在后台统一封装好，你只需遵从协议**（编程语言的本质就是一种协议，操作符是一种协议，SQL 是一种协议，Navigation Graph 也是种协议）**来定义你的声明、并在恰当的地方调用即可，后台会自动根据声明来执行相应的代码**。

正因为是声明式编程，所以你可以用统一的 XML 声明，去匹配不同的 Java 实现。所以这让 Activity 和 Fragment 的路由管理的抽象成为了可能。

并且，正因为是声明式编程，所以更方便要求你填写特定的属性，以便让后台支持如 “可视化编程” 这样功能的展现。

## 那 Navigation 具体是依赖什么机制运作的呢？

首先，既然要声明式编程，那么第一步要做的当然是

### 1.定义声明式编程协议

我们不妨站在源码设计师的角度来想一想，路由跳转从抽象意义上讲，都包含哪些必要的元素呢？

—— **源地址，目标地址，携带参数，转场动画，启动模式**。

主要是上述这 5 个部分，因而我们先定义出

**fragment、action、argument** 这三种元素。

> 其中，fragment 元素对应着一个源 Fragment，

需要属性 name 来指明源地址；

需要属性 id 来帮助其他 Fragment 的 action 元素链接到自己、从而帮助后台找到自己。

同时，每个 fragment 需要包含 action 元素 和 argument 元素，来分别描述 fragment 可以执行跳转的目标，以及携带的参数。

> 在 action 元素中，我们

需要属性 destination 来指明前往的目标 id；

需要 popUpTo 来指明需要跨级返回的源 id；

需要 诸如 launchSingleTop 的属性来表明页面的启动模式；

需要 anim 属性来声明转场动画；

需要属性 id 来帮助后台发现和执行这个 action。

> 在 argument 元素中，我们

需要 name 属性来描述参数名；

需要 argType 属性来描述参数类型；

需要 defaultValue 属性来描述默认值，当没有传参的时候。

![](https://images.xiaozhuanlan.com/photo/2021/d9f8fdaed2fadbb8053db07e123e3523.png)

看，这样梳理一下，是不是很简单呢。

同时，不要忘了，为了支持 “可视化编程”，我们还需在 fragment 元素中定义 tools:layout 属性，这样我们就可以在 Android Studio 中预览这些 fragment 的关系。

![图例来自官网](https://images.xiaozhuanlan.com/photo/2019/7db0d885cd737725b9aa33d33d5c3d70.png)

图例来自官网

### 2.抽象和封装控制器代码

就像页面的布局文件，在 Activity/Fragment 启动时，被统一的控制器遍历和加载，Navigation 也是类似的道理。

为了能够基于声明式编程，我们需要完成的工作如下：

1.为了管理所有的目标，我们需要 **装载声明、创建目标、导航管理**。

2.为了在不同的作用域下管理目标，我们还需要 **管理作用域**。

3.为了实现 “无差别” 的路由导航，我们需要 **抽象导航者**，以便整合 Activity 和 Fragment 的导航。

> 因此，

我们需要准备一个 **NavController**，来专职声明的遍历和装载。

被装载的实际上是一个个被实例化的 **NavDestination**，NavDestination 中务必包含来自 fragment/activity 元素、action 元素、argument 元素的信息。

而且，这些 NavDestination 需要按 “作用域” 来划分管理，也即我们需要寻找一种方式（例如 Map，或者下文描述的方式）来管理作用域、每个作用域分管旗下的 NavDestination。

导航当然也是通过 NavController 发起的，但考虑到它可能有多个手下（谷歌封装好的 ActivityNavigator、FragmentNavigator，以及将来我们自己重写的 Navigator），那么它就需要被设计成是调用抽象接口方法，让具体的 Navigator 实现者去实现具体的路由跳转。

那根据什么来判断选用哪个 Navigator 实现者呢？很简单，根据 NavDestination 对应的是 fragment 还是 activity 元素来判断。

或者，更简单地，直接将创建目标的职责，也下发到每个具体的 Navigator 来实现，因为很可能将来 Navigator 不只有 ActivityNavigator、FragmentNavigator，也可能有传说中的 ViewNavigator 呢。

### 3.对作用域的补充说明

第二小节中提到了作用域。

> 作用域是什么状况呢？为什么会考虑到作用域呢？

例如还是上周接手的这个项目，ContainerFragment 的设计是，包含 FrameLayout 和 BottomNavigationView，也即上边是用于装载 4 个 childFragment，下边是导航栏。与此同时，ContainerFragment 和其他同级别的 Fragment 是装载在 MainActivity 中。

所以这里就存在两个不同的作用域。如果我将 childFragment 和 ContainerFragment 混在同一个作用域中，那么我执行 childFragment 的跳转时，会把 ContainerFragment 直接给覆盖掉，就看不见导航栏了。

> 这也是作用域、及作用域管理存在的缘由。

实现作用域也很简单，有几个作用域，就准备几套声明式 XML。这些 XML 统一存放在 res/navigation 目录中，供不同作用域的 NavController 调取和装载。

于是我们还需要 **在作为 Root 的 Activity 或 Fragment 布局中** 定义一个 fragment 元素，这个 fragment 元素对应的 NavHostFragment 专门用于支持当前作用域 fragment 的 replace、add、hide。

换言之，有多少个作用域，就有多少个 NavHostFragment，每个 NavHostFragment 对应一个 NavController，分别管理自己旗下的 NavDestination。

> 划重点 👆👆👆

![](https://images.xiaozhuanlan.com/photo/2021/0787bf824063303aae32e1e6d3d279db.png)

注意：此处有个精妙的设计细节是：当我们为该 fragment 元素设置 defaultNavHost 设置为 true 时，当即宣誓了返回键会被拦截，直到 NavHostFragment 中没有可回退的 Fragment 才跟随 Activity 退出应用。

## 引入 Navigation 后的世界

是的，从协议的定义，到后台控制器的封装，声明式编程的整体逻辑就是这样啦。

至此，我已经为你就 “Navigation 为何存在” 明确了状况，相信 **在感性认识的加持下**，你能够 **有灵感、有方向地去深究源码**。

这篇文章写于 2019 年，但我相信就算过了 N 年后，这篇文章仍然有效。

为什么呢？因为具体的源码实现可能会变，但抽象的思想却能长久。包括无处不在的模板方法模式、观察者模式等等。理解了这些抽象的东西，那么就可以秒懂源码设计 —— 甚至不看源码，光是猜，也能猜个八九不离十。

话虽如此，我还是想贴个源码让你一睹为快：

![](https://images.xiaozhuanlan.com/photo/2021/012dc08fb7e1d6f6e830e2ff9a095781.png)

对的，你没看错，就这么一句话。

其中，findNavController(view) 是 Navigation 为了根据传入的 view 找到当前页面所属的作用域，好在该作用域中完成页面的路由跳转。

## 综上

Navigation 存在的缘由，是为了解决：

> 1.通过声明式编程，来确保 “应用内导航” 的一致性。
> 
> 2.通过可视化编程，来直观地反映页面的路由关系。
> 
> 3.通过抽象，来整合 Activity 和 Fragment 的路由跳转代码。

-   为了实现声明式编程，Navigation 按照常规的方式，分别定义了协议、和封装了控制器。
-   考虑到管理作用域的需要，因此实现了 Navigation。
-   考虑到 作为被管理的作用域的需要，因此实现了 NavHostFragment。
-   考虑到 装载声明 和 作用域分管的需要，因此实现了 NavController。通过 Navigation.findNavController(view) 可以根据 View 找到当前页面所在的 NavHostFragment（作用域）的 NavController。
-   考虑到 作为被管理的目标的需要，因此实现了 NavDestination。
-   考虑到 创建目标、路由跳转方案分治的需要，因而抽象了 Navigator。

最后，考虑到流程图可以更直观地反映相互间关系，因而作为总结，这里我额外地为你准备了一张依赖关系图：

![](https://images.xiaozhuanlan.com/photo/2021/e5e96833b674e4aa8f92c32be0e3ab0b.png)

## 作为压轴分享的痛点解决方案

那么话说回来，开头我要介绍的嘉宾，以及痛点的解决，是什么状况呢？

就在上周，Navigation 用得正爽的时候，没想到官方对 fragment navigate 方法是设计竟是通过 replace 完成的。**这造成了每次从二级 Fragment 返回一级的、承载多个 childFragment 的 ContainerFragment 都要被重绘，也即造成了转场的卡顿**。

这体验真是糟得不要不要的。

于是我去 google sample 的 GitHub 仓库浏览 issue，发现 1 年前就有人提过这个问题，但官方未解释这样设计的缘由。

[https://github.com/googlesamples/android-architecture-components/issues/414](https://github.com/googlesamples/android-architecture-components/issues/414)

迫于时间紧急，于是我去请教 Drakeet，看看对此他有没有什么好招。

没想到当天下午提问，当晚就收到了回复，**还为我找到了现成的、可 add hide 的 FragmentNavigator 的实现**。（其实就像上一节提到的，正因为 NavController 对 navigator 采取的是抽象的调用，使得我们有机会重写和替换上自己的 FragmentNavigator。正所谓 “对扩展开放、对修改关闭”，无处不在的设计模式原则的强大魅力）

所以也是十分感谢 Drakeet 的这个分享 ~

他负责的频道是我唯一订阅的一个频道。我也不介意在此推荐给大家。因为他和我的目标不同。

他的目标是分享 难得一见的、软件开发中的技巧，以及 **反破解的安全知识**。

我的目标是分享 仅此一家的、**经过深度思考的、体系化的背景知识**，做到就算只读这一篇、你也没白来；就算以后安卓没落、你也能因为对移动开发透彻的理解、而快速迁移技术栈到 Flutter 或其他。

以下是答复的链接，我已设置为公开可见，请慢用

[KunMinX 提问：上周我在新项目中用上了 Jetpack Navigation ...](https://images.xiaozhuanlan.com/photo/2019/ba83fce24e5c6b643264745604cd98b6.jpg)

## Note 2019.8.7 加餐：

### 作为额外附赠的使用说明

  
点击查看详细内容

此外，

关于 Navigation 具体的使用，推荐参考官网的一手资料（以下链接可直接访问，同时可能存在一定的文档更新延迟，有条件的小伙伴可自行访问 com 域名官网）：

[https://developer.android.google.cn/guide/navigation/](https://developer.android.google.cn/guide/navigation/)

关于 Navigation 等其他架构组件的 gradle 依赖配置信息，我专门为你找到了这个页面，请收藏好：

[https://developer.android.google.cn/topic/libraries/architecture/adding-components](https://developer.android.google.cn/topic/libraries/architecture/adding-components)

其中 Navigation 的依赖配置信息在这一页：

[https://developer.android.google.cn/jetpack/androidx/releases/navigation](https://developer.android.google.cn/jetpack/androidx/releases/navigation)

……

如果时间实在短缺 而无暇阅读英文的话，我也不介意在这里 顺带提一提 最基本的使用。

其实在经过上述的思考之后，使用步骤你也能猜个八九不离十了。就算还没用过 Navigation，也八成能感受到：这是个简单易用的应用内导航工具 —— 它本来就是为标准化而生。

**Step1**，你需要准备 >= 3.5 版本的 Android Studio，因为从这个版本开始，才支持 Navigation 工具集。

**Step2**，通过上述链接拿到 Navigation 的依赖配置项，在 gradle 中配置。

**Step3**，就像往常一样，你准备好相关的 Fragment。每个 Fragment 都务必引用不同的布局文件。

**Step4**，在 res/navigation 目录下创建 nav\_graph.xml。在这里，你有两件事可以做，第一件是点击如图 “+” 按钮，往 NavGraph 中添加 fragment 元素。第二件是将任一 fragment 预览图右侧悬浮的蓝色按钮拖放到目标 fragment 之上，来完成 “源 -> 目标” action 的生成。

在 Step3 中要求引用不同的布局，缘由是，Android Studio 是根据 layout 来预览不同的 Fragment，并且在添加 Fragment 到 NavGraph 时，Android Studio 是根据 layout 文件名来识别 id 的，若不同 Fragment 用了同一个 layout，你在列表中就识别不出同名文件哪个是哪个。

![](https://images.xiaozhuanlan.com/photo/2019/9bb9631ace2f8ba975e2b0868641b8a3.png)

**Step5**，你可以从 Design 模式切换到 Text 模式，在 action 元素中补充转场动画等属性。

**Step6**，在作为根的 Activity 布局中添加 fragment 元素，像《对作用域的补充说明》一节提到的那样，注意指明 fragment 元素的 id、name、defaultNavHost、navGraph，这一切都是为了 Activity 在装载布局时，能把 NavHostFragment 装载进来，并且 NavHostFragment 的 NavController 能通过 nav\_graph.xml 去装载该作用域内的 NavDestination。

**Step7**，在页面中需要跳转的地方使用跳转 API，包括 navigate，navigateUp。具体参见《引入 Navigation 后的世界》一节。

无论是在 action 元素中定义了 destination 属性还是 popUpTo 属性，navigate 方法都能精确地为你跳转到或回退到指定的页面。而 navigateUp 就只是返回上一级。

Bingo ~ 基本的使用步骤，最多最多也就这 7 个。

## Note 2019.11.4 加餐：

### 对官方使用 replace 的缘由的理解 和 专属解决方案

  
点击查看详细内容

> 非常感谢读者 Darren 于 2019.11.14 与我分享了 他在使用过程中发现的 Navigation 设计为 replace 的缘由。

Navigation 之所以将 Fragment 的路由导航编写为 replace 而不是 add hide 的方式，或许是考虑到，从次级页面回到上级页面时，**数据变动展示的需要**。

也即，若 detail 页面向 list 页面发起了回调的通知，一个 **符合直觉的、优秀用户体验的** 做法是，待回到 list 页面后，为用户呈现 UI 上的变化，例如 RecyclerView 在此时方才去执行 notifyItemInsert。

如果我们采用 add hide 方式，list fragment 在 hide 的情况下，仍然处于 resume 状态，那么当 liveData 通知它时，它会实时地做出响应，而不会采取滞后的方式。

为了防止 add hide 情况的实时响应，有个办法是在 fragment 的 onHiddenChanged 方法中手动走非 onResume 的生命周期，但此处有个问题是，当你通过 home 键回到桌面再回来时，系统会帮你的 fragment 重走 onResume，那么 **手动控制生命周期的做法就白费了**。

于是这也就是解释了为什么官方将 frgament 路由导航的方式默认设置为 replace。

但 replace 意味着走 unAttached，也即当回到 list 页面时，会重走 onCreateView。**我们不能将 fragment 在 onCreateView 或 onViewCreated 方法中构造好的 View 或 binding 直接注入 ViewModel 来实现托管，因为这样就破坏了单向依赖，使 ViewModel 持有了 Fragment 的成员，有内存泄漏的风险**。

因此我想到的一个最简单的办法是，从 单 Activity 架构变回传统的 多 Activity 架构。

正如我在[《我的碎片很听话，你的 Fragment 有自己的想法》](https://xiaozhuanlan.com/topic/0937256481)这篇文章中提到的，Activity 和 Fragment 的设计目标不同，且 **Activity 独立持有 Window 资源，因而上级 Activity 被压栈时，会进入 onStop 状态，而并不会像 Fragment 那样为了节省宿主的资源，而走 onDestroyView 去清空视图**。

所以，此处我们至少可以界定了，问题的 **根源的根源在于 我们需要营造一个符合直觉的用户体验**。在 LiveData 面市之前，我们要达成这个，只能靠 EventBus 的粘性和 Activity，在 LiveData 面市后，我们依靠 LiveData 和 Activity 就能完成这个。

Navigation 的存在，主要是为了解决 应用内导航的一致性问题，所以通过 replace 来解决 Fragment 的路由跳转，从大局来看，已是最好的选择了。

实在想解决页面返回时重新 inflate 的问题，可以在 Fragment 子类中声明一个 View 的引用，并将初次 onCreateView 时 inflate 的 View 赋值给它，如此可以在页面返回时的 onCreateView 环节直接复用该 View。

## Note 2020.3.31 加餐：

### 不建议通过复用 View 的方式优化 onCreateView，缘由如下：

  
点击查看详细内容

1.在有 DataBinding 的参与下，需要在 DataBindingUtil.bind View 之前，手动为 View setTag（赋予的 tag 内容详见 [《从被误解到 “真香” 的 Jetpack DataBinding》](https://xiaozhuanlan.com/topic/9816742350)），或者直接用 DataBindingUtil.inflate，**但既然需要手动配置和判空，就造成了一致性问题**。 2.同时，保留 View 引用的做法也会造成 ViewPager 等控件的 **“布局语法糖” 失效**（指 仅在 inflate 时会走的装载 child 等语法糖）。 所以，随着 AndroidX 对 Activity 和 Fragment 功能的通用化重构（详见 [《我的碎片很听话，你的 Fragment 有自己的想法》](https://xiaozhuanlan.com/topic/0937256481) 评论区的解析），期待这些问题在后续能够 “不了了之” 地得到化解。

## Note 2020.07.16 加餐：

### GitHub 上的 Navigation Add Hide 修改版的致命通病 和 独家解决方案

  
点击查看详细内容

### GitHub 上的 Navigation Add Hide 修改版的致命通病 和 独家解决方案

感谢读者 @孙致远 于 `2020.07.09` 的反馈和提供的测试 Demo，经检测，目前为止 GitHub 开源的 Navigation Add Hide 修改版 均存在 “popUpToInclusive 越级返回时，未正确计数和出栈 Fragment，导致加载了预期外的 Fragment” 的现象。

基于对该 Demo 的调试，目前已将完美修改版抽取为 Smooth-Navigation 单独维护，后续可直接以远程依赖的方式将 Smooth-Navigation 引入到个人项目。

[https://github.com/KunMinX/Smooth-Navigation](https://github.com/KunMinX/Smooth-Navigation)

## Note 2021.07.10 加餐：

### Navigation 有什么难以替代的好处？

  
点击查看详细内容

正文我们提到了 “Navigation 主要是为了解决 3 个问题”，事实上，基于第一性原理来看，如果要进一步追溯有什么是难以替代的好处，那就是 “**确保初始传参的一致性**”，

> 因为我们知道，Fragment 唯有通过 bundle 传参，在旋转屏幕等环境重建的情况下，初始值才得以保留，而我们经常能在项目中看到，直接通过 Fragment 构造方法传参的写法，这为旋屏重建获得初值埋下了一致性的隐患，毕竟公司项目不同页面可能不同人写的，有的页面遵循了，有的页面没遵循。

那么如何彻底解决这个问题呢？—— 通过 Navigation：

在使用 Navigation 过程中，**使用者无法直接接触 Fragment 实例**，初始值只能通过给 navigate 方法的 args 参数传 bundle，如此来确保了 “初始传参的一致性”。

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

