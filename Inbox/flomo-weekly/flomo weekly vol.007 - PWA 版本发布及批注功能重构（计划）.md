---
created: 2021-11-10
source: https://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247483700&idx=1&sn=ca579c578f1ed749d5ffaf182396420f&chksm=e9212355de56aa431cd71938e620e3c0400b240e2bfe4331915a324fb36f77132e4692fa67b2#rd
author: flomo
---

# [flomo weekly vol.007 - PWA 版本发布及批注功能重构（计划）](https://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247483700&idx=1&sn=ca579c578f1ed749d5ffaf182396420f&chksm=e9212355de56aa431cd71938e620e3c0400b240e2bfe4331915a324fb36f77132e4692fa67b2#rd)


![图片](https://mmbiz.qpic.cn/mmbiz_png/wDNLH7zcd1MQnvHB8DGEXFE93wJFvjB9VyXo9ibR38XxNmlV2fDdnrzzo8ibjaNeXSsxnRW3icp3TjBZXe0sicWJew/640?wx_fmt=png&tp=png&wxfrom=5&wx_lazy=1&wx_co=1)

一别多日，见字如面，这是 flomo weekly 第七期。如果你想参与到 flomo 的内测中来，点击阅读全文，填写内测申请表即可加入。

假期趁着大家都外出游玩的时候，把 flomo 底层进行了一番大改造，目前已经是一个 PWA 应用了（即Progressive Web App，渐进式应用）。不过大家不必深究背后的技术原理，只需要知道几个明显能感知的地方：

•**速度快**：本地因为有了缓存，加载速度极大地提高。虽然达不到秒开，但是几十条 memo 的情况下，大概 2s 以内肯定搞定了。•**可安装**：点击用户名右侧按钮，即可在桌面 Chrome 内核浏览器及 iOS/Android 手机端安装应用，体验如从应用市场下载的差不多。•**弱网可用**：在网络不稳定的情况下，也能利用本地的缓存文件保证基础的使用（如浏览内容）•**更安全**：通过 Https 提供，安装源唯一，防止窥探和确保内容不被篡改。

在还没有提供客户端（精力问题）之前，这个 PWA 版本应该是几乎接近原生 App 体验的了，建议大家都迁移到这个版本来使用（当然是希望你们放在首屏桌面啦），具体方法如下：

•各个平台下，点击用户名右侧的箭头•点击「安装客户端」，根据提示操作即可。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

除此之外还做了一些改进：

•网址从 「link」 改为 「abc.com」 这种格式，方便大家识别网址是什么的时候减少干扰•简化了部分样式，让界面更加干净；增加了登出功能。

___

以上只是假期前半截做的具体工作。假期中和 Lightory 在杭州吃了顿饭，顺便去「小冰岛」放飞了无人机，拍到了一张很有趣的照片：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

一路上也把 flomo 十月份的计划给安排如下：

•**发布框和排序重构：**移动端会有比较大的改变（嗯，打脸了，放在上面了）•**批注功能重设计：**批注的内容将会成为一条独立的 memo，并支持进一步关联和探索•**支持 Pin** ：可以 Pin 一个 tag 或一个 memo 在界面上•**Tag 相关**：支持自动完成及支持自动涌现（估计十月下旬到十一月初）•  **搜索改进：**搜索功能重构，支持结构化搜索

其他非结构性的优化，就不罗列在此了。

___

其中关于批注部分值得单独讲讲，最初的启发来自Twitter 用户 🄰Future

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

在这个主题帖中，他提到了双向链接的另一种可能。

经过一个多月的思考和使用，加上社群中大家不断地反馈，我们发现 flomo 的标签需求逐渐变强。

但细究下来却是最初版本的批注功能没有达到预期，无法成为一个围绕主题不断增加memo 的功能，而是单纯的变成了「备注」功能。而由于此功能的不完善，导致大家开始不断地添加 tag，继而产生了对 tag 更多管理的需求。

所以我们将会把批注功能重新改造（如下图）

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这个版本更新完之后，配合后续的 pin 功能，就可以实现：  

•Pin tag：快速找到某个长期研究的领域（如交易平台）•Pin memo：快速找到当前研究的话题（如会员定价模式）  

假期把《how to take smart note》看完了，后续会在周刊中分享。

但是有一句话让我印象深刻，也是我们无法帮到诸位，只能依靠诸位自己的：**如果没有合适的道路，再快的汽车也没用。如果不改变思维方式，再好的工具也没用。**

**往期回顾：**

[flomo weekly vol.004](http://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247483670&idx=1&sn=f28848a349cfd46ce9265515b343ce71&chksm=e9212377de56aa61cd46d1aa192d701c71ed55bb37588f852916772856d7538f1bf1d1d40577&scene=21#wechat_redirect)  

[flomo weekly vol.005](http://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247483679&idx=1&sn=a4e98d2ebc689d4df0f450e8682911ec&chksm=e921237ede56aa6868fbcf46a465435cc31f84e1353ad9af03068dc052757b00eb9c4839b679&scene=21#wechat_redirect)  

[flomo weekly vol.006 - 「回顾」功能发布](http://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247483690&idx=1&sn=dcdc89cf9ed769993aa85dc759364ce3&chksm=e921234bde56aa5d40bf04eb94a8e909b75980064e88e470962a52ea17fcd69c3f13af68397f&scene=21#wechat_redirect)  

如果你想参与到 flomo 的内测中来，点击阅读全文，填写内测申请表即可加入
