# Writeathon

[www.writeathon.cn](https://www.writeathon.cn/p/5d5904252599106ca0b777b3)hcs

本文转自[维基百科](https://zh.wikipedia.org/wiki/Markdown)

**Markdown**是一种[轻量级标记语言](https://zh.wikipedia.org/wiki/%E8%BD%BB%E9%87%8F%E7%BA%A7%E6%A0%87%E8%AE%B0%E8%AF%AD%E8%A8%80 "轻量级标记语言")，创始人为[约翰·格鲁伯](https://zh.wikipedia.org/wiki/%E7%B4%84%E7%BF%B0%C2%B7%E6%A0%BC%E9%AD%AF%E4%BC%AF "约翰·格鲁伯")（英语：John Gruber）。它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的[XHTML](https://zh.wikipedia.org/wiki/XHTML "XHTML")（或者[HTML](https://zh.wikipedia.org/wiki/HTML "HTML")）文档”。[\[4\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-md-4)这种语言吸收了很多在[电子邮件](https://zh.wikipedia.org/wiki/%E7%94%B5%E5%AD%90%E9%82%AE%E4%BB%B6 "电子邮件")中已有的纯文本标记的特性。

由于Markdown的轻量化、易读易写特性，并且对于图片，图表、数学式都有支持，当前许多网站都广泛使用 Markdown 来撰写帮助文档或是用于[论坛](https://zh.wikipedia.org/wiki/%E7%BD%91%E7%BB%9C%E8%AE%BA%E5%9D%9B "网络论坛")上发表消息。例如：GitHub、reddit、Diaspora、Stack Exchange、OpenStreetMap 、SourceForge等。甚至Markdown能被使用来撰写[电子书](https://zh.wikipedia.org/wiki/%E9%9B%BB%E5%AD%90%E6%9B%B8 "电子书")。

## 历史

John Gruber 在 2004 年创造了 Markdown 语言，在语法上有很大一部分是跟[亚伦·斯沃茨](https://zh.wikipedia.org/wiki/%E4%BA%9A%E4%BC%A6%C2%B7%E6%96%AF%E6%B2%83%E8%8C%A8 "亚伦·斯沃茨")（Aaron Swartz）共同合作的。这个语言的目的是希望大家使用“易于阅读、易于撰写的纯文字格式，并选择性的转换成有效的XHTML（或是HTML）”。 其中最重要的设计是可读性，也就是说这个语言应该要能直接在字面上的被阅读，而不用被一些格式化指令标记（像是RTF与HTML）。 因此，它是现行电子邮件标记格式的惯例，虽然它也借鉴了很多早期的标记语言，如：Setext、Texile、[reStructuredText](https://zh.wikipedia.org/wiki/ReStructuredText "ReStructuredText")。Gruber也编写了的[Perl](https://zh.wikipedia.org/wiki/Perl "Perl")脚本：_Markdown.pl_，用于把markdown语法编写的内容转换成有效的、结构良好的XHTML或HTML内容，并将左尖括号`<`和[`&`](https://zh.wikipedia.org/wiki/Ampersand "Ampersand")号替换成它们各自的[字符实体引用](https://zh.wikipedia.org/wiki/%E5%AD%97%E7%AC%A6%E5%AE%9E%E4%BD%93%E5%BC%95%E7%94%A8 "字符实体引用")。它可以用作单独的脚本，[Blosxom](https://zh.wikipedia.org/wiki/Blosxom "Blosxom")和[Movable Type](https://zh.wikipedia.org/wiki/Movable_Type "Movable Type")的插件又或者BBEdit的文本过滤器[\[4\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-md-4)。Markdown也已经被其他人用Perl和别的编程语言重新实现，其中一个Perl[模块](https://zh.wikipedia.org/wiki/%E6%A8%A1%E5%9D%97_(%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1) "模块 (程序设计)")放在了[CPAN](https://zh.wikipedia.org/wiki/CPAN "CPAN")(Text::Markdown)上。它基于一个[BSD风格的许可证](https://zh.wikipedia.org/wiki/BSD%E8%AE%B8%E5%8F%AF%E8%AF%81#BSD%E9%A3%8E%E6%A0%BC%E7%9A%84%E8%AE%B8%E5%8F%AF%E8%AF%81 "BSD许可证")分发并可以作为几个[内容管理系统](https://zh.wikipedia.org/wiki/%E5%86%85%E5%AE%B9%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F "内容管理系统")的插件[\[7\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-7)[\[8\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-8)。

### 标准化

Markdown已经成为典型的转换为HTML的非正式规范[\[9\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-9)和参考实现。随着时间的推移，出现了许多Markdown实现。人们开发这些主要是由于在基本语法之上需要额外的功能 - 例如表格，脚注，定义列表（技术上的HTML描述列表）和HTML块内的Markdown。其中一些行为偏离了最开始的参考实现。与此同时，非正式规范中的一些含糊不清引起了人们的注意[\[10\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-10)。这些问题促使Markdown解析器的一些开发人员努力实现标准化。

Babelmark[\[11\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-babelmark-2-11)[\[12\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-babelmark-3-12)是一个可用于“\[比较\]各种实现的输出”的工具，以“促进关于如何以及是否应该阐明markdown规范的某些模糊方面的讨论。”[\[13\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-13)Gruber认为不应完全标准化：“不同的网站（和人们）有不同的需求。没有一种语法可以让所有人满意。“[\[14\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-14)

2016年3月发布了RFC 7763和RFC 7764。RFC 7763 从原始变体引入了MIME类型 `text/markdown`。RFC 7764讨论并注册了 MultiMarkdown, GitHub Flavored Markdown (GFM), [Pandoc](https://zh.wikipedia.org/wiki/Pandoc "Pandoc"), [CommonMark](https://zh.wikipedia.org/wiki/Markdown#CommonMark), and Markdown 等markdown变体.[\[15\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-IANA-15)

### CommonMark

从2012年开始，包括Jeff Atwood和John MacFarlane在内的一群人启动了Atwood的标准化工作。[\[18\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-FutureOfMarkdown-18) 一个社区网站现在旨在“记录可用于文档作者和开发人员的各种工具和资源，以及各种markdown实现的实现者 “。[\[19\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-19) 2014年9月，Gruber反对在这一工作中继续使用”Markdown“这个名字，其被更名为CommonMark。[\[20\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-20)[\[21\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-21) CommonMark.org发布了规范、参考实现和测试包的几个版本，以及 “\[计划\]在2018年宣布最终的1.0规范和测试包。”[\[22\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-commonmark.org-22)

### GFM

2017年，GitHub发布了基于CommonMark的GitHub Flavored Markdown（GFM）的正式规范。[\[23\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-23)除了表格、删除线、自动链接和任务列表被GitHub规范作为扩展添加之外，它遵循CommonMark规范。 [\[24\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-24)GitHub还相应地更改了其站点上使用的解析器，这要求更改某些文档 - 例如，GFM现在要求创建标题的哈希符号由空格字符分隔。

### Markdown Extra

Markdown Extra是一种轻量级标记语言，基于在PHP（最初）、Python和Ruby[\[25\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-fortin-2018-25)中实现的Markdown。它添加了普通Markdown语法不具备的功能。内容管理系统支持Markdown Extra，例如Drupal[\[26\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-26)，TYPO3[\[27\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-27)和MediaWiki[\[28\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-28)。

它为Markdown添加了以下功能：

*   HTML块内的markdown标记
    
*   具有id / class属性的元素
    
*   围栏代码块
    
*   表格[\[29\]](https://zh.wikipedia.org/wiki/Markdown#cite_note-29)
    
*   定义清单
    
*   脚注
    
*   缩写
    

### 示例

样式

语法

样式

语法

标题

\# h1,## h2

有序列表

1\. Item2. Item

加粗

**bold text**(快捷键:Ctrl-B)

无序列表

\* Item

斜体

_italic_ 或者 _italic_(快捷键:Ctrl-I)

链接

[Text](https://www.writeathon.cn/p/url)

删除线

strikethrough

引用

\> blockquotes

图片

!\[Alt Text\] (url)(插入图片:Ctrl-Alt-I)

任务列表

\- \[x\]

表格

Header1

Header2---

\---Column1

[查看原网页: www.writeathon.cn](https://www.writeathon.cn/p/5d5904252599106ca0b777b3)