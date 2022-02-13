# 自动化Git提交

[www.iosugar.com](https://www.iosugar.com/2017/02/25/Automated-Git-Submitted/)

## 目标

　　在每天的固定时间自动将今天修改的本地仓库提交到_github_,算是提高生产力的一种方式吧。

## shell脚本

　　首先要了解手工提交的时候执行的操作步骤

### commite\_repo.sh

　　_commite\_repo.sh_用于提交本地仓库到_github_

1-
2-
3-
4-
5-
6-
7-
8-
9-
10-
11-
12-
13-
14-
15-
16-
17-
18-
19-
20-
21-
22-
23-
24-
25-
26-
27-
28-
29-
30-
31-

#!/bin/bash-
-
DATE=$(date +%s)-
\# 要提交到github的工程名称-
CheckList=('Daily\_knowledge\_set','Daily\_sh\_set','Daily\_ui\_objc\_set','Daily\_ui\_set','Daily\_modules','Daily\_leetcode\_set')-
TARGET="你的本地仓库地址"-
\# echo ${DATE}-
cd ${TARGET}-
for file in ${TARGET}/\*-
do-
 cd ${file}-
 echo ${file##\*/}-
 git remote -v | grep fetch | awk '{print $2}' | git pull-
done-
-
for file in ${TARGET}/\*-
do-
if echo "${CheckList\[@\]}" | grep -w ${file##\*/} &>/dev/null; then-
 cd ${file}-
 \# echo ${file}-
 git status | grep "nothing to commit" > /dev/null 2>&1-
 if \[ $? != 0 \]; then-
 echo "提交新的Commit:"${file##\*/}\_${DATE}-
 git add .-
 git commit -m ${file##\*/}\_${DATE}-
 git push-
 else-
 echo "没有更改:"${file##\*/}-
 fi-
fi-
done-

提交博客到_github_

1-
2-
3-

» ~ ~/.blog\_ssh-add.sh-
» ~ sudo hexo g-
» ~ sudo hexo d-

_~/.blog\_ssh-add.sh_是为了解决因为_ssh_的证书不是默认的_id\_rsa_,每次重启_Mac_提交博客,_hexo_都会报_公钥错误_的问题。

剩下两个是生成静态博客与部署到_github_的指令

因为使用了_sudo_,其中有一个密码验证的过程,在[Hexo提交出现Permission denied (publickey)](http://www.iosugar.com/2016/11/24/Hexo-commit-exists-Permission-denied-publickey/)已经使用过_expect_来解决了,照葫芦画瓢写了如下脚本

### exec\_hexo.sh

1-
2-
3-
4-
5-
6-
7-
8-
9-
10-
11-

#!/usr/local/bin/expect -f-
-
spawn sudo hexo g-
expect "Password:"-
send "你的密码\\n"-
interact-
-
spawn sudo hexo d-
expect "Password:"-
send "你的密码\\n"-
interact-

再创建一个脚本去执行上面的两个脚本

### schedule\_commit\_rep.sh

1-
2-
3-
4-
5-
6-
7-
8-
9-
10-
11-
12-
13-
14-
15-

#!/bin/bash-
-
export PATH=脚本存放路径:$PATH-
-
repo\_path="你的本地仓库地址"-
blog\_path="你的本地博客地址"-
-
\# 提交本地仓库-
cd ${repo\_path}-
commit\_repo.sh-
-
\# 提交blog-
cd ${blog\_path}-
~/.blog\_ssh-add.sh-
exec\_hexo.sh-

### 执行效果

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fooo.0o0.ooo%2F2017%2F02%2F25%2F58b199065e6af.jpg)](https://ooo.0o0.ooo/2017/02/25/58b199065e6af.jpg)

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fooo.0o0.ooo%2F2017%2F02%2F25%2F58b199095b050.jpg)](https://ooo.0o0.ooo/2017/02/25/58b199095b050.jpg)

现在脚本已经准备好了,需要做的是加入定时任务

## crontab

根据这篇文章[OSX系统添加定时任务](http://honglu.me/2014/09/20/OSX%E7%B3%BB%E7%BB%9F%E6%B7%BB%E5%8A%A0%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1/),可知_macOS_添加定时任务至少有两种方法,虽然作者推荐用_launchctl_,

> 这个是通过plist配置的方式来实现定时任务的，其优点就是最小时间间隔是一秒

但是这个优点在现在这个场景下并不重要,而且配置上比直接用_crontab_麻烦一些,因此还是选择用_crontab_来创建定时任务。

1-
2-
3-
4-

» ~ sudo crontab -e-
Password:-
crontab: no crontab for root - using an empty one-
crontab: installing new crontab-

进入创建新的计划任务的页面,并设置成每分钟都执行

1-

\* \* \* \* \* /bin/bash 脚本路径/.schedule\_commit\_rep.sh-

等了好几分钟,似乎什么都没有发生,又找了两篇文章,说明了_crontab_的原理是

> 当用户使用crontab命令新建任务计划之后，该项 jobs 就会被 /var/spool/cron/ 目录下，而且以用户账号来创建一个文件，每一项任务计划为一行。-
> crond 检测的时间周期是 “分钟”， 每分钟会读取一次 /etc/crontab， 以及 /var/spool/cron 里面的记录并执行。-
> crond 执行的每一项任务计划，都会被记录到 /var/log/cron 这个日志文件。

另一篇说明的是在_macOS_上如何启用_crontab_,_macOS\*上默认没有_/etc/crontab_文件,用_sudo touch /etc/crontab_创建,等了几分钟后依然没有,查看了并没有_spool/cron_目录,在_/var/log_也没有\*cron_,_macOS_上的_crontab_与一般_Linux_上差别还是有点大。

搜索下_stackoverflow_,感觉比较近的[is crontab broken on OSX El Capitan?](http://stackoverflow.com/questions/32781884/is-crontab-broken-on-osx-el-capitan),

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fooo.0o0.ooo%2F2017%2F02%2F25%2F58b1990762564.jpg)](https://ooo.0o0.ooo/2017/02/25/58b1990762564.jpg)

指定编辑器?我使用_crontab -l_已经能看到了添加的计划任务了,感觉不太符合我的情况

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fooo.0o0.ooo%2F2017%2F02%2F25%2F58b1990a27cc0.jpg)](https://ooo.0o0.ooo/2017/02/25/58b1990a27cc0.jpg)

我尝试了一下

1-

\*/5 \* \* \* \* date >> /tmp/z.date-

确实是生成了,但是觉得并不能解决我的问题。好吧,换个思路吧,既然在_macOS_上推荐用_launchctl_,那就换个思路用它吧

## launchctl

_cd_到家目录下的_Library/LaunchAgents_

1-

» ~ cd /Users/Jason/Library/LaunchAgents-

创建定时任务的_plist_描述文件,比如_com.jason.launchctl.plist_

1-
2-
3-
4-
5-
6-
7-
8-
9-
10-
11-
12-
13-
14-
15-
16-
17-
18-
19-
20-
21-
22-
23-

<?xml version="1.0" encoding="UTF-8"?>-
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd"\>-
<plist version\="1.0"\>-
<dict\>-
 <key\>Label</key\>-
 <string\>com.jason.launchctl.plist</string\>-
 <key\>ProgramArguments</key\>-
 <array\>-
 <string\>脚本路径.sh</string\>-
 </array\>-
 <key\>StartCalendarInterval</key\>-
 <dict\>-
 <key\>Minute</key\>-
 <integer\>30</integer\>-
 <key\>Hour</key\>-
 <integer\>22</integer\>-
 </dict\>-
 <key\>StandardOutPath</key\>-
<string\>脚本执行输出日志路径.log</string\>-
<key\>StandardErrorPath</key\>-
<string\>脚本执行错误日志路径.err</string\>-
</dict\>-
</plist\>-

参考以下指令启动定时任务

1-
2-
3-
4-
5-

launchctl load com.jason.launchctl.plist-
launchctl unload com.jason.launchctl.plist-
launchctl start com.jason.launchctl.plist-
launchctl stop com.jason.launchctl.plist-
launchctl list-

*   要让任务生效，必须先load命令加载这个plist
*   如果任务呗修改了，那么必须先unload，然后重新load
*   start可以测试任务，这个是立即执行，不管时间到了没有
*   执行start和unload前，任务必须先load过，否则报错
*   stop可以停止任务
*   ProgramArguments内不能直接写命令，只能通过shell脚本来执行

用_load_,然后用_start_直接执行一下

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fooo.0o0.ooo%2F2017%2F02%2F25%2F58b199066cd3a.jpg)](https://ooo.0o0.ooo/2017/02/25/58b199066cd3a.jpg)

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fooo.0o0.ooo%2F2017%2F02%2F25%2F58b1990965141.jpg)](https://ooo.0o0.ooo/2017/02/25/58b1990965141.jpg)

已经在执行了,终于可以了。

但是发现提交博客输入密码那步停了,修改_plist_只执行提交_blog\*的那一步,发现_.err\*有内容,打开

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fooo.0o0.ooo%2F2017%2F02%2F25%2F58b19909218d2.jpg)](https://ooo.0o0.ooo/2017/02/25/58b19909218d2.jpg)-
我先试试把_interact_注释看影响操作不。

尝试后发现会影响,搜索后没有发现比较满意的解决方案,**如何用_expect_配合_launchctl_实现定时任务,即该任务要自动输入密码?**

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fooo.0o0.ooo%2F2017%2F02%2F25%2F58b1990ab0906.jpg)](https://ooo.0o0.ooo/2017/02/25/58b1990ab0906.jpg)

我以为是_zsh_终端的关系,换成_bash_在终端下执行也没有问题,而用_launchctl_执行配置文件不管是_bash_还是_zsh_都一样报错。

## 解决方案

用写的脚本提交博客与本地工程,虽然无法定时的自动执行,但与以前相比效率还是稍微提高了一些的😔。

## 参考

*   [OSX系统添加定时任务](http://honglu.me/2014/09/20/OSX%E7%B3%BB%E7%BB%9F%E6%B7%BB%E5%8A%A0%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1/)
*   [在MAC OS X上如何启用crontab](http://www.cnblogs.com/pcy0/p/how-to-enable-crontab-on-osx.html)
*   [crontab 与 crond](http://skypegnu1.blog.51cto.com/8991766/1428632)
*   [mac os 定期任务配置](http://www.netingcn.com/mac-os-plist.html)
*   [is crontab broken on OSX El Capitan?](http://stackoverflow.com/questions/32781884/is-crontab-broken-on-osx-el-capitan)
*   [Mac crontab: Mac OS X startup jobs with crontab, er, launchd](http://alvinalexander.com/mac-os-x/mac-osx-startup-crontab-launchd-jobs)

[查看原网页: www.iosugar.com](https://www.iosugar.com/2017/02/25/Automated-Git-Submitted/)