# [有什么 Roam Research 的替代方案？ - 知乎](https://www.zhihu.com/question/409872334/answer/1367025596)

想要寻找 Roam Research 的替代方案，首先要理解 Roam Research（以下简称为 Roam）解决了什么问题。

本文选自「**[iPad Power User](https://iois.me/product/ipad-power-user-subscriptions)**」，这是一个关注 iPad Pro 与 iPadOS 生产力的付费邮件通讯，欢迎试读、订阅。

___

如果你还不了解 Roam，可以通过少数派的这篇**[文章](https://sspai.com/post/60787)**详细了解其基本的逻辑和方法。

需要说明一点，作为 Roam 相对早期的用户，我已经放弃了这个服务。原因在于，我并不愿意将自己的笔记完全托管在一个云服务上，就像我在上周会员通讯「Deep Reading」里所言，使用云服务本身就是一个关于信任的问题，换句话说，我不相信这样的云服务。

虽然我不再使用 Roam，但在之前使用该服务以及在 TiddlyWiki 的探索过程里，我对于 Roam 所代表的理念有了新的认识。

首先，**Roam 不再追求笔记的收集与保存**，这是与传统笔记工具，如 Evernote 「背道而驰」的产品理念，也正如我在「现代数字笔记指南（1）」里谈到的那样，很多笔记工具总是在制造一种「收集就是知识」的错觉，以至于很多人的笔记工具最后成为了阅读器。

鉴于 Roam 以及诸多「模仿者」，我的确也看到了一股新的笔记产品理念的兴起，真正让「笔记收集」变成「笔记生产」。

其次，**Roam 展现出「信息/知识/内容再生产/再利用」的潜力**。作为内容工作者，我一直在思考「内容再生产/再利用」的有效方式，因为在我看来，我所输出的不应该是一篇篇文章，而应该是一篇篇未来文章的「草稿」，这些「草稿」可以在一个维度上构成文章 A，在另一个维度上可以组合为文章 B，以此类推。

但要实现这个理想状况，有两个巨大的挑战：

-   需要把内容分解为颗粒度更小的内容块，比如一个观点，一个例子等等；
-   需要有丰富的内容链接机制，可以方便、快速地构建不同内容块之间的联系；

Roam 的产品设计里，隐隐约约藏着解决思路。比如通过 Block 的形式，鼓励用户将长篇的内容揉碎，变成一个个类似列表的内容，这与我此前强调的「笔记原子化」有异曲同工之处，我在当时写道：

> 「原子化」笔记带来的是更精准的内容检索，同时，由于每一则笔记的信息含量足够小，可以实现更灵活的笔记组合。

![](https://pic1.zhimg.com/50/v2-d7284202c4c409277ad0c4420395f2e3_720w.jpg?source=1940ef5c)

而如何把这些零碎的 Block 整合在一起呢？Roam 使用的是双向链接，坦率来说，双向链接并不是什么新概念，关于双向链接的历史与现状，可参考这篇**[文章](https://maggieappleton.com/bidirectionals)**。但正如 Roam 创始人所**[指出](https://twitter.com/conaw/status/1264322710118645760?s=12)**的那样，Roam 的双向链接，可以精确到任意一个 Block，这种将最小单位内容链接在一起的能力，使得 Roam 变得与众不同。

更进一步，结合「Graph」功能，block 之间的联系变得足够可视化，便于用户定位、检索不同 Block 之间的联系。

这是 Roam 产品体现出的独特价值，它**改变了笔记工具惯有的逻辑，通过创新的方式构建了一套原子化、链接化、可视化的笔记使用方式，从而提升了信息/内容/笔记再应用、再生产的效率**。理解了这些，我们也可以在其他产品中探索相应的思路，由于我对于任何工具的优先考虑都是是否适配 iPadOS，所以包括 Obsidian、Zetter 等在内的桌面应用都不在我的计划之内，因此下面的一些替代品推荐，要么是基于 iPadOS 的原生应用，要么是可以通过 Safari 访问的在线服务。

比如 Bear，通过原子化的方式将笔记内容变得足够颗粒化，然后通过其提供的\[\[\]\]的功能将其链接起来。

![](https://pic4.zhimg.com/50/v2-a37b3b406b47f5844d8086855471f193_720w.jpg?source=1940ef5c)

再比如，你甚至可以在 Google Doc 里实现原子化、链接化的笔记机制，利用 iPad Pro 的 Safari，你可以完整体验到 Google Doc 的所有功能，下图展示的是快速添加链接，在键盘上敲击「CMD+K」的快捷键后，输入关键词，可以看到相关文档的提示。

![](https://pic1.zhimg.com/50/v2-bc9a8b7cfb4ff08bdd475cb62449d0cd_720w.jpg?source=1940ef5c)

当然，由于缺乏双向链接，在 Bear、Google Doc 上构建「信息/内容/笔记再应用」的机制相对比较困难，需要用户更主动发现内容之间的关系，并在不断整合中产生新的想法。

另一个值得思考的方向就是 Wiki，几乎所有的 Wiki 程序都拥有完善的内部链接功能，而且 Wiki 程序都是开源程序，在代码安全的基础上还可以实现自我托管和搭建，以下是一些值得尝试的 Wiki 程序：

-   Dokuwiki，无需数据库，仅需在 PHP 主机上就可以快速搭建，官方**[说明](https://www.dokuwiki.org/zh:manual)**；
-   Wiki.js 主打更具颜值的 wiki 形态，官方提供了 Docker 的搭建**[教程](https://docs.requarks.io/install/docker)**；
-   TiddlyWiki，仅仅一个 HTML 文件就支撑起了完整 Wiki 服务，丰富的插件还能将其变成各种形态的应用，你可以将其放在本地或者托管到 Github，非常灵活。

而在经过近半年的磨合与实践之后，我将笔记的流程从「Bear+Devonthink to Go」迁移到「TiddlyWiki+N」，这里的「N」，既有 Dropbox、Evernote，也有 AWS S3、Github 等，接下来的「**[iPad Power User](https://iois.me/product/ipad-power-user-subscriptions)**」里，我会系统而详细地介绍围绕「TiddlyWiki+N」的流程，欢迎持续关注。
