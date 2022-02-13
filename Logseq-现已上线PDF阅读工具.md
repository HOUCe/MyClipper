# Logseq: 现已上线PDF阅读工具

[zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/397352305)观雨放羊一个喜欢法律和翻译的小学生取消关注

目录

收起

一、引言：

二、PDF工具介绍：

三、思考建议：

20210813更新：

## 一、引言：

在支持间隔重复记忆、Zotero文献协同后，Logseq紧接着在昨天更新的[0.3.2](https://github.com/logseq/logseq/releases/tag/0.3.2) (Beta Testing) 版本中上线了PDF阅读(批注与摘录)工具。至此，Logseq的科研功能已然齐备，再往后就是各种优化了。从笔者体验来看，刚上线的PDF批注工具非常的简洁优雅，功能一目了然，因此，本文仅将[官网更新记录](https://logseq.github.io/#/page/f3b8092e-ab5c-42c9-883c-81ae055b051d)做一个搬运，方便大家了解，同时也给出自己的思考。

另外，如需了解Logseq的间隔重复记忆、Zotero文献协同功能，可以参考笔者前两篇介绍：

[Logseq: 现已加入间隔重复系统(SRS)32 赞同 · 21 评论文章![](https://image.cubox.pro/article/2021081523320938460/76433.jpg)](https://zhuanlan.zhihu.com/p/392408529)

[Logseq: 现已支持Zotero文献协同22 赞同 · 21 评论文章![](https://image.cubox.pro/article/2021081523321032860/70199.jpg)](https://zhuanlan.zhihu.com/p/395008689)

## 二、PDF工具介绍：

**【功能概览】**

Logseq的PDF批注与摘录

**【使用介绍】**

**1\. 如何添加PDF？**

两种方式，一是如上面视频中所示，直接将PDF拖拽到Logseq中，二是使用“/”命令：直接输入“/upload”。

**2\. 如何高亮文本并摘录？**

(1) 选中目标文本，在弹出的颜色菜单中选择需要高亮的颜色

![](https://image.cubox.pro/article/2021111213253320612/92363.jpg)

（2）将光标定位至需要保存摘录的Block，然后按下粘贴键（Cmd/Ctrl + V）即可。**_点击所摘录的文本可跳转至PDF原文处，如PDF并未打开，则会打开PDF并跳转至原文处。_**

**3\. 如何高亮区域文本并摘录？**

两种方式，一是按住“Cmd”或“alt”键的同时，用鼠标框选出需要高亮和摘录的文本区域，二是点击PDF工具栏中的区域框选按钮，如下图，然后用鼠标进行框选。

![](https://image.cubox.pro/article/2021081523320993626/60140.jpg)

框选区域文本后，在弹出的颜色菜单中选择颜色，如下图，然后可粘贴至目标Block。_**原文跳转功能与前述相同**。_

![](https://image.cubox.pro/article/2021111213253378451/46126.jpg)

**4\. Logseq支持三种颜色主题，其中包括了黑暗主题(Logseq默认的暗夜绿)。**可点击PDF工具栏中的设置按钮进行调整。如下图：

![](https://image.cubox.pro/article/2021081523321093945/57677.jpg)

**5\. Logseq支持PDF大纲工具(目录)，并且可通过大纲(目录)点击跳转相应位置。**前提是PDF文件本身带有大纲(目录)。

## 三、思考建议：

笔者相继体验了Logseq有关文献研究的三项功能——“间隔重复”、“Zotero集成”、“PDF阅读”，每一项都非常便捷优雅。“Zotero集成”实现了文献资料的索引与提取，“间隔重复”有助于文献笔记的回顾与记忆，“PDF阅读”方便了文献内容的批注与摘录。这三项功能结合Logseq底层的笔记链接与管理能力，使Logseq成为了一个颇为理想的学术研究工具。我们可以期待Logseq团队继续对这些功能进行打磨优化，相信会有更多我们意想不到的新体验。

**笔者在此提一个小小的建议**(不知有无价值、可行与否，但很期待能够实现)：索引Zotero文献资料的同时，可**选择**是否将PDF附件一同添加进Logseq。并且，选择添加PDF后，点击PDF可直接在Logseq中打开，按住某一按键（如Cmd/Ctrl等）并点击PDF则打开Zotero并定位至相应位置。目前的功能是点击Zotero索引过来的PDF(应该是一个链接)可打开Zotero并定位至相应位置，如果要在Logseq中打开该PDF则需要手动添加。此外，更进一步，笔者也期望Logseq中的PDF批注与Zotero中的PDF能够相互同步，或者说，Logseq与Zotero共用同一个PDF文件。这些都是笔者的设想，或许不切实际，总归还是期望Logseq越来越强！

## 全文结束。如对您有所帮助，还请多多点赞分享！-

* * *

## 20210813更新：

在昨天上线的[0.3.3](https://github.com/logseq/logseq/releases/tag/0.3.3) (Beta Testing) 版本中，Logseq的PDF阅读工具迎来几个重要更新：

**1，支持从Zotero索引文献时自动“添加”文献PDF。**如下视频所示，在Logseq中成功索引文献后，在“Attchments”一栏，PDF名称(含链接)后新增“annotate”按钮，点击按钮可打开PDF进行批注和摘录。当然，点击PDF名称(含链接)仍然可以启动Zotero并定位到PDF所在位置。

索引Zotero文献PDF并批注

**注意：**要实现该功能需要在设置(Zotero设置)里配置好Zotero文件目录：

![](https://image.cubox.pro/article/2021111213253313921/15444.jpg)

至于Zotero文件目录，可在Zotero设置（首选项）中，如下图所示的位置获得。

![](https://image.cubox.pro/article/2021111213253313268/11824.jpg)

**2，将摘录添加到笔记链接。**

添加摘录到“Linked References”

**3，提供在线PDF的阅读摘录功能。**

添加在线PDF并摘录

发布于 2021-08-07 18:47，编辑于 2021-09-16 22:02

[查看原网页: zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/397352305)