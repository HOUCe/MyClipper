# 快捷键 Mac也能快速锁屏 快速启动屏幕保护 - Automator 小教程

[www.macx.cn](https://www.macx.cn/thread-2133104-1-1.html)

Chrome 35.0.1916.153 Mac OS X 10.10.0

如果是刚刚从 Windows 转换到 Mac 上的用户有一个很不方便的事儿就是很难快速的锁定屏幕. 之前在 Windows 用的很爽的 Windows Key + L 已经不起作用了.-
-
-
1\. 设置触发角-
首先, 如果你需要快速启动屏保或者锁定屏幕的话可以设置屏幕触发角.-
打开系统偏好设置 - 桌面与屏幕保护程序内. 设置触发角, 当你的鼠标移动到屏幕的四角中你设置号的角落的时候就会激活屏保.-
-
![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimages.macx.cn%2Fforum%2F201407%2F14%2F0003003tgb3pipktny9arr.png "1-1.png")

14-7-14 00:03:00 上传

[**下载附件** (528.5 KB)](https://www.macx.cn/forum.php?mod=attachment&aid=NTUwMDI3fDdjM2ZhZjlkfDE2Mzg0Mzg1OTl8MHwyMTMzMTA0&nothumb=yes "1-1.png 下载次数:297")

-
-
![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimages.macx.cn%2Fforum%2F201407%2F14%2F000323h0097w9mmjy6p7aa.png "1-2.png")

14-7-14 00:03:23 上传

[**下载附件** (495 KB)](https://www.macx.cn/forum.php?mod=attachment&aid=NTUwMDI4fGY2Njk4NzA5fDE2Mzg0Mzg1OTl8MHwyMTMzMTA0&nothumb=yes "1-2.png 下载次数:312")

-
-
2\. 增加 Finder 锁屏功能-
在 Finder 菜单栏上增加锁屏的快捷键. 方法如下:-
-
-
![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimages.macx.cn%2Fforum%2F201407%2F14%2F000448cip2rig2m0638pri.png "1-3.png")

14-7-14 00:04:48 上传

[**下载附件** (77.37 KB)](https://www.macx.cn/forum.php?mod=attachment&aid=NTUwMDI5fDg1ZGU5MWQyfDE2Mzg0Mzg1OTl8MHwyMTMzMTA0&nothumb=yes "1-3.png 下载次数:298")

-
-
![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimages.macx.cn%2Fforum%2F201407%2F14%2F000550cs2qcj2jqf24p4hz.png "1-4.png")

14-7-14 00:05:50 上传

[**下载附件** (183.25 KB)](https://www.macx.cn/forum.php?mod=attachment&aid=NTUwMDMwfDE4YWZmMWI2fDE2Mzg0Mzg1OTl8MHwyMTMzMTA0&nothumb=yes "1-4.png 下载次数:372")

-
-
在 应用程序 - 实用工具 内找到 钥匙串访问, 随后打开钥匙串访问的偏好设置(快捷键 Command-逗号 )-
即可在 Finder 菜单栏上显示小锁的标志.-
-
-
-
-
-
-
-
3\. Automator 制作 服务 ,并且设置 快捷键.-
如果如上这两个依然不能改变你的使用习惯,或者你钟情于 快捷键锁屏启动屏保的话. 下面就是方法啦.-
首先 , 打开应用程序内的 Automator , 选择新创建一个 服务.-
-
![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimages.macx.cn%2Fforum%2F201407%2F14%2F000848mu9ioioo55ccd9ug.png "1-5.png")

14-7-14 00:08:48 上传

[**下载附件** (573.41 KB)](https://www.macx.cn/forum.php?mod=attachment&aid=NTUwMDMxfDFkNzE0ODNmfDE2Mzg0Mzg1OTl8MHwyMTMzMTA0&nothumb=yes "1-5.png 下载次数:338")

-
-
在左侧栏中找到 启动屏幕保护 . 将其拖拽到右侧窗口内. 并且修改 服务收到改为 " 没有输入 "-
![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimages.macx.cn%2Fforum%2F201407%2F14%2F0009229y2im9d7m6rmdmiy.png "1-6.png")

14-7-14 00:09:22 上传

[**下载附件** (355.45 KB)](https://www.macx.cn/forum.php?mod=attachment&aid=NTUwMDMyfGRlZjAzYzE0fDE2Mzg0Mzg1OTl8MHwyMTMzMTA0&nothumb=yes "1-6.png 下载次数:305")

-
并且保存, 文件名设置为 启动屏幕保护.-
保存后, 你可以通过鼠标点击访问启动屏幕保护这个服务. 就在 Finder 菜单滥觞的 服务选项中.-
-
![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimages.macx.cn%2Fforum%2F201407%2F14%2F001120aq3zpb2a82q3mbrm.png "1-7.png")

14-7-14 00:11:20 上传

[**下载附件** (517.57 KB)](https://www.macx.cn/forum.php?mod=attachment&aid=NTUwMDM0fGU1MjMxMWMwfDE2Mzg0Mzg1OTl8MHwyMTMzMTA0&nothumb=yes "1-7.png 下载次数:306")

-
-
下面就是给这个服务加上快捷键了.-
-
打开系统偏好设置- 键盘-
点击快捷键的选项卡, 里面左侧找到服务, 右侧找到刚刚创建好的 启动屏幕保护.-
点击右侧的按钮即可设置快捷键了. 这里要注意, Mac OS X 的快捷键很多. 所以不要与其他快捷键冲突为好.-
在这里设置好以后你就可以按一下看看是否立即启动了屏保了.-
![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fimages.macx.cn%2Fforum%2F201407%2F14%2F001227j5q1aq6ll6m6jza5.png "1-8.png")

14-7-14 00:12:27 上传

[**下载附件** (511.87 KB)](https://www.macx.cn/forum.php?mod=attachment&aid=NTUwMDM1fGZlNzVhZjg3fDE2Mzg0Mzg1OTl8MHwyMTMzMTA0&nothumb=yes "1-8.png 下载次数:297")

-
-
怎么样, 解决了你问题不? 请回帖支持一下哦.-

[已有 4 人评分](https://www.macx.cn/forum.php?mod=misc&action=viewratings&tid=2133104&pid=3276681 "查看全部评分")

_苹果_

[收起](javascript:;) _理由_

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D218128%26size%3Dsmall)](https://www.macx.cn/space-uid-218128.html) [kerouac623](https://www.macx.cn/space-uid-218128.html)

\+ 3

很给力!

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D605317%26size%3Dsmall)](https://www.macx.cn/space-uid-605317.html) [最爱贝贝](https://www.macx.cn/space-uid-605317.html)

\+ 2

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D460441%26size%3Dsmall)](https://www.macx.cn/space-uid-460441.html) [Jialin](https://www.macx.cn/space-uid-460441.html)

\+ 1

很给力!

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D13240%26size%3Dsmall)](https://www.macx.cn/space-uid-13240.html) [TRUEMAN](https://www.macx.cn/space-uid-13240.html)

\+ 1

很给力! 谢谢！

总评分: 苹果 + 7 [查看全部评分](https://www.macx.cn/forum.php?mod=misc&action=viewratings&tid=2133104&pid=3276681 "查看全部评分")

[_![](https://image.cubox.pro/article/2021072301081541305/81667.jpg)收藏11_](https://www.macx.cn/home.php?mod=spacecp&ac=favorite&type=thread&id=2133104)

**[_2_#](https://www.macx.cn/forum.php?mod=redirect&goto=findpost&ptid=2133104&pid=3276873 "您的朋友访问此链接后，您将获得相应的积分奖励")**

_14-7-14 10:09:36_

**[zx178032670](https://www.macx.cn/space-uid-568696.html)** _当前离线_

用户组: [☆](https://www.macx.cn/home.php?mod=spacecp&ac=usergroup&gid=10) \[无昵称\]

*   [加好友](https://www.macx.cn/home.php?mod=spacecp&ac=friend&op=add&uid=568696&handlekey=addfriendhk_568696 "加好友")
*   [黑名单](https://www.macx.cn/home.php?mod=space&do=friend&view=blacklist&uuid=568696 "黑名单")
*   [发消息](https://www.macx.cn/home.php?mod=spacecp&ac=pm&op=showmsg&handlekey=showmsg_568696&touid=568696&pmid=0&daterange=2&pid=3276873&tid=2133104 "发消息")
*   [看帖子](https://www.macx.cn/search.php?mod=forum&adv=yes&srchtxt=%20&srchuname=zx178032670&searchsubmit=yes "看帖子")

[![](https://image.cubox.pro/article/2021072104372160838/32341.jpg)](https://www.macx.cn/home.php?mod=space&uid=568696&do=profile "查看详细资料")[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fmagic%2Fcheckonline.small.gif)](https://www.macx.cn/home.php?mod=magic&mid=checkonline&idtype=user&id=zx178032670 "狗仔卡")

最后登录

15-8-27

在线时间

10 小时

赞

2

注册时间

14-2-25

积分

37

帖子

[8](https://www.macx.cn/home.php?mod=space&uid=568696&do=thread&type=reply&view=me&from=space)

精华

0

UID

568696

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D568696%26size%3Dmiddle)](https://www.macx.cn/space-uid-568696.html)

[zx178032670](https://www.macx.cn/space-uid-568696.html) ( ☆ ) ( 赞 2 )

-

Safari 8.0 Mac OS X 10.10

我喜欢直接 shift+control+推出键 关屏~~

[已有 1 人评分](https://www.macx.cn/forum.php?mod=misc&action=viewratings&tid=2133104&pid=3276873 "查看全部评分")

_苹果_

[收起](javascript:;) _理由_

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D237306%26size%3Dsmall)](https://www.macx.cn/space-uid-237306.html) [zmbjcn](https://www.macx.cn/space-uid-237306.html)

\+ 1

很给力!

总评分: 苹果 + 1 [查看全部评分](https://www.macx.cn/forum.php?mod=misc&action=viewratings&tid=2133104&pid=3276873 "查看全部评分")

**[_3_#](https://www.macx.cn/forum.php?mod=redirect&goto=findpost&ptid=2133104&pid=3276748 "您的朋友访问此链接后，您将获得相应的积分奖励")**

_14-7-14 04:43:16_

**[rossiright](https://www.macx.cn/space-uid-51794.html)** _当前离线_

用户组: [♘马上有钱](https://www.macx.cn/home.php?mod=spacecp&ac=usergroup&gid=28) \[无昵称\]

*   [加好友](https://www.macx.cn/home.php?mod=spacecp&ac=friend&op=add&uid=51794&handlekey=addfriendhk_51794 "加好友")
*   [黑名单](https://www.macx.cn/home.php?mod=space&do=friend&view=blacklist&uuid=51794 "黑名单")
*   [发消息](https://www.macx.cn/home.php?mod=spacecp&ac=pm&op=showmsg&handlekey=showmsg_51794&touid=51794&pmid=0&daterange=2&pid=3276748&tid=2133104 "发消息")
*   [看帖子](https://www.macx.cn/search.php?mod=forum&adv=yes&srchtxt=%20&srchuname=rossiright&searchsubmit=yes "看帖子")

[![](https://image.cubox.pro/article/2021072214090785770/66411.jpg)](http://rossirayright@aol.com "查看个人网站")[![](https://image.cubox.pro/article/2021072104372160838/32341.jpg)](https://www.macx.cn/home.php?mod=space&uid=51794&do=profile "查看详细资料")[![](https://image.cubox.pro/article/2021072301081522138/25925.jpg)](http://wwp.icq.com/scripts/search.dll?to=13036151217 "ICQ")[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fmagic%2Fcheckonline.small.gif)](https://www.macx.cn/home.php?mod=magic&mid=checkonline&idtype=user&id=rossiright "狗仔卡")

最后登录

20-1-8

在线时间

8680 小时

赞

26

注册时间

07-10-11

积分

9067

帖子

[3382](https://www.macx.cn/home.php?mod=space&uid=51794&do=thread&type=reply&view=me&from=space)

精华

0

UID

51794

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D51794%26size%3Dmiddle)](https://www.macx.cn/space-uid-51794.html)

[rossiright](https://www.macx.cn/space-uid-51794.html) ( ♘马上有钱 ) ( 赞 26 )

[](https://www.macx.cn/home.php?mod=medal)[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fcommon%2Fhz%2Fhz10.png)](https://www.macx.cn/home.php?mod=medal "十周年")

-

Safari 7.0.5 Mac OS X 10.9.4

用触发角不是更快吗？？？

**[_4_#](https://www.macx.cn/forum.php?mod=redirect&goto=findpost&ptid=2133104&pid=3277417 "您的朋友访问此链接后，您将获得相应的积分奖励")**

_14-7-14 20:09:07_

**[Joshyangg](https://www.macx.cn/space-uid-603925.html)** _当前离线_

用户组: [☆☆☆☆](https://www.macx.cn/home.php?mod=spacecp&ac=usergroup&gid=13) \[无昵称\]

*   [加好友](https://www.macx.cn/home.php?mod=spacecp&ac=friend&op=add&uid=603925&handlekey=addfriendhk_603925 "加好友")
*   [黑名单](https://www.macx.cn/home.php?mod=space&do=friend&view=blacklist&uuid=603925 "黑名单")
*   [发消息](https://www.macx.cn/home.php?mod=spacecp&ac=pm&op=showmsg&handlekey=showmsg_603925&touid=603925&pmid=0&daterange=2&pid=3277417&tid=2133104 "发消息")
*   [看帖子](https://www.macx.cn/search.php?mod=forum&adv=yes&srchtxt=%20&srchuname=Joshyangg&searchsubmit=yes "看帖子")

[![](https://image.cubox.pro/article/2021072104372160838/32341.jpg)](https://www.macx.cn/home.php?mod=space&uid=603925&do=profile "查看详细资料")[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fmagic%2Fcheckonline.small.gif)](https://www.macx.cn/home.php?mod=magic&mid=checkonline&idtype=user&id=Joshyangg "狗仔卡")

最后登录

21-9-20

在线时间

1756 小时

赞

14

注册时间

14-7-11

积分

2782

帖子

[797](https://www.macx.cn/home.php?mod=space&uid=603925&do=thread&type=reply&view=me&from=space)

精华

0

UID

603925

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D603925%26size%3Dmiddle)](https://www.macx.cn/space-uid-603925.html)

[Joshyangg](https://www.macx.cn/space-uid-603925.html) ( ☆☆☆☆ ) ( 赞 14 )

[](https://www.macx.cn/home.php?mod=medal)[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fcommon%2Fhz%2Fhz10.png)](https://www.macx.cn/home.php?mod=medal "十周年")

-

Safari 8.0 Mac OS X 10.10

> lucas 发表于 14-7-14 10:17 [![](https://image.cubox.pro/article/2021072309012828482/86780.jpg)](https://www.macx.cn/forum.php?mod=redirect&goto=findpost&pid=3276886&ptid=2133104) -
> 现在的新机器没有推出键了

-
表示 电源键 跟 推出键 是一样的. 按住 control shift 推出 就能睡屏了

**[_5_#](https://www.macx.cn/forum.php?mod=redirect&goto=findpost&ptid=2133104&pid=3276745 "您的朋友访问此链接后，您将获得相应的积分奖励")**

_14-7-14 04:01:59_

**[msnguo](https://www.macx.cn/space-uid-214043.html)** _当前离线_

用户组: [☆☆☆☆](https://www.macx.cn/home.php?mod=spacecp&ac=usergroup&gid=13) \[无昵称\]

*   [加好友](https://www.macx.cn/home.php?mod=spacecp&ac=friend&op=add&uid=214043&handlekey=addfriendhk_214043 "加好友")
*   [黑名单](https://www.macx.cn/home.php?mod=space&do=friend&view=blacklist&uuid=214043 "黑名单")
*   [发消息](https://www.macx.cn/home.php?mod=spacecp&ac=pm&op=showmsg&handlekey=showmsg_214043&touid=214043&pmid=0&daterange=2&pid=3276745&tid=2133104 "发消息")
*   [看帖子](https://www.macx.cn/search.php?mod=forum&adv=yes&srchtxt=%20&srchuname=msnguo&searchsubmit=yes "看帖子")

[![](https://image.cubox.pro/article/2021072104372160838/32341.jpg)](https://www.macx.cn/home.php?mod=space&uid=214043&do=profile "查看详细资料")[![](https://image.cubox.pro/article/2021072301081522138/25925.jpg)](http://wwp.icq.com/scripts/search.dll?to=13701717016 "ICQ")[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fmagic%2Fcheckonline.small.gif)](https://www.macx.cn/home.php?mod=magic&mid=checkonline&idtype=user&id=msnguo "狗仔卡")

最后登录

21-11-25

在线时间

3278 小时

赞

0

注册时间

11-4-30

积分

3316

帖子

[626](https://www.macx.cn/home.php?mod=space&uid=214043&do=thread&type=reply&view=me&from=space)

精华

0

UID

214043

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D214043%26size%3Dmiddle)](https://www.macx.cn/space-uid-214043.html)

[msnguo](https://www.macx.cn/space-uid-214043.html) ( ☆☆☆☆ )

[](https://www.macx.cn/home.php?mod=medal)[![](https://image.cubox.pro/article/2021072214090735706/51852.jpg)](https://www.macx.cn/home.php?mod=medal "wwdc2016")

-

Safari 7.0 IPad

很棒的教程

**[_6_#](https://www.macx.cn/forum.php?mod=redirect&goto=findpost&ptid=2133104&pid=3276768 "您的朋友访问此链接后，您将获得相应的积分奖励")**

_14-7-14 08:16:40_

**[hn8605](https://www.macx.cn/space-uid-571562.html)** _当前离线_

用户组: [☆☆](https://www.macx.cn/home.php?mod=spacecp&ac=usergroup&gid=11) \[无昵称\]

*   [加好友](https://www.macx.cn/home.php?mod=spacecp&ac=friend&op=add&uid=571562&handlekey=addfriendhk_571562 "加好友")
*   [黑名单](https://www.macx.cn/home.php?mod=space&do=friend&view=blacklist&uuid=571562 "黑名单")
*   [发消息](https://www.macx.cn/home.php?mod=spacecp&ac=pm&op=showmsg&handlekey=showmsg_571562&touid=571562&pmid=0&daterange=2&pid=3276768&tid=2133104 "发消息")
*   [看帖子](https://www.macx.cn/search.php?mod=forum&adv=yes&srchtxt=%20&srchuname=hn8605&searchsubmit=yes "看帖子")

[![](https://image.cubox.pro/article/2021072104372160838/32341.jpg)](https://www.macx.cn/home.php?mod=space&uid=571562&do=profile "查看详细资料")[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fmagic%2Fcheckonline.small.gif)](https://www.macx.cn/home.php?mod=magic&mid=checkonline&idtype=user&id=hn8605 "狗仔卡")

最后登录

18-1-20

在线时间

369 小时

赞

0

注册时间

14-3-5

积分

312

帖子

[23](https://www.macx.cn/home.php?mod=space&uid=571562&do=thread&type=reply&view=me&from=space)

精华

0

UID

571562

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D571562%26size%3Dmiddle)](https://www.macx.cn/space-uid-571562.html)

[hn8605](https://www.macx.cn/space-uid-571562.html) ( ☆☆ )

-

Safari 7.0.5 Mac OS X 10.9.4

用触发角的表示看的很累啊

**[_7_#](https://www.macx.cn/forum.php?mod=redirect&goto=findpost&ptid=2133104&pid=3276784 "您的朋友访问此链接后，您将获得相应的积分奖励")**

_14-7-14 08:51:33_

**[123qq1](https://www.macx.cn/space-uid-460635.html)** _当前离线_

用户组: [⭐](https://www.macx.cn/home.php?mod=spacecp&ac=usergroup&gid=31) \[无昵称\]

*   [加好友](https://www.macx.cn/home.php?mod=spacecp&ac=friend&op=add&uid=460635&handlekey=addfriendhk_460635 "加好友")
*   [黑名单](https://www.macx.cn/home.php?mod=space&do=friend&view=blacklist&uuid=460635 "黑名单")
*   [发消息](https://www.macx.cn/home.php?mod=spacecp&ac=pm&op=showmsg&handlekey=showmsg_460635&touid=460635&pmid=0&daterange=2&pid=3276784&tid=2133104 "发消息")
*   [看帖子](https://www.macx.cn/search.php?mod=forum&adv=yes&srchtxt=%20&srchuname=123qq1&searchsubmit=yes "看帖子")

[![](https://image.cubox.pro/article/2021072104372160838/32341.jpg)](https://www.macx.cn/home.php?mod=space&uid=460635&do=profile "查看详细资料")[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fmagic%2Fcheckonline.small.gif)](https://www.macx.cn/home.php?mod=magic&mid=checkonline&idtype=user&id=123qq1 "狗仔卡")

最后登录

21-12-2

在线时间

2731 小时

赞

16

注册时间

13-2-2

积分

5111

帖子

[1567](https://www.macx.cn/home.php?mod=space&uid=460635&do=thread&type=reply&view=me&from=space)

精华

0

UID

460635

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D460635%26size%3Dmiddle)](https://www.macx.cn/space-uid-460635.html)

[123qq1](https://www.macx.cn/space-uid-460635.html) ( ⭐ ) ( 赞 16 )

[](https://www.macx.cn/home.php?mod=medal)[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fcommon%2Fhz%2Fhz10.png)](https://www.macx.cn/home.php?mod=medal "十周年")

-

Chrome 35.0.1916.114 Windows 7

有一个运行屏幕保护之后上密码的选项啊，安全里面

**[_8_#](https://www.macx.cn/forum.php?mod=redirect&goto=findpost&ptid=2133104&pid=3276803 "您的朋友访问此链接后，您将获得相应的积分奖励")**

_14-7-14 09:11:17_

**[carlchou00](https://www.macx.cn/space-uid-354733.html)** _当前离线_

用户组: [♘马上有钱](https://www.macx.cn/home.php?mod=spacecp&ac=usergroup&gid=28) \[无昵称\]

*   [加好友](https://www.macx.cn/home.php?mod=spacecp&ac=friend&op=add&uid=354733&handlekey=addfriendhk_354733 "加好友")
*   [黑名单](https://www.macx.cn/home.php?mod=space&do=friend&view=blacklist&uuid=354733 "黑名单")
*   [发消息](https://www.macx.cn/home.php?mod=spacecp&ac=pm&op=showmsg&handlekey=showmsg_354733&touid=354733&pmid=0&daterange=2&pid=3276803&tid=2133104 "发消息")
*   [看帖子](https://www.macx.cn/search.php?mod=forum&adv=yes&srchtxt=%20&srchuname=carlchou00&searchsubmit=yes "看帖子")

[![](https://image.cubox.pro/article/2021072104372160838/32341.jpg)](https://www.macx.cn/home.php?mod=space&uid=354733&do=profile "查看详细资料")[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fmagic%2Fcheckonline.small.gif)](https://www.macx.cn/home.php?mod=magic&mid=checkonline&idtype=user&id=carlchou00 "狗仔卡")

最后登录

21-11-24

在线时间

2049 小时

赞

7

注册时间

12-6-2

积分

7163

帖子

[3403](https://www.macx.cn/home.php?mod=space&uid=354733&do=thread&type=reply&view=me&from=space)

精华

0

UID

354733

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D354733%26size%3Dmiddle)](https://www.macx.cn/space-uid-354733.html)

[carlchou00](https://www.macx.cn/space-uid-354733.html) ( ♘马上有钱 ) ( 赞 7 )

[](https://www.macx.cn/home.php?mod=medal)[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fcommon%2Fhz%2Fhz10.png)](https://www.macx.cn/home.php?mod=medal "十周年") [![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fcommon%2Fhz%2Fzuiaishafa.gif)](https://www.macx.cn/home.php?mod=medal "沙发达人")

-

MSIE 8.0 XP

触发角容易误操作-
还是automator好用，谢谢大妈的教程

**[_9_#](https://www.macx.cn/forum.php?mod=redirect&goto=findpost&ptid=2133104&pid=3276886 "您的朋友访问此链接后，您将获得相应的积分奖励")**

_14-7-14 10:17:09_ [发自iPhone客户端](https://itunes.apple.com/app/mai-ke-cha/id705708933?ls=1&mt=8)

**[lucas](https://www.macx.cn/space-uid-68.html)** _当前离线_

用户组: [⭐](https://www.macx.cn/home.php?mod=spacecp&ac=usergroup&gid=31) \[无昵称\]

*   [加好友](https://www.macx.cn/home.php?mod=spacecp&ac=friend&op=add&uid=68&handlekey=addfriendhk_68 "加好友")
*   [黑名单](https://www.macx.cn/home.php?mod=space&do=friend&view=blacklist&uuid=68 "黑名单")
*   [发消息](https://www.macx.cn/home.php?mod=spacecp&ac=pm&op=showmsg&handlekey=showmsg_68&touid=68&pmid=0&daterange=2&pid=3276886&tid=2133104 "发消息")
*   [看帖子](https://www.macx.cn/search.php?mod=forum&adv=yes&srchtxt=%20&srchuname=lucas&searchsubmit=yes "看帖子")

[![](https://image.cubox.pro/article/2021072104372160838/32341.jpg)](https://www.macx.cn/home.php?mod=space&uid=68&do=profile "查看详细资料")[![](https://image.cubox.pro/article/2021072301081586611/88297.jpg)](http://wpa.qq.com/msgrd?V=1&Uin=289664513&Site=MacX&Menu=yes "QQ")[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fstatic%2Fimage%2Fmagic%2Fcheckonline.small.gif)](https://www.macx.cn/home.php?mod=magic&mid=checkonline&idtype=user&id=lucas "狗仔卡")

最后登录

21-6-18

在线时间

9647 小时

赞

169

注册时间

05-5-8

积分

34159

帖子

[16614](https://www.macx.cn/home.php?mod=space&uid=68&do=thread&type=reply&view=me&from=space)

精华

2

UID

68

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.macx.cn%2Fu%2Favatar.php%3Fuid%3D68%26size%3Dmiddle)](https://www.macx.cn/space-uid-68.html)

[lucas](https://www.macx.cn/space-uid-68.html) ( ⭐ ) ( 赞 169 )

-

?

> zx178032670 发表于 14-7-14 10:10 [![](https://image.cubox.pro/article/2021072309012828482/86780.jpg)](https://www.macx.cn/forum.php?mod=redirect&amp;goto=findpost&amp;pid=10&amp;ptid=6) 我喜欢直接 shift+control+推出键关屏~~

-
现在的新机器没有推出键了

**[_10_#](https://www.macx.cn/forum.php?mod=redirect&goto=findpost&ptid=2133104&pid=3276915 "您的朋友访问此链接后，您将获得相应的积分奖励")**

_14-7-14 10:38:37_

JayHangyu _该用户已被删除_

-

提示: _作者被禁止或删除 内容自动屏蔽_

[查看原网页: www.macx.cn](https://www.macx.cn/thread-2133104-1-1.html)