# [git配置Beyond Compare - 简书](https://www.jianshu.com/p/6b85cf8664aa)

[![](https://upload.jianshu.io/users/upload_avatars/6951529/877d92df-e9dd-46cd-ad1b-8e6f0bc2b0f9.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/jpg)](https://www.jianshu.com/u/8826e78037b6)

0.0562017.07.18 11:14:04字数 1,043阅读 5,416

之前有很多人都发过配置BC的教程，而且有人也说得很详细。这里我只是说一下我自己配置的时候的具体步骤和遇到的问题吧~ 先说一下，本文章只适用于windows电脑，至于mac，[请移步这里](https://www.jianshu.com/p/0b5fc056534f)

**本文章具有局限性，仅供参考，不喜勿喷**

1.第一步，不多BB，[下载Beyond Compare](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.scootersoftware.com%2Fdownload.php)。我没有那么厉害，搞不到破解版的，就直接下的正版试用的那种。反正三十天试用期到了，卸载再重新下一个就是了（亲测可行，而且只要两次路保存的路径一样，还不用多次配置。嘿爽歪歪）

2.第二步，查看电脑当前系统支持的git diff/git merge插件

```
git difftool --tool-help
git mergetool --tool-help
```

运行结果如下所示：

![](https://upload-images.jianshu.io/upload_images/6951529-bd0f3a1fd7797614.png?imageMogr2/auto-orient/strip|imageView2/2/w/633/format/png)

image.png

如果你的运行结果中，没有出现bc或者bc3的话，那基本上可以放弃了，电脑可能会不支持。

但是因为我周围的人，都有显示bc或者bc3，所以我也不知道到底会不会不支持，如果有谁运行完了之后没有显示，可以贴上图我们一起研究一下~

3.第三步，difftool/mergetool配置

difftool

```
git config --global diff.tool bc4
git config --global difftool.bc4.path "bcomp.exe的路径"
```

mergetool

```
git config --global merge.tool bc4
git config --global mergetool.bc4.path "bcomp.exe的路径"
```

这里要注意的是：**"bcomp.exe的路径"**这个东西  
我一开始的时候以为是有人给简写了，所以我找到了"BCompare.exe"这个东西，错错错！！！不是他，是**"bcomp.exe"**

上图：

![](https://upload-images.jianshu.io/upload_images/6951529-b153aa7fb5bf812a.png)

image.png

千万记得这个文件，不要错了。不用区分大小写。但是路径要写全，例如我的路径是：

```
D:\Beyond Compare 4\bcomp.exe
```

4.第四步：如果出现虽然安装了bc但mergetool不可用的情况，可以通过修改用户目录下的 gitconfig追加difftool和mergetool的配置

其实我觉得这一步是必须的。。。。。

内容如下，mergetool 的名字可以自定，路径修改为本地 bcomp.exe 的路径即可

首先要找到你需要改的文件**".gitconfig"**，下图是我的文件位置。

![](https://upload-images.jianshu.io/upload_images/6951529-3246cdcf1e902de2.png)

image.png

然后就是把你的difftool和mergetool追加进去了~

你可以用一万种方式打开那个 ".gitconfig" 文件

只需要改动以下部分就好了：  
**_注意：_**  
1.不要忘记改成像我一样的**"cmd = "**;  
2.看清楚路径的链接的斜线是往哪个方向斜的。不要咔咔一顿怼，全给怼上向右斜的了；  
3.将示例中的路径换成自己的。。。对没错，你用的不是我的电脑，所以写我的路径不一定好使。  
4.不要忽略每一段路径后面的那个空格，不管是直接写的路径，还是环境变量，后面都有个空格，不要忽略掉。要不会报错。。（不要问我怎么知道的）

```
[diff]
    tool = bc4
[difftool "bc4"]
    cmd = \"D:/Beyond Compare 4/bcomp.exe\" \"$LOCAL\" \"$REMOTE\" 
[merge]
    tool = bc4
[mergetool "bc4"]
    cmd = \"D:/Beyond Compare 4/bcomp.exe\" \"$LOCAL\" \"$REMOTE\" \"$BASE\" \"$MERGED\"
```

```
D:/Beyond Compare 4/bcomp.exe     
```

千万记得

这一步配置结束之后，就可以使用Beyond Compare来merge或者diff你的代码了~  
个人认为这个工具还是比较Diao的，你修改了什么，一目了然。哪句想留下，哪句想扔掉，随意~

ok~结束，这就是我配置的时候遇到的一些坑。。。打完收工...  
还是那句话，我只是一个前端小小小小白。。。以上所有，仅仅是我个人的一些小见解和小看法，如有不妥之处。还请各位大佬批评指正。大家一起学习一起进步！

以后打算慢慢的把工作中遇到的问题和填过的别人的坑拿出来分享一下，供大家一起交流~  
也让自己再重温一下，以后避免同样的问题。

**仅供参考，不喜勿喷，转载不用注明出处，给钱就行（说的好像有人真要转一样 =\_=）**

更多精彩内容，就在简书APP

"小礼物走一走，来简书关注我"

还没有人赞赏，支持一下

[![  ](https://upload.jianshu.io/users/upload_avatars/6951529/877d92df-e9dd-46cd-ad1b-8e6f0bc2b0f9.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/100/h/100/format/jpg)](https://www.jianshu.com/u/8826e78037b6)

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

-   Spring Cloud为开发人员提供了快速构建分布式系统中一些常见模式的工具（例如配置管理，服务发现，断路器，智...
    
    [![](https://upload-images.jianshu.io/upload_images/7328262-54f7992145380c10.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/46fd0faecac1)
-   Git是目前最流行的版本管理系统，也是最先进的分布式版本控制系统(distributed version cont...
    
    [![](https://upload-images.jianshu.io/upload_images/3151492-945d747c3e6e249a.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/fe038a97bb3c)

-   Ubuntu的发音 Ubuntu，源于非洲祖鲁人和科萨人的语言，发作 oo-boon-too 的音。了解发音是有意...
    
-   Git 与 SVN 区别 Git不仅仅是个版本控制系统，它也是个内容管理系统(CMS),工作管理系统等。如果你是一...
    
    [![](https://upload-images.jianshu.io/upload_images/723261-97a342f71647b4ed.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/0c4dbd5ac9f9)
-   1、 此前一直自信心爆满的朋友A最近倍受打击，九月份从上一家公司辞职出来后我的耳根就没有消停过，倒不是工作难找，而...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/7-0993d41a595d6ab6ef17b19496eb2f21.jpg)汤圆16](https://www.jianshu.com/u/8f0dfb6d7fb6)阅读 116评论 1赞 3
    
-   Designed by Vigor Lai
    
    [![](https://upload.jianshu.io/users/upload_avatars/4224273/ec9be1c6-8a08-4568-aa83-1b3e4ace0861.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/48/h/48/format/jpg)么么哒嗨](https://www.jianshu.com/u/7705f88c5390)阅读 111评论 0赞 0
    
    [![](https://upload-images.jianshu.io/upload_images/4224273-5607d43dd700a0b0.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/jpg)](https://www.jianshu.com/p/b58fc76664b5)
