# 干货！Android 各大版本的差异(安卓4+版本)

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzIyNTY4NjU0OQ==&mid=2247512288&idx=3&sn=7316dbbd5211eeccb8a61afe0758fbee&chksm=e879159adf0e9c8c7d1c41af498bda68e4355f69d8b36fdb1fbbce49f621081a05ac74359b1b&mpshare=1&scene=1&srcid=1223zHA8jxm2KyfGbHTEOyzE&sharer_sharetime=1640227598662&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)点击关注 👉 秦子帅

每次去面试，面试官或多或少都会问到这问题，所以，我百度一番，大致总结一下我找到的结果。

在安卓4以前的版本不作为讨论对象，在安卓4之前的版本，谷歌一度想闭源安卓，可惜失败了，而且安卓那时的开放性不高，可设计性也不高。而且手机普及性不高，流量少，市场趋势还没有趋向移动端发展。

**一、安卓4.X**

1、引入“Holo”界面，在设计追求简约上面充满了浓浓的工程师风格，慢慢脱离苹果风格，而且在往后版本中也开始注重对界面的设计。

2、重新恢复开源，第三方刷机包开始变多。

**二、安卓5.X**

这是一个里程碑的版本

1、“Material Design”中文名 材料设计，安卓界面开发采用卡片化，扁平化，在原来的XY轴的基础上添加Z轴的设计理念。

2、添加更多类型的传感器。

3、添加卡片显示的后台进程查看

4、添加通知栏浮动通知

5、添加了新的摄影技巧以及虚拟摄像机API，为开发者提供更丰富的摄像头控制

6、Android运行时由Android核心库集和Dalvike虚拟机改成Android核心库集和ART。两者的区别就是Dalvike虚拟机采用了一种被称为JIT（just-in-time）的解释器进行动态编译,而ART模式则在用户安装App是进行预编译AOT（Ahead-of-time）。将android5.X的运行速度提高了3倍左右。

**三、安卓6.X**

1、动态权限的出现，这是对安卓开发最大变化。

2、Doze电量管理功能，在“Doze”模式下，手机会在一段时间未检测到移动时，让应用休眠清杀后台进程减少功耗，谷歌表示，当屏幕处于关闭状态，平均续航时间提高30%，这个区别于IOS的墓碑机制。在安卓开发，需要后台运行时，最好在前台留有进程，防止被误杀。

3、从Android6.X起，Ecilpse ADT不再更新支持Android开发。

4、谷歌正式将指纹识别加入系统底层，开发相关的API，加大指纹开发的安全性。

5、谷歌还加入了Android Pay进一步强化移动支付，同时也是为了对抗Apple Pay。

**四、安卓7.X**

1、原生的分屏模式的加入

2、Doze电量管理的优化

3、更便捷的通知栏，自动将多条通知合并。

4、引入了全新的VulkanAPI 图形处理器API，可以大幅减少系统动画对CPU的占用。

5、支持app应用签名v2的打包方式（在AS2.2后，在打包签名应用时，可勾选jar打包（v1）和全应用打包（v2），详情自行百度）

**五、安卓8.X**

1、安装未知来源的第三方开关被移出，变成了每次安装未知的第三方都要手动授权。

2、通知功能的改变，应用收到通知时，会在应用的右上角显示一个红点，长按会跳出一个弹出菜单。

3、画中画功能的加入。

4、支持自动填写的功能。

**六、Android P（预览版）**

1、WIFI RTT进行室内高精度定位。

2、对凹口屏幕的支持，提供API供开发者开发。

3、对多摄像头的开发支持。

4、处理图像解码，提供ImageDecoder替换原来BitmapFactory

5、加大了对Kotlin的支持，对编译器进行优化

**七、Android Pie（正式版）**

1、动态电量变化。利用机器学习技术对系统资源进行有限分配。

2、文本识别与Smart Linkify

利用机器学习模型，能够识别出类似日期或者航班这样的信息。此外，Smart Linkify还允许开发者通过Linkify API使用文本识别模块完成多项操作。

3、新增神经网络API1.1

增加了9个新算子的支持，分别是Pad、BatchToApaceND、SpaceToBatchND、TransPose、Strided Slice、Mean、DIv、Sub和Squeeze。

4、凹口屏的支持

5、增加文本放大镜

6、默认使用HTTPS

7、隐私权限的优化

8、通过WI-FI RTT室内定位

**八、android Q Bate**

1、加入“黑暗模式”，暗黑模式适用于任何地方，如果应用不支持暗黑模式，那么系统将自动设置一个暗黑模式。这个功能看来是民心所向，再也不用当心晚上玩手机伤眼了。

2、对权限开发放做了进一步限制，在权限管理加多了一个“仅运行时权限”选项，即当应用在退到后台时关闭相应的权限。

3、不允许从后台获得剪切板的内容。

Android Q 增加了名为“READ\_CLIPBOARD\_IN\_BACKGROUND”的新权限。顾名思义，新的权限将阻止随机的后台应用程序访问剪贴板内容。

4、截图都要带刘海

所有自带圆角、黑边和刘海的屏幕截图在Android Q Beta 1 会在截屏后根据设备屏幕切割状态自动裁剪截图形状，让最终截屏效果更加接近真实观感

5、修改了媒体和图形相关部分的代码

https://developer.android.google.cn/preview/features#media

以上有部分内容是借鉴其他博客，出于整理的目的进行摘录，今后新版本面世，进行继续补充！！

**\- END -**

 

 

 

 **【IT搬砖圈】各种黑科技，IT技术博文！** 

 

![](https://image.cubox.pro/article/2021111821555841253/83502.jpg)

 

 

**（更多精彩值得期待……）**

 

## ![](https://image.cubox.pro/article/2021071922493383101/71760.jpg "音符")

 

 

 

##  

 

**推荐我的微信号**

 

 

![](https://image.cubox.pro/article/2021091017551750074/76931.jpg)

 

 

 **加微信送Android，Java,Python资料****，交流学习** 

 

##  

 

我的视频号，已经录制了几十期,有工具推荐，有教育知识、副业赚钱但都是原创用心输出。-

 

## 

![](https://image.cubox.pro/article/2021091017551750013/79339.jpg)

 

 

 

让我知道你在看

 

 

 ![](https://image.cubox.pro/article/2021091017551737567/60935.jpg) 

 

 

 

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzIyNTY4NjU0OQ==&mid=2247512288&idx=3&sn=7316dbbd5211eeccb8a61afe0758fbee&chksm=e879159adf0e9c8c7d1c41af498bda68e4355f69d8b36fdb1fbbce49f621081a05ac74359b1b&mpshare=1&scene=1&srcid=1223zHA8jxm2KyfGbHTEOyzE&sharer_sharetime=1640227598662&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)