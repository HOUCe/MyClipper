# Ulysses 与 Zotero 的梦幻工作流，轻松搞定参考文献 - 少数派

[sspai.com](https://sspai.com/post/68678)Ulyssesapp

### **专栏文章 首页推荐**

少数派专栏是 Matrix 社区中的一部分，我们会不定期挑选专栏中最优质的文章，展示来自用户和开发者的真实体验和观点。[点此](https://sspai.com/post/65959) 了解什么是少数派专栏，[点击](https://sspai.com/columns) 查看全部少数派专栏。

本文来自 [Ulysses](https://sspai.com/column/256) 专栏，Ulysses 拥有面向 Mac、iPhone 和 iPad 的一站式写作环境，优雅、专注、高效、灵活。关注专栏，随时掌握最新资讯。

文章代表作者个人观点，少数派仅对标题和排版略作修改。

* * *

在过去的几个月，我一直攻读我的博士学位。为此我建立了一个工作流程，以便我的 Markdown 文档可以快速地转变成 Word 文档，就像魔术一样。或许我也可以选择发邮件给我的导师，想他解释 Markdown 是一种简单高效的标记语言。但是，当你尝试过后就会发现，他没有时间来听我废话。 因此，我创立了一个工作流程，可以将你的所有引文自动转换成相应的参考文献 （无论你使用的是什么论文格式，Chicago、 MLA、 APA、MHRA……） 以及所有适当的 ibids. 、页码等等。

为此，本指南既是为了我自己，也是为了予人方便--列出了为此所需的一切准备工作。只需要几下点击就能瞬间将 Markdown 变成 Word 文档。我们的确生活在未来！

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2021%2F09%2F09%2F3c32c71bae3cebfedca11eae0d3a6aca.png%3FimageView2%2F2%2Fw%2F1120%2Fq%2F40%2Finterlace%2F1%2Fignore-error%2F1)

首先，我的论文是通过 Ulysses 完成的。Ulysses 是一款优美的 Markdown 文字处理器。它能在拥有像Scrivener 这样强大的写作工具的功能同时，提供给用户沉浸式写作体验，使用户专注于写作。我很喜欢 Ulysses，它很好。我只在它的默认设置上进行了小的微调，以及一个为支持 Zotero 使用的小改动，这一改动我将在下一章节介绍。

## 参考文献工具

当涉及到学术参考资料，我使用了一个相当复杂的系统。但是它可以放在后台，并不会打扰正常工作，此外这套系统的安装也非常简单，当我重装 macOS 时，我只花了 15 分钟就把它重新安装好了。我把这个系统命名为 Zotero/BetterBibTeX/zotpick-applescript/Service，很抱歉这个名字不能随我的想象力发挥，因为这就是这个系统的组成：

### 文献管理工具 - Zotero

如果你不使用 Zotero 而是其他文献管理工具，那你就太倒霉了。虽然就我所知 Papers 已经内置了类似 （而且略微漂亮一点的）系统，此外 Ulysses 也有专门的教程说明 [如何在 Ulysses 里使用 EndNote 和 Bookends](https://ulysses.app/answers/reference-managers)。但是，我还是选择了 Zotero，因为我觉得它的使用方式美丽优雅，界面丑陋却又迷人，对我而言 Zotero 就是一款完美文献管理工具。我曾尝试过几乎所有的文献管理工具，但 Zotero 依旧是我的首选。 如果有人想要一份关于 Zotero 的指南，可以给我发邮件 （raphaelkabo@hey.com），我会履行我的承诺。

### 安装插件 - Zotero Better BibTeX

这款插件可以为所有的文献添加 citation key，这款插件也因此得名。[下载链接](https://github.com/retorquere/zotero-better-bibtex)，在此还可以找到有关此插件的 [具体介绍](https://github.com/retorquere/zotero-better-bibtex/wiki/Installation)。

插件的安装流程如下：

*   打开 Zotero， 选择 `工具 > 插件`。
*   点击右上方的 齿轮图标，选择 从文件中添加插件，并选择刚刚下载的文件。
*   此外，还需要改变一下默认的 citation key 格式：`首选项 > Better BibTeX`， 选择 Citation keys， 将此改为：

![](https://image.cubox.pro/article/2021102823002227241/93409.jpg)

*   更改后，所有的文献都将以类似的当时显示：

![](https://image.cubox.pro/article/2021102823002282016/14044.jpg)

每个 citation key 都对应着独一无二的文献，例如：Judith Butler 2009 年的战争框架 （Frames of War）这本书的 citation key 是 **butler2009**，当我再添加 Butler 2009 的 Performativity, Precarity, and Sexual Politics，它的 citation key 将会是 **butler2009a** 以便其与上一本进行区分。当然，你也可以按照你的想法改动 citation key 。

为了保证所有的 citation key 可以自动更新，需要创建一个含有所有 citation key 自动更新的文件： `文件 > 导出文献库`， 选择格式为 Better BibTex，下方设置选择 keep update，其余选项都无需勾选，保存位置可自由选择，我将其保存在了我的 Dropbox 里。

在创建完这个文件后，还需要回到 Zotero， 选择 `首选项 > Better BibTex`，查看 「Automatic export」 可以看到 现在的选项为 「On Change」。 也就是说现在一旦你在 Zotero 里有任何的改动，文献库俊江自动更新。

Zotero 的设置部分就完成了。

### 设置「自动操作」

首先需要下载 [zotpick-applescript](https://github.com/davepwsmith/zotpick-applescript)，它是一段神奇的代码，它可以帮助你在 Ulysses 的编辑器中使用你刚刚设置的 citation key。你只需要其中打开名为「zotpick-pandoc.applescript」的文件并复制里面的内容。

然后打开 `自动操作（Automator）`，这是 Mac 默认自带的软件。点击 `新建文稿`， 选择`快速操作`。然后在右边的顶部选择工作流程收到「没有输入」位于「任何应用程序」， 然后将「运行 AppleScript」拖拽至右边的主窗口，用之前复制的代码代替原来的文本，复制保存即可。

![](https://image.cubox.pro/article/2021102823002287249/43370.jpg)

![](https://image.cubox.pro/article/2021102823002252310/60231.jpg)

设定好「快速操作」后，还需要为其设置快捷键：进入「系统偏好设置…」，选择 `键盘 > 服务`（侧边栏）， 找到之前保存的文件并为其添加快捷键即可，例如这里我将文件命名为 「Zotpick」，快捷键设置的是 `⌥ ⌘-`。

![](https://image.cubox.pro/article/2021102823002211852/39654.jpg)

### 插入参考文献

当你需要添加参考文献时，首先输入两个方括号，然后将光标放在它们中间，然后按下你刚刚设置的快捷键，搜索你需要的文献，选择它后直接输入数字即可添加页码。完成后，直接按下回车键，完整的 citation key 就会自动添加到你的编辑器中了。

![](https://image.cubox.pro/article/2021102823002280220/44989.jpg)

-
但是因为默认情况下，方括号会被自动识别为链接。所以，我们需要创建新的标记，以便方括号在 Ulysses 内不代表任何标记语言：`偏好设置… > 新建标记…` ，在新标记里你可以将链接的标记更改为其他符号，使方括号不属于标记语言。创建好新的标记后，选择 `编辑 > 转换标记 > 选择新创建的标记`。

![](https://image.cubox.pro/article/2021102823002292596/60561.jpg)

## 导出

在导出前我们需要再确认一下，你是否有：

*   一个包含所有 citation key 的可自动更新的 Zotero BibTex。
*   文稿中的所有引用是否是以方括号的形式被引用 （无论是内联参考文献还是脚注），我使用的是脚注。 将 citation key 放在句号前，剩下的就交给导出引擎吧。

### Pandoc

[Pandoc](http://pandoc.org/) 是一个文本转换工具，它几乎可以将所有常用的文件格式转换成任何其他常用格式。唯一的缺点是： 它是一个命令行工具，需要通过「终端」使用。而我们现在要将这个命令行工具变成一个简单的软件：

开始前， 需要从 [网页](https://github.com/jgm/pandoc/releases/tag/2.1.1) 上下载这个名为「pandoc-2.1.1-macOS.pkg」的文件，并安装这个程序包。或者，若你能熟练使用 Homebrew，通过 Homebrew 安装 Pandoc:brew install pandoc。若安装被系统拦截了，可以点击 `系统偏好设置… > 安全性与隐私 > 通用`， 同意打开这个文件。

测试 Pandoc 是否安装成功了。打开「终端」，输入 pandoc-v 。 如果你看到一些有关 Pandoc 版本的信息，代表你已经成功安装 Pandoc 了，然后就可以关闭 Pandoc 了。

现在你需要设置两件事儿，以便 Pandoc 可以将你的文稿导出为优雅的 Word 文档。 首先就是你需要选择适合你的 [CSL (Citation Style Language](https://citationstyles.org/)) 文件。在这里你几乎可以找到你需要的所有参考文献模式，文件包含了几乎所有自动转换成引用的所有必要规则。在 [CSL 资源库](https://github.com/citation-style-language/styles) 找到你喜欢的文献格式 （我是用的是「芝加哥模式」，没有 ibid. ）下载并保存 （我在 Dropbox 的博士论文文件夹中创建了一个名为「Scripting」的文件夹）。

其次你需要创建一个带有预定样式模版的 Word 参考文档，以便 Pandoc 知道要输出怎样的文件。这个过程有点复杂，你可以在 [Pandoc 手册](http://pandoc.org/MANUAL.html) 上搜索「reference-doc」。或者，你也可以 [下载我做的文件](https://raphaelkabo.com/assets/reference.docx)，它也不错。你也可以通过手册调整查看它，并把它保存在合适的地方。

现在再次打开你的「自动操作」，而这次需要选择「应用程序」。

![](https://image.cubox.pro/article/2021102823002237537/12807.jpg)

在右侧选择搜索「运行 Shell 脚本」并拖拽至右边的主窗口。选择 Shell 是 '/bin/bash‘，且你的传递输入是「作为自变量」。 然后将以下内容复制到文本框：

    title=$(basename "$@" .md)
    echo -e "\n\n# Bibliography" >> "$@"
    /usr/local/bin/pandoc -s --filter=/usr/local/bin/pandoc-citeproc --bibliography /PATH/TO/BIBTEX/LIBRARY/Zotero.bib --csl /PATH/TO/CSL/FILE/FILENAME.csl --reference-doc /PATH/TO/REFERENCE/DOC/reference.docx -f markdown+smart -t docx -o /PATH/WHERE/YOU/WANT/TO/EXPORT/"$title".docx "$@"
    open /PATH/WHERE/YOU/WANT/TO/EXPORT/"$title".docx

第一行可以使你导出的文件名和导入的文件名一样。第二行将标题「Bibliography」放在文件末尾，这样 Pandoc 自动创建的「Bibliography」就会有一个标题。如果你不需要，可以删除这一行。第三行是 Pandoc 命令。你需要编辑这段命令中用大写字母标记的路径，以便你导出的 BibTeX Zotero 文库， CSL 文件，参考文献文档以及你转换后的文件的最终位置都正确。如果你不确定文件的具体路径是什么，可以在 Finder 窗口选取你想要保存的文件夹，并拖拽这个文件夹至文本窗口，文本窗口就会出现这个文件夹的保存路径。很棒，不是吗？ 如果你的路径中有空格， 可以将它放在双引号里，像这样：「/Users/bobross/Happy Squirrels/reference.docx」。

然后我把「显示通知」模块添加到了主自动操作窗口的下方，这样我就可以知道转换何时结束。在运行时，所有的操作几乎在一瞬间就完成了。以下是我的设置：

![](https://image.cubox.pro/article/2021102823002261580/77032.jpg)

将你的自动操作程序保存在你的「应用程序」文件夹中，我将其命名为 DocDown，这样就完成了从命令程序到简单程序的步骤。

## 运行

现在终于到了最酷的部分了。 你可以简单地将任意扩展名为 .md 的文本拖到 DocDown 应用程序，转换工作就开始了。而在 Ulysses 上，你甚至不需要离开编辑器就可以完成这个神奇的操作：

*   选择导出 > 文本 > Markdown；
*   点击「A」图标，选择「其他…」；
*   点击选择 DocDown。

![](https://image.cubox.pro/article/2021102823002261820/72127.jpg)

当前的 Ulysses 文稿会被立即转化并导出至你之前设置好的位置的 Word 文档，参考文献是通过 Zotero 实现的，CSL 文件完成格式编辑部分。

>  You are academia god. We all bow down.

* * *

![](https://image.cubox.pro/article/2021102823002278925/45435.jpg)

译者：超凡

[查看原网页: sspai.com](https://sspai.com/post/68678)