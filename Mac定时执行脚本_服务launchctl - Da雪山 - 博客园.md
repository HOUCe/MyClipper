# [Mac定时执行脚本_服务launchctl - Da雪山 - 博客园](https://www.cnblogs.com/daxueshan/p/11120947.html)

**Mac 设置自动执行定时任务, 步骤:**

1\. 编写plist

![](https://img2018.cnblogs.com/blog/855712/201907/855712-20190702152108307-1351297736.png) 

 2.将plist放入该目录下

~/Library/LaunchAgents 

3.命令启动

添加： launchctl load /System/Library/LaunchAgents/com.test.plist
移除： launchctl unload /System/Library/LaunchAgents/com.test.plist
查看： launchctl list 
立即执行任务：launchctl start  com.aigo.launchctl.plist
停止执行任务：launchctl stop   com.aigo.launchctl.plist

参考:

好文要顶 关注我 收藏该文 ![](https://common.cnblogs.com/images/icon_weibo_24.png) ![](https://common.cnblogs.com/images/wechat.png)

[![](https://pic.cnblogs.com/face/855712/20151211173116.png)](https://home.cnblogs.com/u/daxueshan/)

[Da雪山](https://home.cnblogs.com/u/daxueshan/)  
[关注 - 3](https://home.cnblogs.com/u/daxueshan/followees/)  
[粉丝 - 32](https://home.cnblogs.com/u/daxueshan/followers/)

+加关注

0

0

[«](https://www.cnblogs.com/daxueshan/p/11116475.html) 上一篇： [xcodebuild自动打包上传到蒲公英的shell脚本](https://www.cnblogs.com/daxueshan/p/11116475.html "发布于 2019-07-01 20:14")  
[»](https://www.cnblogs.com/daxueshan/p/11131239.html) 下一篇： [iOS OpenGL ES简单绘制三角形](https://www.cnblogs.com/daxueshan/p/11131239.html "发布于 2019-07-04 11:13")

posted @ 2019-07-02 15:25  [Da雪山](https://www.cnblogs.com/daxueshan/)  阅读(1149)  评论(0)  [编辑](https://i.cnblogs.com/EditPosts.aspx?postid=11120947)  收藏  举报
