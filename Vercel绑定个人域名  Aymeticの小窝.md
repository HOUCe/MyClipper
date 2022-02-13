# Vercel绑定个人域名 | Aymeticの小窝

source:: https://aymetic.com/post/935c9419.html

之前我们将Github Pages部署到Vercel上面，充分享受了Vercel在国内高速的解析速度，接下来我们将自己的域名绑定到Vercel，这样我们就能省下一笔不小的服务器开销😁

## 购买自己的域名

第一步当然是购买自己的域名，要不然咋绑定？

我的域名是在腾讯云购买的，下面推荐一些域名代理商，其实在哪一家买都一样，无非就是价格和服务不同。

国内：[腾讯云](https://dnspod.cloud.tencent.com/)、[阿里云域名](https://wanwang.aliyun.com/)、[西部数码](https://www.west.cn/)、[百度智能云](https://cloud.baidu.com/product/bcd.html)

国外：[Namesilo](https://www.namesilo.com/register.php)、[Godaddy](https://sg.godaddy.com/)

国内申请域名需要实名注册，根据注册平台提示走就行

## 配置Vercel项目

进入[Vercel面板](https://vercel.com/dashboard)，找到你想绑定域名的项目，点击上面菜单栏的`Domain`

[![进入Domains](https://gitee.com/yzeven/blog-pic/raw/master//image/1616123351-e7e03b.png)](https://gitee.com/yzeven/blog-pic/raw/master//image/1616123351-e7e03b.png)

点击`Add`

[![点击Add](https://gitee.com/yzeven/blog-pic/raw/master//image/1616123420-a00705.png)](https://gitee.com/yzeven/blog-pic/raw/master//image/1616123420-a00705.png)

选择你想要绑定的项目，这里我选择我的项目

[![选择项目](https://gitee.com/yzeven/blog-pic/raw/master//image/1616123479-8952d1.png)](https://gitee.com/yzeven/blog-pic/raw/master//image/1616123479-8952d1.png)

然后点击`Continue`，输入你申请的域名，然后添加即可

这里只需要输入`域名`+`后缀`，即`aymetic.com`即可，不需要`www`和`https`

[![](https://gitee.com/yzeven/blog-pic/raw/master//image/1616123732-fc5ad1.png)](https://gitee.com/yzeven/blog-pic/raw/master//image/1616123732-fc5ad1.png)

然后我们就可以看到域名已经添加入我们的项目，接下来我们需要将DNS解析到Vercel的服务器即可~

## 配置DNS

配置完项目后就开始配置域名解析，由于我是用腾讯云申请的，故在此以腾讯云操作做展示，其余平台大同小异。

这里我们先来看一下Vercel官方的[域名解析指南](https://vercel.com/docs/custom-domains)

[![](https://gitee.com/yzeven/blog-pic/raw/master//image/1616123896-48065e.png)](https://gitee.com/yzeven/blog-pic/raw/master//image/1616123896-48065e.png)

这里我们看到Vercel官方推荐使用`A Record`，我们就先以这个方法设置一遍。首先将`Value`对应的IP复制下来，然后进入域名管理后台，找到解析管理：

[![点击“解析”](https://gitee.com/yzeven/blog-pic/raw/master//image/1616122985-6216b1.png)](https://gitee.com/yzeven/blog-pic/raw/master//image/1616122985-6216b1.png)

按照我的样子添加一条新的记录，如下：

[![添加解析记录](https://gitee.com/yzeven/blog-pic/raw/master//image/1616124258-766aab.png)](https://gitee.com/yzeven/blog-pic/raw/master//image/1616124258-766aab.png)

保存一下即可~配置将在15分钟内生效，完成后就是这样，Vercel会自动帮我们申请SSL证书并续费，是不是超级省心😋

[![image-20210319112609603](https://gitee.com/yzeven/blog-pic/raw/master//image/1616124369-966d67.png)](https://gitee.com/yzeven/blog-pic/raw/master//image/1616124369-966d67.png)

教程到此就结束啦，如果有什么问题，欢迎添加我的好友交流哦~

版权声明: 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。转载请注明来自 [Aymeticの小窝](https://aymetic.com/)！
