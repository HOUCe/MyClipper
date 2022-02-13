# Zotero 和它的朋友们: 一个文献阅读生态 - 少数派

source:: https://sspai.com/post/57943

上周用 Mendeley 整理论文的时候, 一不小心把本地论文全给弄乱了, 已经是第二次, 头都大了. 一气之下, 打算换一款文献管理软件, 于是转投了 Zotero. 用了一个星期, 乐不思 Mendeley, 太好用了! 推荐给各位.

对 Mendeley 感兴趣的同学可以看[我以前写的 Mendeley 的文章](https://mp.weixin.qq.com/s?__biz=MzA5NDgyMTQ4NQ==&mid=2247483722&idx=1&sn=775723381c10d3fb8f0a9845415fb241&chksm=90498c48a73e055eccaad683b3467a7b2d1cecea26469e7cfb999c68b93f314e270516f5c227&token=1728353099&lang=zh_CN#rd).

___

Zotero 是什么? 它是一款开源的, 跨平台的文献管理软件, 自身功能很强大, 同时还有丰富的插件可扩展.  

本文试图展示一个文献阅读生态: 以 Zotero 为核心, 通过插件连通其他应用/服务, 打造出一个高效的文献阅读软环境.

我的文献阅读生态使用了以下应用/服务, 括号中为供参考的替代品, 读者阅读后可根据喜好自行选择:

-   文献管理应用 - [Zotero](https://www.zotero.org/)

-   附件管理插件 - [ZotFile](http://zotfile.com/)

-   阅读器 - 福昕阅读器 (PDF Expert, Adobe Acrobat Reader, Xodo, ...)
-   浏览器 - Brave (Chrome, Firefox, ...)

-   Zotero 配套插件 - [Zotero Connector](https://www.zotero.org/download/connectors)
-   文本高亮插件 - [Weava Highlighter](https://www.weavatools.com/) (LINER, ...)

-   存储用网盘 - 坚果云
-   同步用网盘 - Dropbox (OneDrive, ...)
-   应用协同助手 - [zapier](https://zapier.com/) (IFTTT)
-   待"读"清单 - Todoist (Microsoft To-Do, ...)

将上述应用/服务组合起来, 我的收集-->管理-->阅读的工作流如下所示:

![](https://cdn.sspai.com/2019/12/18/b6d1feb0579bb33f5e02c04ef63d2026.png)

以 Zotero 为核心的阅读流

围绕着 Zotero, Zotero Connector 用于资料收集, 位于工作流的前端; Zotero 用于文献管理, 位于整个工作流的中心; ZotFile 用于本地附件 (文档) 管理, 一个功能工作在工作流的前端, 一个功能工作于工作流的后端. 以下, 将顺着工作流, 按照收集, 管理, 阅读的次序进行展示.

## 收集

#### Zotero Connector

资料的收集以 Zotero Connector 为主. 不夸张地说, 有没有 Zotero Connector 的 Zotero 是两个 Zotero.

> Zotero Connector makes Zotero greater.

Zotero Connector 除了能剪藏期刊/会议的论文 (如果论文是可免费下载, 将默认同时下载 PDF 格式的文档), 还能剪藏其他任何形式的网页, 比如维基百科, 博客文章, 视频, 等等.

![](https://cdn.sspai.com/2019/12/18/8c127722168a4c5eb9b6eb887bca073d.jpeg?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

Zotero Connector 剪藏 (图片来自互联网)

一切皆可剪藏的好处显而易见: 我们能够保存所有于研究有帮助的资料.

看完一篇论文, 除了做自己的笔记, 我们可能还想看看其他同学写的文章, 作者的代码, 网友的讨论, 研讨会的视频. 剪藏这一切, 而不只是论文, 对于论文的理解和后续研究的展开, 都大有裨益.

> Zotero Connector 直接剪藏到本地 Zotero 客户端, 速度很快; Mendeley Web Importer 先和服务器通讯, 可能是服务器在海外的原因, 反应很慢, 本地 Mendeley 客户端需要同步才能接收.

#### ZotFile 功能一: 目录监测

对于无法直接从网络收集的资料, 比如付费的论文, 可以先下载到本地, 然后拖拽到 Zotero 中. Zotero 能自动提取 PDF 的元数据, 补全论文的基本信息.

> 就个人体验而言, Zotero 的识别率极高, 100 篇论文都没出错, 但是导入 Mendeley 的每一篇论文, 我都要担心是不是识别成另一篇论文了.

除了手动拖拽, 借助 ZotFie 的目录监测功能, 我们只需将文件下载或移动到 ZotFile 监测的目录下, 它将自动被添加到 Zotero 中.

下载文件到 ZotFile 监测的目录, 和使用 Zotero Connector 一样便捷. 但是后者在剪藏时, 可以指定 Zotero 的分类, 并添加标签. 因此两者皆可的情况下, **推荐使用 Zotero Connector**.

## 管理

#### Zotero

![](https://cdn.sspai.com/2019/12/18/d5895108b72ac822524adea459c3299a.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

我的 Zotero 主界面

Zotero 的主界面如上所示. 功能区的划分很清晰, 不言而明, 从左到右从上到下依次是: 1. 组织结构, 2. 标签们, 3. 条目们, 4. 选中条目对应的基本信息/笔记/标签/关联条目们.

前文提到 Zotero Connector 能剪藏一切, 在此基础上, 我个人将 Zotero 中的资料先按研究领域进行了划分, 再根据类型划分到不同的分类下. Others 下, 我剪藏了代码仓库和数据仓库, 方便以后实验所需.

如果没用 Zotero Connector 在剪藏时分类, 资料将统一保存在_未分类条目 (Unfiled Items)_ 下. **建议, 尽早分类**.

左下角的标签栏, 展示了选中分类下的所有标签, 除了标记, 还能起到过滤器的作用, 并支持多选多重过滤.

因为我只分了研究领域的大类, 就把关键词以及细分的研究领域等制成了标签, 如此, 筛选论文就方便多了. 此外, 我创建了一个 _unread_ 的标签, 用于过滤未读的论文.

Zotero 在识别文档的时候, 有时候也能自动打标签, 比如从 arXiv 下的预印版就会带上 arXiv 的分类.

> Mendeley 没有标签功能. Mendeley 转 Zotero, 需要一一添加标签, 是一件相对繁琐的事情. 我的标签寥寥, 也是有苦自知.

中间栏, 与同一份资料相关的文档/笔记等, 都会被管理在自动创建的条目下. 我遇到过的一个意外是, 审稿中的会议论文, 以单独的 PDF 形式保存在 Zotero 中. 这时候可以手动创建条目, 再将文档移动到条目下.

查看 Zotero 的存储目录, 可以看到一个条目就是一个目录 (文件夹), 组织得很清晰, 只是目录名是哈希值, 不打开就无可知晓是什么资料.

以条目而非文档的形式保存资料的好处是, 一个条目下可以保存所有相关的资料, 自成一体. 可以多做笔记, 但是, **不建议**把相关的文档/网页都放在同一个条目下. 条目下如果是本地可打开的文档 (PDF, Word 等), 双击即可打开; 如果是剪藏的网页 (百科, 博客, 视频等), 将在浏览器中打开. 笔记不影响条目的打开, 但条目下如果有多份主文档, 双击条目只能打开其中之一, 反而显得混乱了.

更合理的关联相关资料的做法是, 用右边栏的_关联文献 (Related)_. 图中, 我将选中的条目与用到该技术的论文以及配套的代码仓库进行了关联.

关联, 可以做成类似施引与被施引的关联形式, 也可以做成论文与代码/数据/博客/视频的关联形式. 具体如何使用. 存乎一心.

右边栏的其他功能就不一一赘述了, 几个亮点如下:

-   笔记支持富文本, 借助[markdownhere4zotero](https://github.com/fei0810/markdownhere4zotero)还能支持 Markdown;
-   Zotero 的导出功能很丰富, 笔记可单独导出, 如下所示;
-   除了条目本身, 条目下的文档, 笔记, 截图等都可以添加标签, 进行关联.

![](https://cdn.sspai.com/2019/12/18/d03c68e74ed453bf1cdfc23ccf88e45c.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

Zotero 的导出形式

> 和 Zotero 相比, Mendeley 的笔记功能太原始了, 就像 Windows 的记事本一样.

#### 坚果云

Zotero 为每个注册用户免费提供了 300MB 的存储空间, 不够用的话 (迟早的事), 可以付费购买存储空间.

存储空间不够又想免费用? Zotero 支持 WebDAV 外挂网盘, 这里强烈推荐**坚果云**. 坚果云为使用免费版的用户提供了每月 1GB 的上传流量和 3 GB 的下载流量, 更重要的是, **不限存储空间**. 这样的方案, 对于大型软件或者海量照片, 可能是奢望; 对于文献管理, 却是绝佳之选.

事实上, Zotero 正是[坚果云生态圈](https://www.jianguoyun.com/static/others/partner/partner_list.html)中的一员. 坚果云官方还提供了[如何在Zotero中设置webdav连接到坚果云](http://help.jianguoyun.com/?p=3168)的教程.

外挂并启动坚果云之后, Zotero 管理下的文献有任何更新, 将自动同步到坚果云. 而我们, 只需要关心如何用 Zotero 管理好文献, 如何读好文献.

## 阅读

#### 阅读器

Zotero 并不提供 PDF 阅读功能. 专注于文献管理, 在我看来, 这反而是件好事.

PC 端, 我用福昕阅读器 (Foxit Reader) 看 PDF 文档, 主要用它的**高亮和注释**功能. 读者可根据平台和喜好自行选择 PDF 阅读器, 也欢迎推荐好用的阅读器.

鉴于 Zotero 能剪藏一般的网页, 我还推荐_浏览器+高亮插件_的组合, 将 PDF 论文阅读的习惯迁移到网页浏览中. 如此一来, 在 Zotero 中双击某条目, 要么打开高亮的 PDF, 要么打开高亮的网页, 阅读和回顾各种形式的文档都很方便.

![](https://cdn.sspai.com/2019/12/18/fa27f06db2748c765d88f758c9304284.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

Weava Highlighter 效果

我用的是 Chrome 系浏览器 (Brave) +_Weava Highlighter_. 这款插件支持高亮, 注释, 以及注释导出. 在打开读过的文章时, 它还能自动高亮并注释.Weava Highlighter 的一个缺点是, 国内用起来不是太稳定, 偶尔会失联, 需要刷新网页.

> Mendeley 自带 PDF 阅读器, 在其客户端中能直接打开 PDF 文件, 但是对于一般网页剪藏, 无可奈何.

#### ZotFile 功能二: 发送附件到指定目录并收回

在我的 Zotero 主界面上, 有两个前面没介绍的分类 _Tablet Files_ 和 _Tablet Files (modified)_. 它们是由 ZotFile 创建的, 目的是方便将文档发送到移动端阅读, 并在阅读后收回. Tablet Files 分类下就是我们发送的条目, 而 Tablet Files (modified) 下是在移动端阅读并修改过的文档的条目.

![](https://cdn.sspai.com/2019/12/18/917f405d06ca871db5c7a1438973fbf2.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

ZotFile 的附件管理功能

Zotero 是面向条目的, 以条目为单位进行管理. 甚至于, 它保存在云端的, 是上文提到的目录对应的压缩包. 这使得直接通过坚果云来同步文档很不方便.

实际上, 通过 ZotFile 发送到指定目录的, 是条目下的文档. 要实现桌面端与移动端的无缝连接, 这个目录需要是一个特殊目录, 即_网盘的自动同步目录_. 移动端的阅读器, 只要挂载相应的网盘, 就可以下载阅读, 如下:

![](https://cdn.sspai.com/2019/12/18/a76296e04f44648521ddc58fb418902d.jpg?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

PDF Expert 挂载网盘

因为操作系统的原因 (Linux), 我用的是 Dropbox. 但是, 对使用 Windows 的同学, 更**推荐微软家的 OneDrive**, 同步功能无差别, 还能直接用.

#### Dropbox + zapier + Todoist

既然已经使用了自动同步来对接桌面端与移动端, 一个很自然的想法是, 能不能在此基础上, 创建待读清单? 即在将文档发送到指定目录, 同步到云端的同时, 在我的 Todoist 中创建一条文献阅读的任务.

选择 Todoist 的原因很简单, 太好用了!

网上有不少服务能实现应用间的协同, 较知名的有: _zapier_ 和 _IFTTT (IF This Then That)_. 就此时对接的应用, zapier 可能是更好的选择. 它能对接微软家的应用, 但是 IFTTT 不能. 如果只想同步文档, 复用坚果云也完全可以.

![](https://cdn.sspai.com/2019/12/18/cb917e2098685c9e849d56ea30333dad.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

用 zapier 协同网盘与 Todoist

我在 Todoist 下创建了一个 _Reading List_ 项目. 文档自动同步到云端的同时, 将在 Reading List 下创建一条阅读论文的任务.

> 买书如山倒, 读书如抽丝.

如果你和我一样, 读一篇读论文, 结果多出来四五篇相关论文, 论文越找越多, 那么将紧要的论文加入待读清单, 安心阅读, 可能是不错的选择.

除了同步到云端和加入待读清单两个优点外, 将文档发送到指定目录还能节省流量. 我有一个阅读习惯, 高亮或注释之后习惯性地按 Ctrl+s. 频繁地更新 PDF, 本地和云端将一直处于更新中, 带来不必要的流量浪费. 因此, 即使在桌面端, 我还是习惯性地从同步目录打开文档.

如果论文是发送到"移动端"阅读的, 切记, 读完之后把论文再通过 ZotFile 收回来. 如此, 坚果云上存储的才会是带高亮有注释的读厚了的文档.

收回之后, Tablet Files 和 Tablet Files (modified) 下对应的条目将被移除.

然后, 循着高亮和注释, 再整理一遍文章的思路和重点, 做下笔记.

最后, 在 Todoist 中确认完成论文阅读, 完成感满满.

## 结语

本文展示了一个以 Zotero 为核心的, 集收集, 管理, 阅读于一身的文献阅读生态:

-   Zotero - 用于文献管理, 比如识别, 分类, 打标签, 做笔记, 关联其他资料
-   Zotero Connector - 用于资料收集, 什么有用就剪藏什么
-   ZotFile - 用于附件管理, 它主要提供了 3 个功能:

1.  自动将下载或移动到被监测目录下的文档加入 Zotero
2.  将文档发送到网盘同步目录, 无缝对接移动端, 读完取回
3.  统一附件命名规范 (文中未介绍)

-   Weava Highlighter - 提供浏览器高亮注释功能, 假装浏览器就是 PDF 阅读器
-   坚果云 - 用于存储 Zotero 数据
-   Dropbox - 用于文档同步
-   Todoist - 文献待读清单
-   zapier - 用于应用间协同, 将 Dropbox 的新文档转为 Todoist 的待读信号

Zotero 的可扩展性和可玩性相当高, 本文只是介绍了与阅读流相关的冰山一角, 希望对各位有所帮助和启发.
