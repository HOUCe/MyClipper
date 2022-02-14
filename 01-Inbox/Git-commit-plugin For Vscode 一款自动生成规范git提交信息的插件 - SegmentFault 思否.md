# [Git-commit-plugin For Vscode 一款自动生成规范git提交信息的插件 - SegmentFault 思否](https://segmentfault.com/a/1190000021875143)

## 初衷

在公司由于大家随意提交 **git-commit** 的信息，导致提交的风格千奇百怪，写的信息不明确，不知道这次提交具体是修复 **bug** 呢？还是增加新功能，还是单纯改了一些配置文件，亦或是重构了一些不好的代码。只能靠大家自己去猜测，就算是自己提交的信息，也可能因为时间长导致自己也不清楚具体这次提交是为了干啥，只能去提交记录里翻代码，长此以往，不利于产品的迭代，以及对于 **bug** 的定位。

## 为什么写这个插件

基于这个原因，我们开始寻找比较符合规范的提交格式，**Angular** 团队的 [Angular Team Commit Specification](https://link.segmentfault.com/?enc=0YIfJid42FJsxn3xr5YFzw%3D%3D.stKl1kxGtkRJVrnP4puWl9ziKgq3i0Ti1AJmEwKqdNNKOxFoLdllZBMXtUojFCJFECOfYlL%2BtR4JXGvF0PSBVE9%2FBYTBxPpg%2FjjV6H4u4J7Yv4kdGC09D612rbsa4yY4) 进入了我们的视野，格式如下：

<type\>(<scope\>): <subject\>
<BLANK LINE\>
<body\>
<BLANK LINE\>
<footer\>

___

清晰的信息展现，让我们觉得这个就是我们正在寻找的！为此我们开始搜**IDE** 有没有对应的插件可以使用，幸运的是后端 **Java** 团队使用的 **IDEA** 直接就有现成的插件可以使用，苦逼的我们前端团队都是统一用的 **Vscode** ，看到了几款插件，但是都不符合我们的要求，为了前端团队不拖后腿，于是乎就想着自己写一款符合要求的插件来供团队使用。

## 如何使用

1.  首先我们需要去 **Vscode** 插件市场搜索 `git-commit-plugin` 并且进行安装。![安装](https://segmentfault.com/img/bVbDWPr "安装")
2.  安装完之后可以使用组合键 `Command + Shift + P` 呼出 **指令行**，并键入指令 `show git commit template` 或者点击 **git** 插件栏上的小图标唤醒插件界面。![open.gif](https://segmentfault.com/img/bVbDWPK "open.gif")
3.  根据自己当前提交所要表达的意义，选择对应的 **type** 类型去编写 **commit** 信息![edit.gif](https://segmentfault.com/img/bVbDWP0 "edit.gif")

## 结语

写插件的时候也踩了不少坑，官网文档为了找个 **API** 也是看这看那的，不过最终解决了问题也是值得的。如果觉得本项目对你有帮助的，别吝啬你手里的✨给 [本项目](https://link.segmentfault.com/?enc=nrHKJm8w%2FNzgFwtSFULKkg%3D%3D.kfXL%2FefcoVhgT1wvTesjFpirOLXdEhNaJWDiVR3oqh9UwYYIrGgIJRoyqXk9YW6T) 点个 **star**✨，您的鼓励就是对作者最大的支持！发现 **bug** 或者有啥希望改进的点，也欢迎在项目底下提 [issue](https://link.segmentfault.com/?enc=URCRdSiMmDoPeZvgaihegQ%3D%3D.DmBSTwGgBM5%2Bhxx6QkYnnUCYkjjFYdp30j2kX%2BA3vWPWpgjj0mSOFJQoViGt4WtV3YSptv6HU90A4MsmGr91vA%3D%3D) 😘。
