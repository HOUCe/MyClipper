# [Mac - 服务管理 launchd、launchctl、LaunchAgent、LaunchDaemon、brew services详解 | Bruce's Blog](https://www.xiebruce.top/983.html)

### 什么是launchd？

[Wikipedia](https://en.wikipedia.org/wiki/Launchd)对launchd的定义：

> In computing, launchd, a unified operating system service management framework, starts, stops and manages daemons, applications, processes, and scripts in macOS.

**我大概对它进行翻译：** **launchd**是一套统一的开源服务管理框架，它用于启动、停止以及管理后台程序、应用程序、进程和脚本。**launch**由苹果公司的Dave Zarzycki所编写，在OS X Tiger系统中首次引入并且获得Apache授权的许可证。

事实上，launchd是macOS第一个启动的进程，它的PID为1，整个系统的其他进程都是由它创建的。

当launchd启动后，它会扫描`/System/Library/LaunchDaemons`和`/Library/LaunchDaemons`中的plist文件并加载它们；

当你输入密码登入系统后，launchd会扫描`/System/Library/LaunchAgents`、`/Library/LaunchAgents`、`~/Library/LaunchAgents`这三个目录中的plist文件并加载它们。

每一个plist文件，都叫一个“Job”(即一个任务)，当然加载了不代表会运行plist文件里所描述的服务，只有plist里设置了`RunAtLoad`为true或`keepAlive`为true时，才会在加载这些plist文件的同时启动plist所描述的服务。

### 什么是守护进程？

守护进程，英文叫**Daemon**，守护进程其实就是在后台运行的程序，它没有界面，你看不到它，一般使用命令来对它进程管理控制，守护进程常被设置为开机自动启动(当然也可以开机后手动用命令启动)，很多软件的“服务器端”一般都是以守护进程的方式运行，比如数据库`mysql/mongodb`、内存缓存`redis/memcache`、Web服务器`nginx/apache`等等都是以守护进程方式运行的，它们通过“接口”对外提供服务(如Unix socket / tcp 方式等等)。

### 什么是Daemons和Agents？

**Daemon**我们已经知道，中文叫**“守护进程”**，那**Agent**又是什么鬼？Agent你要翻译的话，那就只能翻成成**“代理”**，其实我认为在这里没有必要翻译，只需要理解这两个是干什么的就好了。

Daemons和Agents都是launchd所管理的后台程序，它们的区别是**Agent**是属于当前登录用户(就是你开机后输入密码时的那个用户名)，它们是以当前登录的用户权限启动的，而**Daemon**则属于root用户，但由于有root用户权限，所以它可以指定以什么用户运行，也可以不指定（不指定就是以root用户运行）。

### launchd怎样管理后台程序？

launchd是通过以**“.plist”**后缀结尾的xml文件来定义一个程序的开机自启动的，我们一般称它为plist文件，下面展示的是nginx的plist文件：

**plist文件分别放在以下几个目录：**

类型

位置

以什么用户运行

用户Agents

~/Library/LaunchAgents

当前登录用户

全局Agents

/Library/LaunchAgents

当前登录用户

全局Daemons

/Library/LaunchDaemons

root或指定的用户

系统Agents

/System/Library/LaunchAgents

当前登录用户

系统Daemons

/System/Library/LaunchDaemons

root或指定的用户

其中/System目录下的我们不用管，那些是系统本身的一些进程，我们要关注的是以下三个目录：

```
~/Library/LaunchAgents
/Library/LaunchAgents
/Library/LaunchDaemons
```

也就是说，只要在这三个目录中的plist文件所定义的服务，都会“开机自启动”（前面说过，这实际上是开机/登录时，自动加载plist文件，但并不一定会启动plist文件指定的服务，只有当plist文件中设置`RunAtLoad`为true时，才会在加载的同时启动plist对应的服务）。

首先说一下`~/Library/LaunchAgents`和`/Library/LaunchAgents`的区别，它们都属于“当前登录用户”，它们启动的时候，也都是以“当前登录用户”权限来启动的，而区别，是一个属于所有用户，一个属于“当前设置的用户”，因为`~`代表当前用户家目录，对于mac来说，假设你当前用户名是`zhangsan`，那么`~`就代表`/Users/zhangsan`目录，而`~/Library/LaunchAgents`就代表`/Users/zhangsan/Library/LaunchAgents`，试想一下，如果你在`zhangsan`用户下往`/Users/zhangsan/Library/LaunchAgents`添加了用于开机自启动的plist文件，然后你用“访客用户”登录你的mac，那么你登录之后，这个plist会起作用吗？很明显不会，因为用户之间的目录是不可见的，也就是访客用户是看不到`/Users/zhangsan/Library/LaunchAgents`这个目录的。这个目录对应到访客用户是`/Users/Guest/Library/LaunchAgents`。

而`/Library/LaunchAgents`目录就不一样了，它属于所有用户，不管是`zhangsan`还是访客用户又或者你创建了其他用户，对这个目录都可见，也就是说如果你往`/Library/LaunchAgents`目录中添加了开启自启动的plist文件，那么不管你用什么用户登录，都会在该用户登录时自启动，这就是`~/Library/LaunchAgents`和`/Library/LaunchAgents`的区别。

`LaunchAgents`与`LaunchDaemons`的区别，其实前面已经说过其中一个区别，就是`LaunchAgents`是以当前用户权限运行的，而`LaunchDaemons`则是以root用户或你指定的其他用户权限运行的。第二个区别，就是`LaunchDaemons`中的配置文件所配置的服务，是在开机但未输入密码登录的时候就运行了，而`LaunchAgents`中的plist文件定义的服务则要你输入密码登录进去后，才会运行。

### launchctl的使用

launchctl是launchd的管理工具，它用于管理plist文件对应的服务的启动、停止、重启等等。

列出已加载的Job(即plist文件)：

以下是部分列表：

```
PID Status  Label
299 0   com.apple.trustd.agent
-   0   com.apple.MailServiceAgent
-   0   com.apple.mdworker.mail
-   0   com.apple.mdworker.shared.0E000000-0000-0000-0000-000000000000
81609   0   com.apple.mdworker.shared.04000000-0000-0000-0000-000000000000
-   0   com.apple.cvmsCompAgent3425AMD_i386
-   0   com.apple.cvmsCompAgent3425AMD_i386_1
287 0   com.apple.cfprefsd.xpc.agent
-   0   com.apple.SafariHistoryServiceAgent
-   0   com.apple.progressd
-   0   com.google.keystone.user.xpcservice
-   0   net.yanue.V2rayU.Launcher
60030   -15 com.apple.Finder
339 0   com.apple.homed
393 0   com.apple.SafeEjectGPUAgent
-   0   com.apple.quicklook
-   0   com.apple.parentalcontrols.check
465 0   com.zzd.Xnip.47064
-   0   com.apple.PackageKit.InstallStatus
-   0   com.apple.mediaremoteagent
-   0   com.apple.FontWorker
332 0   com.apple.bird
-   0   com.svend.uPic.66512
5264    0   com.coderforart.MWeb3.61188
419 0   DZQ5YNVEU2.com.smartisan.IdeaPillsHelper
-   0   com.apple.familycontrols.useragent
-   0   com.apple.AssetCache.agent
667 0   com.zzd.XnipHelper.65636
372 0   com.apple.universalaccessAuthWarn
307 0   com.apple.nsurlsessiond
5547    0   abnerworks.Typora.65528
830 0   com.apple.ActivityMonitor.50852
-   0   com.apple.syncservices.uihandler
348 0   com.apple.iconservices.iconservicesagent
-   0   com.apple.ContactsAgent
896 0   com.apple.SafariBookmarksSyncAgent
-   0   com.apple.ManagedClientAgent.agent
79711   0   yanue.v2rayu.v2ray-core
-   1   homebrew.mxcl.openresty
-   0   com.apple.screensharing.agent
358 0   com.apple.commerce
-   0   com.apple.TMHelperAgent.SetupOffer
-   0   com.apple.AddressBook.SourceSync
-   0   com.apple.installerauthagent
```

其中`-`开头的表示虽然加载了，但是未启动，数字开头的表示已启动且这个数字就是它的pid。

我们已经知道，开机的时候系统会自动加载LaunchDaemons目录下的plist文件，登入系统时会自动加载两个LaunchAgents目录中的plist文件，而如果你刚刚添加的plist文件，你想加载，可以用`launchctl`手动加载一个任务：

永久加载一个任务：

**注：** 说实话我不太明白永久加载的意思，因为重启后都会自动扫描，感觉这个没啥意义，但我平时用的时候都会加上`-w`。

禁用一个任务(加载的反操作)：

永久禁用一个任务(永久加载的反操作)：

启动一个任务：

**注：** 前面说过加载了并不一定启动了，如果启动了当然就不用再用这个命令启动了。

停止一个任务：

### brew services

忘了从哪个版本开始，brew就添加了brew services的功能，它可以管理用[brew](https://www.xiebruce.top/720.html)安装的需要后台运行(即以守护进程方式运行)的软件。

它的原理，就是通过向`~/Library/LaunchAgents`或`/Library/LaunchDaemons`目录中添加或删除指定的plist文件，来实现启动/停止/开机自启动这些操作，不过`brew services`只能管理用`brew`安装的服务。

查看服务列表：

![screenshot.jpeg](https://img.xiebruce.top/2019/05/31/f43a7807dadd2a3cae257afca29eaa6f.jpeg)

由上表我们可以看到它有四列，第一列是brew已安装的服务名称，第二列是启动状态，第三列是已启动的服务的启动用户，第四列是已启动的服务对应的plist文件。

启动服务：

启动服务的原理，其实就是向`/Library/LaunchDaemons`(使用了sudo)或`~/Library/LaunchAgents`(未使用sudo)添加plist文件(plist文件是brew官方制作安装包时就写好的，我们用brew安装后就已经存在了)并运行`launchctl load -w 服务对应的plist文件路径`。

停止服务：

停止服务其实就是做启动服务的反操作，先用`launchctl`停止对应任务，然后从`/Library/LaunchDaemons`或`~/Library/LaunchAgents`中删除对应的plist文件。

重启服务：

当然如果你不想用命令，其实也有人写了一个brew services的客户端：[LaunchRocket](https://github.com/jimbojsb/launchrocket)，这个客户端专门用来管理用brew安装的服务软件，在这个软件列表里出来的服务，其实就是`brew services list`命令输出的列表，不过我还是建议在懂得原理之后再用LaunchRocket会更好一点。  
![screenshot.jpeg](https://img.xiebruce.top/2019/05/31/44e838a5774074db9326e37b45d0faf5.jpeg)

___

参考资料：  
[www.launchd.info](https://www.launchd.info/)  
[Daemons and Agents](https://developer.apple.com/library/archive/technotes/tn2083/_index.html)  
[Creating Launch Daemons and Agents](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLaunchdJobs.html)
