---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/6105792348
author: 
---

# [重学安卓：提神醒脑 MVP、MVVM 关系精讲 － 小专栏](https://xiaozhuanlan.com/topic/6105792348)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

## 前言

大家好，我是[《Jetpack MVVM 精讲》](https://xiaozhuanlan.com/topic/0129483567)的独立原创作者 KunMinX，GitHub star 8.7k，专注深度思考和 Jetpack MVVM 的分享。

关于 MVP 和 MVVM 本质和区别的文章，本来我是不想单独写的，因为经过长达一年的耳濡目染 和对方法论的试炼，相信 **但凡沉下心阅读过**《重学安卓》体系化文章的读者，多已练就 **透过表象迅速抓住本质** 的稀缺能力。

专栏每天都有新的读者加入，然而没想到的是，1 年了，仍然时不时的会被咨询、或是在各个社区看到人们众说纷纭地在谈论 MVP 和 MVVM 谁 “好” 谁 “坏”。

## 并不是每一个提问都值得被回答

> 爱因斯坦说，“提出正确的问题，问题就已解决了一半”。

换言之，并不是每一个提问都值得被回答。**一次提问所包含的信息量，其实远远超出内容本身**。

透过提问者的提问，几乎可以瞬间感知到，提问者对事实状况的掌握程度，并由此来决定到底值不值得继续交流。

“高” 质量的提问交流 让人感到如沐春风 —— 几句话就先自己把背景交代清楚，然后就某个细节提出自己的困惑 —— 这让我不免想要与之多聊几句，把我知道的毫无保留地分享出去。

反之，“低” 质量提问 让人感到 **不舒服，甚至不对劲** —— 明明不遗余力地在多处 **划重点 反复交代**，明明白纸黑字写得清清楚楚，甚至段落、链接给他指出来，却视而不见，就好像从未发生过。

更有甚者，为了满足抬杠的快感，不惜浪费彼此的时间 ...

> 注：我从不在技术交流中使用 “高”、“低”、“好”、“坏”、“轻”、“重” 之类的主观描述，此处只是以多数人方便理解的方式来介绍。文中使用到的主观描述一律加上双引号。

![](https://images.xiaozhuanlan.com/photo/2021/c967e971669b96c5fb19b9eaaef4ad22.png)

## 本质和区别，我只说一遍

事实上，我并不会去判断来者是否是来抬杠，只须透过对方所说的话，即可瞬间判断对方说的是事实，还是自顾自地扯淡 —— **你没法和一个前来扯淡的人交流**，你会发现 这种对话往往 **存在巨大的代沟**，并且抬杠者无意谋求和缝合对事实的理解，他本来就是为了 “来得快” 的快感而来。

> 不同于主观的偏好取向，**事实必有特定背景和目的来约束**，一切脱离事实特征的意见和观点都是瞎扯淡，没有讨论的前提、不值得参与 —— ©KunMinX

所以，本文只写给那些 真的想搞清楚事实 的有缘人，只为有缘人铺路。

并且关于 MVP 和 MVVM 各自的本质及区别，我就只说这么一遍，所以请认真阅读。

## 文章目录一览

-   前言
-   并不是每一个提问都值得被回答
-   本质和区别，我只说一遍
-   先说结论
-   所以二者的区别是什么？
-   Jetpack MVVM 和 MVVM 模式的关系
-   我为什么能瞬间感知沟通质量的 “好” 与 “坏” ？
-   综上
-   **Note 2020.09.10 加餐：**
    -   Jetpack MVVM 或借鉴了 MVI 开发模式
-   **Note 2020.12.31 加餐：**
    -   “依赖倒置” 的适用场景 及 源码设计分享

## 先说结论

MVP 本质：**是广义上的架构模式**，适用于面向实体或虚拟用户接口的开发。

> 它主要是在 MVC 的背景下，通过 **依赖倒置**，来解决 **逻辑复用** 难、**实现更替** 难 的问题。

MVVM 本质：**是狭义上的架构模式，专用于页面开发**。

> 它主要是在多人协作的软件工程的背景下，通过只操作 ViewModel 中映射的视图数据 来刷新视图状态，以此来解决 **视图调用的一致性问题** 从而规避不可预期的错误。

## 所以二者的区别是什么？

区别就在于：

**一个是广义上的架构**：

> 你可以通过同一套逻辑去驱动不同品牌设备的实体用户接口（比如不同品牌的耳机线控），或虚拟用户接口（比如 Android 视图，但存在一致性问题而不推荐）；

![](https://images.xiaozhuanlan.com/photo/2021/e5c1d7d9acc70bb00e881f3e720471aa.png)

**一个是狭义上的架构**：

> 专用于可视化页面的开发，通过解决一致性问题 来规避不可预期的错误。

![](https://images.xiaozhuanlan.com/photo/2021/5a30f744858c5c74544403c85babc8bb.png)

所以轻易地你就可发现，二者分别适用于 在各自的专场下 解决不同的问题，根本没有可比性，更没有所谓的 谁 “好” 谁 “坏” 之分。

而且除了没有可比性，二者之间其实也没任何关系，MVP 的特质是 **依赖倒置**，MVVM 的特质是 **数据驱动**，二者没有说谁演化自谁的关系。回到刚刚所说的：“根本就是 特定场景下解决特定问题 的两种截然不同的架构模式”。

> 没有所谓的 MVVM == MVP + DataBinding，正如没有所谓的 雷峰塔 == 雷锋 + 塔。

## Jetpack MVVM 和 MVVM 模式的关系

Jetpack MVVM 是 MVVM 模式在 Android 开发中的一个具体落实，也即它不仅仅包含了 MVVM 模式用于解决 “视图调用的一致性问题” 这一本质，还兼顾了 Android 页面开发中其他不可预期的错误。

正如我在[《Jetpack MVVM 精讲》](https://juejin.im/post/5dafc49b6fb9a04e17209922)中介绍到的：

> Lifecycle 的存在，主要是为了解决 **生命周期管理 的一致性问题**。
> 
> LiveData 的存在，主要是为了解决 **消息分发结果同步 的一致性问题**。
> 
> ViewModel 的存在，主要是为了解决 **状态管理 和 页面通信 的问题**。
> 
> DataBinding 的存在，主要是为了解决 **视图调用 的一致性问题**。
> 
> 它们的存在 大都是为了 在软件工程的背景下 解决一致性的问题、将容易出错的操作在后台封装好，**方便使用者快速、稳定、不产生预期外错误地编码**。

![](https://images.xiaozhuanlan.com/photo/2021/2765c30f6f243d9c4a7c2c7913fce249.png)

所以，Jetpack MVVM 的高频核心架构组件，和 iOS、WPF 的实现一定是有区别的，只不过抓住本质，我们就能举一反三，以不变应万变。

通过[《一通百通 “声明式 UI” 扫盲干货》](https://xiaozhuanlan.com/topic/2356748910)一文的介绍我们可知，基于 “自动化生成中间代码来实现数据绑定” 的 **DataBinding，其唯一挑战者是 基于函数式编程思想的数据驱动 UI 框架**，也即发源自前端 Elm 框架的 React、Flutter、Jetpack Compose、SwiftUI 之流。

所以像 Flutter 这种，算不算 MVVM 呢？不算，但二者本质上都是去 **解决视图调用的一致性问题**。

所以 ...

## 我为什么能瞬间感知沟通质量的 “好” 与 “坏” ？

事实上，在 “先说结论” 一节介绍完本质后，按照惯例，我是会以 “如果这样说还没有理解的话” 来引出下文，开始交代 “Before” 和 “After”，

因为每天都有新的读者加入，为方便新读者能够 结合自己的兴趣 随机阅读，**专栏几乎每一篇文章 都是以独立专辑的完整度来发行**。

这也是为什么，我的每一篇文章，都当做自己是第一次和读者见面、不遗余力地贯彻 **全网独家的深度思考写作风格**，让每一位新读者都有机会和我注入到文章的灵魂发生交流。

然而这一次，我不得不小小地任性一把，因为如果上述这样说了一通，还是不理解的话，答案早就写在以下几篇里：

> [《是 “虽冷门但有用” 的 背景缘由拾遗》](https://xiaozhuanlan.com/topic/0378514692) 的 “饭后甜点不能当主食吃” 一节；
> 
> [《基本功：是随时随地可受用的 深度思考原则》](https://xiaozhuanlan.com/topic/9837051426) 的 “所以如何正确地思考” 一节；
> 
> [《这是一份 简洁有力的 认知地图》](https://xiaozhuanlan.com/topic/9074561823) 的 “认知地图” 一节；

这几处都有 **就正确的思维方式 提供方法论依据 以及手把手示范**。

一旦熟悉了这套方法论，那些没完没了的争论，你会不会参与？我想大概率不会，因为你一眼就看出 这些言论中不包含基于事实的思考，不过是 **以争夺优胜为目的** 的自说自话。

参与到这种扯淡中 是对自己的不尊重，所以你不会参与。

## 综上

MVP 的本质是对 MVC 的依赖倒置，借此来解决 逻辑复用难 以及 实现替换难 的问题，

> 因为在 MVP 下，UI 逻辑和业务逻辑全在 Presenter 中写，UI 和 Model 的实现 可以随意替换，
> 
> 也即如上文介绍的，**通过同一套 Presenter 中的逻辑，可以驱动不同品牌不同型号的耳机的线控**。（注意 UI 的全称是 “用户接口”，台湾的术语更准确，叫 “用户介面”。UI 不是狭义上的页面，UI 就是 UI）

MVVM 的本质是对 View 数据的映射，借此来在软工背景下解决 视图调用的一致性问题。

> MVP 和 MVVM 二者的区别在于，**前者是广义的架构模式，普遍适用**；**后者是狭义上的架构模式，专用于页面开发**，可以解决例如 视图调用的一致性问题，来规避不可预期的错误。
> 
> 也即 **MVP 和 MVVM 本质上毫无关系，没有谁演化自谁**。

Jetpack MVVM 是 MVVM 模式在 Android 下的一个落地，也即除了解决 视图调用的一致性问题，还因地制宜地解决了其他一致性问题，从而规避各种不可预期的错误，让软件开发的工作得以完成得又快又好。

## Note 2020.09.10 加餐：

### Jetpack MVVM 或借鉴了 MVI 开发模式

刚刚有小伙伴私下分享了他对 MVI 开发模式 的理解，这里顺带对 Jetpack MVVM 的结构做个简要的补充。

[国外网友绘制的 MVVM + MVI 开发模式架构图](https://images.xiaozhuanlan.com/photo/2020/f283f9bbd68dddaba0014f4f533f0c8b.png)

正如我们在上文谈到的，**MVVM 开发模式的本质是 “解决视图调用的一致性问题”**，也即 **透过 DataBinding 等技术手段做到 100% 规避 “视图实例的 null 安全问题”**，因而 MVVM 开发模式从技术实现的角度来说 可以简单归结为 “对 DataBinding 框架的使用”。

与此同时，Jetpack MVVM 还包含了 LiveData 这个用于 “在表现层接收请求到的数据 并将数据分发给订阅它的页面”，也即 LiveData **透过 MVI 响应式编程的思想实现了 页面对 ViewModel 的 “单向依赖”**，以期规避不可预期的 **内存泄漏** 等问题。

然 LiveData 的本质主要集中在 **“解决数据分发的一致性问题”** 和 **“解决生命周期安全的一致性问题”**，也即 **透过 “唯一可信源” 的理念来规避不可预期的推送**，并且能自动解注册、始终保证 仅在视图控制器处于激活等状态下 透过回调接触视图控制器的成员。因而严格来说，LiveData 对 MVI 开发模式只是有一定的借鉴，是作为对 Jetpack MVVM 架构组件在 MVVM 开发模式下 对表现层数据分发的补充。

## Note 2020.12.31 加餐：

### “依赖倒置” 的适用场景 及 源码设计分享

通过上文的介绍，我们已知，出于对 “视图调用的 null 安全一致性问题” 等因素的考虑，基于 “依赖倒置原则” 的 MVP 模式不适用于 “页面开发”。

不过在实际开发过程中，仍然有不少地方可以借助 “依赖倒置” 来做到充分的 “逻辑可复用”。

> 关于依赖倒置，可以简单理解为 “java 的 **向上转型 + 抽象调用**”，也即，去调用接口类型的对象，而非具体类型的对象，从而得以 **通过同一套接口方法 去访问不同的实现**。实际上也就是对设计模式中 “工厂模式” 的使用。

例如多年前我在 GitHub 开源的 Linkage-RecyclerView，通过 “依赖倒置” 的设计，使得同一个 Adapter 逻辑可以去驱动不同的 ViewHolder 实现。

RxJava Magician

Eleme Linear

Eleme Grid

![](https://images.xiaozhuanlan.com/photo/2020/3673bbd29ba8233726dab0a24e057ea1.gif)

![](https://images.xiaozhuanlan.com/photo/2020/7ea77ad0617b221a8d262d8988aa3b53.gif)

![](https://images.xiaozhuanlan.com/photo/2020/f017c3232e63a4dd207ebb6a655ad13a.gif)

换言之，比起页面开发的场景，“依赖倒置” 更适用于开源库，也即逻辑在开源库内部封装好，使用者无需知道其内部设计，而只需根据 “已设计好 并对外开放的接口” 去实现自定义配置，就可以直接使用。

![](https://images.xiaozhuanlan.com/photo/2020/f1f9018e9a8cca653ebd25bedd202365.jpeg)

> 如图，在一级、二级列表的 Adapter 中调用了抽象的 IConfig 接口，使用者实现这些接口，就能定制不同的 Item 界面，而 Adapter 逻辑始终都是同一套，最大程度上做到了复用。
> 
> 具体可参见 [GitHub: Linkage-RecyclerView](https://github.com/KunMinX/Linkage-RecyclerView) 的源码

·

> **便捷链接：**
> 
> [GitHub : 重学安卓 配套项目](https://github.com/KunMinX/Relearn-Android)
> 
> [GitHub : Jetpack-MVVM-Best-Practice](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)
> 
> **拓展阅读：**
> 
> [致创作者：免疫网络暴力和打压 的 高频认知补丁](https://xiaozhuanlan.com/topic/3475690128)

## 版权声明

> Copyright © 2019-present KunMinX

文中提到的 关于 “MVP 和 MVVV 各自的本质及区别” 的具体描述、**“用户介面与耳机线控” 的举例**、架构设计图例、“DataBinding 与函数式编程数据驱动框架的关系” 的具体描述、“Jetpack MVVM 和 MVVM 关系” 的描述、“Lifecycle、LiveData、ViewModel、DataBinding 等架构组件的存在意义”、“通过解决一致性问题来规避不可预期的错误” 等多处 **对特定现象及其本质的概括，均属于本人独立原创的成果**，本人对此享有最终解释权。

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

大家好，我是[《Jetpack MVVM 精讲》](https://xiaozhuanlan.com/topic/0129483567)的独立原创作者 KunMinX，GitHub star 8.7k，专注深度思考和 Jetpack MVVM 的分享。

关于 MVP 和 MVVM 本质和区别的文章，本来我是不想单独写的，因为经过长达一年的耳濡目染 和对方法论的试炼，相信 **但凡沉下心阅读过**《重学安卓》体系化文章的读者，多已练就 **透过表象迅速抓住本质** 的稀缺能力。

专栏每天都有新的读者加入，然而没想到的是，1 年了，仍然时不时的会被咨询、或是在各个社区看到人们众说纷纭地在谈论 MVP 和 MVVM 谁 “好” 谁 “坏”。

## 并不是每一个提问都值得被回答

> 爱因斯坦说，“提出正确的问题，问题就已解决了一半”。

换言之，并不是每一个提问都值得被回答。**一次提问所包含的信息量，其实远远超出内容本身**。

透过提问者的提问，几乎可以瞬间感知到，提问者对事实状况的掌握程度，并由此来决定到底值不值得继续交流。

“高” 质量的提问交流 让人感到如沐春风 —— 几句话就先自己把背景交代清楚，然后就某个细节提出自己的困惑 —— 这让我不免想要与之多聊几句，把我知道的毫无保留地分享出去。

反之，“低” 质量提问 让人感到 **不舒服，甚至不对劲** —— 明明不遗余力地在多处 **划重点 反复交代**，明明白纸黑字写得清清楚楚，甚至段落、链接给他指出来，却视而不见，就好像从未发生过。

更有甚者，为了满足抬杠的快感，不惜浪费彼此的时间 ...

> 注：我从不在技术交流中使用 “高”、“低”、“好”、“坏”、“轻”、“重” 之类的主观描述，此处只是以多数人方便理解的方式来介绍。文中使用到的主观描述一律加上双引号。

![](https://images.xiaozhuanlan.com/photo/2021/c967e971669b96c5fb19b9eaaef4ad22.png)

## 本质和区别，我只说一遍

事实上，我并不会去判断来者是否是来抬杠，只须透过对方所说的话，即可瞬间判断对方说的是事实，还是自顾自地扯淡 —— **你没法和一个前来扯淡的人交流**，你会发现 这种对话往往 **存在巨大的代沟**，并且抬杠者无意谋求和缝合对事实的理解，他本来就是为了 “来得快” 的快感而来。

> 不同于主观的偏好取向，**事实必有特定背景和目的来约束**，一切脱离事实特征的意见和观点都是瞎扯淡，没有讨论的前提、不值得参与 —— ©KunMinX

所以，本文只写给那些 真的想搞清楚事实 的有缘人，只为有缘人铺路。

并且关于 MVP 和 MVVM 各自的本质及区别，我就只说这么一遍，所以请认真阅读。

## 文章目录一览

-   前言
-   并不是每一个提问都值得被回答
-   本质和区别，我只说一遍
-   先说结论
-   所以二者的区别是什么？
-   Jetpack MVVM 和 MVVM 模式的关系
-   我为什么能瞬间感知沟通质量的 “好” 与 “坏” ？
-   综上
-   **Note 2020.09.10 加餐：**
    -   Jetpack MVVM 或借鉴了 MVI 开发模式
-   **Note 2020.12.31 加餐：**
    -   “依赖倒置” 的适用场景 及 源码设计分享

## 先说结论

MVP 本质：**是广义上的架构模式**，适用于面向实体或虚拟用户接口的开发。

> 它主要是在 MVC 的背景下，通过 **依赖倒置**，来解决 **逻辑复用** 难、**实现更替** 难 的问题。

MVVM 本质：**是狭义上的架构模式，专用于页面开发**。

> 它主要是在多人协作的软件工程的背景下，通过只操作 ViewModel 中映射的视图数据 来刷新视图状态，以此来解决 **视图调用的一致性问题** 从而规避不可预期的错误。

## 所以二者的区别是什么？

区别就在于：

**一个是广义上的架构**：

> 你可以通过同一套逻辑去驱动不同品牌设备的实体用户接口（比如不同品牌的耳机线控），或虚拟用户接口（比如 Android 视图，但存在一致性问题而不推荐）；

![](https://images.xiaozhuanlan.com/photo/2021/e5c1d7d9acc70bb00e881f3e720471aa.png)

**一个是狭义上的架构**：

> 专用于可视化页面的开发，通过解决一致性问题 来规避不可预期的错误。

![](https://images.xiaozhuanlan.com/photo/2021/5a30f744858c5c74544403c85babc8bb.png)

所以轻易地你就可发现，二者分别适用于 在各自的专场下 解决不同的问题，根本没有可比性，更没有所谓的 谁 “好” 谁 “坏” 之分。

而且除了没有可比性，二者之间其实也没任何关系，MVP 的特质是 **依赖倒置**，MVVM 的特质是 **数据驱动**，二者没有说谁演化自谁的关系。回到刚刚所说的：“根本就是 特定场景下解决特定问题 的两种截然不同的架构模式”。

> 没有所谓的 MVVM == MVP + DataBinding，正如没有所谓的 雷峰塔 == 雷锋 + 塔。

## Jetpack MVVM 和 MVVM 模式的关系

Jetpack MVVM 是 MVVM 模式在 Android 开发中的一个具体落实，也即它不仅仅包含了 MVVM 模式用于解决 “视图调用的一致性问题” 这一本质，还兼顾了 Android 页面开发中其他不可预期的错误。

正如我在[《Jetpack MVVM 精讲》](https://juejin.im/post/5dafc49b6fb9a04e17209922)中介绍到的：

> Lifecycle 的存在，主要是为了解决 **生命周期管理 的一致性问题**。
> 
> LiveData 的存在，主要是为了解决 **消息分发结果同步 的一致性问题**。
> 
> ViewModel 的存在，主要是为了解决 **状态管理 和 页面通信 的问题**。
> 
> DataBinding 的存在，主要是为了解决 **视图调用 的一致性问题**。
> 
> 它们的存在 大都是为了 在软件工程的背景下 解决一致性的问题、将容易出错的操作在后台封装好，**方便使用者快速、稳定、不产生预期外错误地编码**。

![](https://images.xiaozhuanlan.com/photo/2021/2765c30f6f243d9c4a7c2c7913fce249.png)

所以，Jetpack MVVM 的高频核心架构组件，和 iOS、WPF 的实现一定是有区别的，只不过抓住本质，我们就能举一反三，以不变应万变。

通过[《一通百通 “声明式 UI” 扫盲干货》](https://xiaozhuanlan.com/topic/2356748910)一文的介绍我们可知，基于 “自动化生成中间代码来实现数据绑定” 的 **DataBinding，其唯一挑战者是 基于函数式编程思想的数据驱动 UI 框架**，也即发源自前端 Elm 框架的 React、Flutter、Jetpack Compose、SwiftUI 之流。

所以像 Flutter 这种，算不算 MVVM 呢？不算，但二者本质上都是去 **解决视图调用的一致性问题**。

所以 ...

## 我为什么能瞬间感知沟通质量的 “好” 与 “坏” ？

事实上，在 “先说结论” 一节介绍完本质后，按照惯例，我是会以 “如果这样说还没有理解的话” 来引出下文，开始交代 “Before” 和 “After”，

因为每天都有新的读者加入，为方便新读者能够 结合自己的兴趣 随机阅读，**专栏几乎每一篇文章 都是以独立专辑的完整度来发行**。

这也是为什么，我的每一篇文章，都当做自己是第一次和读者见面、不遗余力地贯彻 **全网独家的深度思考写作风格**，让每一位新读者都有机会和我注入到文章的灵魂发生交流。

然而这一次，我不得不小小地任性一把，因为如果上述这样说了一通，还是不理解的话，答案早就写在以下几篇里：

> [《是 “虽冷门但有用” 的 背景缘由拾遗》](https://xiaozhuanlan.com/topic/0378514692) 的 “饭后甜点不能当主食吃” 一节；
> 
> [《基本功：是随时随地可受用的 深度思考原则》](https://xiaozhuanlan.com/topic/9837051426) 的 “所以如何正确地思考” 一节；
> 
> [《这是一份 简洁有力的 认知地图》](https://xiaozhuanlan.com/topic/9074561823) 的 “认知地图” 一节；

这几处都有 **就正确的思维方式 提供方法论依据 以及手把手示范**。

一旦熟悉了这套方法论，那些没完没了的争论，你会不会参与？我想大概率不会，因为你一眼就看出 这些言论中不包含基于事实的思考，不过是 **以争夺优胜为目的** 的自说自话。

参与到这种扯淡中 是对自己的不尊重，所以你不会参与。

## 综上

MVP 的本质是对 MVC 的依赖倒置，借此来解决 逻辑复用难 以及 实现替换难 的问题，

> 因为在 MVP 下，UI 逻辑和业务逻辑全在 Presenter 中写，UI 和 Model 的实现 可以随意替换，
> 
> 也即如上文介绍的，**通过同一套 Presenter 中的逻辑，可以驱动不同品牌不同型号的耳机的线控**。（注意 UI 的全称是 “用户接口”，台湾的术语更准确，叫 “用户介面”。UI 不是狭义上的页面，UI 就是 UI）

MVVM 的本质是对 View 数据的映射，借此来在软工背景下解决 视图调用的一致性问题。

> MVP 和 MVVM 二者的区别在于，**前者是广义的架构模式，普遍适用**；**后者是狭义上的架构模式，专用于页面开发**，可以解决例如 视图调用的一致性问题，来规避不可预期的错误。
> 
> 也即 **MVP 和 MVVM 本质上毫无关系，没有谁演化自谁**。

Jetpack MVVM 是 MVVM 模式在 Android 下的一个落地，也即除了解决 视图调用的一致性问题，还因地制宜地解决了其他一致性问题，从而规避各种不可预期的错误，让软件开发的工作得以完成得又快又好。

## Note 2020.09.10 加餐：

### Jetpack MVVM 或借鉴了 MVI 开发模式

刚刚有小伙伴私下分享了他对 MVI 开发模式 的理解，这里顺带对 Jetpack MVVM 的结构做个简要的补充。

[国外网友绘制的 MVVM + MVI 开发模式架构图](https://images.xiaozhuanlan.com/photo/2020/f283f9bbd68dddaba0014f4f533f0c8b.png)

正如我们在上文谈到的，**MVVM 开发模式的本质是 “解决视图调用的一致性问题”**，也即 **透过 DataBinding 等技术手段做到 100% 规避 “视图实例的 null 安全问题”**，因而 MVVM 开发模式从技术实现的角度来说 可以简单归结为 “对 DataBinding 框架的使用”。

与此同时，Jetpack MVVM 还包含了 LiveData 这个用于 “在表现层接收请求到的数据 并将数据分发给订阅它的页面”，也即 LiveData **透过 MVI 响应式编程的思想实现了 页面对 ViewModel 的 “单向依赖”**，以期规避不可预期的 **内存泄漏** 等问题。

然 LiveData 的本质主要集中在 **“解决数据分发的一致性问题”** 和 **“解决生命周期安全的一致性问题”**，也即 **透过 “唯一可信源” 的理念来规避不可预期的推送**，并且能自动解注册、始终保证 仅在视图控制器处于激活等状态下 透过回调接触视图控制器的成员。因而严格来说，LiveData 对 MVI 开发模式只是有一定的借鉴，是作为对 Jetpack MVVM 架构组件在 MVVM 开发模式下 对表现层数据分发的补充。

## Note 2020.12.31 加餐：

### “依赖倒置” 的适用场景 及 源码设计分享

通过上文的介绍，我们已知，出于对 “视图调用的 null 安全一致性问题” 等因素的考虑，基于 “依赖倒置原则” 的 MVP 模式不适用于 “页面开发”。

不过在实际开发过程中，仍然有不少地方可以借助 “依赖倒置” 来做到充分的 “逻辑可复用”。

> 关于依赖倒置，可以简单理解为 “java 的 **向上转型 + 抽象调用**”，也即，去调用接口类型的对象，而非具体类型的对象，从而得以 **通过同一套接口方法 去访问不同的实现**。实际上也就是对设计模式中 “工厂模式” 的使用。

例如多年前我在 GitHub 开源的 Linkage-RecyclerView，通过 “依赖倒置” 的设计，使得同一个 Adapter 逻辑可以去驱动不同的 ViewHolder 实现。

RxJava Magician

Eleme Linear

Eleme Grid

![](https://images.xiaozhuanlan.com/photo/2020/3673bbd29ba8233726dab0a24e057ea1.gif)

![](https://images.xiaozhuanlan.com/photo/2020/7ea77ad0617b221a8d262d8988aa3b53.gif)

![](https://images.xiaozhuanlan.com/photo/2020/f017c3232e63a4dd207ebb6a655ad13a.gif)

换言之，比起页面开发的场景，“依赖倒置” 更适用于开源库，也即逻辑在开源库内部封装好，使用者无需知道其内部设计，而只需根据 “已设计好 并对外开放的接口” 去实现自定义配置，就可以直接使用。

![](https://images.xiaozhuanlan.com/photo/2020/f1f9018e9a8cca653ebd25bedd202365.jpeg)

> 如图，在一级、二级列表的 Adapter 中调用了抽象的 IConfig 接口，使用者实现这些接口，就能定制不同的 Item 界面，而 Adapter 逻辑始终都是同一套，最大程度上做到了复用。
> 
> 具体可参见 [GitHub: Linkage-RecyclerView](https://github.com/KunMinX/Linkage-RecyclerView) 的源码

·

> **便捷链接：**
> 
> [GitHub : 重学安卓 配套项目](https://github.com/KunMinX/Relearn-Android)
> 
> [GitHub : Jetpack-MVVM-Best-Practice](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)
> 
> **拓展阅读：**
> 
> [致创作者：免疫网络暴力和打压 的 高频认知补丁](https://xiaozhuanlan.com/topic/3475690128)

## 版权声明

> Copyright © 2019-present KunMinX

文中提到的 关于 “MVP 和 MVVV 各自的本质及区别” 的具体描述、**“用户介面与耳机线控” 的举例**、架构设计图例、“DataBinding 与函数式编程数据驱动框架的关系” 的具体描述、“Jetpack MVVM 和 MVVM 关系” 的描述、“Lifecycle、LiveData、ViewModel、DataBinding 等架构组件的存在意义”、“通过解决一致性问题来规避不可预期的错误” 等多处 **对特定现象及其本质的概括，均属于本人独立原创的成果**，本人对此享有最终解释权。

**本文为专栏私有内容，谢绝转载**。

读者订阅了专栏，即享有了专栏文章的阅读权。

文章 **原创的引言、切入点、思路、结论** 凝聚了作者 KunMinX 本人的心血 及与参与者互动演化的结果，作者本人及作者在文中提名的有效贡献者 对相应的成果享有所有权。

**当您 借鉴或引用文中的引言、切入点、思路、结论 进行二次创作并打算发行时，须注明链接出处**，否则我们保留追责的权利。对此如有疑虑请及时沟通，或在发行前将作品交由本人核对。

当您看见有人 [洗稿](https://baike.baidu.com/item/%E6%B4%97%E7%A8%BF/20862574?fr=aladdin)、[剽窃](https://baike.baidu.com/item/%E5%89%BD%E7%AA%83/9398357?fr=aladdin)、未经授权转载本专栏的文章内容时，请及时向本人举报。

最后感谢您对专栏内容的阅读和喜欢。