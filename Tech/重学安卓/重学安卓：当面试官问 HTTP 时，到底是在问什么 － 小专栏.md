---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/7184635029
author: 
---

# [重学安卓：当面试官问 HTTP 时，到底是在问什么 － 小专栏](https://xiaozhuanlan.com/topic/7184635029)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

## 前言

自从 2007 年起 iPhone 和 Android 手机的相继问世，以及 2013 年 4G 网络的正式商用，使得在全球范围内催生了全新的 “**移动互联网**” 时代。

这个时代打从一开始就与互联网产生紧密联系，通过移动互联网，我们得以尝试许多不同以往在 PC 端上做的事，例如

> 上街买菜时，我们可以扫码解锁共享单车，可以给摆摊的老板扫码支付；
> 
> 工作生活中，可以在通勤路上刷短视频、可以在午餐时间点外卖；
> 
> 外出旅游时，可以通过社交软件分享一路上拍摄的美景等等。

![图例截自 “高德地图、抖音、酷安” 客户端](https://images.xiaozhuanlan.com/photo/2021/b0c6c769d07dd8de546fd8b0c0c3adc0.png)

图例截自 “高德地图、抖音、酷安” 客户端

与此同时，通过上一期我们在[《存储访问适配》](https://xiaozhuanlan.com/topic/5240638917)篇分享的关于当下 “中国移动互联网流量接入” 的统计图可知，近几年随着 4G 网络的普及，人们对移动互联网的需求呈指数级攀升，

所以 **对客户端开发来说，对网络请求乃至 HTTP 的掌握是重中之重，许多面试也经常会将此类技术作为考察的重点**。

然而通过过去一年的交流发现，有不少小伙伴存在 “**框架会用，玩的贼 6，但基础知识一问三不知**” 的情况：

> “感觉自己会用啊，好像 HTTP 也没什么可学的呀，好像也感觉不到自己哪不会啊”，但当面试时面试官一问 “**谈谈对 HTTP 的理解**”，瞬间就懵了，毫无头绪，只得默默吃暗亏 ——
> 
> 用是会用，答却答不上来，那到底是会还是不会呢？怎么才能让对方 “心领神会” 呢？

所以在遭遇类似情况时，我们不妨首先跳出来想一想：**面试官问这类问题的时候，究竟是在问什么？到底怎么回答，才能四两拨千斤地通过面试？**

## 比起技术点，更多的是对思维模式的考察

事实上，比起具体的技术点，面试官更关心的是借对技术点的考察，来间接评估你惯用的思维模式，从而判断你是否为 面试官乃至部门发展所需 的目标人选。

其实 **换位思考 —— 站在面试官乃至企业的角度想一想** 就不难理解，面试官也是人，他的一天也是 24 小时，精力十分有限，所以就是面试官，也不可能面面俱到，不可能什么技术都通晓，因而在有限的面试时间里，他的主要任务就是 **透过应试者的表述，来瞬间判断表述背后所隐含的思维模式，从而判断值不值得继续交流乃至招纳** ——

> 企业需要的是 “**不可替代的可替代人才**”，啥意思呢？所谓 “不可替代” 就是指 你能提供稀缺价值：例如来了一个新任务，涉及全新的技术栈，普通人一脸懵逼找不到头绪切入，你却因为已具备 **经过面试官认证的 “通用学习能力”**，而能够 **拎清问题的边界** 从而快速掌握和出成果，这就是你的 “不可替代” 之处。
> 
> 与此同时，企业又希望你是个 “可替代” 的人才，也即你能 **快速抓住技术的本质，并分享给其他人**，于是这能够 **帮助企业降低风险**，例如当你生病时，企业可以派几个你带出来的实习生暂时顶一顶。

所以显而易见的，**这都是很现实的问题**，“会 HTTP” 很重要，与此同时， **对 “无论学什么都有机会抓住本质” 的思维模式的训练和掌握** 同样不容忽视。

> 所以这一期，我们继续借着 “**深度思考原则**” 追根溯源，以便形成 **精炼的、可复述给别人听的 “关于 xxx 技术的理解”**。
> 
> 考虑到 “授人以鱼不如授人以渔”，因而就算不去 hold 住面试官，也请务必跟随本文的脚步，将 网络请求乃至 HTTP 的本质及边界 无痛地过一遍！

## 文章目录一览

-   前言
-   比起技术点，更多的是对思维模式的考察
-   **网络及网络请求的本质是什么**
    -   网络的发展史
    -   网络体系结构的由来
    -   所以为什么要设计为分层结构
    -   网络结构中每一层的作用
-   互联网的本质是什么
-   **HTTP 的本质是什么**
    -   **HTTP 问世前的混沌世界**
    -   **HTTP 解决了什么问题**
-   HTTP 的发展都经历了什么
    -   截至目前，HTTP 都能做什么
-   客户端开发主要需要掌握关于 HTTP 的哪些
    -   常用的场景和头部属性
-   综上
    -   “谈谈你对 HTTP 的理解”

## 网络及网络请求的本质是什么

要分析这个问题，我们可以分别 **从 “生活经验” 以及 “技术点的发展史” 入手**。

从当下生活经验的角度出发，轻易地我们可以得知，**网络请求的本质不过就是 “数据的收发”**，例如 打开朋友圈，加载信息流列表，这个过程中首先发生的是，客户端向服务端 **发送** 了对 “信息流列表数据” 的请求，从而有机会 **收到** 来自服务端处理和响应的结果。

### 网络的发展史

从发展史的角度出发，我们得知，计算机乃至计算机网络，是 “**军用技术转民用**” 的科技产品之一，计算机及其网络的存在和发展，都与时代背景有着密不可分的联系。

> 众所周知，1946 年问世了第一台电子计算机，其存在缘由是美国军方用于 **辅助完成弹道的测算**。这个时期的计算机都是单机运行。
> 
> 到了上世纪 60 年代，由于苏联 “古巴导弹危机” 的压力，使得美军迫切需要一种方式，使能够维持全国多地 作战指挥所 “**两两之间**” 的通信和信息共享（也即要求例如指挥所 A 被炸了，其他指挥所之间仍能相互通信）。

![](https://images.xiaozhuanlan.com/photo/2021/11a709ca51668b0d019ca711d5bbc6f7.png)

计算机网络应运而生。

### 网络体系结构的由来

同任何一门经久不衰的技术一样，计算机网络的存在并非一蹴而就。随着计算机网络概念设计的流出，美国的一些商业公司 和世界上其他国家的组织 都纷纷效仿，试图制定标准和抢占市场。

所以这种 能够满足人类需求的科技产品 一旦流入市场，就不再单纯的只是学术用途，多多少少会 **被卷入商业的演化进程**中，最终谁制定的标准更容易被行业广泛接受，谁便延续了下来。

因而，当我们这一代人接触互联网时，我们便已经是在使用 延续下来了的 **基于 “TCP/IP 4 层体系” 设计的计算机网络**。

![](https://images.xiaozhuanlan.com/photo/2021/097019dba7b64d63ec7e833b9cc99c9b.png)

> 注意它是一种商业标准，与之对应的是学术界的 “OSI 7 层体系设计”。我们客户端开发既然是民用，显然使用的是商业标准。

### 所以为什么要设计为分层结构

这个问题很好解答，分层在本质上涉及了 “封装”，也即 “对调用者屏蔽细节 / 对调用者透明”，类似于对 设计模式原则中 **“最小知道原则” + “依赖倒置原则”** 的应用，

在网络体系的分层结构我们看到，它们的关系是上层调用下层，用 Java 来描述，即 “上层无需知道下层的实现，上层只需调用下层的接口，下层的实现可以根据实际需要来替换”。

如此设计的好处是什么呢？

> 正如我们在[《MVP、MVVM 关系精讲》](https://xiaozhuanlan.com/topic/6105792348)篇文末关于 “依赖倒置” 的加餐中解析到，“**调用抽象而非实现，可以使具体实现被替换时，调用者本身一行代码也不用改**”，
> 
> 类推到 “网络体系结构” 即，“**上层调用下层的抽象，使得下层的具体实现被替换时，TCP/IP 结构本身及上层协议本身都不受牵连**”。
> 
> （例如现如今我们开始推广的 5G 通信网络，实际动刀的就是最底层的 “网络接口层”，然而整个 TCP/IP 结构和上层的协议都不因此受影响。）

这就是 “封装” 乃至 “分层” 的存在意义。

> 划重点 👆 👆 👆

### 网络结构中每一层的作用

所以在有了 TCP/IP 网络分层结构后，人们对每一层的作用有了如下的约定：

**1.应用层：**用于约定通信双方构造和解析元信息（MetaData）及主体数据，通俗来说就是，将数据存放在信封中，信封上填写接收方赖以解析的注意事项，从而接收方的机器在收到信封时，能自动根据 “**彼此预先约定好的程序**” 来读取注意事项从而将数据解析还原。

**2.传输层：**用于在信封上填写目标机器的端口，和标注通信双方 **连接和传输控制方式**。

**3.网络层：**用于在信封上填写目标机器的网络地址，以便于在网络上**寻址和路由转发**。

**4.网络接口层：**用于在信封上填写附近邮局的硬件地址，以便信件首先被送往邮局中转。

### 互联网的本质是什么

至此，我们知道了，互联网的本质是 “**分布式的通信网络**”，诞生于冷战背景下 **“指挥所” 对通信时容灾的需要**。

并且，为了方便修改特定层的内容而不影响其他层，而引入并最终确立了分层结构。

## HTTP 的本质是什么

所以显而易见的，在谈到 HTTP 的本质时，首先它是一种应用层协议。

### HTTP 问世前的混沌世界

上世纪八十年代，受限于 “芯片和操作系统” 研发能力的薄弱，计算机的造价十分昂贵，是少数国家的 少数公司及学术机构 才用的起的 “高精尖设备”。

所以人们对计算机和 “网上冲浪” 十分佛系，“**能用上就已经很不错了**”，坐在办公室里，**足不出户** 就能通过 SMTP 协议（始于 1982 年）让身处异地总部的领导 在几秒内收到邮件、让会计通过 FTP 协议（始于 1971 年）即可访问分公司最新整理的财务文档。

![图片来自 Unsplash](https://images.xiaozhuanlan.com/photo/2021/e157f62816277b08d207088c7de4169a.jpeg)

图片来自 Unsplash

然而这只是对写字楼里的商务白领来说。

彼时在 欧盟量子物理实验室 工作的科学家们，经常需要互访彼此的最新文档，然而文档中每当看到某个关键信息，正想接着往下看时，发现标注了诸如 “此处不做累述，请打开 `ftp://Document/量子物理/量子纠缠研究报告/1989年/1月/最新成果/ …`” 的提示，于是又要最小化当前窗口，去 “文件管理器” 中逐级寻找 … 难受啊！

### HTTP 解决了什么问题

蒂姆·贝纳斯·李（TimBerners—Lee）再也坐不住了，于是在 1989 年组织了一批计算机科学家，耗时两年将 HTTP、HTML、浏览器 给制定和研发。

这 HTTP，首先它是和 SMTP、FTP 平级的应用层协议，也即它约定了通信双方收发数据的格式，因而正如 \*\*经过长足的演化并最终定型下来 \*\*的其他应用层协议一样，它的报文主要是包含 “**行、头部、主体**” 三个部分，分别用于标识 **请求响应的状态信息；请求响应以 “键值对” 形式传递的约定和属性；以及请求响应传输的数据**。

![](https://images.xiaozhuanlan.com/photo/2021/e39aeec04d65c8178b24217a37e1921b.png)

> 划重点 👆 👆 👆

所以相比 SMTP 和 FTP 等协议，HTTP 究竟是能做什么呢？—— 传输 “超文本”。

初代的 HTTP 就是专门用于约定对 “超文本” 的传输，这 “超文本” 其实就一普通文本，只不过内容中包含了 HTML 标准中约定的可视化交互元素，例如

`<a href="http:112.154.1.123/Document/量子物理/量子纠缠研究报告/1989年/1月/最新成果/">《量子纠缠研究报告 1989年1月 最新成果》</a>`

这段标记文本通过自带 HTML 解析引擎的浏览器一解析，就能直接展示一串深蓝色的 “超链接”，例如 [《量子纠缠研究报告 1989年1月 最新成果》](https://quantum.ustc.edu.cn/web/progress)，从而用户点击这个超链接 就能直接跳转到目标网页 查看目标文档内容，而不再需要像此前一般 翻箱倒柜地在 “文件管理器” 中寻找目标文档。

## HTTP 的发展都经历了什么

在随后的几十年里，计算机和互联网刚好迎来了飞速的发展期，随着 “芯片和操作系统” 研发能力的不断提升，计算机的造价也一路走低，从最初的几万美金一台，到后来的几千甚至几百美金，乃至计算机在寻常百姓家普及开来。

与此同时，由于 HTTP 和 HTML 的直观和使用简便，使得相比 SMTP、FTP 等协议更容易坐拥一大批使用者，乃至 **顺理成章地成为互联网上主流的协议之一，因而 HTTP 得不断改善自己，以便能 hold 住大众的需求**。

### 截至目前，HTTP 都能做什么

从生活经验的角度出发就可推知，我们目前使用 HTTP 协议无非就是用于完成 **对 “文本和二进制数据” 的收发**。

> 也即相比初代 HTTP 协议，最大的区别就是，在主体中允许对二进制数据的承载。

其他技术标准也都是依附于 HTTP 协议发展起来的，例如 头部中有个属性名为 `content-type`，其实就是对应着 “互联网媒体类型 `MediaType`” ，例如我们常常看到的 `application/json`，`text/plain` 等等，都是该属性的值，以便服务端根据读取的该值来选择返回 json 还是普通文本。

## 客户端开发主要需要掌握关于 HTTP 的哪些

这个其实同样从生活经验的角度出发就能想明白。

还是 “网络分层结构” 一节提到的，一旦一个东西涉及商业化，就不可避免地被卷入演化的进程中。当今人们使用的 App，无一不是商业化的产物，因而 **有些东西在演化中延续至今，说明这是个经过千锤百炼的普适设计，是值得引起重视的存在**。

### 常用的场景和头部属性

就 HTTP 客户端来说，目前我们打开一个新安装的 App，无非就是要经历 “注册登录”、“加载信息流列表”、“加载图片音视频”、“缓存图片” 等等，

![例如我们常用的 b 站等客户端的设计](https://images.xiaozhuanlan.com/photo/2021/913fa1d149a7ea0ba23847619e0ef18e.png)

例如我们常用的 b 站等客户端的设计

而这些无非就是涉及 “头部属性” 中的 “认证”（Authorization）、“内容类型”（content-type）、“缓存配置”（cache-control）等等。

因而对于这些内容，如果平时工作中很少接触的话，我们需要 **多给自己一点时间**，规划个半年或一年，专门在网上找相应的 “工具书”、“全栈实战课” 等资料（文末会介绍）来刻意练习，通过步步为营的训练，来 **接触实际开发中潜在的复杂样本，从而将来面试时有更多的谈资可以展开**。

## 综上

互联网的本质是 **分布式** 的计算机网络， 它如此设计的初衷是为了 **支持容灾**。

网络请求的本质是 “**对数据的收发**”。当代的网络请求是基于 **TCP/IP 分层协议**，通过分层，可方便替换下层的实现，而不至于影响到上层。

其中应用层协议的作用主要是，**约定通信双方对数据编解码的格式和属性**，以便顺利地完成数据的编码、发送；接收、解码、处理和回执。

因而，当面试官问及 “对 HTTP 的理解” 时，不妨这么回答：

> Tip：一个小小的建议是，面试时带着纸和笔，**一边说，一边画图示意**，必要的时候可酌情将上述 “网络缘起” 的部分也加进来一起

HTTP 的本质是 **“用于对接超文本传输” 的应用层协议**。

它的存在 **最开始是为了** 配合浏览器和 HTML 来 **优化 “浏览信息时的用户体验”**。

因为我们知道，早期时候人们是基于 SMTP 和 FTP 等协议来分享和查阅信息，从一个文档跳到另一个文档时，往往需要重新到 文件管理器或邮箱 里去找寻，十分不便，HTTP 就是为解决这个问题而出现。

然后由于它所带来的优秀体验，使得很快成为互联网主流技术，所以 **后期基于大众的需要，做了一系列改进**，例如目前 HTTP 主要支持对 **文本以及二进制数据** 的收发，日常 “登录客户端、获取信息流列表、加载图片等等” 都离不开这门技术。

> “之前我们项目中做的 XXXX 功能，就是基于 HTTP 的 XXXX 特性，对此我可以具体展开给你讲一讲吗？” ……

全文完。

> **快捷访问：**
> 
> **[基本功：是随时随地可受用的 深度思考原则](https://xiaozhuanlan.com/topic/9837051426)**
> 
> [GitHub : 重学安卓 配套项目](https://github.com/KunMinX/Relearn-Android)
> 
> [GitHub : Jetpack-MVVM-Best-Practice](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)

**HTTP 资料推荐：**

> **工具书：**
> 
> [MDN Web Docs](https://developer.mozilla.org/zh-CN/docs/Web/HTTP)（Mozilla 组织维护的 HTTP 工具书文档，**是目前世界范围内关于 Web 技术最全、保持更新的文档网站**。除了 HTTP，还包含 HTML、JavaScript 等技术，推荐收藏和随时查阅）
> 
> [图解 HTTP](https://weread.qq.com/web/reader/3da32b505dd9f43da9a1aca) （是一本图文并茂、浅显易懂的 HTTP 工具书，透过它能对 HTTP 的结构和延伸的技术点形成最基本的了解）
> 
> [透视 HTTP 协议](http://gk.link/a/103SQ) （目前我在《极客时间》阅读过的觉得不错的关于 HTTP 协议的专栏，主要是对现代 HTTP 协议（例如 HTTPS、HTTP2.0、HTTP3.0 等）特性的铺垫和解析）
> 
> **实战工具：**
> 
> [Github - OpenBug](https://github.com/OpenBug-Android/OpenBug-Android) （《重学安卓》读者群小伙伴分享的 自己封装的 HTTP 请求框架及周边的工具（如 JSON 容错框架等））
> 
> 后续如有看到觉得不错的工具书或实战课，会于此处继续补充，如小伙伴们在平日里对 HTTP 开发十分感兴趣，并且收藏有个人觉得不错的内容，可随时私信留言或在群里交流。

### 版权声明

> Copyright © 2019-present KunMinX

**本文为专栏私有内容，谢绝转载**。

读者订阅了专栏，即享有了专栏文章的阅读权。

文章 **原创的引言、切入点、思路、结论** 凝聚了作者 KunMinX 本人的心血 及与参与者互动演化的结果，作者本人及作者在文中提名的有效贡献者 对相应的成果享有所有权。

**当您 借鉴或引用文中的引言、切入点、思路、结论 进行二次创作并打算发行时，须注明链接出处**，否则我们保留追责的权利。对此如有疑虑请及时沟通，或在发行前将作品交由本人核对。

当您看见有人 [洗稿](https://baike.baidu.com/item/%E6%B4%97%E7%A8%BF/20862574?fr=aladdin)、[剽窃](https://baike.baidu.com/item/%E5%89%BD%E7%AA%83/9398357?fr=aladdin)、未经授权转载本专栏的文章内容时，请及时向本人举报。

最后感谢您对专栏内容的阅读和喜欢。

