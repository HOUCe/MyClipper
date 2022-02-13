# [Mac创建定时任务 - 简书](https://www.jianshu.com/p/a7db52965545?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

[![](https://upload.jianshu.io/users/upload_avatars/101810/0fae68c108ca.png?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/png)](https://www.jianshu.com/u/9b722b9acb00)

0.12017.01.09 23:53:37字数 958阅读 5,275

创建定时任务主要就是为了每天固定运行一下脚本之类的。比如`cocoapods`仓库每天总是有新的第三方库提交，那么`pod update`的时候就会更新master分支，所以我就需要每天定时更新master，省得到时候再去pull master。

## launchctl 定时任务

一般最常用的就是`launchctl`这种定时方式了。它是通过plist配置的方式来实现定时任务的。

### plist文件格式

![](https://upload-images.jianshu.io/upload_images/101810-515a6ea52a715284.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/png)

上图就是一个简单的定时任务的plist文件。下面来简单说一下里面Key的意思。

-   Label（String）  
    任务名称，建议和文件名一样
    
-   Program（String）  
    要定时执行的脚本，绝对路径
    
-   ProgramArguments (Array)  
    要定时执行的脚本和一些参数，绝对路径。如果上面的Program省略的话执行的就是`ProgramArguments`里的第一个元素。
    
-   StandardErrorPath（String）  
    脚本执行错误时的输出日志，绝对路径
    
-   StandardOutPath（String）  
    脚本输出的内容，绝对路径
    
-   StartCalendarInterval（Dictionary）  
    脚本运行的时间。Minute, Hour, Day, Month, Weekday。
    
-   StartInterval（Number）  
    间隔运行的时间，单位为秒。
    
-   Disabled（Boolean）  
    是否不可用，默认为NO可用。
    
-   LimitLoadToSessionType（String）  
    限制访问的类型。AQUA：一个GUI剂，即限制访问所有GUI服务。这个Key好像没什么用，可用不填。
    
-   RunAtLoad（Boolean）  
    标识launchd在加载完该项服务之后立即启动路径指定的可执行文件。默认值为false。
    
-   KeepAlive（Boolean）  
    这个key值是用来控制可执行文件是持续运行呢，还是满足具体条件之后再启动。默认值为false，也就是说满足具体条件之后才启动。当设置值为ture时，表明无条件的开启可执行文件，并使之保持在整个系统运行周期内。
    

### plist文件放置处

-   ~/Library/LaunchAgents 由用户自己定义的任务项（推荐）
-   /Library/LaunchAgents 由管理员为用户定义的任务项
-   /Library/LaunchDaemons 由管理员定义的守护进程任务项
-   /System/Library/LaunchAgents 由Mac OS X为用户定义的任务项
-   /System/Library/LaunchDaemons 由Mac OS X定义的守护进程任务项

建议放在 ~/Library/LaunchAgents 下面。

**下面再来理解几个基础概念：**

/System/Library和/Library和~/Library目录的区别？

> /System/Library目录是存放Apple自己开发的软件。  
> /Library目录是系统管理员存放的第三方软件。  
> ~/Library/是用户自己存放的第三方软件。

LaunchDaemons和LaunchAgents的区别？

> LaunchDaemons是用户未登陆前就启动的服务（守护进程）。  
> LaunchAgents是用户登陆后启动的服务（守护进程）。

### launchctl 命令

添加： launchctl load /System/Library/LaunchAgents/com.test.plist  
移除： launchctl unload /System/Library/LaunchAgents/com.test.plist  
查看： launchctl list  
立即执行任务：launchctl start com.aigo.launchctl.plist  
停止执行任务：launchctl stop com.aigo.launchctl.plist

**注意：**

1.  你所运行的脚本需要有权限才能执行：`chmod a+x test.sh`
2.  要让任务生效，必须先load命令加载这个plist
3.  如果任务被修改了，那么必须先unload，然后重新load
4.  start可以测试任务，这个是立即执行，不管时间到了没有
5.  执行start和unload前，任务必须先load过，否则报错
6.  ProgramArguments内不能直接写命令，只能通过shell脚本来执行

### launchctl的GUI工具

[LaunchControl](https://link.jianshu.com/?t=http://www.soma-zone.com/LaunchControl/)，用这个工具可以查看到所有的launchctl定时任务。并用GUI的方式进行修改执行等。

## 参考

[OSX系统添加定时任务](https://link.jianshu.com/?t=http://honglu.me/2014/09/20/OSX%E7%B3%BB%E7%BB%9F%E6%B7%BB%E5%8A%A0%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1/)  
[Mac OSX的开机启动配置](https://link.jianshu.com/?t=http://www.tanhao.me/talk/1287.html/)  
[Mac上，执行定时任务：launchctl](https://link.jianshu.com/?t=https://my.oschina.net/shede333/blog/470377)

更多精彩内容，就在简书APP

"请我撸个串吧"

还没有人赞赏，支持一下

[![  ](https://upload.jianshu.io/users/upload_avatars/101810/0fae68c108ca.png?imageMogr2/auto-orient/strip|imageView2/1/w/100/h/100/format/png)](https://www.jianshu.com/u/9b722b9acb00)

[齐滇大圣](https://www.jianshu.com/u/9b722b9acb00 "齐滇大圣")无论多老，做个两眼有光的人<br>

总资产525共写了13.7W字获得3,302个赞共1,768个粉丝

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

-   launchctl是一个统一的服务管理框架，可以启动、停止和管理守护进程、应用程序、进程和脚本等。launchct...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/14-0651acff782e7a18653d7530d6b27661.jpg)繁著](https://www.jianshu.com/u/67eb7ed414d3)阅读 45,585评论 8赞 42
    
-   Ubuntu的发音 Ubuntu，源于非洲祖鲁人和科萨人的语言，发作 oo-boon-too 的音。了解发音是有意...
    

-   Mac下添加定时任务 编写任务脚本 把要执行的任务写好 编写任务描述文件 mac的任务描述文件是plist格式的。...
    
    [![](https://upload-images.jianshu.io/upload_images/1001271-70f7a4485662a3d7.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/jpg)](https://www.jianshu.com/p/b4f31bf47b5d)
-   去年因公司需要给电视盒子做一个自定义键盘，今天偶然想起来， 想趁此机会记录一下。当时好像是下载了一个别人的例子，参...
    
    [![](https://upload-images.jianshu.io/upload_images/2648882-7952946237a155a2.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/bad096d378cb)
-   人非圣贤，孰能无过，这是再简单不过的道理，谁都明白，很多人不明白的是，犯了错又该如何？在单位，别说你犯了错，即使没...
    
    [![](https://cdn3.jianshu.io/assets/default_avatar/10-e691107df16746d4a9f3fe9496fd1848.jpg)一只笨蛋](https://www.jianshu.com/u/4d9ce8644bb7)阅读 581评论 2赞 4
    
-   Monad不就是个自函子范畴上的幺半群，这有什么难理解的（A monad is just a monoid in ...
    
    [![](https://upload-images.jianshu.io/upload_images/217988-81dc30327d901c40.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/324c9ce77317)
