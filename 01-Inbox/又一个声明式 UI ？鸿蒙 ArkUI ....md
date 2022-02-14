# 又一个声明式 UI ？鸿蒙 ArkUI 问世

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=Mzg5MzYxNTI5Mg==&mid=2247489719&idx=1&sn=4a949d71ee69428049016fd07af69499&chksm=c02d7564f75afc720aa595b00f6396b643973f5070500c41cb190cde3e82f3620fb0b556c3ca&mpshare=1&scene=1&srcid=1224iKbgR5LVpgfnipCTSMIP&sharer_sharetime=1640305679015&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)yagamis AndroidPub

相信关注前端开发的同学，一定听过近些年，DSL 描述式的 UI 构建写法，大有取代传统命令式布局的趋势。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fbmc8z80Q8fDwdzNny2KOmwZNW6qSL8zmZEUSC2UkeiaxC4ura9pKV4VLIAzP9luia7L8EbZ0liay27vgCGm8tU5ibA%2F640%3Fwx_fmt%3Dpng)

传统上，写一套 UI 代码，需要根据数据的逻辑，手动的在业务代码里，去改变界面 UI 元素的状态，造成业务代码和 UI 代码搅在一起，黑话=“耦合性极高”。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fbmc8z80Q8fCOMsYMNsoHic2g5jYh7OOU12nv7PIbqGicahzQP76TjmyY2tR6Dw0xy3gDiaZIZ88VeVl19mHUK1QYQ%2F640%3Fwx_fmt%3Dpng)

结果就是，把人人都炼成了一身诸哥的本事，事必躬亲，鞠躬尽瘁…

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fbmc8z80Q8fCOMsYMNsoHic2g5jYh7OOU1MlKibCSyqXW64drPp4Fn1ibbJZibnOdGPQYhBZicO9h4XFwqDUU64cPlNA%2F640%3Fwx_fmt%3Dpng)

好的。话说在网页开发的远古时代（2013 年以前），想写一个炫酷的网页，往往需要直接操作 HTML 元素，比如控制网页上一个价格数字根据选择不同优惠券的变化，需要直接操作这个价格文字元素。

类似这样的代码：

    if(chooseShuang12) {    document.getElementById("oldPrice").value *= 2.5   //原价提升2.5倍    document.getElementById("price").value *= 0.5     //现价显示为0.5倍}

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fbmc8z80Q8fCOMsYMNsoHic2g5jYh7OOU1IicNtWlZcmicevsACgYb5OEa4jjn9t9AGKGQrOKHt0WGw3JXmDv8OfFQ%2F640%3Fwx_fmt%3Dpng)

这种写法对互联网老板来说不能说有问题，只能说不利于程序员偷懒。-

时间来到 2013 年，Facebook 里有一个大神年入百万刀，终于 bear 不了这种年复一年的低端手写操作 UI 元素“low”代码，于是他搞成这样的写法：

    <div>        {this.price}</div>

{}花括号里是一个关于价格的变量，div 是价格 UI 元素的容器，只要 price 发生变化，div 就自动更新，不再需要去设置它的值。这样前端程序员就不用再费脑子去更新 UI 了。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fbmc8z80Q8fCOMsYMNsoHic2g5jYh7OOU1zFWicOqMicXvnHIGrCW4xiaFOoMViawZFaKuicOzCPJgrrz2ibNPDSQicQm2g%2F640%3Fwx_fmt%3Dpng)

至于如何去更新的，啥时候更新，他写了一套算法，叫内存差分算法，大致是把页面元素结构都事先拷贝到内存中。

如果变量发生了变化，比较一下前后的差异，只更新发生变化的那部分，因为有了内存缓存和算法比较，更新效率远远超出了原有的直接设置的方法。

然后的事，你就知道了，这一套东西叫 React，强势垄断了前端开发的前沿领域，不会 React，可能面试都通不过。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fbmc8z80Q8fCOMsYMNsoHic2g5jYh7OOU1jXcUiboaQjOtrN7HOomnCBEV70kC7pDYsCQJEiaia65smlqrJnOHYQicvA%2F640%3Fwx_fmt%3Dpng)

仅仅一年后的 2014，有一个在谷歌纽约分部实习的中国留学生，叫尤雨溪，同样写 UI 也写烦了，然后想写个框架，正好吸收了 React 的先进理念并改进、优化了一下语法：

    <div>  {{ price }}</div>

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fbmc8z80Q8fCOMsYMNsoHic2g5jYh7OOU1g4IBUgBB4wfdBIibJeE3YqQxRB1xhCtxIT9WrMc2RMjrfnEoJgh0V3Q%2F640%3Fwx_fmt%3Dpng)

他赢了！尤雨溪的 Vue 框架确实比 React 可能更接地气，更轻巧，React 有的，基本上 Vue 都有，而且更好用好学。

支持国人吸收好东西创新，并且让国人开发者的学习和普及成本大幅降低，这是功德无量的事。

看到这里，你是不是看到了 ArkUI 中的 JS UI 是怎么来的了？没错，ArkUI 的 JS UI 范式，就是 Vue 的写法。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fbmc8z80Q8fCOMsYMNsoHic2g5jYh7OOU1rHd9MohCibnvHNmvDhXL3Hj8B7om3gmgvhGy8aeIFdGk5ExVQfMTVEw%2F640%3Fwx_fmt%3Dpng)

故事刚刚开始，2015 年，Facebook 看 React 这么火爆，直接成立了专门的组来深度开发，于是看重了手机端原生跨平台的市场，推出了 React Native。

直接用 React 的语法，可以编译出 iOS 和 Android 的原生 App，性能远比 H5 的网页要好多了，抢了不少开发者，生态搞的有声有色。-

苹果、谷歌的大佬们看的很不是滋味，这不是釜底抽薪么？

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fbmc8z80Q8fCOMsYMNsoHic2g5jYh7OOU1aibjbpchhMUITldHGD1CeJ6XfibicrpLUjR75KRTdcppTPClBibBZYPUVg%2F640%3Fwx_fmt%3Dpng)

故事渐入佳境，前端开发领域此刻暗流涌动，新一轮的 iOS 和 Android 端 UI 开发框架军备竞赛悄然发力。这次是主角们登场了。-

憋足 2 年，谷歌率先发力，强势推出 Flutter 开发框架。

基于 Chrome 浏览器渲染网页的 Skia 2D 引擎，号称开发出来的 App，几乎无需更改任何代码就可以跨平台。

iOS 和 Android 端表现没有任何差异，而且比 React Native 性能更好，生态更繁荣，这个确认他们做到了，而且更新迭代以天为单位，诚意非常足。

然后唯一的缺陷就是，搭载一个谷歌内部无人问津的编程语言 Dart，这个 Dart 语法非常啰嗦，写起 UI 来嵌套很严重，但是基于生态和性能优势，开发者也就一直忍了，毕竟没有完美。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fbmc8z80Q8fCOMsYMNsoHic2g5jYh7OOU1aXXrUO5EcL7RjP4b7VCA56ISrAQ9ibc4licjQJzTyGkIF1GJKacv138Q%2F640%3Fwx_fmt%3Dpng)

Flutter 确实发展太快了，几乎占据了新原生 App 开发市场的半壁江山，对纯 iOS 和 Android 原生开发者简直是雪上加霜。-

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fbmc8z80Q8fCOMsYMNsoHic2g5jYh7OOU1CUwAbgfCrZlL2ibbbCcGIZWhn9LOmj6maF6AiblcKw6zPAzklEKGvT4A%2F640%3Fwx_fmt%3Dpng)

2019 年，苹果再也忍不住了，在开发者大会上重磅推出 SwiftUI 框架，从名字就知道，苹果命名是非常精准的，就是写 UI 用的。

发布会上的 keynote 对比，把原先的 iOS 上的 UIKit 简直虐成渣，台下听众纷纷欢呼，好像苹果重新发明了开发框架一样。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fbmc8z80Q8fCOMsYMNsoHic2g5jYh7OOU1e2pJIcA7FZoruBCWLbv3CGn8qvkJUkI9B4AibUPbmvygk3PaS6bsaLw%2F640%3Fwx_fmt%3Dpng)

我其实一直在搞 iOS 开发，看到这样的东西，简直是着了魔，代码简化后少了 90% 而且更易懂，加上实时预览器，启动模拟器调试的时间也少了 90%。惊呼这玩意简直就是未来。

到了 2020 年 11 月华为开完 HDC 后，一个非常偶然的机会，在 51 社区的引荐下，参与到鸿蒙的开发体验中来，其实有了前面的“世面”，我对 JavaUI 是不感冒的，认为太土了，啥年代还用小米加步枪。

一直没有深入 Java 相关的开发者，就在要丧失信心之际。2021 年 5 月，ArkUI 组就带来了这震惊业界的消息，鸿蒙即将支持 DSL 开发，而且语法致敬了 SwiftUI、Flutter，React，10 月 HDC 正式随 SDK 7 推出了技术预览版， 编辑器和预览器一应俱全，非常有诚意。

经过一段时间的实际项目和课程实践，我总结了相比较鸿蒙既有框架的 5 个优势：

*   减少了编译步骤，保证了最大的UI性能。实际体验，比 JS UI 提升 20%。
    
*   使用 DSL 代码，比 Java UI 节省代码 90% 以上。200 行的代码，如今只需要 20 行。
    
*   使用扩展的 TS 语法，强类型，相比 JS UI 减少了可能的运行时错误，更安全。
    
*   配套更好用的实时预览器，大大减少了启动模拟器的次数。
    
*   更使用的 Preview 修饰符，无需运行，即可单独调试一个组件的 UI 所有状态成为可能。
    

相比 Flutter：

*   ArkUI 大大减少了 Dart 嵌套语法地狱的问题，写法上更简洁明快。
    
*   预览器和 Preview 修饰符是调试 UI 不可或缺的工具，效率提升 100 倍的利器。
    

相比 SwiftUI：

*   状态管理更完善，有非常清晰的状态管理和存储修饰符配套。
    
*   SwiftUI 预览器缺少组件树功能，ArkUI 的组件定位功能十分好用。
    

当然，相比较上述 2 大框架，ArkUI 尚处于技术预览阶段，自带组件数量不足、缺失和 Bug。

以及最要紧的生态库尚未建立，需要时间去完善，根据官网规划，相信到 2022 年中段，这方面会有长足的进步，期待更好的 ArkUI！

官网：

https://developer.harmonyos.com/cn/develop/arkUI/

\- END -

推荐文章

*   [HarmonyOS 开发，从 Sample 学起](http://mp.weixin.qq.com/s?__biz=Mzg5MzYxNTI5Mg==&mid=2247488307&idx=1&sn=a33ae98cb94b9b55996e5aa37df4b55d&chksm=c02d7ee0f75af7f67999afbb2c71777ec457849e1496f0c880356c4c46934514b6a7c6a93226&scene=21#wechat_redirect)-
    
*   [字节前端团队点评 HarmonyOS](http://mp.weixin.qq.com/s?__biz=Mzg5MzYxNTI5Mg==&mid=2247486596&idx=1&sn=79553e5bb66d0ace62ff23f91fa706d9&chksm=c02d6157f75ae84179b59f77c7d199eddf452d171fcb60525d9b41f2981a0324d00563d1f0b9&scene=21#wechat_redirect)-
    
*   [开发鸿蒙应用能引入 JAR 或 AAR 吗?](http://mp.weixin.qq.com/s?__biz=Mzg5MzYxNTI5Mg==&mid=2247487248&idx=1&sn=03dcb0df983f3e76cafa81d2cbc847fd&chksm=c02d62c3f75aebd58bec63fd49284cfd31000469418c7a59d884dd2fe4160e347b79eabc0ea1&scene=21#wechat_redirect)
    

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=Mzg5MzYxNTI5Mg==&mid=2247489719&idx=1&sn=4a949d71ee69428049016fd07af69499&chksm=c02d7564f75afc720aa595b00f6396b643973f5070500c41cb190cde3e82f3620fb0b556c3ca&mpshare=1&scene=1&srcid=1224iKbgR5LVpgfnipCTSMIP&sharer_sharetime=1640305679015&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)