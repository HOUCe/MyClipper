# [Alfred打磨之路. 入手Alfred已经有几个年头，真心觉得实乃利器一枚，这里梳理总结下目前的常用点… | by Alan He | Medium](https://alanhg.medium.com/alfred%E6%89%93%E7%A3%A8%E4%B9%8B%E8%B7%AF-561f8f9df81)

[![Alan He](https://miro.medium.com/fit/c/56/56/0*rL51jHRTi3aDDXae.)](https://alanhg.medium.com/?source=post_page-----561f8f9df81-----------------------------------)

> _入手Alfred已经有几个年头，真心觉得实乃利器一枚，这里梳理总结下目前的常用点，方便自己回顾，兴许也可帮助部分同道中人。_

目前Alfred已经是我Mac电脑上检索，查询等的第一入口了。登录电脑之后进行的一切操作，都要从这一个输入框开始。

![](https://miro.medium.com/max/60/0*F4tbLY6hdymjgbrB?q=20)

废话不多说，开搞！

Mac自带Spotlight其实也蛮不错的，但是奈何Alfred更为强大，所谓一山不容二虎，没办法，干掉.

-   设定->键盘，Spotlight快捷键`取消选择`
-   设定->Dock,选择`自动隐藏和展示Dock`
-   Alfred-快捷键设定，推荐 `double ⌘`,选择登录启动

设定后，你会发现桌面会很干净，双击⌘，将会弹出Alfred输入框，一切将从这里开始，即使是各个APP的启动，也从这里可以方便执行。

对于常用的网站检索服务，可以将其配置为搜索，比如我这里经常进行 `京东，和NPM` ，直接配置下，以后只需要唤起Alfred, 输入关键词，回车即可直接进入到相关网页的检索结果页。

![](https://miro.medium.com/max/60/0*ObbHCThyYmIJp5C4?q=20)

![](https://miro.medium.com/max/60/0*D7GVrpNzp-DixOTl?q=20)

比如我经常需要寻找文件，翻看finder各种去找实在太不方便了，这时你需要这么做，唤起Alfred，输入 ‘ 或者空格，然后输入文件部分名称，回车即可打开。或者 ⌘ ⏎，在finder中打开

![](https://miro.medium.com/max/60/0*Hn015GPnm3alzDTT?q=20)

1.  支持文件及文件夹，文件夹默认⏎即打开
2.  ‘ 为英文字符，中文不work，so`采用空格`比较方便
3.  按住⌥回车，则会在finder中打开选中的项
4.  检索APP时，假如想`浏览最近打开的文件列表`，只需要在选中特定APP时，按➡️，选择recent documents，再➡️即可

这个功能之前太不重视了，现在用的不亦乐乎。本质就是同步了浏览器书签记录，以前我们打开一个网页需要， `浏览器-》书签中的找到收藏网页-》回车浏览` ，现在呢，当你输入的时候，直接匹配书签网址，选择目标网址，回车即可。

比如我在Chrome下添加收藏了redux网址 `https://github.com/camsong/redux-in-chinese` ，平时想打开时，只需要这样

![](https://miro.medium.com/max/60/0*3avqqmiEj5MtrOju?q=20)

![](https://miro.medium.com/max/60/0*0MJtXyFk5syfx5Fz?q=20)

有时需要复制微信中的消息-用户名，密码，切换到网页进行使用，来回复制粘贴，切换窗口效率很低。这个功能就可以临时保存了多条剪贴板信息，选择性粘贴，效率瞬时提高多倍。

![](https://miro.medium.com/max/60/0*gOsMV66MIQFwMIZT?q=20)

日常计算，我们即可直接在输入框中进行计算，不用再单独打开计算器。

![](https://miro.medium.com/max/60/0*-1rFgnPA-o1YOJJ7?q=20)

联系人功能选中，我们则可以在输入框中直接检索到常用联系人，快速复制电话，邮箱等

![](https://miro.medium.com/max/60/0*17G10f6h-tfOvjkJ?q=20)

这个功能话说，也是神器啊。就是特定文本关键词的输入，触发文本拓展。比如我平时经常很多地方，比如家庭地址，烦不胜烦，利用Snippet就好解决了

![](https://miro.medium.com/max/60/0*JyUUcdHJsUB6Bob1?q=20)

![](https://miro.medium.com/max/60/0*vQwS4aUIeQneO74q?q=20)

> _注意_
> 
> _前缀很重要，这样可以避免无谓的文本拓展，同时Snippet支持一些变量，比如日期之类的，这些都很实用,比如我每天项目CodeReview做会议记录，需要记录开始时间等参数，就会写个Snippet。_

如果是1password使用用户，可以设定集成下，快速访问1P存储的网页书签，不是该类用户可忽视这部分。

需要在1P的APP中进行下第三方集成设定，否则，Alfred该页面下底部会显示 `Unable to find 1Password Data`

![](https://miro.medium.com/max/60/0*bqFOk3SqmRyXKCXL?q=20)

设置成功，在Alfred的1Password设定页面，选择右下角的高级，勾选自动返现，如果正常显示了1P数据地址，即OK，如果本身是选中状态，就取消再次勾选下。关闭后，会发现页面显示了大量的密码条目，注意安全性没问题，因为1P只是提供了地址给Alfred，密码本身还是在1P下。

![](https://miro.medium.com/max/60/0*Zth3KfvO6rfhq4MI?q=20)

这个也是蛮不错的哟，比如锁屏，重启，弹出硬盘，软件等，都是常用命令。

![](https://miro.medium.com/max/60/0*v4rv6LFFPkr8FK9P?q=20)

之前遇到过，输入lock回车锁屏，不起作用的问题，发现是因为与QQ中的快捷键冲突了，遇到这个，索性直接去掉QQ的快捷键设定。关于这个问题，还有问题，可移步 [这里](https://github.com/alanhg/others-note/issues/1)

Alfred中也可以直接输入终端命令，这个对于程序猿实用些，如果你不是猴子，请跳过这一节吧。

因为我平时使用Iterm2替代了系统终端，所以这里需要设定下。关于设定，请移步 [这里](https://github.com/alanhg/others-note/issues/25)

`注意，集成只是解决了快速浏览器访问1P存储的站点，具体的账户密码输入还需要浏览器本身集成1P来解决。`

![](https://miro.medium.com/max/60/0*WCBNzO5RL8hjQnMh?q=20)

以上的功能还不是最强大的，最强大的就是workflow了。

编程常说的一句话是，不要重复造轮子，这里也适用，既有的轮子，我们拿来用即可。这里贴出我常用的几个轮子

最近，朋友安利的这个，发现太爽了，微信聊天纳入到Alfred，效率提升了些许。

> _在家经常需要登录公司VPN，每次都是重复的操作，为了爽，写了个工作流，每次打开手机，查看Code码，唤起Alfred,输入VPN校验码，回车，即可登录上内网。_

![](https://miro.medium.com/max/60/0*1Kk8Xp3xVYyZXVvb?q=20)

> _平时搞开发，经常有以下操作，复制一段文本，然后创建一个文件，将文本粘贴进去。为了爽，直接做个工作流，输入makefile命令，则自动将系统粘贴板的文本放入一个文件中，然后自动打开文件所在文件夹。爽不爽，开不开心。这样不是很敏捷吗？_

![](https://miro.medium.com/max/60/0*XPr_WomAk8SS3iU7?q=20)

> _平时工作每天CodeReview，需要从好多个服务下down下来新代码，想想都心累，为了方便，索性写个循环脚本，将各个服务的代码都拉下来，这样每天只需要输入gl命令，即可。_
> 
> _程序员免不了多看GitHub仓库的源码，有时还需要实际run下，每次都是复制下仓库地址，打开终端，输入git clone repositoryURL。这个过程很麻烦。如今只需要复制下仓库地址，输入git指令，选择clone workflow即可。_

关于这几个Workflow，可以移步 [这里](https://github.com/alanhg/alfred-workflows)

Alfred输入框，按Shift会预览当前选中的条目结果，比如我们执行 yy 屠龙,因为参数为中文，而输入法切换中英文默认是Shift，所以会造成困扰，解决方法如下

![](https://miro.medium.com/max/60/0*CpbqMXn2-GelkrYx?q=20)

比如终端执行结果会打印111，那么111可以作为参数，继续接下来的指令吗？可以的。比如bash，你需要echo输出，比如JS，你需要console.log输出即可。

> _因为我本人是开发者，平时经常使用IDEA，DataGrip这些开发APP，其中也有剪贴板历史，快捷键默认是_ `_⌘ ⇧ V_` _，习惯性对于Alfred这种全局剪贴板热键也想设置成如此，但假如这样设置后，就会导致IDE中的失效。如何解决呢。_

首先，Alfred中剪贴板历史，目前只有排除特定APP复制内容的设定，没有排除特定APP触发热键的设定，所以只能绕弯子解决。

1.  删除剪贴板原热键
2.  创建热键Workflow，触发快捷键就设定为`⌘ ⇧ V`
3.  相关APP中设定，IDEA，DataGrip不聚焦，热键才触发
4.  创建Key Combo,设定快捷键`⌥ ⌘ ⇧ V`，这里越复杂越好
5.  剪切板热键设定为`⌥ ⌘ ⇧ V`

![](https://miro.medium.com/max/60/0*kH_gDfRnzoYAjNUW?q=20)

这样，当我们在非IDEA，DataGrip聚焦的情况下，按下 `⌘ ⇧ V` ，就可以触发Alfred的剪贴板历史，当在IDEA或者DataGrip中时，就可以继续使用软件本身的剪切板历史。一样的快捷键，不一样的剪切板。完美衔接！

因为Alfred的各种命令都是英文，即使是搜索中文文件名的文件，也是可以拼音检索的，所以默认Alfred是英文较为方便。否则每次唤起Alfred，我们都需要先按⇪切换输入法，这点非常麻烦。

好在Alfred支持缺省语言设定。

![](https://miro.medium.com/max/60/0*zZWzoaqOYMjg-f5l?q=20)

## 注意

-   默认为EN，但我们还是可以通过⇪切换键盘语言
-   如果该选项为空，则会走当前系统键盘语言

双击热键 `只对特定修饰键modifier keys[⌃ ⌥ ⌘ ⇧ ]支持` ，比如双击t是不支持的。

![](https://miro.medium.com/max/60/0*4h8_f90gWYLjoS0z?q=20)

## 为什么要这么设计

因为比如连续输入t这种操作是很常见，假如我们实现双击T唤起某个操作，那么将会与日常操作冲突。

如上，我们辛辛苦苦打磨的工具并不容易，假如系统损坏，或者误操作软件崩溃，配置丢失都是有可能的。为了确保安全，建议做下同步备份。推荐使用 `OneDrive` ，注册即送5G空间，并且大厂服务靠谱些。

进入高级=》同步=》选择同步文件夹即可。

![](https://miro.medium.com/max/60/0*mOuvBc4quRSg9Wwx?q=20)

## 注意

Update界面=》下拉选择 `pre-releases`

![](https://miro.medium.com/max/60/0*bZMpDMzvvpqFxCJy?q=20)

比如想搜索自己的某个workflow，后者某个snippet。输入`?` 即可关键词检索，搜索范围将是Alfred中的所有设定项。

![](https://miro.medium.com/max/60/0*g87AMrEprfU-6Sd0?q=20)

等待遇见

以上的功能点涵盖了Alfred所有，你可以不断摸索Alfred的各种拓展来支持做什么。但这里我来描述下Alfred的局限性，也就是它做不到什么。

1.  Alfred的设计决定了它的触发无非是一个组合热键，一个特定文本snippet表达式，当然还有外部脚本触发。但它不能是一个菜单项，也就是它的入口是固定的。比如我想在finder中实现右键菜单项，实现一键拷贝当前目录路径，可以吗？不可以。但是热键实现呢，可以，但是谁又能记住这么多的快捷键呢，所以不现实。
2.  触发都是即时性的，假如我想定时打开一个网页，发送一封邮件呢，单单靠Alfred可以做到吗？显然是不现实的。

以上两点是它的设计短板，也或者是设计天花板。这里我推荐两款软件可以互补它的不足，解决类似如上我提到的需求。

![](https://miro.medium.com/max/60/0*Xc3Ef1wWh1eXqlJ3?q=20)

讲了这么一堆，一句话，Alfred，效率人值得拥有。还有什么好玩的呢，且玩且探索吧。

晒下我的用量报告

![](https://miro.medium.com/max/60/0*_WMrdn43b6NwcEtE?q=20)
