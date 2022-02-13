---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/4568971203
author: 
---

# [重学安卓：Activity 的快乐你不懂！ － 小专栏](https://xiaozhuanlan.com/topic/4568971203)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

## 前言

本文本来是自己复盘 Android 知识梳理用的，没想到在一次部门内部的知识测评中发现，同事们对这些基础知识的掌握参差不齐，甚至可以说是模棱两可。

是网上关于 Activity 的教程太少了吗？

不是的，恰恰相反，网上的信息多如牛毛，**却没有一篇愿意费哪怕一丝丝的笔墨** 来介绍 Activity 的起源、它的职责边界、它的存在到底是为了解决什么问题、我们学习它，到底学到什么程度才算掌握。🤔

> 并且正是由于对这些 **最基本而必要的概念** 模棱两可，使得教程再多、再优秀，也没多少人能消化、能记住。

于是我抱着试试看的心态，在经过几番润色过，将自己复盘的结果，在小会上分享出来供同事们享用。想不到原本不屑听的几个同事，在听完这番讲解后，连说真香。

所以如果你因为本文，而对 `Activity` 乃至向上追溯的 `View`、`Window`、`WindowManager`、`WindowManagerService`、`Surface`、`Surface Flinger` **各自的起源、职责边界 以及 相互间的关系** 有了最基本的感性认识，继而不知不觉地开始有了一丝丝好奇，推动你深入地去探究，那我的愿望也就达到了。

## 文章目录一览

-   前言
-   我是一块板砖
-   我是 Surface Flinger
-   我是 Window
-   我是一个会套娃的 View
-   …… 所以，Window 成了摸鱼般的存在吗？
-   于是我改名叫 Activity
-   综上
-   **Note 2019.11.9 加餐：**
    -   从设计模式角度 解析 Activity 和 Window 的关系
-   **Note 2020.1.29 加餐：**
    -   从视图系统架构设计的角度 解析 PhoneWindow 和 ViewRootImpl 二者的本质和区别

## 我是一块板砖

![](https://images.xiaozhuanlan.com/photo/2021/42a501a2f381cfac047c4393c157e95f.png)

我是一块运行着原始 Android 系统的板砖。我有一块屏幕，人们只要通过硬件抽象层（HAL）的代码对屏幕发起指令，屏幕上就可以显示人们想看到的内容。然而这么做过于原始，也不契合板砖的使用场景。

于是有人考虑在 HAL 之上的运行时层（ART）用 C++ 封装一个服务，该服务的名称就叫 Surface Flinger。

## 我是 Surface Flinger

我是 Surface Flinger，我的职责是专门负责 UI 内容的渲染。

人们想要在屏幕上渲染出什么内容，都可以通过我来间接地与屏幕打交道。这就好比你 **在电脑上排版好的图文，只需通过 “打印机驱动程序” 这个中介，就能帮助你将文档内容输出到纸上**。

![](https://images.xiaozhuanlan.com/photo/2021/2116a07c402a33475fba4a0159e2e746.png)

至于内容本身究竟有些什么，这我不管，我只负责统一地、有序地将内容安排成 “输出设备” 能理解的方式，来实现输出。

## 我是 Window

这块板砖的主人不仅想要渲染 UI，还想要窗口，于是在应用框架层，通过 Java  
封装了我。

人如其名，我就是一个窗口，我负责可视化内容的排版，然后将排版结果，通过我的上司 WindowManager，通过进程通信的方式，去与后台服务 WindowManagerService 通信，最终递交到 Surface Flinger 来输出和呈现。

![](https://images.xiaozhuanlan.com/photo/2021/8b22d5af1af2d775d061db164abeb80a.png)

Surface Flinger 为我们每一个 Window 都映射了一块 Surface，来用于管理和渲染屏幕内容。

然而作为一个 Window，我也有我的苦衷。

## 我是一个会套娃的 View

主人因为经常听 Window 大哥抱怨排版的负担太重，于是用 **组合模式** 封装了我。我的 “有容乃大” 版本：ViewGroup，因为组合模式，而能够在自身内部存在更多的 View 或 ViewGroup，这使得我们从结构上来看，就像套娃。

![](https://images.xiaozhuanlan.com/photo/2021/92538eb7e490a952d8cd492a322e969c.png)

托递归的福，我们的排版工作：Measure、Layout、Draw，可以自己通过如此般的递归，自下往上地完成。然后 Window 大哥就可以直接拿着我们的排版结果，去向上司交差啦。

## …… 所以，Window 成了摸鱼般的存在吗？

本来 Window 正寻思着，日子过得这般清闲自在，没想到好日子到头 —— 主人不仅要一个窗口，还想要多窗口。这多窗口它就涉及到窗口间的切换、通信等等，甚是麻烦，这些脏活累活要是交给以后的开发者来干，那我不得留下一世骂名、遗臭万年？……

想到这里我就感觉哆嗦，不行，为了我一世英名，我得向主人进言。

根据神话故事的记载，其实早在 20000 多年前，女娲造人的时候，便采用了神级的 **模板方法模式**，将一系列的通用功能都封装好，只暴露一些 DNA 接口，以供后来者随机输入和演变。

换言之，主人只需以 “模板方法模式” 的方式将我重新封装，并且编写一套用于管理窗口的 “任务和返回栈机制” 在背地里运筹帷幄，那么未来的开发者就只需继承我 来得到一个简练的配置模板，然后在模板上面输入他们的定制内容，便可得到他们想要的结果。

## 于是我改名叫 Activity

![](https://images.xiaozhuanlan.com/photo/2021/19663d97c1ea4fbd2ffbf238495796aa.png)

Window 成了我永恒不变的信仰，存留在我的体内。从应用开发的角度来说，我就是个待继承的 Activity，**使用者通过继承我，拿到的就是一个个简练的模板**。

具体而言，对系统来说，我的本质仍是被管理的窗口，系统能够管理我和其他窗口的切换和通信。

而对开发者来说，我的本质是视图控制器，开发者通过我可以控制 View 以他们想要的方式进行排版，并且在特殊状况下保存和恢复 View 的排版内容。

## 综上

最开始只有一块运行着原始 Android 系统的板砖。

Surface Flinger 的出现是为了更加方便地完成 UI 渲染。

Window 的出现是为了管理 UI 内容的排版。

Window 不堪重负 于是将责任下发到 View 身上。

View 通过组合模式，在递归的帮助下 蹭蹭蹭地完成排版工作。

Activity 的出现是为了满足 “多窗口管理” 和 “傻瓜式视图管理” 的需要。

所以 Activity 的知识边界无非就是 **“生命周期、环境变化导致的重建、多窗口跳转、视图的加载和优化”** 等等。

![](https://images.xiaozhuanlan.com/photo/2021/5f00806f7b7a985950d7b0ef8013f37f.png)

至此，我们简要完成了对 Activity 起源的追溯 和对职责边界的确立，后续我们逐一就 Activity 相关的高频技术点作详细分析。

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

## Note 2019.11.9 加餐：

### 从设计模式角度 解析 Activity 和 Window 的关系

这里对 Activity 和 Window 之间的关系 做个简要的补充说明：

Activity 是基于 **模板方法模式** 和 **组合模式** 的实现，目的是为了将后台的机制屏蔽，留给开发者的，就只是简单的、干净的 Activity 子类模板。

也即 Activity 的职责包含 Window，但不限于 Window。

Activity 的内部，通过组合模式，将多个职能的实例 安排到一起，根据需要 来取用它们。

这样既能调到自己需要的，又能为 Activity 子类 屏蔽掉不需要的方法（例如 Window 那些管理窗口的功能）。

## Note 2020.1.29 加餐：

### 从视图系统架构设计的角度 解析 PhoneWindow 和 ViewRootImpl 各自的本质和区别

上文分别从 “追根溯源” 及 “设计模式” 的视角 揭示了 Activity 和 Window 的关系。

如果此前小伙伴们涉足过 PhoneWindow、ViewRootImpl 等源码，且吃够了错综复杂、找不到头绪的苦，推荐拓展阅读 视图系统章节的[《过目难忘 Android GUI 关系梳理》](https://xiaozhuanlan.com/topic/2073915486)篇，

该篇以独家的 “二分法视角”，兵分两路地揭示 PhoneWindow、ViewRootImpl 在可视化交互系统中所处的环节和各自的本质，感兴趣的小伙伴可趁热打铁，获得双倍的快乐。

## 版权声明

> Copyright © 2019-present KunMinX

**本文为专栏私有内容，谢绝转载**。

读者订阅了专栏，即享有了专栏文章的阅读权。

文章 **原创的引言、切入点、思路、结论** 凝聚了作者 KunMinX 本人的心血 及与参与者互动演化的结果，作者本人及作者在文中提名的有效贡献者 对相应的成果享有所有权。

**当您 借鉴或引用文中的引言、切入点、思路、结论 进行二次创作并打算发行时，须注明链接出处**，否则我们保留追责的权利。对此如有疑虑请及时沟通，或在发行前将作品交由本人核对。

当您看见有人 [洗稿](https://baike.baidu.com/item/%E6%B4%97%E7%A8%BF/20862574?fr=aladdin)、[剽窃](https://baike.baidu.com/item/%E5%89%BD%E7%AA%83/9398357?fr=aladdin)、未经授权转载本专栏的文章内容时，请及时向本人举报。

最后感谢您对专栏内容的阅读和喜欢。