# [vscode 体验记 - SegmentFault 思否](https://segmentfault.com/a/1190000014935298)

之前开发一直用 **jetbrains** 家的产品，产品是不错，也的确不错。可日复一日，那启动速度实在有点...那啥，不过这也可以理解，毕竟软件做的那么复杂那么强大嘛。于是就开始着手寻找替代品，之前也配置过 **sublime**，也写过[文章](https://link.segmentfault.com/?enc=IXDdsr0ArjNlVk0hoaYpmg%3D%3D.jI2LvQ1NAhWrvQl0dvlg1dBH0AqsI7zljKb2wcu8Y7OW090IZcAC17dTVu4s%2BjU1AEy%2BLONzqnikUPcLcqQjJtHz%2FgeWSZfNZemO5NQl0Z9TFu%2BpKREi0BdCE%2FvG0ra8)，可不知道为啥总也喜欢不起来。  
后来，毫不费力地找到了 **vscode** 这款神奇的软件，作为一个喜欢倒腾各种（手机/电脑）软件的人，好软件当然要搞一搞。  
下面介绍下这款每天使用时间最长的软件 vscode。

> VSCode是微软推出的一款轻量编辑器，采取了和VS相同的UI界面。

官方网站上是这么说的：

> Visual Studio Code is a lightweight but powerful source code editor which runs on your desktop and is available for Windows, macOS and Linux. It comes with built-in support for JavaScript, TypeScript and Node.js and has a rich ecosystem of extensions for other languages (such as C++, C#, Java, Python, PHP, Go) and runtimes (such as .NET and Unity).

使用这个软件的一个重要原因是轻量级，类似于 sublime，插件丰富，关键它是**免费的**！  
**微软出品, 品质保证!** 嗯，事实证明，你大爷永远是你大爷！  
呐，使用过一段时间下来呢，发现的确很不错，这里对使用过程中的一些配置以及我日常用到的插件做一下梳理(有点多)。让你知道，我用过之后是这个样子，你用过也是这个样子，**Duang~Duang~** 奸笑脸...

## 安装

在[官网](https://link.segmentfault.com/?enc=3qCN3GSjE9UqJXyiKS3f5g%3D%3D.1FJm%2B3zokgqwtkyJOOOdmchA1iQCibMADAx9RFl04hw%3D)下载这个软件，可以选择相应的平台版本进行下载，对，它是跨平台支持的！  
官方文档可以看[这里](https://link.segmentfault.com/?enc=FFX%2FKNy2isM3%2FlwjoKhLAg%3D%3D.UrtomdrfgfbalCoCaPkzFfIUdOFJZitrtNk5EpNZfYWWT52Heqh6HKUvacYwUfd3),可以解决日常遇到的大部分问题。  
它的外观大概是这个样子，hmm...蛮不错的吧？  
![vscode 外观](https://segmentfault.com/img/remote/1460000014935301 "vscode 外观")

## 常用插件

这里只罗列一些我常用到的插件，因为不是做前端工作，虽然前端方面也有一些很优秀的插件，但是因为我暂时用不到，因此没有做介绍。  
**注意:** 以下插件都可以在 vscode 自带的扩展商店中找到。

### Material Icon Theme

这个是我最喜欢的一款图标主题，切换到这个主题后，图标大概是这样的  
![Material Icon Theme](https://segmentfault.com/img/remote/1460000014935302 "Material Icon Theme")  
文件夹颜色、关联图标也是可以根据参数调整，这里看[插件文档](https://link.segmentfault.com/?enc=pk9j2Q39ePgjR6CwWpJE%2Fw%3D%3D.3PvKByeRTwDBt7q%2FbJjlB9vQTItZMh3%2Birh3yGt3ZRSS68wetTI5%2B%2FPq%2BWXzT1ZaQ8a4tfKSZkvBQVz9Ctgb1YTFL2J4EBAyAeTbtILShwk%3D)，不过我觉得默认配置就行了，别浪费时间折腾了。

### One Dark Pro

配置完了图标主题以后，再来个代码颜色主题（对某些人来说，这很重要），我墙裂推荐的就是这款 **One Dark Pro** 啦，Atom 的这款颜色主题真是漂亮，从几百万的下载量来看，我的审美还是符合大众审美的..emm... 大概就是上图这个样子（上图是 cpp 渲染效果），总之我是很满意的。

___

OK！折腾完门脸儿，下面切入正题！

### Go

作为一个 golang 程序员怎么能少的了 golang 支持插件，隆重推出这款微软官方出的插件。  
插件文档可以看这个 [Go for Visual Studio Code](https://link.segmentfault.com/?enc=KHNw%2F9rtltWf%2Bwz1708u%2Bw%3D%3D.62N4FzJC%2BboFCjHZdKu4Pe4Dq1g%2FF%2FsYqLsYh1JuMtfvdbv5oQmY8sGtRGs3DStbUfFLXvDcdMXjXN%2B6Xq0WnwrrA31blcGztGOeUUS0AcU%3D)。  
该插件依赖一些 go 工具来完成跳转和格式化等，第一次使用需要手动安装（**可能需要番·羽·土·啬**）。

go get -u -v github.com/nsf/gocode
go get -u -v github.com/rogpeppe/godef
go get -u -v github.com/golang/lint/golint
go get -u -v github.com/lukehoban/go-outline
go get -u -v sourcegraph.com/sqs/goreturns
go get -u -v golang.org/x/tools/cmd/gorename
go get -u -v github.com/tpng/gopkgs
go get -u -v github.com/newhook/go-symbols
go get -u -v golang.org/x/tools/cmd/guru

配置的话，默认配置基本就可以，**前提是没装已经在系统中配置好了 go**，在配置页面搜索 `go`，查看所有相关的配置项。

### C/C++

微软出品的 c/c++ 插件，支持函数跳转、查看声明、语法检测和格式化等各种功能，体验不错。

### Python

微软出品的 python 支持插件，上千万的下载量！插件文档可以看看[这里](https://link.segmentfault.com/?enc=lRV6opIOt2UInn5RLNshFA%3D%3D.H9GigMY0ys%2Fm4Oq5CsiCrhfE5aiY%2BQack7wYSnbXs75IFTMf%2BtaLkyuTJzGD1m7mCyPlB2eqxFfXucDt8JeouFNreJEIz468uRnUvMiGdWM%3D)。

___

### Git History

这个插件做了件什么事儿呢？就是优化默认集成的 git log和 diff 等，将更多的内容可视化，更加直观，操作体验不输于 **github**，提供一个截图看一下  
![gitlog](https://segmentfault.com/img/remote/1460000014935303 "gitlog")  
用起来很方便！！一个不容错过的 vscode 插件。更多信息请查看[插件文档](https://link.segmentfault.com/?enc=bg%2FBzB9ediwrriZXX12o8g%3D%3D.2ux%2BWJ2Op%2FPwp9u6wtsS%2BChggfG07zVq3n94kDYdRxpEBoBhqKTo8cMMhfRsD1x4l9eP9VBqdofq5LEFdrpNbun%2F2C9EEkr8YQhCLiGdBlA%3D)。

### GitLens — Git supercharged

官网是这样介绍的，感受一下。

> GitLens supercharges the Git capabilities built into Visual Studio Code. It helps you to visualize code authorship at a glance via Git blame annotations and code lens, seamlessly navigate and explore Git repositories, gain valuable insights via powerful comparison commands, and so much more.

装上以后，是这个样子，每一行 code 的作者、提交时间、commit log 等信息，一目了然。  
![git len](https://segmentfault.com/img/remote/1460000014935304?w=727&h=600 "git len")

___

### Code Outline

这个插件做的是会把代码中的函数、变量、结构体定义等都提出来显示，（vscode 控制面板下输入 **@** 符号也可以实现），这个插件效果大概是这样：  
![code-outline](https://segmentfault.com/img/remote/1460000014935305 "code-outline")  
主流语言都支持了！

### Bracket Pair Colorizer

上个图就知道它是干嘛的了。  
![Bracket Pair Colorizer](https://segmentfault.com/img/remote/1460000014935306 "Bracket Pair Colorizer")  
括弧匹配，很直观，再也不怕漏掉了。  
还有些颜色配置的，可以看下[插件文档](https://link.segmentfault.com/?enc=vTLzmEuSZYXMk3X2G5zq0A%3D%3D.5yRyPt%2F%2BSFLvXfu%2Frx%2FgpnP0j6vKIqMpLDzadM0O0C00rSIAba5MjiWDWAMgwgiXdVqss2hnPRAK5J8ee%2BsxAyDOCdjugP7pMTagO0M63lGEmQd%2FScXMCH4Xbv2EpYQ9)，看兴趣自己配置下吧，默认就可以。

### _Guides_

这个插件是搞缩进线的..emm...自我感觉也不错。  
![Guides](https://segmentfault.com/img/remote/1460000014935307 "Guides")

### TODO Highlight

这种插件是少不了的,不同的颜色区分 **TODO** 和 **FIXME**，还可以在控制面板列出各项 **TODO**，多的不说，可以看下[插件文档](https://link.segmentfault.com/?enc=sYptPe1q0kFfRfkhmNVTyA%3D%3D.wLJJgXik1yqVKOc%2FG5L0B5Q6aVNsxqitAm54uGFGEy9XMSkJSdHzyhnvyiyZjSSyB2C5bnO0dBInJe1BCK2sykM3rP%2FuVXpgr%2BFvuIC2xq8%3D)。

### change-case

这个插件做的事情，看个图就明白了。  
![change_case](https://segmentfault.com/img/remote/1460000014935308 "change_case")

___

### _Code Runner_

这个插件可以跑简单的脚本，像这样  
![code_runner](https://segmentfault.com/img/remote/1460000014935309 "code_runner")  
很方便，对不对？还有一些各种各样的配置，可以在[插件文档](https://link.segmentfault.com/?enc=m6NaXPkGiZ8Z4oEGQ5DQZA%3D%3D.mTgj7bKfwGh9wveClFhpcjIiBDxDtVDjAJiTLOY9bw6KFtivn1SDfu9RDDJjcajzwjVs8SZYl96hIat5wsufLS2Y5Njf%2BymUNZrjBMqoo0U%3D)查看。

### _Settings Sync_

最后一个插件！  
你可能会想，我配置了这么多插件，换了电脑再来一遍的话，岂不是浪费时间？...emm...很有道理，那么隆重推出这款插件 **Settings Sync**，它可以同步插件配置，只需要配置一次，以后就不用再麻烦了。配置这个插件的话跟着[插件文档](https://link.segmentfault.com/?enc=I6NKzDPPlzdRk96eZJiAMg%3D%3D.ol%2Bqg5FCH0%2FhRpl2K2REkTdR2qUJmhn%2FjTkxm4pUN84FQGiFqfPN1D9BV1kdiwH%2B1pUnkynVSQLkEH8IhVeTfv9QIf%2BL64h9ibB%2BcWfMRUI%3D)走一遍，很容易搞定的。

## 常用配置项

{
    "editor.fontSize": 14,
    "editor.lineHeight": 22,
    "editor.cursorBlinking": "smooth",
    "workbench.colorTheme": "One Dark Pro Vivid",
    "workbench.iconTheme": "material-icon-theme",
    "workbench.editor.enablePreview": false,
    "gitlens.advanced.messages": {
        "suppressShowKeyBindingsNotice": true
    },
    "gitlens.historyExplorer.enabled": false,
    "editor.fontFamily": "Fira Code, Menlo, Monaco, Courier New, monospace",
    "editor.quickSuggestions": {
        "other": true,
        "comments": true,
        "strings": true
    },
    "guides.normal.color.dark": "rgba(91, 91, 91, 0.6)",
    "guides.normal.color.light": "rgba(220, 220, 220, 0.7)",
    "guides.active.color.dark": "rgba(210, 110, 210, 0.6)",
    "guides.active.color.light": "rgba(200, 100, 100, 0.7)",
    "guides.active.style": "dashed",
    "guides.normal.style": "dashed",
    "guides.stack.style": "dashed",
    "guides.enabled": true,
    "files.trimTrailingWhitespace": true,
    "files.trimFinalNewlines": true,
    "\[cpp\]": {
        "editor.autoIndent": true
    },
    "editor.renderIndentGuides": false,
    "python.autoComplete.addBrackets": true,
    "python.pythonPath": "/usr/local/bin/python",
    "python.linting.enabled": true,
    "material-icon-theme.folders.color": "#42a5f5",
    "editor.tabSize": 4,
    "editor.renderWhitespace": "all",
}

以上各项的含义不解释了，vscode 的配置非常人性化，为啥这么说？你试试就知道了....  
还有一个事情是字体推荐 **Fira Code**，你值得拥有！

## 常用快捷键

以下快捷键针对 mac ，且只是部分常用快捷键

### 全局

快捷键

含义

Command + Shift + P / F1

显示命令面板

Command + P

快速打开

Command + Shift + N

打开新窗口

Command + W

关闭窗口

### 基本

快捷键

含义

Command + X 剪切

（未选中文本的情况下，剪切光标所在行）

Command + C 复制

（未选中文本的情况下，复制光标所在行）

Command + I

选中当前行

Option + Up/Down

向上/下移动行

Option + Shift + Up/Down

向上/下复制行

**Command + Shift + K**

删除行

Command + Enter

下一行插入

Command + Shift + Enter

上一行插入

Command + Shift + \\

跳转到匹配的括号

Command + \[/\]

减少/增加缩进

Command + /

添加、移除行注释

### 其他

快捷键

含义

Option + 点击

插入多个光标

Command + F

查找

Command + D

向下选中相同内容

Command + T

显示所有光标所在的符号 【速度有点慢】

Ctrl + G

跳转至某行

Ctrl + -

后退

Ctrl + Shift + -

前进

Command + \\

编辑器分屏

Command + 1

切换到第一分组

Command + Shift + v

Markdown预览窗口

Ctrl + \`

显示终端

cmd+k z

进入 zen 模式，esc 退出

___

更多 vscode 使用技巧，可以查看 [vscode-tips-and-tricks](https://link.segmentfault.com/?enc=LuKivm3A%2BUIYN1S%2BdWjuVw%3D%3D.hctF35%2F7wXbrLLw%2FFKqEhwpPWZdPNWHP3Kzj5Cwpp%2FuBoTJQK%2Bkqu5DbGfDzxDpfpA0%2FtxheKf7wHqxZwK2Q55GMYluflUYs814iXA5rrdo%3D)。
