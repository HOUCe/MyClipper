# [Mac执行定时任务之launchctl - 简书](https://www.jianshu.com/p/b65c1d339eec)

[![](https://upload.jianshu.io/users/upload_avatars/2269857/fd49b2e5-eb3a-42d9-9097-ab51818dc506.png?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/png)](https://www.jianshu.com/u/ed45a6dad518)

2017.07.06 14:16:49字数 344阅读 8,180

### Mac执行定时任务之Launchctl

> launchctl是一个统一的服务管理框架，启动、停止和管理守护进程、应用程序、进程和脚本。下面讲述一下如何在Mac上使用launchctl执行定时任务。

#### 一、编写一个plist文件

launchctl 将根据这个plist文件的信息来启动任务，plist文件中的关键字可以在 [苹果官方文档](https://link.jianshu.com/?t=https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man5/launchd.plist.5.html#//apple_ref/doc/man/5/launchd.plist) 找到，值得注意的是 `Label` 对应的值需要保证唯一性，作为任务的唯一标示。可以使用如下命令来验证plist格式的正确性(不代表命令有效)：

```
$ plutil-lint /Users/denglibing/Library/LaunchAgents/com.denglibing.checkin.plist
```

这个是一个完整的 plist 文件：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.denglibing.checkin</string>
    <key>RunAtLoad</key>
    <true/>
    <key>ProgramArguments</key>
    <array> <string>/Users/denglibing/Desktop/denglibing_checkin/denglibing_checkin_request.sh</string>
    </array>
    <key>StartCalendarInterval</key>
    <array>
        <dict>
            <key>Weekday</key>
            <integer>1</integer>
            <key>Hour</key>
            <integer>8</integer>
            <key>Minute</key>
            <string>58</string>
        </dict>
        <dict>
            <key>Weekday</key>
            <integer>2</integer>
            <key>Hour</key>
            <integer>8</integer>
            <key>Minute</key>
            <string>52</string>
        </dict>
    </array>
    <key>StandardOutPath</key>
    <string>/Users/denglibing/Desktop/denglibing_checkin/outlog</string>
    <key>StandardErrorPath</key>
    <string>/Users/denglibing/Desktop/denglibing_checkin/errorlog</string>
</dict>
</plist>
```

#### 二、编写定时脚本

即上面plist文档中的 `denglibing_checkin_request.sh` 脚本，以最简单的为例 (打开脚本)：

```
# denglibing_checkin_request.sh
$ open /Users/denglibing/Desktop/denglibing_checkin/denglibing_checkin_request.sh
```

值得注意的是，需要设置这个脚本为可执行文件：

```
$ chmod a+x /Users/denglibing/Desktop/denglibing_checkin/denglibing_checkin_request.sh
```

#### 三、plist文件的位置

```
* ~/Library/LaunchAgents 由用户自己定义的任务项
* /Library/LaunchAgents 由管理员为用户定义的任务项
* /Library/LaunchDaemons 由管理员定义的守护进程任务项
* /System/Library/LaunchAgents 由Mac OS X为用户定义的任务项
* /System/Library/LaunchDaemons 由Mac OS X定义的守护进程任务项
```

**建议放在 ~/Library/LaunchAgents 下面。**

```
/System/Library和/Library和~/Library目录的区别？
/System/Library目录是存放Apple自己开发的软件。
/Library目录是系统管理员存放的第三方软件。
~/Library/是用户自己存放的第三方软件。

LaunchDaemons和LaunchAgents的区别？
LaunchDaemons是用户未登陆前就启动的服务（守护进程）。
LaunchAgents是用户登陆后启动的服务（守护进程）。
```

#### 四、加载命令

```
# 加载任务, -w选项会将plist文件中无效的key覆盖掉，建议加上
$ launchctl load -w com.denglibing.checkin.plist

# 删除任务
$ launchctl unload -w com.denglibing.checkin.plist

# 查看任务列表, 使用 grep '任务部分名字' 过滤
$ launchctl list | grep 'com.denglibing'
```

#### 五、总结

launchctl在定时启动任务非常简单和方便，值得注意的地方就是 plist 文件了。

```
1、Label：对应的需要保证全局唯一性；
2、Program：要运行的程序；
3、ProgramArguments：命令语句
4、StartCalendarInterval：运行的时间，单个时间点使用dict，多个时间点使用 array <dict>
5、StartInterval：时间间隔，与StartCalendarInterval使用其一，单位为秒
6、StandardInPath、StandardOutPath、StandardErrorPath：标准的输入输出错误文件，这里建议不要使用 .log 作为后缀，会打不开里面的信息。
7、定时启动任务时，如果涉及到网络，但是电脑处于睡眠状态，是执行不了的，这个时候，可以定时的启动屏幕就好了。
```

#### 六、相关链接

[Mac上，执行定时任务：launchctl](https://link.jianshu.com/?t=https://my.oschina.net/shede333/blog/470377)

[苹果官方文档：The Mac OS X launchd plist format | launchd plist file format (valid keys) | alvinalexander.com](https://link.jianshu.com/?t=https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man5/launchd.plist.5.html)

[多个时间点启动任务](https://link.jianshu.com/?t=https://stackoverflow.com/questions/3570979/whats-the-difference-between-day-and-weekday-in-launchd-startcalendarinterv)

更多精彩内容，就在简书APP

"小礼物走一走，来简书关注我"

还没有人赞赏，支持一下

[![  ](https://upload.jianshu.io/users/upload_avatars/2269857/fd49b2e5-eb3a-42d9-9097-ab51818dc506.png?imageMogr2/auto-orient/strip|imageView2/1/w/100/h/100/format/png)](https://www.jianshu.com/u/ed45a6dad518)

[hd\_hd](https://www.jianshu.com/u/ed45a6dad518 "hd_hd")是差点运气，可是我一直在努力💪

总资产5共写了2.2W字获得138个赞共75个粉丝

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

-   Ubuntu的发音 Ubuntu，源于非洲祖鲁人和科萨人的语言，发作 oo-boon-too 的音。了解发音是有意...
    
-   创建定时任务主要就是为了每天固定运行一下脚本之类的。比如cocoapods仓库每天总是有新的第三方库提交，那么po...
    
    [![](https://upload.jianshu.io/users/upload_avatars/101810/0fae68c108ca.png?imageMogr2/auto-orient/strip|imageView2/1/w/48/h/48/format/png)齐滇大圣](https://www.jianshu.com/u/9b722b9acb00)阅读 5,276评论 1赞 8
    
    [![](https://upload-images.jianshu.io/upload_images/101810-515a6ea52a715284.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/a7db52965545)

-   linux资料总章2.1 1.0写的不好抱歉 但是2.0已经改了很多 但是错误还是无法避免 以后资料会慢慢更新 大...
    
-   launchctl是一个统一的服务管理框架，可以启动、停止和管理守护进程、应用程序、进程和脚本等。launchct...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/14-0651acff782e7a18653d7530d6b27661.jpg)繁著](https://www.jianshu.com/u/67eb7ed414d3)阅读 45,587评论 8赞 42
    
-   正则表达式是对字符串操作的一种逻辑公式，就是用事先定义好的一些特定字符、及这些特定字符的组合，组成一个“规则字符串...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/7-0993d41a595d6ab6ef17b19496eb2f21.jpg)丨勿言](https://www.jianshu.com/u/1301109f3295)阅读 107评论 0赞 0
    
-   活着就是一件很麻烦的事，一个人过两个人过都是麻烦的。虽然各有各的麻烦，但是既然都很麻烦的话，一起度过，不也是种方法...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/14-0651acff782e7a18653d7530d6b27661.jpg)比持卡丘](https://www.jianshu.com/u/354788b609d4)阅读 146评论 0赞 0
    

-   厨房柜门的下方 有个花盆 不是为了托花 而是为了等人 人来了 洗菜刷碗 柜门沿流下来的水 一滴接着一滴 打在盆沿上...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/10-e691107df16746d4a9f3fe9496fd1848.jpg)四知录](https://www.jianshu.com/u/c2e32d048206)阅读 90评论 1赞 2
