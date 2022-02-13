# [mac版QQ聊天信息备份与导出方法 - 简书](https://www.jianshu.com/p/5b12d1aa60d3)

[![](https://upload.jianshu.io/users/upload_avatars/99517/234abbfc-9136-443f-a7d4-bf91b95bd685.PNG?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/png)](https://www.jianshu.com/u/893e02c03bbc)

0.1882018.05.12 23:43:49字数 546阅读 17,032

### 前言

最近，我司终于更换新电脑的计划落实啦！！！

> Mac mini 3.0GHz 双核 Intel Core i7 处理器 (Turbo Boost 高达 3.5GHz)  
> 16GB 1600MHz LPDDR3 SDRAM  
> 1TB 融合硬盘  
> Intel Iris Graphics 图形处理器

非常值的可贺！然而，就是新电脑，一切都是新！一切都是白！！非常多工具的数据需要迁移，开发环境需要配置，最近也打算总结一下新电脑配置方面的文章，作为自己备份，或者给新手参考，相信有很大帮助。今天就先从QQ说起吧~

### 正题

说回来，因为QQ内容是工作的主要记录，所以，

#### 企业QQ聊天内容迁移

将下面目录：

```
/Users/用户名/Library/Containers/com.tencent.eimmac/Data/Library/Application Support/QQ 
```

复制目录下所有内容到新电脑，就可以啦!

#### 用户版QQ聊天内容迁移

将下面目录：

```
/Users/用户名/Library/Containers/com.tencent.qq/Data/Library/Application Support/QQ/
```

复制目录下所有内容到新电脑，

如果需要把聊天中的图片也迁移，就需要在复制目录：

```
/Users/用户名/Library/Containers/com.tencent.qq/Data/Documents/
```

注意：

-   上面目录中 `用户名` 是你电脑的账户名
-   企业QQ是在com.tencent.eimmac目录下，而用户版QQ是在 com.tencent.qq 下

#### 授鱼&授渔

如果是其它功能的内容迁移，道理相似，把对应的软件的目录的内容复制到新电脑就可以了。当然，想方便查看软件的目录备份内容，可以用 CleanMyMac 卸载器 查看：

![](https://upload-images.jianshu.io/upload_images/99517-6c67db109fd07a8d.png)

20180508-CleanMyMac\_unloader.png

### 总结

作为程序员，越来越觉得云端的好处，迁移数据是一件痛苦（辛苦）的事件，如果是电脑小白，那更加是的。所以，有必要作一些更好的方法，比如云端备份软件的配置，用脚本来操作迁移过程，因为不是经常性换电脑，所以这个就不作进一步实践了。

-   如有疑问，欢迎在评论区一起讨论！
-   如有不正确的地方，欢迎指导！

> 注：本文首发于 [iHTCboy's blog](https://link.jianshu.com/?t=http%3A%2F%2FiHTCboy.com)，如若转载，请注来源

更多精彩内容，就在简书APP

"不劳而获，感谢码友！"

还没有人赞赏，支持一下

[![  ](https://upload.jianshu.io/users/upload_avatars/99517/234abbfc-9136-443f-a7d4-bf91b95bd685.PNG?imageMogr2/auto-orient/strip|imageView2/1/w/100/h/100/format/png)](https://www.jianshu.com/u/893e02c03bbc)

[iHTCboy](https://www.jianshu.com/u/893e02c03bbc "iHTCboy")[![  ](https://upload.jianshu.io/user_badge/b4853dc7-5c16-4875-a2cd-7cf764bbd934)](https://www.jianshu.com/mobile/creator)码不停蹄～<br><br>iHTCboy <br>破得迷，了得知，方能学海无边；<br>热爱移...

总资产236共写了15.4W字获得1,693个赞共814个粉丝

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

-   feisky云计算、虚拟化与Linux技术笔记posts - 1014, comments - 298, trac...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/4-3397163ecdb3855a0a4139c34a695885.jpg)不排版](https://www.jianshu.com/u/97702e424c5d)阅读 3,109评论 0赞 5
    
-   Spring Cloud为开发人员提供了快速构建分布式系统中一些常见模式的工具（例如配置管理，服务发现，断路器，智...
    
    [![](https://upload-images.jianshu.io/upload_images/7328262-54f7992145380c10.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/46fd0faecac1)

-   背景: 阅读新闻 12C CDB模式下RMAN备份与恢复 \[日期:2016-11-29\] 来源:Linux社区 作...
    
-   摘自：https://blog.csdn.net/ouyang\_peng/article/details/7707...
    
    [![](https://upload-images.jianshu.io/upload_images/11727607-5480069a750f32cc.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/0b01a7fd8d80)
-   Ubuntu的发音 Ubuntu，源于非洲祖鲁人和科萨人的语言，发作 oo-boon-too 的音。了解发音是有意...
    
-   常听到： “客户是个混蛋、客户太小气了，客户太难搞定了； 上班无趣、公司不好、管理不善、氛围糟糕…… 工资少、环境...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/8-a356878e44b45ab268a3b0bbaaadeeb7.jpg)时代小强](https://www.jianshu.com/u/760330f1acc9)阅读 483评论 0赞 0
    

-   周末，带着家人来到新昌天姥山脚下，跟随着无数前人的足迹，再一次探访这座有着一千多年历史的司马悔桥。 司马悔桥，又称...
    
    [![](https://upload-images.jianshu.io/upload_images/6293104-74b560cc9e6dcefd.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/jpg)](https://www.jianshu.com/p/90f39d793b6c)
-   好快的时光 爱是爱了，不爱也是不爱了，如此而已，没有不可能的可能。 尘归尘，土归土，一切归于沉寂。 今日的主题，回...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/6-fd30f34c8641f6f32f5494df5d6b8f3c.jpg)北鈺](https://www.jianshu.com/u/e18b228a85a1)阅读 70评论 0赞 0
    
-   八月末 夏季的炎热刚经历了暴雨的洗礼 在这样的日子 站在阳台 可以感受到凉风轻拂面庞 家中的小白喵翻着肚皮呼呼大睡...
    
    [![](https://upload-images.jianshu.io/upload_images/6315544-cc0a1c805ff5447f.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/jpg)](https://www.jianshu.com/p/f018c5bd1e1d)
-   每天都想着今晚一定要早睡！到了晚上，吃晚饭，看电视，看新闻，刷屏。看到新鲜事就激动不已，学到点什么又想继续刷屏，总...
    
    [![](https://upload.jianshu.io/users/upload_avatars/1500121/3c1c88783fe6.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/48/h/48/format/jpg)不二咩](https://www.jianshu.com/u/05ce968a6ca6)阅读 70评论 0赞 1
