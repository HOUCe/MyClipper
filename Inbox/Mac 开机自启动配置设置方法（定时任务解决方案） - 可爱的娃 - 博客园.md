# [Mac 开机自启动配置设置方法（定时任务解决方案） - 可爱的娃 - 博客园](https://www.cnblogs.com/linusflow/p/7365802.html)

　　在linux下执行定期任务可以使用crontab，目前mac os也可以使用它，不过已不推荐使用。推荐做法是采用plist脚本，plist脚本可以设置执行的动作，时间间隔等其他一些信息。另外crontab的最小时间间隔是一分钟，使用plist脚本原则上时间间隔可以为一秒。

　　plist脚本存放路径为/Library/LaunchDaemons或/Library/LaunchAgents，其区别是后一个路径的脚本当用户登陆系统后才会被执行，前一个只要系统启动了，哪怕用户不登陆系统也会被执行。可以通过两种方式来设置脚本的执行时间。一个是使用StartInterval，它指定脚本每间隔多长时间（单位：秒）执行一次；另外一个使用StartCalendarInterval，它可以指定脚本在多少分钟、小时、天、星期几、月时间上执行，类似如crontab的中的设置。

一个简单例子如下：

![复制代码](https://common.cnblogs.com/images/copycode.gif)

 1 <?xml version="1.0" encoding="UTF-8"?>
 2 <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd"\>
 3 <plist version\="1.0"\>
 4 <dict\>
 5     <key\>Label</key\>
 6     <string\>com.yangyz.cron.test.plist</string\>
 7     <key\>ProgramArguments</key\>
 8     <array\>
 9         <string\>/Users/yangyz/plist-test.sh</string\>
10     </array\>
11     <key\>KeepAlive</key\>
12     <false/>
13     <key\>RunAtLoad</key\>
14     <true/>
15     <key\>StartInterval</key\>
16     <integer\>60</integer\>
17 </dict\>
18 </plist\>

![复制代码](https://common.cnblogs.com/images/copycode.gif)

其中key是plist脚本定义的属性，紧跟着的下一行是该属性对应的值。上述脚本是每间隔60秒执行一次/Users/yangyz/plist-test.sh这个shell脚本，也可以使用StartCalendarInterval来替换StartInterval达到同样的效果，例如：

<key\>StartCalendarInterval</key\>
<dict\>
  <key\>Minute</key\>
  <integer\>0</integer\>
</dict\>

上述设置的意思为每天的每个小时的第0分钟执行，也即使每60秒执行一次。

plist脚本中定义的属性以及具体的含义，可以参看苹果官方网站的说明，地址为：[launchd.plist(5) Mac OS X Manual Page](http://www.voidcn.com/link?url=https://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man5/launchd.plist.5.html "爬虫采集 SEO 机器翻译垃圾网站")

launchctl命令可以控制plist脚本停止或重新加载。例如：

1

2

3

4

5

`launchctl unload` `/Library/LaunchDaemons/com``.yangyz.``cron``.``test``.plist`

`launchctl load` `/Library/LaunchDaemons/com``.yangyz.``cron``.``test``.plist`

如果执行上面命令看到launchctl: Dubious ownership on file (skipping): /Library/LaunchDaemons/com.yangyz.cron.test.plist这样的错误，其原因是该脚本的owner和当前执行操作用户不一致。使用chown修改一下即可。

参考资料：  
[http://www.devdaily.com/mac-os-x/launchd-plist-examples-startinterval-startcalendarinterval](http://www.voidcn.com/link?url=http://www.devdaily.com/mac-os-x/launchd-plist-examples-startinterval-startcalendarinterval "爬虫采集 SEO 机器翻译垃圾网站")  
[http://www.devdaily.com/mac-os-x/mac-osx-startup-crontab-launchd-jobs](http://www.voidcn.com/link?url=http://www.devdaily.com/mac-os-x/launchd-plist-examples-startinterval-startcalendarinterval "爬虫采集 SEO 机器翻译垃圾网站")
