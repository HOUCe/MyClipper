# [Markdown本地图片一键上传至图床（利用Automator）_albert12336的博客-CSDN博客](https://blog.csdn.net/albert12336/article/details/106887532)

最近整理电脑上的项目，写了好多 Mardown 文档，一下子发现图片还真的很多，插入图片，管理图片非常不方便。所以想起建立自己的图床，统一管理，并且每次插入图片的时候，把本地图片自动上传至图床。

图床指的是一类提供云存储图片的网站，用户上传图片后会获得图片的公共链接。

还有有时候我们下载别人的 Markdown，想把里面的图片一键上传至自己的图床，也可以使用下面的方法来实现。

先来效果图1：

![Operation1](https://img-blog.csdnimg.cn/2020062121422761.gif)

有没有发现最后粘贴 Markdown 链接的时候图片没有立马显示出来，原因总共6张图，还有一个gif，全加起来文件大，没有一下子缓冲下来，而且 GitHub 国内速度也慢一些。

效果图2:

![Operation2](https://img-blog.csdnimg.cn/20200621222311374.gif)

## 图床选择

建立图床有好多种办法，比如，自己买个云服务建立私人图床，或者使用第三方提供的云图床。

先在这里推荐几个图床：

-   [https://sm.ms![sm.ms](https://img-blog.csdnimg.cn/20200621165956352.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)](https://sm.ms/)
-   [七牛云图床![qiniu](https://img-blog.csdnimg.cn/20200621175921443.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)](https://portal.qiniu.com/signup?code=1hfa535g588cy)
    
-   还有 又拍、阿里云 OSS 、Imgur 、Flickr 、Amazon S3等等。
    
-   利用新浪微博上传，免费。
    
-   上传至 CSDN，把 csdn 当图床用。
    
-   GitHub 创建图库，无需输入密码，网上教程很多，很方便。
    
-   还有什么图床方案大家留言噢。
    

我这里选择了 GitHub 来创建自己的图床。

## 图床辅助工具

图床辅助工具是，帮助我们把图片上传至图床库。通过图床提供的接口,可以编写辅助工具,从而在本地使用图床服务,不再需要打开浏览器。

有好几种图床辅助工具，最常见的这几个：

### 1，大家都熟悉的 [iPic](https://toolinbox.net/iPic/)

### ![iPic](https://img-blog.csdnimg.cn/2020062118103595.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)

可以在 App Store 下载，目前支持好多种图床。在 Mac App Store 下载安装 iPic 之后虽然可以免费使用，但只能将图片匿名上传至微博图床。从下图可以看到其他图床都是加锁的无法使用。如果想使用所有图床，则要将 iPic 从免费版升级为高级版。

![iPic0](https://img-blog.csdnimg.cn/20200621181629485.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)

iPic 就是这样一个神器，，支持一键上传图片并获取地址，它支持：

-   拖拽图片批量上传
-   截图后自动上传
-   支持自定义配置七牛、又拍等第三方服务作为存储空间

![iPic1](https://img-blog.csdnimg.cn/20200621181242109.gif)

假如有钱不用往下看了，因为我选择的是 GitHub 作为图床方案，iPic 不支持 GitHub。但想要了解 MacOS Automator的强大功能的继续往下看，电脑上的好多重复性操作，我们可以交给 Automator 处理。

**然后想用 iPic，我这里推荐完美组合：Markdown 神器+图床神器**

**![Typora_iPic](https://img-blog.csdnimg.cn/20200621182231202.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)**

[Typora](https://www.typora.io/) 是我最爱的 Markdown 编辑神器， 极致简洁的设计理念、所见即所得的编辑方式，让你更专注于内容输入。

看看这两位的神合作：

### 1.1，一键上传本地图片

如果已经在 Markdown 中使用的本地图片、需要将其上传至图床，只要一键。

![Typora_iPic1](https://img-blog.csdnimg.cn/20200621182545463.gif)

### 1.2， 插入图片时自动上传至图床

如果开启了此选项，则在插入本地图片时，自动上传至图床、并替换图床链接。

![Typora_iPic2](https://img-blog.csdnimg.cn/20200621182651478.gif)

## 2，还有一个是 PicGo

GitHub 地址： [https://github.com/Molunerfinn/PicGo/](https://github.com/Molunerfinn/PicGo/)

PicGo 支持 本地图片/剪贴板 上传,内置多达八种图床,并且支持第三方插件。图床中包括易于使用的 SM.MS。

### 2.1，链接格式选择 Markdown

![PicGo](https://img-blog.csdnimg.cn/20200621183514803.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)

### 2.2，支持众多图床，包括 GitHub

![PicGo2](https://img-blog.csdnimg.cn/20200621183826931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)

### 2.3，PicGo 快速使用流程：

-   截图
-   Ctrl + Shift + P 上传至图床(可修改快捷键)
-   Ctrl + V 粘贴链接至编辑器

**注意隐私：**

不要将私密信息上传至图床,因为任何人都可以看到，注意 PicGo 同时提供了删除功能

谁也无法保证一个图床网站是否一直稳定可靠。所以对于图片的重要程度，需要自己有一个清晰的认识。如果是非常重要的图片，你需要更加可靠的地方来存放你的图片，比如本地或者知名大型网站。

**代码托管网站**是一个不错的选择，PicGo 支持 GitHub，你只需要在 GitHub 的用户设置中创建一个 token，并新建一个专门用于存放图片的仓库，最后将相关信息配置到 PicGo 即可立即使用。

![Picgo_GitHub](https://img-blog.csdnimg.cn/20200621184426978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)

唯一的问题是 GitHub 在国内的访问速度不是很理想。好在 PicGo 插件 GitHub-plus 添加了对 Gitee 的支持，配置方法则与 GitHub 类似。

完美主义者应该会认为截图后直接粘贴图片才是完美的体验，而即时上传其实在一定程度上打断了用户的操作流畅性。因为多了一个上传的步骤，而且用户必须在看见/听见上传成功的提示后才能粘贴 url。如果还考虑到网络波动，也许会有1~4秒的时间，用户是处于等待状态。因此，更好的解决方案也许是 Migrate - 迁移。

### 有这么好的 PicGo，为什么还要使用 Automator？

因为最近发现通过 Automator ，可以把系统里一些烦躁的操作一键化。通过 Automator 的 Quick Action，不仅能一键把本地图片上传至图床，还可以一键把图片链接上传至图床。比如你看别人写的文章，里面有张图片非常喜欢的，一键就把那张图片上传至自己的图床，并自动形成 Markdown 或者 URL 链接。

网上还有人写了一些工具，可以把图片上传至 GitHub 图库：

-   [在Markdown编辑中自动上传图片](https://blog.csdn.net/zz133110/article/details/79635774 "垃圾中文技术性网站")
-   [花了一整天写了个下载markdown图片到本地的库](https://juejin.im/post/5c532590e51d45221f2428c6) \---》[markdown-image-localizer](https://github.com/gylidian/markdown-image-localizer)

下面就介绍，怎样用 Automator 把本地图片上传至自己的 GitHub 图库。

在 MacOS 系统中打开以下设置：

![Setting_Service](https://img-blog.csdnimg.cn/20200621191825105.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)

这里的**服务**（**Service**）选项，一般你安装了新的软件后，有些软件提供的**服务**快捷键都会自动加在这里。

比如《使用iPic上传》是安装了 iPic 后自动形成的，使用的操作：

![ServiceDemo.gif](https://img-blog.csdnimg.cn/20200621192502491.gif)

大家看到《上传至GitHub》服务了吗？

![Setting_Service2](https://img-blog.csdnimg.cn/20200621192723841.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)

那就是我通过 Automator 的 Quick Action 加进去的，选择几张图片，右击 -> Service -> 上传至GitHub，就能把几张图片上传到 GitHub 图库。

首先你得在自己的 GitHub 上创建一个 **repository** 叫 **ImageGo**，（名字随便取），然后同步到电脑上，放在一个固定目录（我直接放在 /Users/albert/Desktop/ImageGo）。

打开 Automator 选择服务：

![Automator1](https://img-blog.csdnimg.cn/20200621193120554.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)

然后选择 **_`运行 Shell 脚本`_** 拖拽到右边，程序可以选 **_`finder`_** 或者 **_`任何应用程序`_**

![Automator2](https://img-blog.csdnimg.cn/2020062119330129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)

`shell` 类型务必选择 **_`/bin/bash`_** ！即使你安装了 `zsh` 也不要用！

![Automator3](https://img-blog.csdnimg.cn/20200621193437188.png)

直接输入以下 Code（Shell Script）：

```
for (( i = 0; i < length; i++ )); do    [a-zA-Z0-9.~_-]) printf "$c" ;;    *) printf "$c" | xxd -p -c1 | while read x;do printf "%%%s" "$x";doneImageGo_dir="/Users/albert/Desktop/ImageGo"fileName1="${fileName// /_}"    //图片文件名里如果有空格直接用下划线替代ImageGo_dir1="$ImageGo_dir/${fileName1}"mv "$file" "$ImageGo_dir1"      //把选择的图片移动到 ImageGo 项目目录下GitHubLink="![](https://raw.githubusercontent.com/{用户名}/{项目名}/master/$(urlencode $fileName1))"if [ "$links" == "" ]; then    links=$links"\n"$GitHubLinkgit commit -m "SYNCH Shell"
```

-   pbcopy 命令会把 echo 中的内容放置到系统粘贴板中；
-   关于 urlencode ：在上传测试过程中，发现一旦选择的文件列表中含有中文命名的文件，就会导致文件链接构造异常，最后也到不了系统粘贴板中，具体原因不明，所以在构造链接时做一次编码就好，反正浏览器本身也会对编码的链接自行识别；
-   echo 的两个参数可以参考[该文](https://blog.csdn.net/lizhi200404520/article/details/8819762 "垃圾中文技术性网站")；
-   请把资源链接的域名改成你对应的

再添加漂亮的通知。

这可以通过Apple Script实现。 设置方法是在Automator中添加一个 “运行 AppleScript” ， 内容是

```
display notification "Markdown链接已自动复制" with title "图片上传成功" sound name "default"
```

最终的 Quick Action 为：

![Quick Action](https://img-blog.csdnimg.cn/20200621195353217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsYmVydDEyMzM2,size_16,color_FFFFFF,t_70)

然后保存。就这样，选择几张图片，右击 -> Service -> 上传至GitHub，就能把几张图片上传到 GitHub 图库。

**方案缺陷**

这个方案唯一的缺陷就是不能自动上传剪贴板中的图片，所以在截图的时候，需要将图片保存到磁盘上。这个看看后期能不能解决。

有人在这里已经实现了《[利用七牛 qshell 和 Automator 打造快捷上传服务](https://segmentfault.com/a/1190000012625867)》，我也是参考他的方案的。

实现了上面的功能后，想着万一下载了别人的 README，突然想把其中的一张图片一键上传至自己的图库，并且自动形成 Markdown 或者 URL 链接，这个怎么做呢？

原理跟上面的一样，上面的 Shell Script 输入是 图片文件，我们再创建一个输入为 图片链接（其实是 文字 Text）的 Quick Action 服务就可以了。

![Automator4](https://img-blog.csdnimg.cn/20200621201348202.png)

服务收到的对象是 文件或文件夹，改为 文本（text）：

![Automator5](https://img-blog.csdnimg.cn/20200621201550592.png)

其实这里也可以选择 URL 作为服务输入，但是我在 Shell Script 上加了判断这输入是 URL 还是 目录（Directory）的判断。比如，图片的链接是 URL 还是 Directory。

以下 Code（Shell Script）：

```
for (( i = 0; i < length; i++ )); do    [a-zA-Z0-9.~_-]) printf "$c" ;;    *) printf "$c" | xxd -p -c1 | while read x;do printf "%%%s" "$x";doneImageGo_dir="/Users/albert/Desktop/ImageGo"regex='(https?|ftp|file)://[-A-Za-z0-9\+&@#/%?=~_|!:,.;]*[-A-Za-z0-9\+&@#/%=~_|]'if [[ $string =~ $regex ]]    fileName1="${fileName// /_}"    ImageGo_dir1="$ImageGo_dir/${fileName1}"    curl -o "$ImageGo_dir1" "$string"    fileName1="${fileName// /_}"    ImageGo_dir1="$ImageGo_dir/${fileName1}"mv "$file" "$ImageGo_dir1"links="![](https://raw.githubusercontent.com/{用户名}/{项目名}/master/$(urlencode $fileName1))"git commit -m "SYNCH Shell"
```

再来个 AppleScript：

```
on run {input, parameters}display notification ((input) as string) with title "图片同步操作" sound name "default"
```

**缺陷：**

上传时间慢。时间主要花在 GitHub 同步。如果大家觉得慢，直接用第三方提供的图床。

**扩展：**

通过类似原理，可以扩展 Quick Action，比如把 Markdown 上的图片链接下载到本地指定的图库，把图片链接下载到本地等等。

下面推荐几个网友通过 Automator 实现的一些服务：

[【MacOS】快速复制文件路径 —— Quick Action的创建](https://blog.csdn.net/weixin_42317507/article/details/89073036 "垃圾中文技术性网站")

___

### 后续会继续更新
