# 如何发布logseq成为博客 #发布#教程 - 分享 - Logseq 中文社区

source:: https://cn.logseq.com/t/topic/82

这个帖子只是抛砖引玉,可能有更多更好的方式,请在底下留言;而且这个教程有时效性,未来logseq应该会提供更加方便快捷的发布方式.  
这个教程主要参考了logseq发布教程以及一些简单的部署知识. **同时注意要使用logseq的github同步版(http://logeseq.com并登录你的github)而不是本地版,本地版暂时还不支持.**

## 0\. 预备知识

1.  先温习一下logseq的小白教程[Hsun Cheung：Logseq小白系列教程入门篇一 107](https://zhuanlan.zhihu.com/p/343854552).
    
2.  你的笔记根目录下有一个"logseq"文件夹,里面躺着的config.edn的文件就是所有设置,其中你会发现很多行行首有";;"双冒号字样,这是代码注释,一旦删掉这个双冒号就会让后面的指令生效.
    
3.  最好有一定的git基础,因为你需要在本地把博客笔记同步到gitee或者github上.个人习惯使用git bash,如果喜欢界面的同学推荐sourcetree.
    

## 1\. 选择自己要发布的页面

有两种方式:

1.1 一种是把所有页面全部可以公开,这种方式要慎重,适用于平时只是把笔记当作自己的学习输出平台或者博客仓库的同学

打开上述提到的`config.edn`,找到"all-pages-public? true"那一行,把前面的双冒号去掉,然后保存.

1.2 第二种方法更定制化,就是到你想发布的每个页面里面,点击标题右侧的三个点,选择最下方的选项发布,然后在弹出来的对话框中选择最下面的那个"到处HTML时发布此页面".

1.3 如何**定制首页**呢,比如你想让你的`overview`页面作为博客首页,还是打开`config.edn`这个文件, 然后输入如下内容:

```
:default-home {:page "overview"
               :sidebar "Contents"  }
```

最后别忘了保存并推送/上传到github你的笔记代码仓库里!

## 2\. 下载博客模板

到这里下载最新的博客模板Releases · logseq/logseq. 下载后解压缩到一个文件夹,我以后就称呼这个文件夹为"**博客笔记**",用于跟你平时记笔记的文件夹"笔记仓库"相区分.

进入http://logseq.com你的笔记系统里点击头像,选择"导出公开页面",会自动下载"index.html",把这个文件粘贴到你的博客笔记中去,这样你的博客笔记就变成了这样的结构:

![](https://cn.logseq.com/uploads/default/original/1X/efbf091a4ead9cb32810686f9c9bb9c7969c8278.png)

## 3\. Gitee上部署你的笔记(待定)

Gitee相当于国内的github,你可以把你的笔记博客上传到这上面来使用它的静态托管功能.之所以选择它是因为国内网络环境可能对github的访问不是太友好. **(有小伙伴说现在Gitee page暂时还有点问题,那就请大家直接跳到第四点吧)**

当然如果你喜欢折腾,个人更推荐专业的静态托管网站Vercel和Netlify,免费绑定域名,带宽,上传限制等等都远远好于Gitee.

1.  首先注册一个帐号,然后用你的用户名新建一个仓库,这样未来你部署好了你的博客地址就是`http://用户名.gitee.io/`.创建过程中底下的一些初始设置比如选择语言啊添加开源许可证等都可以暂时忽略.

![](https://cn.logseq.com/uploads/default/original/1X/8adcb65042130d41942031fb0e934db97ffa2496.jpeg)

2.  打开git bash或者能识别git指令的命令行,进入你的博客笔记里面.你新建仓库后网页会给你一个如下所示的教程,这个很重要!

![](https://cn.logseq.com/uploads/default/original/1X/34a2256e3cea43be7069dbff9ce10cf7c6be9f27.jpeg)

首先如图所示,输入git的设置`git config user.name "用户名"`, `git config user.email "邮箱"`.如果你主力用的是gitee就按照图里所示加上`--global`字样,如果你平时用github, gitlab那么就不要加上global,不然会覆盖你的设置

然后在你的博客笔记根目录下按照图中所示依次输入

```
git init
git add .
git commit -m "set up my first note"
git remote add origin xxx
git push -u origin master
```

如果你看到输出"Branch ‘master’ set up to track remote branch ‘master’ from ‘origin’"字样,说明你上传成功了.

点击右上方的"服务"选项,选择里面的"Gitee Pages",就可以部署你的页面.由于本人没法绑定手机,所以暂时无法测试.

## 4\. Vercel上部署

我就是用的Vercel托管的我的笔记zhanghanduo.com.

首先注册Vercel,绑定你的github帐号.

然后用第3点类似的方法把你的博客笔记(带有index.html的那个)上传到github里面去,比如我的仓库名叫"log\_pub",参见我的小号做成的例子handuozh/log\_pub.

在Vercel里面点击"New Project",选择github仓库,然后选择"log\_pub":

[![](https://cn.logseq.com/uploads/default/optimized/1X/dfa5c4929ed40ce3ee82d196309f43dcc7ef106c_2_351x500.jpeg)](https://cn.logseq.com/uploads/default/original/1X/dfa5c4929ed40ce3ee82d196309f43dcc7ef106c.jpeg)

选择Import,然后不用动任何改动,直接"Deploy",等一会儿你就会发现成功了,选择"Visit"就可以访问你的博客了!记住这个网址.

以后想要**更新**博客,就需要重新在logseq上生成这个index.html,然后替换进你的博客笔记里,用git把代码上传同步

```
git add .
git commit -m "update content"
git push origin master
```

稍等几分钟,也许你还会收到Vercel发来的邮件表示成功或者失败,没问题的话这样就大功告成啦!

绑定域名的方法以后有机会再跟大家分享,因为不同的域名供应商的设置可能略有不同.  
如果你有兴趣,请参见[https://vercel.com/docs/custom-domains 21](https://vercel.com/docs/custom-domains)这篇英文教程
