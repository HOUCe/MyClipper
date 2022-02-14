# [Mac中的定时任务利器：launchctl - 简书](https://www.jianshu.com/p/4addd9b455f2)

[![](https://cdn2.jianshu.io/assets/default_avatar/14-0651acff782e7a18653d7530d6b27661.jpg)](https://www.jianshu.com/u/67eb7ed414d3)

22017.09.01 23:42:10字数 775阅读 45,581

> launchctl是一个统一的服务管理框架，可以启动、停止和管理守护进程、应用程序、进程和脚本等。  
> launchctl是通过配置文件来指定执行周期和任务的。

当然mac也可以像linux系统一样，使用crontab命令来添加定时任务，这里就不赘述，具体可参见：[OS X 添加定时任务](https://link.jianshu.com/?t=http://codingpub.github.io/2016/10/27/OS-X-%E6%B7%BB%E5%8A%A0%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1/)

下面将手把手教你在mac上创建定时任务。（任务目标：每天晚上十点定时执行/Users/demo/helloworld.py的python程序）

### 1\. 创建run.sh脚本

进入 `helloworld.py`程序所在目录  
`cd /User/demo`  
创建run.sh脚本  
`vi run.sh`  
添加执行`helloworld.py`的命令

```
#!/bin/sh

# 记录一下开始时间
echo `date` >> /Users/demo/log &&
# 进入helloworld.py程序所在目录
cd /Users/demo &&
# 执行python脚本（注意前面要指定python运行环境/usr/bin/python，根据自己的情况改变）
/usr/bin/python helloworld.py
# 运行完成
echo 'finish' >> /Users/demo/log
```

`：wq`保存退出

注意，脚本要改成可执行的权限  
`chmod 777 run.sh`

### 2\. 编写plist文件

launchctl 将根据plist文件的信息来启动任务。  
plist脚本一般存放在以下目录：

-   `/Library/LaunchDaemons` -->只要系统启动了，哪怕用户不登陆系统也会被执行
    
-   `/Library/LaunchAgents` -->当用户登陆系统后才会被执行
    

更多的plist存放目录：

> ~/Library/LaunchAgents 由用户自己定义的任务项  
> /Library/LaunchAgents 由管理员为用户定义的任务项  
> /Library/LaunchDaemons 由管理员定义的守护进程任务项  
> /System/Library/LaunchAgents 由Mac OS X为用户定义的任务项  
> /System/Library/LaunchDaemons 由Mac OS X定义的守护进程任务项

进入`~/Library/LaunchAgents`，创建一个plist文件`com.demo.plist`

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  
  <key>Label</key>
  <string>com.demo.plist</string>
  
  <key>ProgramArguments</key>
  <array>
    <string>/Users/demo/run.sh</string>
  </array>
  
  <key>StartCalendarInterval</key>
  <dict>
        <key>Minute</key>
        <integer>00</integer>
        <key>Hour</key>
        <integer>22</integer>
  </dict>

<key>StandardOutPath</key>
<string>/Users/demo/run.log</string>

<key>StandardErrorPath</key>
<string>/Users/demo/run.err</string>
</dict>
</plist>
```

### 3\. 加载命令

`launchctl load -w com.demo.plist`  
这样任务就加载成功了。

更多的命令：

```

$ launchctl load -w com.demo.plist


$ launchctl unload -w com.demo.plist


$ launchctl list | grep 'com.demo'


$ launchctl start  com.demo.plist


$ launchctl stop   com.demo.plist
```

> 如果任务呗修改了，那么必须先unload，然后重新load  
> start可以测试任务，这个是立即执行，不管时间到了没有  
> 执行start和unload前，任务必须先load过，否则报错  
> stop可以停止任务

### 番外篇

##### plist支持两种方式配置执行时间：

-   StartInterval: 指定脚本每间隔多长时间（单位：秒）执行一次；
-   StartCalendarInterval: 可以指定脚本在多少分钟、小时、天、星期几、月时间上执行，类似如crontab的中的设置，包含下面的 key:

```
Minute <integer>
The minute on which this job will be run.
Hour <integer>
The hour on which this job will be run.
Day <integer>
The day on which this job will be run.
Weekday <integer>
The weekday on which this job will be run (0 and 7 are Sunday).
Month <integer>
The month on which this job will be run.
```

##### plist部分参数说明：

1.  Label：对应的需要保证全局唯一性；
2.  Program：要运行的程序；
3.  ProgramArguments：命令语句
4.  StartCalendarInterval：运行的时间，单个时间点使用dict，多个时间点使用 array <dict>
5.  StartInterval：时间间隔，与StartCalendarInterval使用其一，单位为秒
6.  StandardInPath、StandardOutPath、StandardErrorPath：标准的输入输出错误文件，这里建议不要使用 .log 作为后缀，会打不开里面的信息。
7.  定时启动任务时，如果涉及到网络，但是电脑处于睡眠状态，是执行不了的，这个时候，可以定时的启动屏幕就好了。

> 更多的参数参见:[mac官方文档](https://link.jianshu.com/?t=https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man5/launchd.plist.5.html)

### 参考：

[Mac执行定时任务之Launchctl](https://link.jianshu.com/?t=http://blog.csdn.net/u012390519/article/details/74542042)

更多精彩内容，就在简书APP

"小礼物走一走，来简书关注我"

还没有人赞赏，支持一下

[![  ](https://cdn2.jianshu.io/assets/default_avatar/14-0651acff782e7a18653d7530d6b27661.jpg)](https://www.jianshu.com/u/67eb7ed414d3)

[繁著](https://www.jianshu.com/u/67eb7ed414d3 "繁著")公众号：南强说晚安<br><br>个人主页：www.weiweiblog.cn

总资产372共写了7.8W字获得886个赞共895个粉丝

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

-   创建定时任务主要就是为了每天固定运行一下脚本之类的。比如cocoapods仓库每天总是有新的第三方库提交，那么po...
    
    [![](https://upload.jianshu.io/users/upload_avatars/101810/0fae68c108ca.png?imageMogr2/auto-orient/strip|imageView2/1/w/48/h/48/format/png)齐滇大圣](https://www.jianshu.com/u/9b722b9acb00)阅读 5,275评论 1赞 8
    
    [![](https://upload-images.jianshu.io/upload_images/101810-515a6ea52a715284.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/a7db52965545)
-   前言 一不小心写成上下两篇了.真是有些过意不去.毕竟,写的太多就少了一部分读者(少了一部分赞额). 之所以拆成上下...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/6-fd30f34c8641f6f32f5494df5d6b8f3c.jpg)无与童比](https://www.jianshu.com/u/9a7e0b9da317)阅读 5,297评论 8赞 81
    

-   需求情景 我在Mac上有好几个分散在各处的文件夹需要备份到移动硬盘上，而且最好是增量备份的方式以节约时间。\[1\]需...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/1-04bbeead395d74921af6a4e8214b4f61.jpg)水哥叔叔](https://www.jianshu.com/u/a508c9751b83)阅读 11,645评论 2赞 15
    
    [![](https://upload-images.jianshu.io/upload_images/803494-381a8ad8ff19fc57.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/40a5c07908cb)
-   亲爱的芬兰，是你的蓝惹了我 ...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/14-0651acff782e7a18653d7530d6b27661.jpg)张惜姿](https://www.jianshu.com/u/d9b3bc04ab5e)阅读 359评论 0赞 3
    
    [![](https://upload-images.jianshu.io/upload_images/7283252-c1e1deb8ef2ca0dc.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/8fd09d4c05ae)
-   留给自己的，全部都摆在那里，沉沉的，又浑噩。一只海鸥似乎对此感兴趣，窥伺着，徘徊不去。 “ 下来吧！都给你。”我庆...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/14-0651acff782e7a18653d7530d6b27661.jpg)讴真](https://www.jianshu.com/u/8df9f443517f)阅读 55评论 0赞 2
    
-   重归朝九晚六的上班生活，在阶段目标完成了之后，动力开始不足。明显放松了下来，书也懒得看，工作也只是保证完成...
    
    [![](https://upload.jianshu.io/users/upload_avatars/416900/1af8b8993976.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/48/h/48/format/jpg)阿元](https://www.jianshu.com/u/8738e45d0a5b)阅读 85评论 0赞 0
