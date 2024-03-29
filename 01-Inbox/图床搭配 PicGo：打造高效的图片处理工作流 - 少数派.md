# [图床搭配 PicGo：打造高效的图片处理工作流 - 少数派](https://sspai.com/post/65716)

**Matrix 首页推荐**

[Matrix](https://sspai.com/matrix) 是少数派的写作社区，我们主张分享真实的产品体验，有实用价值的经验与思考。我们会不定期挑选 Matrix 最优质的文章，展示来自用户的最真实的体验和观点。

文章代表作者个人观点，少数派仅对标题和排版略作修改。

___

我们在写文章时，通常需要单独存储图片资源，不仅占用磁盘空间，而且不好管理。自从全面使用 Markdown 进行个人文件记录后，深感其在这图片处理的无力。

但是，用了图床之后，一切都不一样了。

## 写在前面

Markdown 语法处理图片返回的是一个 URL 链接。

如果上传的是本地图片，则会显示文件的本地路径，既然是本地路径，分享文件时就显得极不方便了。它需要将文章中的图片一并发送，接受者还要配置本地环境与发送者图片路径一致才可看到，或者根据自己的目录，修改文章中图片路径。如果使用的是图床工具显示的是外链，可以「随时随地」查看，并不需一并发送图片，也无需自行配置本地环境。

或许上面说的过于抽象，我们设想这样一个场景：

你在 Typora 里写好一篇图文文章（为了显示差异，这篇图文采用本地图片和图床链接两种形式），然后发送这个 `.md ` 文件给协作者进行编辑修改。

![](https://cdn.sspai.com/2021/04/01/fb1fc5c65b451f387bf66d7ddecce13c.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

必定不久你的协作者就会一脸疑惑，说 Ta 根本就看不到第一张图片，因为在 Ta 那里，是这样的：

![](https://cdn.sspai.com/2021/04/01/3cd55374a29f6eec7048ac4859a1d616.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

如果你又不小心删除了第一张图片，那么连你也无法在 Typora 看到它了。而你试着删除使用图床外链的第二张图片，你发现仍然能够使用，实际上的效果和上图一样。

想必好奇的你一定会惊喜「图床」是什么？为什么图床会这么方便？！

图床，顾名思义就是进行图片存储的服务器，同时允许对外连接网络，所有人可以访问。

## 好用图床推荐

图床服务分为免费和付费两种。如果你对图片资源管理和版权要求不是很高，或者仅是偶尔写写文章的话，免费的图床也是足够了。如果你想更有效地掌握自己的图床资源，还是建议采用付费云服务。

以下列举了几个常见的和相对好用的图床服务。

### 免费图床

免费的代价：众多免费图床服务注册条款里均有禁止商用说明，万一哪天你使用的服务挂掉了或者关闭了图片外链，那你所有的链接都无法访问了，对你造成的损失或许不小。比如，前几年的微博图床。

[SMMS](https://sm.ms/)：稳定且快捷，是不少人主力图床的选择，同时开放 API，虽然已经推出付费套餐，但免费版本已经足够能用，不过公众号无法识别 SMMS 图床链接。

[ImgURL](https://imgurl.org/)：对游客有限制，每日最多上传 10 张，单张图片不能超过 5M，偶尔用用还是挺好的。

[GitHub](https://github.com/)、[Gitee](https://gitee.com/)：代码托管云服务网站，帮助开发者存储和管理其项目源代码，且能够追踪、记录并控制用户对其代码的修改。不同的是，GitHub 服务器在国外，Gitee 服务器在国内，访问速度更快。甚至你可以更简单粗暴地把它理解为一个巨型网盘，可以存储任何东西。

如果想要免费使用，极推荐使用这两款服务，毕竟跑路的可能性极小。

在 [这里](https://www.githubs.cn/post/what-is-github) 和 [这里](https://gitee.com/help/articles/4105#article-header0) 你可以更详细地了解它们。

### 付费图床

[_腾讯云_](https://gitee.com/help/articles/4105#article-header0)_：下面是个人版定价表。_

![](https://cdn.sspai.com/2021/04/01/afc42c2d684e3d5873efac45ca40ddcf.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

[_阿里云_](https://cn.aliyun.com/)_：目前国内最大的云计算服务商，_[_这里_](https://cn.aliyun.com/price/detail/oss) _是阿里云 OSS 的定价单。_

[_七牛云_](https://www.qiniu.com/)_：提供的免费额度还挺高，是不少博主的选择。这里查看_ [_七牛云定价_](https://www.qiniu.com/prices/kodo)

虽然比较来看，七牛云价格可能稍便宜些，但作为图床的云存储来说，一年或许也就几十块钱，没太大差距。而且腾讯云对于微信公众号的运营来说会更流畅些。

更多图床的介绍可以移步：[无需注册、打开即用，这 8 个免费好用的图床工具值得一试](https://sspai.com/post/55032)

## 图片上传器：PicGo

> **PicGo: 一个用于快速上传图片并获取图片 URL 链接的工具** ——来自官网的自述

有了图床，我们可以将图片（或 GIF 等）返回一个 URL 链接，而不需要专门编辑、存放、管理众多杂乱无章的图片，直接复制，粘到 Markdown 编辑器里就能显示出来。嗯挺好，但不舒服。

一篇文章，尤其是教程文章，势必会用大量图片，一张张上传、复制、粘贴，不断切换应用和浏览器，不仅繁琐的流程让人烦，打断的思路更难以寻回。

此时就需要一款图片上传工具了，支持全平台的有 [PicGo](https://github.com/Molunerfinn/PicGo/)、仅限 macOS 系统的有 [uPic](https://github.com/gee1k/uPic)。

我仅使用过的 PicGo，这里就推荐他了（如果你对uPic对同样感兴趣，可以看看 [我派的这篇文章](https://sspai.com/post/64169)）。

PicGo 不仅是图片上传工具，同时也提供简易的图床相册管理功能。PicGo 开源且免费，跨平台支持 Windows、macOS、Linux 系统，使用极为简单。

可以在 [这里](https://github.com/Molunerfinn/PicGo/releases) 下载软件安装包，或者通过包管理器 [Scoop](https://scoop.sh/)、[Chocolatey](https://chocolatey.org/install)、[Homebrew](https://brew.sh/) 一键安装。

Chocolatey：`choco install picgo`

Scoop：`scoop install picgo`

Homebrew：`brew install picgo --cask`

下载之后，界面是这样：

![](https://cdn.sspai.com/2021/04/01/4737b24fc9462682bf2d806d499eae71.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

初始配置好存储方案，PicGo 支持 SMMS、腾讯云、阿里云、七牛云、GitHub、又拍云、Imgur 等，同时支持 Gitee、青云等第三方图床服务。使用时，拖放图片到主窗口或 mini 窗口（macOS 为顶部状态栏），PicGo 会自动上传至云服务器并返回链接到剪贴板，直接粘贴即可使用。

PicGo 的 UI 界面不算好看，但也不难看。虽设计普通简洁，却上手容易，小白也能理解。

选择图片上传，PicGo 会自动返回链接到剪贴板，只需要在相应编辑器里粘贴即可。

![](https://cdn.sspai.com/2021/04/01/6bbfd4cc94547473b7b802877c2d9784.gif)

当然，除了该有的功能，PicGo 也有不少用心的地方。

### 快捷上传

支持直接将剪贴板图片或 URL 一键上传。

### 丰富的链接格式

PicGo 支持 Markdown、HTML、URL、UBB 和 Custom 五种格式的链接，对于 Markdown 写作者而言，最常用的就是 Markdown 格式，当然也支持自定义 Custom 。

### 多样的的上传方式

PicGo 支持的上传方式也比较丰富。除了主窗口上传之外，Windows 和 Linux 用户支持 Mini 小窗上传，macOS 用户则还可选择顶部状态栏上传，使用更加便捷。

![](https://cdn.sspai.com/2021/04/01/5c4e357e7691fcfe7ac245124182bdca.gif)

![](https://cdn.sspai.com/2021/04/01/af6df8d9da61af9f073c319dcfb16205.gif)

### 图片管理

PicGo 不仅是图片上传工具，同时也提供简易的图床相册管理功能。

打开「相册区」，可以看到当前图床中所有的图片集合。支持复制、修改 URL 和删除的操作，分别对应每张图片左下方的三个小按钮。

![](https://cdn.sspai.com/2021/04/01/d12ec00bbb1f0f7b3c2610a138104f42.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

![](https://cdn.sspai.com/2021/04/01/9936c6f4f4255adcfa78d6d8b9a9fb9d.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

同时，也支持批量复制、删除或全选操作。如果你同时使用多个图床，还可以单独显示每个图床的情况。

值得注意的是，「删除」操作仅仅删除本地数据，从而不在相册区展示，但不会影响图床存储。

### 插件

PicGo 支持丰富的插件系统，这里是 [官方插件](https://github.com/PicGo/Awesome-PicGo) 页面，截至 2021 年 1 月底，已经推出了三十几款插件应用，涵盖「图床上传器」「图片压缩」「图片编辑」「图床迁移」等多方面。 

![](https://cdn.sspai.com/2021/04/01/7c086e1d490d69505ee791adf4c72bb9.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

2.0 版本之后，你可以简单通过「插件设置」页面，搜索安装插件。安装完成后，可以点击插件右下方齿轮图标，进行更新、禁用、卸载、配置及使用等功能。

![](https://cdn.sspai.com/2021/04/01/a410ef4b575bcc7f83f730549ac780b2.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

**注意：由于 PicGo 要使用** `**npm**` **来安装插件，所以用户必须先安装** [**Node.js**](https://nodejs.org/en/)**。**

#### 推荐的插件

-   [vs-picgo](https://github.com/PicGo/vs-picgo)：在上文已经提到，VScode 中使用。
-   [picgo-plugin-super-prefix](https://github.com/gclove/picgo-plugin-super-prefix#readme)：自定义图像文件名和前缀
-   [picgo-plugin-pic-migrater](https://github.com/PicGo/picgo-plugin-pic-migrater)：Markdown 文件中图片的**图床迁移**。不过我还没有尝试过，我看的 [@思考问题的熊](https://m.okjike.com/originalPosts/5f6228ec2161af00184394df?s=eyJ1IjoiNjAzNDU1N2I1MzhiNjkwMDE3ODBiNmFlIn0%3D) 评价很稳定。
-   [picgo-plugin-quick-capture](https://github.com/PicGo/picgo-plugin-quick-capture)：**仅支持 2.2.0+ 版本的 PicGo**，提供一键截图 + 上传的功能。
-   [picgo-plugin-pic-migrater](https://github.com/PicGo/picgo-plugin-pic-migrater)：Markdown 文件中图片的**图床迁移**。之前看到 [@思考问题的熊](https://m.okjike.com/originalPosts/5f6228ec2161af00184394df?s=eyJ1IjoiNjAzNDU1N2I1MzhiNjkwMDE3ODBiNmFlIn0%3D) 评价很稳定，于是也测试了一番。本来想着还比较麻烦，但试用了下发现太简单方便了！「三步走」就完事了：
    -   进入 PicGo 插件设置，文件名后缀设置为生成转换后的文件名后缀。比如，要转换  `test.md`，则当前目录下转换后的文件名为 `test_nex.md`。
    -   只包含 / 不包含：仅（不）转换包含自定义设置的图床链接。
    -   上传文件（文件夹）。等待转换完成，片刻即可在当前目录生成自定义文件名后缀的转换后的文件了。

![](https://cdn.sspai.com/2021/04/01/706c72388eb8dd0b59ee89898191069d.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

![](https://cdn.sspai.com/2021/04/01/2a684005abbf4328de32df6507e2d203.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

### 其他设置

进入 PicGo 的设置页面，你可以自定义个人使用习惯。

![](https://cdn.sspai.com/2021/04/01/83d19bd60117e403156aa1d3a686be23.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

比如，修改快捷键：

![](https://cdn.sspai.com/2021/04/01/1ce45f66406af38094cc0f72fc8dc1be.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

比如，进行自定义链接样式、选择是否上传前重命名、是否开机自启、是否接受更新等，还可以进行代理设置、Server 设置等。

## 图床配置

打开「PicGo 设置 - 选择显示的图床」一栏，可看到默认的 7 个图床，如果有其他需求，比如使用 Gitee 或青云图床等，可以使用第三方插件自行配置。

PicGo 本体支持如下图床：

-   七牛图床 v1.0
-   腾讯云 COS v4\\v5 版本 v1.1 & v1.5.0
-   又拍云 v1.2.0
-   GitHub v1.5.0
-   SM.MS V2 v2.3.0-beta.0
-   阿里云 OSS v1.6.0
-   Imgur v1.6.0

本体不再增加默认的图床支持。你可以自行开发第三方图床插件。详见 [PicGo-Core](https://picgo.github.io/PicGo-Core-Doc/) ——来自官网介绍

每个图床配置虽然看起来不同，但实际上大同小异。具体配置信息可以查看 [这里](https://picgo.github.io/PicGo-Doc/zh/guide/config.html#%E9%80%89%E6%8B%A9%E5%A4%8D%E5%88%B6%E7%9A%84%E9%93%BE%E6%8E%A5%E6%A0%BC%E5%BC%8F%EF%BC%88v2-0%EF%BC%89)。

本文仅挑选 SMMS、GitHub、腾讯云三个典型图床为例。

### SMMS

`{`  
`"token": "" // 通过SMMS后台获取的api token值`  
`}`

### GitHub

`{`  
`"repo": "", // 仓库名，格式是username/reponame`  
`"token": "", // github token`  
`"path": "", // 自定义存储路径，比如img/`  
`"customUrl": "", // 自定义域名，注意要加http://或者https://`  
`"branch": "" // 分支名，默认是main`  
`}`

Gitee 图床配置与 GitHub 几乎相同。不过，PicGo 没有默认提供 Gitee 选项，所以需要通过插件开启第三方服务。首先在插件市场打开搜索 `Gitee`，点击任意一个，即可完成安装和所有配置。

![](https://cdn.sspai.com/2021/04/01/c8d7d50e1bd7a9d0c2f7b414b1df925a.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

### 腾讯云（阿里云图床与腾讯云几乎一致）

`{`  
`"secretId": "",`  
`"secretKey": "",`  
`"bucket": "", // 存储桶名，v4和v5版本不一样`  
`"appId": "",`  
`"area": "", // 存储区域，例如ap-beijing-1`  
`"path": "", // 自定义存储路径，比如img/`  
`"customUrl": "", // 自定义域名，注意要加http://或者https://`  
`"version": "v5" | "v4" // COS版本，v4或者v5`  
`}`

#### 免费服务：Gitee + SMMS

我使用腾讯云COS 作为主力图床的也有两个月了，最开始也是对 Markdown 的图片管理摸索了好久。不过一年多折腾下来，也打磨出了一套很好用的免费方案 —— Gitee 作为主力图床，搭配 SMMS。

为什么选择这组「套餐」呢？

首先，Gitee 作为国内最大的代码托管平台，经过几年发展，已经成为 GitHub 的国内替代产品，专业性强，相比于其他免费服务，风险性很低。

之所以选择组合 SMMS 使用，不仅是因为其在 PicGo 中可直接开启服务，还有 Gitee 对于单张图片限制在 1M 以内，所以一些体积较大的 GIF 文件，可能不够用，而 SMM 的限额更高，为 5M。

### PicGo 与 Gitee

既然是免费服务，首先需要建立自己的 Gitee 图床库，详细步骤如下。

-   注册个人 Gitee 账号
-   新建图床仓库

![](https://cdn.sspai.com/2021/04/01/5913a2d5bd12cd5da593b781e4defc16.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

-   获取 token：点击头像，进入 [个人设置](https://gitee.com/profile) ，点击左侧「私人令牌 - 生成新令牌 - 修改私人令牌权限 - 简单私人令牌描述 - 提交」即可生成私人令牌 token。

![](https://cdn.sspai.com/2021/04/01/d26cd7b332f3b13c505f722b414d7826.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

注意：token 只会明文出现一次，注意保密，尽量不要丢失，否则又要重新生成。

回到 PicGo。因为 PicGo 没有默认提供 Gitee 选项，所以需要通过插件开启第三方服务。首先在插件市场打开搜索 `Gitee`，点击任意一个，即可完成安装和所有配置。

![](https://cdn.sspai.com/2021/04/01/a44de20e31107a576b046e18fa980f25.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

![](https://cdn.sspai.com/2021/04/01/6fb10e8fbcbfb5f8aab2dfefba14fd48.jpeg?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

### Picgo 与 SMMS

进入 [SMMS图床官网](https://sm.ms/)，注册登录之后，点击右上角「User - Dashboard」，在「APItoken」中可以查看个人专属 Secret Token。

![](https://cdn.sspai.com/2021/04/01/a4e6923db396786a0e9bfbcfa56a6464.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

PicGo 中，打开「插件设置」，搜索「SMMS」，即可安装，然后将 token 复制过来，完成配置。

![](https://cdn.sspai.com/2021/04/01/fb579205bebfe606698adaf37a9b615b.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

![](https://cdn.sspai.com/2021/04/01/5f46c971812d4a43353f2ef02a49d559.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

### 付费服务：PicGo 与 腾讯云

付费服务显得就从容多了，没有体积大小的要求，也没有每日次数的限制，一切「按量计费」或者购买套餐，具体计价表在上面已经给出连接。只要数据没有销毁，费用就要继续，不过确实足够便宜。

对于付费服务，这里以腾讯云为例，演示在 PicGo 中的配置。

-   在腾讯云产品列表中找到「对象存储」，注册开通腾讯云对象存储 COS 服务。

![](https://cdn.sspai.com/2021/04/01/bb0bf7fe6d5f05561684fdfe72297b15.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

-   点击「存储桶列表 - 创建存储桶」，名称和地域自定，访问权限设置为「公有读私有写」。

![](https://cdn.sspai.com/2021/04/01/e1dfb5d6df147c7c87598ad08cf01968.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

![](https://cdn.sspai.com/2021/04/01/c46bbe8f8f7b5ac54cbc97bf0d6a04f9.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

-   在「基础配置」中，记住「存储桶名称」「地域代码」「访问域名」，以后要用到。

![](https://cdn.sspai.com/2021/04/01/767b808aa77471ac9a45809227f4cc7d.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

-   进入 [腾讯云 API 密钥管理](https://console.cloud.tencent.com/cam/capi)，点击「切换使用子账号密钥」，快速创建。

![](https://cdn.sspai.com/2021/04/01/786936616155e40dbb6ded06284849ff.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

-   在设置用户信息中，输入用户名，点击「用户权限」，搜索「QcloudCOSFullAccess」，勾选并确定，之后点击「创建用户」。

![](https://cdn.sspai.com/2021/04/01/e0f4865084f70f9b20ea693d9c98c2d3.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

![](https://cdn.sspai.com/2021/04/01/b6db0614cd3fd2b2e1583a54354e82a3.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

-   进入「[腾讯云用户列表](https://console.cloud.tencent.com/cam) - API 密钥 - 新建密钥」：

![](https://cdn.sspai.com/2021/04/01/9257f66d29e1535c4aeb4ddaba9620ef.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

-   记住这里的 SecretID 和 SecretKey。

![](https://cdn.sspai.com/2021/04/01/79888737f14fb446c0675d6f8723bdcf.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

此时，进入 PicGo 设置，勾选显示腾讯云 COS，然后进入详细的腾讯云设置界面，点选使用 v5 版本，再按照提示依次填入个人账号信息。自行选择勾选是否设为默认图床。

![](https://cdn.sspai.com/2021/04/01/23b0b06636825cb99880c514859f2c70.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

![](https://cdn.sspai.com/2021/04/01/dafd5a0693c4492b269708e8f27d1fce.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

## 与其他软件的联动使用

### PicGo + VScode

VScode 是微软推出的一款轻量级文本编辑器，常年位居「最受欢迎的编辑器」前三甲。使用人数众多、扩展丰富、社区活跃、生态良好。

不少人将 VScode 作为「All in One」生产力和效率工具，不光用来写程序，它同时还具备写作（Markdown）、做计划（看板）、画流程图、查阅 PDF 或 Office 文件等不同功能，甚至还有插件可以用来看知乎、小霸王游戏机、查股票等「高级操作」。

我也在常在 VScode 写 Markdown 文章，所以也希望可以配置好图床，上传图片后，直接在光标处粘贴。

VScode 有 PicGo 插件，需要在扩展市场先安装再配置。

![](https://cdn.sspai.com/2021/04/01/cf38f0bec1c0da758c569f6ed8ce9922.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

插件安装完成之后，进入设置界面，搜索 picgo，默认使用 SMMS 图床，如果有其他不同需求，可以更改。

![](https://cdn.sspai.com/2021/04/01/b3a698d7224cbb9aea119d45c0720b82.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

比如，下面是我配置的腾讯云图床示例，与 PicGo 界面几乎别无二致。

![](https://cdn.sspai.com/2021/04/01/1d5d08a0406e2ae26b03df0b9d5e76ca.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

安装完成之后，即可使用快捷键 `Control + Shift + U ` 完成上传。看起来这与不配置插件并无区别，同样是截图 -> 快捷键上传 -> 粘贴，流程一步没少，没区别啊。

实际上，这款插件主要是给那些没有安装 PicGo 客户端。却又使用 VScode 写 Markdown 文章的读者有用，通过插件，可以实现与客户端相同的功能。

### PicGo + Typora

在 Windows 平台，我更常用的 Markdown 编辑器就是 Typora 了。

[Typora](https://typora.io/) 是一款简洁高效、功能全面、执行优雅的全平台 Markdown 编辑器，也是目前最受欢迎本地 Markdown 编辑器之一（尤其是在 Windows 下）。它将源码编辑和实时预览合二为一，真正实现了「所见即所得」的渲染效果，给予作者极高的沉浸体验。

除了上述功能，Typora 搭配 PicGo 可以实现比 VScode（或者其他笔记工具） 写 Markdown 文章更流畅的体验了。

直接拖拽或直接将剪贴板图片粘贴进 Typora，便可实现自动上传到 PicGo 图床。实现的效果是这样的：

![](https://cdn.sspai.com/2021/04/01/21e64c9d55aad7a541d75f95ff461333.gif)

打开 Typora，使用快捷键 `Ctrl + ,` 进入偏好设置，找到「图像」修改相应设置选项。

1.  插入图片勾选「对本地图片应用规则」「对网络图片应用规则」「插入图片自动转义」。
2.  上传服务设定：PicGo (app)，PicGo 路径：自己主机中安装的 `PicGo.exe` 的存放路径。
3.  点击「验证图片上传选项」。

![](https://cdn.sspai.com/2021/04/01/9adb29e2f0fe7a467e4fd070259d934c.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

![](https://cdn.sspai.com/2021/04/01/82316683f20137735246e06e0cd61091.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

此时，PicGo 里多出两张图片，就说明连接成功了。

![](https://cdn.sspai.com/2021/04/01/fdcd6b24b21859366d8b6a409714130a.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

在这篇文章《[还在用 Word 做笔记？Markdown 开启你更高效工作的第一步](https://mp.weixin.qq.com/s/2jxi-lyJIfMrXo6i7DMhsQ)》中，我介绍了 Typora 的下载安装、基本界面与使用，如果你对这部分感兴趣，不妨先读读它。

## 结语

其实上面详述的免费图床（SMMS、Gitee）就很好用，在我使用它们的近一年来，基本没有出现很大问题。如果你刚学使用 Markdown 不久，并且也是刚刚了解「图床」这个概念，那么我建议你搭建自己的免费图床先用用，或许有了更高要求再转付费平台也不错。

## 参考文献

-   [什么是对象存储？COS 能做什么？这里有份快速入门教程](https://sspai.com/post/59704)
-   [如何简快好省地搭建图床](https://kaopubear.top/blog/2019-12-14-haotouseimagehosting/)
-   [PicGo 官方配置文件](https://picgo.github.io/PicGo-Doc/zh/guide/)
-   [PicGo - 免费开源的图片上传与管理工具 (Markdown写作贴图 / 跨平台图床应用)](https://www.iplaysoft.com/picgo.html)
-   [PicGo v2.2 更新，快捷键系统与一波插件推荐](https://sspai.com/post/58223)
-   [VS Code 代码编辑器入门指南下篇-场景化应用介绍](https://kaopubear.top/blog/2020-04-21-usevscodepart2/)
-   [Scoop安装详解](http://boyinthesun.cn/post/scoop/)
-   [在 Windows 10 上安装软件，你现在可以用微软的包管理工具了：WinGet](https://sspai.com/post/60592)

\> 下载 [少数派 2.0 客户端](https://sspai.com/page/client)、关注 [少数派公众号](https://sspai.com/s/J71e)，解锁全新阅读体验 📰

\> 实用、好用的 [正版软件](https://sspai.com/mall)，少数派为你呈现 🚀
