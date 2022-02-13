# [真正提升你的输入效率，从用好 Alfred 的这个功能开始：Alfred Snippets - 少数派](https://sspai.com/post/46034)

如果要推荐 Mac 上的效率工具，Alfred 绝对榜上有名。这款快速启动器经常出现在各种「Mac 必备工具」榜单上，少数派也对它进行过 [详细介绍](https://sspai.com/search/article?q=alfred)。

Alfred 之所以出色，除了它支持海量的拓展 Workflow，很重要的一点是它本身的自带功能也很优秀。3.0 版本中加入的文字快速拓展功能，可以帮我们把一些日常经常使用的信息，例如个人邮箱、地址和联系方式等保存成 Snippets，之后每次只需要打几个简单的字符就能快速输入完整内容。

![](https://cdn.sspai.com/2018/08/07/815d6c9f35d5af1df107e7a51f506fc2.gif)

举个例子

## 创建你的第一个 Snippets

（注：Snippets 为 Alfred 的付费功能，需购买 [Powerpack](https://www.alfredapp.com/powerpack/) 后才能使用。）

打开 Alfred 的设置菜单，找到 Features 里面的 Snippets，你可以看到下图这个设置面板：

![](https://cdn.sspai.com/2018/08/07/2bf59a786c43f7c7761d00a925e828e1.jpg?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

设置

想要创建 Snippets，首先要建立一个 Collection（集合）。点击左侧 Collection 底部的 「+」号，输入集合的名字。

在设置中，你可以选择是否为这个 Collection 设置一个前缀或者后缀，这个功能的主要目的是为了方便区分，当你在使用时，通过输入前缀或者后缀可以快速显示某一个集合内的所有 Snippets。我们在这里给这个名为「Personal」的 Collection 添加一个「!」作为前缀。

![](https://cdn.sspai.com/2018/08/07/9b4adfc97c8d87711d95e26d64de84ac.jpg?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

添加 Collection 后，就可以来创建你的第一个 Snippets 了。点击右侧底部的「+」，输入 Snippets 的名字和关键词，然后在下方输入你希望拓展的内容。在这里以添加个人邮箱为例，在上面的 Keyword 里填入「GM」作为关键词，然后下方输入 myemail@gmail.com，点击 Save 来保存，这样我们就创建了一条 Snippets。

![](https://cdn.sspai.com/2018/08/07/2afb774daa296cfd0fb567c84c50dd1b.jpg?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

## 如何使用 Snippets

接下来我们来看看如何使用刚刚创建的 Snippets。回到 Snippets 设置菜单，在右上角你可以找到 Automatically expand snippets by keyword，打开这个选项后你才能在 macOS 中直接输入关键词来进行拓展（第一次打开时需要在「系统设置 - 隐私 - 辅助功能」中开启服务），否则就需要每次手动进行粘贴。

如果你希望在一些应用中关闭 Snippets 拓展，可以选择 Auto Expansion Options，打开 Finder 里的应用程序，将希望关闭 Snippets 的应用拖到列表里即可。

![](https://cdn.sspai.com/2018/08/07/e00502b2fce4d3476c918ea862033686.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

设置完成后，你就可以在 macOS 里输入关键词「!GM」使用 Snippets 了。值得一提的是，Alfred 的快速拓展功能支持中文输入法下使用，这样你就不用来回切输入法了。

![](https://cdn.sspai.com/2018/08/07/ade8289592e9bcce9edb919158dc67a2.gif)

效果

## 进阶一步，用 Snippets 输入年月日

写邮件的时候，往往需要在最后附上当天的日期。对于这个需求，我们也可以用 Snippets 来完成。

![](https://cdn.sspai.com/2018/08/07/222d90ddd23428b9860a85445b8c88ec.gif)

创建一个新的 Snippets，点击下方左侧的 { }，选择 Date and Time。在这里你可以看到一些默认设置好的动态占位符，你可以根据需要选择显示日期（Date）、时间（Time）和同时显示日期和时间（Date and Time）。

![](https://cdn.sspai.com/2018/08/07/451c7b09a1c10e340fc6c93e6a1d3e05.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

这里需要注意的是时间显示的格式。你可以看到在选项的后面有 full、long、medium 和 short 这四种格式，这分别对应着系统设置中「语言和地区」选项里的四种格式，如果有需要的话可以根据情况自己修改。

![](https://cdn.sspai.com/2018/08/07/9187ec97311bdca76c09f86c5a63e840.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

略有遗憾的是，目前 Alfred 的时间输入还不支持中文，所以一般都会使用 short 的格式进行显示，希望日后 Alfred 可以更新支持。

## 配合剪切板，用 Snippets 进行批处理

Alfred 内置了一个剪切板工具，我们可以用它来配合 Snippets 使用。例如，一位 HR 需要向多位面试者发送邮件，根据面试安排表告知他们明天上午或者下午来公司参加面试。那么我们通过创建一个 Snippets 来提高写邮件的效率。

首先，我们需要前往 Alfred 设置，找到 Feature 中的 Clipboard，打开 Clipboard History 中的 Keep Plain Text。

![](https://cdn.sspai.com/2018/08/07/d122b5cc29a99fc48820d2b680bc8369.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

然后创建一个新的 Snippets，关键词为「MfI」，输入以下内容：

```
{clipboard:2} 先生/女士，感谢您对我司的关注！我们收到您的求职简历，很高兴地通知您已经通过我公司的初步筛选，特邀请您前来参加我公司组织的首轮面试，具体安排如下：面试时间：2018 年 X 月 X 日 {clipboard:1}面试地点：XXX 路 XXX 号 XXX 大厦 {clipboard:0}……
```

这里的 `{clipboard:0}`、`{clipboard:1}` 和 `{clipboard:2}` 指的是剪切板里的第一、第二和第三条信息（在 Alfred 的剪切板中，0 代表第一条信息）。

![](https://cdn.sspai.com/2018/08/07/adbfc86f4da58b1adf31ab8bc8885434.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

建立这个 Snippets 后，我们只需要依次复制表格上参与面试者的姓名、面试时间和面试地点，然后输入关键词「!MfI」即可快速生成邮件。

![](https://cdn.sspai.com/2018/08/07/42afe7b217769411a6e9fa929218318f.gif)

## 小结

Alfred 的文字快速拓展功能还有很多用法，本文介绍的只是其中的「冰山一角」。目前 macOS 上有不少专门用于文字拓展的付费工具，例如 [TextExpander](https://sspai.com/post/33636)（一年订阅价为 39.96 美元） 和 [Typinator](http://www.ergonis.com/products/typinator/)（售价 24.99 欧元），但它们售价相对比较昂贵，同时功能繁多。

对于没有重度使用需求的用户来说，Alfred 的文字快速拓展功能非常适合用来处理日常的重复信息输入，你也不需要额外再购买和安装一个 App 来解决这个需求。

最后，为了帮助你更快地上手文字快速拓展功能，Alfred 官方提供了一些实用的 [Snippets 集合](https://www.alfredapp.com/extras/snippets/)，包括 Mac 符号（例如 ⌘）、Emoji 和颜文字等等，下载后直接点击即可添加使用。

你可以前往官网免费下载 [Alfred](https://www.alfredapp.com/)，Snippets 等高级功能需要购买 Powerpack 后才能使用，个人版售价 19 英镑。

\> 下载少数派 [客户端](http://sspai.com/s/nqQk)，关注 [少数派公众号](http://sspai.com/s/KEPQ)，让你的数字设备更好用 📱
