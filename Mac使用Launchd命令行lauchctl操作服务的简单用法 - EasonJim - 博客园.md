# [Mac使用Launchd命令行lauchctl操作服务的简单用法 - EasonJim - 博客园](https://www.cnblogs.com/EasonJim/p/7173859.html)

注意：操作时前面比如带上sudo，不然只能操作当前用户的服务，会出现无法操作一些root用户的服务的问题。系统版本为Mac 10.12。

1、配置好plist之后：

#加载一个服务到启动列表
sudo launchctl load -w /System/Library/LaunchDaemons/ssh.plist #卸载一个服务
sudo launchctl unload /System/Library/LaunchDaemons/ssh.plist 

2、查看所有服务：

3、查看服务状态

sudo launchctl list | grep <<Service Name>>

输出具有以下含义：

-   第一个数字是进程的PID，如果它正在运行，如果它不运行，它显示一个' - '。
-   第二个数字是进程的退出代码，如果它已经完成。如果是负数，则是杀死信号的数量。
-   第三列是进程名称。

4、服务操作

![复制代码](https://common.cnblogs.com/images/copycode.gif)

#停止
sudo launchctl stop <<Service Name>>
#开始
sudo launchctl start <<Service Name>>
#kill
sudo launchctl kill <<Service Name>> 

![复制代码](https://common.cnblogs.com/images/copycode.gif)

5、更多的用法直接输入：launchctl help进行查看。

参考：

[https://stackoverflow.com/questions/36594650/command-to-get-the-service-status-of-mac-os](https://stackoverflow.com/questions/36594650/command-to-get-the-service-status-of-mac-os)

[https://serverfault.com/questions/194832/how-to-start-stop-restart-launchd-services-from-the-command-line](https://serverfault.com/questions/194832/how-to-start-stop-restart-launchd-services-from-the-command-line)
