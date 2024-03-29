---
created: 2021-11-09
source: https://help.flomoapp.com/about-us/about-us/development-concept.html
author: 
---

# [flomo 的开发理念 | flomo 101](https://help.flomoapp.com/about-us/about-us/development-concept.html)


## [#](https://help.flomoapp.com/about-us/about-us/development-concept.html#%E6%8A%80%E6%9C%AF%E9%80%89%E5%9E%8B) **技术选型**

最初面临的决策是，**第一个版本选用什么平台和什么关键技术**。许多的笔记软件选择了 iOS 和 iCloud，自然也有其理由，而 flomo 偏不。

**为什么不选择 iCloud？**

iCloud 是 Apple Only 的技术，使用 iCloud 便意味着抛弃 Windows 和 Android 用户。

我们希望服务尽可能多的用户，同时也希望能满足单一用户的多个场景——或许你不知道，并不是每个 iPhone 用户都在用 Mac。

**为什么不选择 iOS？**

我其实是积极考虑过 iOS 作为第一个版本的，还向[图拉鼎 (opens new window)](http://mp.weixin.qq.com/s?__biz=MjM5OTA5MDg2MQ==&mid=2649029849&idx=1&sn=3edcfb09f103158f9791d078d05ac254&chksm=bed0931b89a71a0d203e9ffc1e7fa8589bad3f5c5031c727ab2df465e2da4a460328a76042db&scene=21#wechat_redirect)请教过一些技术问题。但最终还是选择了 Web。

Web 技术意味着**更好的兼容性**，真正的**一次编写、多端复用**。事实上，flomo 最初只实现 Web 端；额外花一点点精力后，便支持了手机端；再使用 PWA 的一些些特性，便在手机上拥有了接近于 Native App 的体验。

Web 技术也意味着**更快的迭代速度**。产品早期，尚未定型，迭代速度非常重要。Native App 的迭代，再快也得按天来；而 Web App，可以做到高频的迭代更新。\\

**为什么 iOS App 和 Android App 又选择了原生技术？**

早期阶段会有很多取舍。而度过早期阶段，长期价值的权重会提高。作为一款富交互的 App，原生技术无疑能够提供更好的体验。

## [#](https://help.flomoapp.com/about-us/about-us/development-concept.html#api) **API**

flomo 的最小单位是 memo，一条简短的内容。所以，它对界面的依赖度是低的，也就是天生适合 API 交互。

flomo 也早早就有推出 API 的计划，原计划在 2021Q2。实际，上线于 2020 年 11 月。

为什么能大大提前？

因为想清楚两个事情：

**1\. 并不需要功能完备的 API。需求最旺盛的是输入 API。**

**2\. 并不需要做那么严谨的认证（OAuth 是人类的敌人好吗）。一串随机字符足以解决问题。这对开发者实际也更加友好。**

想清楚后就简单了。我用很短的时间就完成了实现。这支撑了目前丰富的第三方工具生态，覆盖了 iOS 捷径、Chrome 扩展、Alfred App、Telegram Bot 等等等等。

当然，也要感谢诸多热心的开发者。

这里多说几句。我观察到，对于 API 有两种广泛误区：

1\. 直接拒绝 API，拒绝开放。

2\. 无条件热情拥抱 API。

都没有必要。客观一点朋友们，也不要原教旨主义。

API 不过就是诸多路径之一，不是目的。虽然也是个不错的路径，但不必把路径当作目的。

## [#](https://help.flomoapp.com/about-us/about-us/development-concept.html#%E6%8B%92%E7%BB%9D%E5%81%9A%E5%AE%8C%E7%BE%8E%E7%9A%84%E4%BA%A7%E5%93%81) **拒绝做完美的产品**

少有人知道的事实是，没有必要做一款完美的产品。

即便是大家认为对细节最最最吹毛求疵的 Steve Jobs（是不是猜错了？不是罗永浩），即便是他最有代表性的作品、也就是大家认为最最最完美的 iPhone，在早期也是最最最不完美的产品。

甚至于，每一款 iPhone 的发布，都会被热心网友拿来和石头做一番比较。

![](https://flomo-resource.oss-cn-shanghai.aliyuncs.com/101/images-124.png)

而在 Tim Cook 时代，你很难再对 iPhone 挑出太多毛病，但同时也被广泛认为遗失了 Jobs 时代的创新精神。\\

**拒绝做完美的产品，把精力聚焦于用户价值的核心。**

flomo 的核心是什么？memo 相关的一系列功能。

非核心呢？其它所有。

所以你会看到，设置页面大都有些粗糙，边缘场景也未必照顾到位。甚至有些基础功能，比如“修改昵称”，至今都还没有。

一个最为直观的例子是，flomo 的付费功能，最早只是用第三方服务 MikeCRM 驱动的表单。即便今日，部分付费场景也依旧靠表单驱动。

在这些地方，用户体验确实是差的。正如早期的 iPhone 甚至不支持彩信。

## [#](https://help.flomoapp.com/about-us/about-us/development-concept.html#%E5%9B%A2%E9%98%9F%E7%94%9F%E4%BA%A7%E5%8A%9B) **团队生产力**

flomo 的许多运营策略，大都经历了从不 scale（aka 人肉）到 scale 的过程。

最初不 scale 是因为：

1\. 判断永远可能出错；

2\. 快速启动是一个重要约束；

3\. scale 的前置条件是标准化，而标准化意味着信息损失和容错性低下。

而之后 scale 是因为：机器可以在许多地方提高效率。

就本质而言，工程师的超能力是**使用计算机器作为杠杆**，以高效率地完成重复性工作。

**自己的开发工作要多使用杠杆，也要保持觉察，适时为你的伙伴递上杠杆。**

## [#](https://help.flomoapp.com/about-us/about-us/development-concept.html#fin) **Fin**

就举这么几个例子。

其实我想说的是：**工程师也可以直接对业务产生影响**。

近年来，看过太多的工程师，主动放弃自己的可能性，只安心实现别人提出的需求，甚至于对需求价值都不做思索，乐于将自己作为一项优质的执行资源。

但残酷的事实是，对业务缺乏思考的工程师，也很难成为优质的执行资源。

我不会劝你们不要背叛业务或团队，我只希望**曾经有梦想的工程师，不要背叛自己。**

借用建硕的话结束本文。

> 虽然工程师需要具备一些开发者的技能，比如写代码，但**从根本上，工程师的能力，和代码无关。**
