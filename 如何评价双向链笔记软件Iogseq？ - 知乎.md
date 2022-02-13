# [如何评价双向链笔记软件Iogseq？ - 知乎](https://www.zhihu.com/question/412118689/answer/2205580101)

笔记软件历史

\========

最简单的笔记可以记录在word里。

对word格式进行简化就是markdown。软件为typora。

当积累的笔记多了，需要管理起来，就发展出了树形笔记。界面为双板。左面是树形节点，右面是节点内容。软件有为知笔记、tirliimn、mempad、notion、leoeditor。

笔记是大脑内知识的外显，而知识是结构化的，所以人们将word和markdown的结构化部分，也就是层级标题，拿出来优化操作的便利性，这就是大纲笔记。大纲笔记结构化编辑的便利性在于回车就建立新的标题，缩进就建立子标题。大纲软件有workflowy、dynalist、幕布、orgmode。最初的大纲笔记是单板界面。后来也有dynalist这样的双板界面。大纲笔记以及word和markdown的层级标题都是单页内的树状结构，而树状笔记是页面间的树状结构。

word和markdown是单页面笔记。多个单页面笔记之间用类似互联网网页的超链接相互链接起来就是wiki笔记。wiki笔记不再是只能以树状结构来组织笔记，而是任意两个页面之间都可以建立连接，这样的结构是网状结构。一开始从一个网页通往其它页面只能通过点击页面内的链接来操作。后来发展出以zim为代表的双链的wiki，双链指在一个页面的视图里还可以看到有哪些页面链接到此页面。tiddlywiki在此基础上发展出更多功能，不仅是双链wiki，而且可以用页面的标签系统建立另一套网状结构。tiddlywiki的插件tiddlymap可以将网状结构渲染为图谱，图谱能更直观的看到网状结构的全貌。直接在图谱上编辑笔记的软件是thebrain。

近期出现的软件roam research，简称rr，是一种新型的笔记软件。rr在双链wiki和图谱的基础上，将页面的操作改为大纲操作。rr还用双板给双链wiki带来树状笔记界面。rr在大纲内部的条目之间也实现了双链。rr还带来了tiddlywiki、leoeditor、triliumn的嵌入功能，嵌入功能可以嵌入影子节点。

没有提到的笔记功能有待办事项、时间管理、表格、属性、可保存的查询、文学编程、节点克隆。

介绍

\========

A privacy-first, open-source platform for knowledge sharing and management. 隐私优先的开源知识分享和管理。

A local-first, non-linear, outliner notebook for organizing and sharing your personal knowledge base. 本地优先的非线性大纲笔记。

上面是logseq在github上的自我介绍。logseq已经全面实现了这些介绍的特性。

首先说非线性大纲笔记。非线性大纲笔记的代表是目前在笔记爱好者圈子里大火的roam resaerch。roam research简称rr。rr在国内外有很多模仿者。logseq就是rr的模仿者之一。logseq实现了rr的所有关键功能。

logseq是数据和程序的双离线，所以是本地优先隐私优先的。数据离线目前是调用的chrome86的本地文件api，暂时只支持桌面端。移动端的本地离线数据功能需要等待后续开发。程序离线指按logseq在github上的指南就能自己编译出logseq的代码。编译后是纯静态网页文件。可以在本地启动任意http服务器来访问这个静态网页文件。

开源指logseq前端已经完全在github开源。基于agpl3协议。由于logseq数据可以本地离线，所以可以完全不需要后端。logseq是clojure语言编写的，将来会开放js语言的api。

知识分享。logseq在github上的release代码是将内容发布为静态网页的代码。按照logseq的文档来一步步发布可选的笔记内容。这样内容可以公布在自己的网站。公布的内容原汁原味的保留了logseq的全部功能，除了不能编辑之外。

还要提到的是同步功能。logseq笔记目前可以和github同步。将来会提供更多同步目标。

另外很有意义的功能是logseq正试图一步步兼容markdown格式和org格式。logseq可以选择以markdown格式存储还是org格式存储。markdown格式和org格式都可以在github得到显示支持。这两种格式在本地都可以用其它编辑器打开。可以用mardown编辑器打开markdown格式。可以用emacs打开org格式。用大纲操作markdown文件我目前只见到了logseq这一个实现。logseq对两种格式的兼容目前还未完全实现。

和其它笔记工具的大致比较。和obsidian比具有大纲操作。和rr比，开源、本地、脱离软件发布。当然，logseq也有很多不如其它软件的地方，等待logseq的1.0版发布再看。

选择

\========

如果不需要图谱、wiki、双板功能，可以选择dynalist、notion、workflowy。它们最近都实现了双链。

如果不认为笔记最重要的是隐私的保护，不认为只有本地文件才能真正保护隐私，可以选择rr以及rr的国内模仿者roamedit、葫芦笔记。

如果不需要大纲部分，可以选择obsidian和思源笔记。

如果以上都需要，logseq是一个好选择。

另外，md粉和org粉会喜欢将logseq作为补充。

什么样的软件让人放心

\========

一个是开发商赚到钱的软件，这样开发商就有可能把软件持续开发下去；

一个是开源的软件，作者放弃后其它人还能接手开发；

最让人放心的是数据格式简单的软件，就像markdown软件，主要就是标题的井号，分分钟就能开发一个编辑器；

还有就是离线的软件，服务器关闭了也不受影响；

—more—还有墙内的软件，不怕屏蔽，不过国内的软件会因为违规或被举报突然死亡；不随意杀死自己产品的大企业的软件；
