# [一文讲解Windows/Linux/MacOS平台设置定时执行python脚本 - 知乎](https://zhuanlan.zhihu.com/p/281225054?utm_source=cn.ticktick.task&utm_medium=social&utm_oi=38400975437824)

![一文讲解Windows/Linux/MacOS平台设置定时执行python脚本](https://pic1.zhimg.com/v2-e3ab62890044dc6b4a0d10d28af32a1f_1440w.jpg?source=172ae18b)

在深度学习调参的过程中，比如凌晨几点把代码跑在服务器上，定时发送邮件给邮箱。或者日常使用一些小工具的时候，经常需要定时执行一些任务，python无疑是最好的选择。

一. Windows 平台（以Windows10为例）

1.  点击开始-搜索任务计划程序-点击进去

![](https://pic3.zhimg.com/v2-0fc271b2715fd309e3ebef20803a234e_b.jpg)

2\. 右键任务计划程序库-创建任务

![](https://pic2.zhimg.com/v2-195251d13b9e0ba3aa34e8036a490875_b.jpg)

3\. 填写任务的名称和概述信息

![](https://pic1.zhimg.com/v2-60a115a2b3af292bb50e941677510bbc_b.jpg)

4\. 填写任务的触发器信息，这里是设置定时的时间的设定

![](https://pic1.zhimg.com/v2-9f5583367782e01d98230cb4264161d0_b.jpg)

5.填写定时任务的操作，这里选择python脚本的位置即可

![](https://pic3.zhimg.com/v2-44457cdda88b50c64b6f11c6651b246a_b.jpg)

二. Linux平台

实现linux定时任务有：cron、anacron、at,使用最多的是cron任务

1.  安装：

```
yum -y install vixie-cron
yum -y install crontabs

vixie-cron 软件包是 cron 的主程序；
crontabs 软件包是用来安装、卸装、或列举用来驱动 cron 守护进程的表格的程序。
```

2\. 配置：

```
service crond start     //启动服务
service crond stop      //关闭服务
service crond restart   //重启服务
service crond reload    //重新载入配置
service crond status    //查看crontab服务状态
```

3\. 运行定时任务

```
*   *　 *　 *　 *　　command
分　时　日　月　周　 命令
```

4\. 格式说明

![](https://pic2.zhimg.com/v2-d682b17cb3828ea2c6af0ab23e0cae11_b.jpg)

三. MacOS平台

MacOS平台也可以使用crontab，和Linux差不多，不过要记得给脚本加可执行的权限。

这里另外介绍一下MacOS的launchctl

1.  编写plist

![](https://pic2.zhimg.com/v2-d6a8482a11e54b67129c2f7956a1f321_b.jpg)

2\. 将plist放入该目录下

3\. 相关命令

```
添加： launchctl load /System/Library/LaunchAgents/com.test.plist
移除： launchctl unload /System/Library/LaunchAgents/com.test.plist
查看： launchctl list 
立即执行任务：launchctl start  com.aigo.launchctl.plist
停止执行任务：launchctl stop   com.aigo.launchctl.plist
```

参考文档：

> [Mac定时执行脚本\_服务launchctl - Da雪山 - 博客园](https://www.cnblogs.com/daxueshan/p/11120947.html)  
> [Mac电脑使用Launchctl定时执行脚本\_sunflowerph的博客-CSDN博客](https://blog.csdn.net/sunflowerph/article/details/105411715 "垃圾中文技术性网站")  
> [windows中怎么添加定时任务 - 秋寻草 - 博客园](https://www.cnblogs.com/gcgc/p/11594467.html)  
> [linux定时任务cron配置 - 舒艾青 - 博客园](https://www.cnblogs.com/shuaiqing/p/7742382.html)

发布于 2020-11-09 16:46

### 文章被以下专栏收录

[

![python](https://pic2.zhimg.com/4b70deef7_xs.jpg?source=172ae18b)



](https://www.zhihu.com/column/c_1308720740120551424)

Drag to outliner or Upload

Close
