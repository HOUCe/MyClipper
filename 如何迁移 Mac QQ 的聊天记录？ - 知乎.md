# [如何迁移 Mac QQ 的聊天记录？ - 知乎](https://www.zhihu.com/question/25667176)

参考了前面几位的答案，经过实验，用身体证明了，之前几位说的确实不完整，会导致部分聊天记录里的图片丢失。。。

![](https://pic1.zhimg.com/50/v2-6f1f0919ae819d97b0a36238c46106ad_720w.jpg?source=1940ef5c)

说说我的情况，2006开始使用苹果电脑，（因为穷）到目前为止只换过两次机器，上一次还是5年前，当时怕麻烦使用的Mac的迁移助手把老机器上的环境迁移过来，因此所有软件配置记录都能完美保留。  
最近机器开始花屏黑屏，正赶上2018新MBP，心痒无比，但是无良奸商Apple不断涨价，新机自己已经买不起了，遂黑着脸向公司申请了一台新机。考虑到上一次用迁移助手虽然方便，但是难免有些永远不会再使用的垃圾软件及其配置信息会占用新机空间。所以这次决定自己迁移工作环境。其实除了一些软件需要重新安装外，需要迁移记录的就两个软件：邮件和QQ

因为MacQQ使用时间较长，有近10多年的聊天记录，文件夹大小已超过50G，期间还经历过早期MacQQ不完善，之后各种版本升级等等，因此其实聊天记录的目录结构比较复杂，所以使用前面几位的简略版会导致部分聊天记录里的图片无法显示。

下面详细说一下QQ记录的迁移

\---------- 分割线 ----------

1）新、旧机器都是 macOS 10.13.6，且新旧机器上都只有一个同名的账户（这样的环境，遇到文件权限问题的可能性非常小）

2）迁移前 把新、旧机器上的 QQ 都升级到 最新版本 6.5.0

3）旧机器上升级完QQ后一定用新版QQ重新登录一次（目的是让新版QQ对数据进行必要的升级）

4）旧机器上退出QQ（我特意单独列出这一步，就是拍你漏掉）

5）新机器上新版QQ启动一次，但是不需要登录（目的是生成一些必要的目录结构）。

6）新机器上退出QQ（我特意单独列出这一步，就是拍你漏掉）

7）把旧机器上的 /User/你的用户名/Library/Containers/com.tencent.qq 目录整个复制到新机器的“下载”目录下（最方便的方法就是通过 隔空投送AirDrop）。

8）将目标目录（/Users/你的用户名/Library/Containers/com.tencent.qq/Data/Documents）里面的同名文件改名或备份后，在 终端窗口中 执行

```
mv /Users/你的用户名/Downloads/com.tencent.qq/Data/Documents/* /Users/你的用户名/Library/Containers/com.tencent.qq/Data/Documents 
```

9）将目标目录（/Users/你的用户名/Library/Containers/com.tencent.qq/Data/Library/Application\\ Support/QQ）改名或备份后，在 终端窗口中 执行

```
mv /Users/你的用户名/Downloads/com.tencent.qq/Data/Library/Application\ Support/QQ /Users/你的用户名/Library/Containers/com.tencent.qq/Data/Library/Application\ Support 
```

10）将目标目录（/Users/你的用户名/Library/Containers/com.tencent.qq/Data/Library/Caches）改名或备份后

```
mv /Users/你的用户名/Downloads/com.tencent.qq/Data/Library/Caches /Users/你的用户名/Library/Containers/com.tencent.qq/Data/Library 
```

11）这时在新机器上启动QQ登录即可。

\---------- 分割线 ----------

注意，我这是新电脑上只启动过一次QQ，没有登录过任何账号（即没有任何有价值的记录）的前提下。因此可以用电脑上的目录直接覆盖。

如果你要合并新旧电脑上的QQ记录可能要更加复杂，对此我无力研究，你可以在我这个步骤的基础上继续前行，祝好运。
