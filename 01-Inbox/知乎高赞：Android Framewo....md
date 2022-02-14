# 知乎高赞：Android Framework如何学习，如何从应用深入到Framework？

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650840810&idx=1&sn=f3dbf250d356698895c819103c11a17f&chksm=80b74a74b7c0c3621539f2dff260d13e5ce55dd4f45c9dac859bad4376d4300b5a43f24260ee&mpshare=1&scene=1&srcid=1207UGZldVQOzRZuznxkbeTb&sharer_sharetime=1638828272766&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)鸿洋

**Framework 知识广泛应用在Android各个领域中，重要性显而易见。**-

**Framework始终穿插在 App 整个研发生命周期中，不管是从 0 到 1 的建立阶段，还是从 1 到 N 打磨阶段，都离不开Framework。**成为一名Android Framework高手，也是目前招聘过程中非常稀缺的人才，可以成为你的敲门砖。

下面这张图想必大家都看过，Google官方提供过一张经典的平台架构图，从下往上依次分为：Linux内核、硬件抽象层、Native层、Java Framework层、App层，每一层都包含大量的子模块或子系统。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FMOu2ZNAwZwO92aJYbALiaCB83IVY7T8OBqQKlYEEeWco0ic4KISiaHib4a0UJRRG6PZCKRW1ticoDkTwpygMIyibX5oQ%2F640%3Fwx_fmt%3Dpng)

可以看到具体app的下面就是Framework层的支撑。所以掌握Framework层非常有助于我们开发出一个性能良好的App，另外在大厂的面试过程中，Framework也是高阶面试时必问的问题：-

例如:

1\. 为什么Zygote通信fork进程，使用的是socket，而不是Android的Binder？-

2\. 为什么是从zygote进程fork App，而不是其他进程？

3\. Binder在做数据传输过程中，最大的数据量限制是多少？

4\. 打开一个Activity的过程中经历过几次跨进程调用？

5\. ANR弹框的原理是什么？

等等这类问题却在大厂面试中经常问到。

在所有的Framework知识中，要数最重要的还是AMS，主打和Activity,Service,ContentProvider,Broadcast等交互：-

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2F9Ymo5qnwPibM8XROxluu5KYL9P54Q3HGWdor8yqHqR3A0ibRykRk3mRVt50nLneqe8LFNxhRicLLkibGNEf50Rpaaw%2F640%3Fwx_fmt%3Dpng)-

看一下上图，Activity启动，涉及到ActivityThread,AMS,H类，上述过程还涉及到多次跨进程调用，涉及到各种binder的知识。-

搞清楚这些：我们就可以去研究各种黑科技，例如在做插件化的时候，你需要占坑Activity等，hook代码等都是在和AMS斗智斗勇；在做性能优化的时候，你也要了解AMS是如何调度Activity的，消息队列是如何运转的；

不少开发者也只是有了解到一些入门级的概念，比如：

1.  init进程是android系统中用户空间的第一个进程。
    
2.  Zygote进程由init进程孵化而来。同时init进程会孵化出ServiceManager。（Binder 相关）
    
3.  Zygote进程是Android系统的第一个Java进程(即虚拟机进程)，Zygote是所有Java进程的父进程。
    
4.  System Server是由Zygote fork而来。（Android中最重要的两个进程 Zygote，SystemServer）
    
5.  我们的AMS，PMS，WMS都是SystemServer进程中的一个线程。
    
6.  Launcher进程是我们App的第一个进程。（桌面）
    

比如下面这张Android启动流程图，不少人都看过，但少有人沉下心去仔仔细细的研究过。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2F9Ymo5qnwPibPelrFwKtb2ov0aBrPVbicdTmayAz0JTHRd7SDweHaj9VYwFxkQPI4c51wZwvVDDO2KuqMeORLxWyg%2F640%3Fwx_fmt%3Dpng)

当然大多数开发者更多的还是在做业务开发，对于Framework基本停留在“不够了解”的阶段，其中不乏一些工作多年的 Android 工程师。

如果想要精进，**不仅要对底层原理充分了解，还要知道如何利用Framework知识指导我们代码实践开发**，像Android App 的启动机制、AMS、PMS、WMS、Handler、Binder等...

这样才能够真正说得上是精通Framwork。

精通意味着：

1.  首先在大厂面试环节，Framework是必问项，你可以展示出个人实力；
    
2.  一旦你进入大厂，对Framework了解越多，你能够做的事情就越多，产出也会越多，而且可以持续不断的去做。
    

**作为过来人，发现很多学习者和实践者都在 Android Framework上面临着很多的困扰，比如：**

*   工作场景中遇到难题，**往往只能靠盲猜和感觉**，用临时性的补救措施去掩盖，看似解决了问题，但下次同样的问题又会发作，**原因则是缺乏方法论、思路的指引以及工具支持**；
    
*   能力修炼中，缺乏互联网项目这一实践环境，**对Framework只能通过理论知识进行想象，无法认识其在工作实战中的真实面目和实操过程**；
    
*   职场晋升中，只管功能开发，不了解底层原理，**缺少深入地思考与总结，无法完成复杂系统设计**这类高阶工作，难以在工作中大展拳脚，而有挑战的工作往往留给有准备的人。
    

**总之，一旦遇到问题，很少人能够由点及面逆向分析**，最终找到瓶颈点和最优解决方案，**而Framework是Android开发的深水区，也是衡量一个Android程序员能力高低的标准**。

如果你还没有掌握Framework，现在想要在最短的时间里吃透它，那么必须要跟着真正有实力的大佬一起学习！

这里特别邀请**腾讯T12级专家 Jett**，为大家带来《**Android系统运行流程与AMS源码实战**》系列直播分享。**帮助大家****深刻理解Android系统运行流程与AMS特性，****掌握其中原理，带你解决日常项目开发过程中的各种问题。**

**原价298元**的**《Android系统运行流程与AMS源码实战》**训练营，现在报名**限时免费**（**限量100个名额**）。

****大家手速要快，赶紧识别下方图中二维码加入学习！****

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2FMOu2ZNAwZwOnM9mXYFDs991BAm6w00vJYA4AQa6VawQvtJMiagRdbMez88QGM1w2h8YGic3Ar0NJ55TeyRVFxEXA%2F640%3Fwx_fmt%3Djpeg)

******报名学习后还将附赠一套系统的Android开发进阶资料，帮助大家在技术的道路上更进一步。******-

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2F9Ymo5qnwPibPIibNiaMLzwynf4MEl1yCEQsJkETF47p1y1nAcoygziajWa1xh45Cqy4IbtFMzovlNeuVSkDwRsZI1Q%2F640%3Fwx_fmt%3Dpng)

**上述所有内容全部随课程附赠！**

**赶紧扫码报名获取资料，开启你的学习之旅！**

**（扫码添加时记得备注："AMS"快速通过）**

**【如遇扫码频繁+VX：mm1591314250】**

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2FMOu2ZNAwZwOnM9mXYFDs991BAm6w00vJwEoliaeTONoSeZPBNoU94Zt7gUNUE7BnvmBX1xBkf5G6x8N8OSPNpVQ%2F640%3Fwx_fmt%3Djpeg)

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650840810&idx=1&sn=f3dbf250d356698895c819103c11a17f&chksm=80b74a74b7c0c3621539f2dff260d13e5ce55dd4f45c9dac859bad4376d4300b5a43f24260ee&mpshare=1&scene=1&srcid=1207UGZldVQOzRZuznxkbeTb&sharer_sharetime=1638828272766&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)