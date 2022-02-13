---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/2869301475
author: 
---

# [重学安卓：Intent 好比你的择偶标准 － 小专栏](https://xiaozhuanlan.com/topic/2869301475)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

## 前言

周末去了趟前同事家叙旧，晚饭时分，在同事的电脑上播放起了相亲节目。台上男嘉宾就自己的事业和收入做了相应的介绍，正对面的女嘉宾们则是根据各自的价值偏好 为心仪的男嘉宾亮起了灯。

> 本来这相亲节目看看也就过去了，没想到，回头猛一想，小姐姐们根据择偶标准发起了请求，这择偶标准不正是 Intent 的本质么？

于是回到家我吭哧吭哧地写下了这篇《择偶标准指南》，**并且开源了 3 个配套的案例，方便大家深入理解和实操**（不要慌，文末链接给出）。

所以如果通过这篇，你对 Intent 的设计依据、职责边界，以及和其他路由成员的关系 建立起感性的认识，那我的目的也就达到了。

## 文章目录一览

-   前言
-   不得不先讲的 Intent 匹配机制
-   关于 Intent 的 7 个深度思考
    -   为何存在路由的设计？
    -   为何存在三大组件的设计？
    -   为何要让 Binder 来当媒婆？
    -   Intent 存在哪几种类型？为何存在这样的区别？
    -   隐式 Intent 的匹配依据？
    -   IntentFilter 的概念和作用为何？
    -   基于 Intent 和 IntentFilter 的匹配规则，提炼出的 4 个要点
-   关于 Intent 的 3 个实用方法
-   Intent 使用的 7 个注意事项？
-   综上

## 不得不先讲的 Intent 匹配机制

前几期，我们借由

> [《Activity 生命周期的 3 个辟谣》](https://xiaozhuanlan.com/topic/0213584967)
> 
> [《绝不丢失状态的 Activity 重建机制》](https://xiaozhuanlan.com/topic/7692814530)
> 
> [《你丢了 offer，只因拎不清 Activity 任务和返回栈》](https://xiaozhuanlan.com/topic/7812045693)

这 3 篇文章，分别深入浅出地介绍了 **进程模式**、**生命周期**、**重建机制**、**状态管理** 的设计依据、职责边界，以及相互间关系，并且正确区分了 **任务** 和 **返回栈**，乃至正确理解了 **启动模式** 的本质。

原以为这样 Activity 的知识复盘 便圆满告一段落，没想到掐指一算，还差一篇 —— Activity 的页面跳转路由机制，

并且由于下一篇打算介绍 Fragment，且其中涉及对 “Activity 和 Fragment 在路由上的区别和联系” 的解析，

因而本篇决定先就 “路由”、“组件” 以及 “Intent 匹配机制” 的设计缘由 做个全面的铺垫。就算对择偶不感兴趣，也请务必认真阅读本篇。

## 为何存在路由的设计？

路由的本质即寻址和数据转发。路由的概念源于计算机网络。

正因为这是一种 **普适的、标准化的、用于解决页面跳转和通信的 解决方案**，因而安卓也是沿用了路由机制，来完成页面的跳转和通信。

下文我们都用“路由”来指代“页面跳转和通信”。

## 为何存在三大组件的设计？

在 [《Activity 的快乐你不懂！》](https://xiaozhuanlan.com/topic/4568971203) 一文中，我们已经分析了 Activity 存在的缘由：

是基于模版方法模式的、将页面跳转和生命周期管理 交给系统在背后运筹帷幄、**开发者只需拿到继承的简易干净的模板 去填充自己的代码**。

因而就像 Activity 是对 Window 的模板化，Service、Broadcast 这 2 个组件，本质上也是待继承和填充的模板，是为了方便开发者的使用而存在。

也因此，它们背后拥有同一套“进程间通信”的路由机制，来帮助它们完成相互间的路由。

**在这套机制中，Binder 是通风报信的媒婆，Intent 则是择偶标准**。

## 为何要让 Binder 来当媒婆？

我们知道，Binder 是 Android 下进程间通信的方式，

> **那为什么路由的事，要设计成是交给 Binder 去做呢？**

—— 因为考虑到有进程间通信的需要 —— 因为 Android 系统被设计为，你不仅可以和当前 App 中的组件通信，你也可以去调用其他 App 的组件，而这时候就需要 Binder 来完成进程间通信。

> **那为什么要设计成可以调用其他 App 的组件呢？**

—— 就比如开发一个日记软件，为了添加照片，难道非要自己写一个相册模块吗？未必，通过路由机制，你就能调用系统相册，来选取和回调所需的照片。

在这个过程中，Binder 通风报信，Intent 则是你选取照片的标准，例如你只要 PNG 格式的照片。

## Intent 存在哪几种类型？为何存在这样的区别？

这主要看“人”。

> 世界上只存在两种人：一种是目标明确、知道自己想要什么的；另一种是目标不明确的、只能给出个大概的条件。

那么前一种人，她给出的择偶标准，就是显式 Intent，具体的表现就是，她会事先明确地指出“**我要翻某人的牌**”。

也即，通过`setComponent()`、`setClass()`、`setClassName()` 或 `Intent` 构造函数，来 **明确地指定要启动的组件的名称或类，就是显式 Intent**。例如：

![](https://images.xiaozhuanlan.com/photo/2020/da3dbe411ad243e98c90a0eb9775adfb.png)

后一种人，她给出的是一系列指标，凡是处于这些指标的基准区间的，都可以考虑。

也即，**隐式 Intent 的特点是，不事先明确地指出所要启动的组件，而是通过给出条件项，来匹配潜在的、符合条件的组件**，从而在对应的组件列表中，供使用者挑选心仪的那一个。例如：

![](https://images.xiaozhuanlan.com/photo/2020/57f65346eb36a3071de4e40247a1f8eb.png)

> **这里特别要提到的一点是，网上对 显式、隐式 Intent 区别的 讹传 屡见不鲜。**

首先他们并没有正确地区分 显式和隐式 的概念，认为“只有往 Intent 传入 class 的那种写法才是显式”，并且认为只有 “隐式” Intent 可以去访问别的 App 的组件。

这样的说法显然是没有事实根据的。

显式和隐式的本质区别，正如本小节中提到的。

并且，不管是显式 Intent 还是 隐式 Intent，只要被访问的外部 App 的目标组件在 menifest 中为该组件配置的 exported 属性为 true，那么就可以访问。

> 划重点 👆👆👆

因而退一万步，在对一个知识点存疑的情况下，**就算等不及《重学安卓》系列文章，也请务必首先查阅官方文档！**

[https://developer.android.google.cn/guide/components/intents-filters](https://developer.android.google.cn/guide/components/intents-filters)

那么用于匹配的指标都有哪些呢？

## 隐式 Intent 的匹配依据？

![](https://images.xiaozhuanlan.com/photo/2019/206e8ae801d2ecda3260d821c1eb5fc2.png)

拿去和谁匹配呢？匹配的规则是什么呢？

## IntentFilter 的概念和作用为何？

IntentFilter 是专门用于 **声明 隐式启动的 筛选条件**。

系统正是通过匹配 Intent 和 IntentFilter 的条件项，来启动相应的组件。

IntentFilter 是在清单文件中配置，作为组件 XML 元素的子元素。**一个组件元素可以包含多个 IntentFilter 元素**。

IntentFilter 包含 action、category、data 三种子元素，且 **这三种子元素都可以分别配置多个**。

![](https://images.xiaozhuanlan.com/photo/2020/011601cae9384464db7e675889bdfc52.png)

## Intent 和 IntentFilter 匹配规则的 4 个要点

> 一个隐式 Intent 只能指定一个 action。

当 IntentFilter 中有一个 action 匹配上，就算是完成 action 的匹配。并且 **action 是必填项，每个 IntentFilter 和隐式 Intent 都必须有，否则不成功**。

> 一个隐式 Intent 中可以不添加、或添加多个 category。

**一旦指定了多个，Intent 中的 category 唯有成为 IntentFilter 中配置的 category 的子集，才有机会完成 category 匹配**。此外，无论是否有添加 category，系统都会默认在启动组件时为 Intent 添加 DEFAULT category，因此务必在每一个 IntentFilter 中添加一个 android.intent.category.DEFAULT 的 category。

（但这又怎么解释 Launcher 的情况呢？Launcher 的 Filter 中可不包括 Default。对此读者可以思考下，或在下方评论留言）

> **Data 的匹配，Intent 的 setData 和 setType 是互斥的。**

setData，指定的是 URI，setType，指定的是 MineType。（为什么设计成互斥的呢？读者可以思考一下）

**和路由一样，URI 也是 普适的、标准化的 一种解决方案。它用作资源的定位，并不是安卓平台独有，并且常常与路由结伴出现。**

> 划重点 👆👆👆

URI 的匹配具体包括：scheme、host、port、path，也即：  
`<scheme>://<host>:<port>/<path>`

从左到右，对 URI 的约束指定的条件越少，条件就越宽松，比如若只设置了  
`<data android:scheme = "http"/>`，那么任何 `Intent.setData(Uri.parse('http://xxx'))` 的情况都能匹配上。

> MimeType 的匹配

MimeType 起源于 TCP/IP 协议簇应用层的邮件协议，直译的内涵为：多用途 Internet 邮件扩展，也即支持富媒体内容。**同 URI 一样，现已作为 普适的、标准化的 一种解决方案。它用作格式筛选依据，广泛用于互联网等领域。**

> 划重点 👆👆👆

常见的 MimeType 选项如 `image/*`，`text/plain`。

若 IntentFilter 只声明了 MimeType，那么系统会假定组件支持 content: 和 file: 数据，也即在 Intent 中如指定 URI，则务必将 URI 配成 `content://xxxx` 或 `file://xxxx`。

正由于 URI 和 MimeType 的普适，关于它们的使用细节，可在互联网上自行搜寻和拓展阅读，本文不再累述。

## 关于 Intent 的 3 个实用方法

### 向目标 Activity 传递数据

可以通过向 Intent 的 extra 字段注入携带的数据，来完成数据的传递。

Extra 是一个 Bundle，**Bundle 的本质是进程间通信数据的载体，可接收基本数据类型或被序列化过的引用数据类型**。

> 为保持连贯的阅读体验，此处我就不贴代码了，事后可详见《重学安卓》开源项目中的 IntentTestActivity。

### 向原 Activity 回调数据

整体流程：

-   在原 Activity 通过 startActivityForResult 来启动目标 Activity。
-   在目标 Activity 完成事宜后，通过 setResult 来将结果数据返回给原 Activity。
-   在原 Activity 通过 onActivityResult 来回调得到的数据。

> 代码详见《重学安卓》开源项目 IntentTestActivity 页面。

### 为目标 Activity 设置 FLAG

例如单独设置 FLAG\_ACTIVITY\_NEW\_TASK、FLAG\_ACTIVITY\_CLEAR\_TOP 或联合设置，来使目标 Activity 以某种模式与任务发生关系。

> 代码详见《重学安卓》开源项目 IntentTestActivity 页面。

具体常见的 FLAG 的使用效果以及和 LaunchMode 的区别，可以参见 [《你丢了 offer，只因拎不清 Activity 任务和返回栈》](https://xiaozhuanlan.com/topic/7812045693) 全网独家提供的 简明、到位、正确的解读。

## Intent 使用的 7 个注意事项？

-   当没有可以匹配的组件时，启动组件被设计为抛出异常让 App 崩溃。

—— 为什么做出这样粗暴的设计呢？因为 “覆水难收”（已上路、没有回头路）、又不能降级处理（这也是原生路由的缺憾之一），所以只能强迫其崩溃。

不过呢，对此可以通过事先判断组件是否存在的方式，来规避崩溃：

```
if (intent.resolveActivity(getPackageManager()) != null){
　　startActivity(intent);
}
```

> 划重点 👆👆👆

-   隐式 Intent，如果系统中存在多个条件匹配的组件，每次会弹出界面让你选择其中一个启动。
-   显示 Intent 和隐式 Intent 的共同点在于，二者都可以访问 App 内部或其他 App 的组件。当某个组件想被其他 App 访问，需要在清单中为组件设置 exported = true，否则设置为 false（默认为 false）
-   当 Intent 包含显式条件和隐式条件时，系统会以显式条件为准。（当然通常我们不会这么干）
-   显式 Intent 使用 setClassName 或 setComponent() 可以避免类耦合。
-   Intent 传递数据的大小有一定限制，具体的原因可以参考这篇，本文不再累述：  
    [《学Android 这么久，Intent 传递数据最大多少呢？》](https://www.jianshu.com/p/4537270be897)
-   Android 7.0 及以后，App 间传递文件，需要适配 FileProvider，不然会抛出 FileUriExposedException。这么设计是出于安全考虑。具体的原因和适配方法，可参考  
    [《重学安卓：存储访问适配篇》](https://xiaozhuanlan.com/topic/5240638917) 的解析。

## 综上

-   路由被广泛地应用于对目标的寻址和数据转发。
-   三大组件的本质皆是简易的配置模板。
-   三大组件的路由依靠背后的路由机制统一管理。
-   三大组件通过 Binder 完成路由。
-   Intent 存在显式和隐式两种类型。
-   显式 Intent 是明确指出目标组件，隐式 Intent 是通过条件匹配目标组件或组件群。
-   隐式 Intent 是和目标组件在清单文件中配置的 IntentFilter 去匹配。
-   IntentFilter 条件项包含 action、category、data、type，其中 data 和 type 用到的 URI 和 MimeType 是普适的、标准化的资源定位和格式筛选依据。
-   Bundle 可以装载基本数据类型或序列化的引用类型数据，由 Intent 携带，通过 Binder 完成进程间通信。

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