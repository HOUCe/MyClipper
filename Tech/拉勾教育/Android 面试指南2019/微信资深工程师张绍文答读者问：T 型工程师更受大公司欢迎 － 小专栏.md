---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/8024963571
author: 
---

# [微信资深工程师张绍文答读者问：T 型工程师更受大公司欢迎 － 小专栏](https://xiaozhuanlan.com/topic/8024963571)


这是答读者问第一期的最后一个文章了，本文主要是大家问问题最多的一个作者也是Android 面试指南最资深的一位工程师张绍文给大家做的解答集合，本文依旧采取隐去提问者名称的方式。

![](https://images.xiaozhuanlan.com/photo/2017/2479cdfc32b6dc79556d6462500890f8.png)

> 问 2 个比较具体的问题好了：  
> 1.学习 kotlin 开发目前是不是有必要的，在面试时会加分吗？  
> 2.移动开发与深度学习结合那些东西是可以实践一下的，求教求教？

**张绍文：**

1、微信当前在一些页面也在尝试使用kotlin，而面试是否会加分取决于你面试的公司以及你自身的工作年限。例如在微信对于工作三年以内的候选人，我们重点考察基本编程能力，使用何种语言并不是关键。

值得一提的是，在第一届Kotlin专题会议kotlinConf上宣布kotlin会[同时支持iOS与Web开发](https://mp.weixin.qq.com/s/yBBAl62tG9qYa39x1uPumA)，结合JW大神到Google大力推广kotlin，它的确解决了java开发的一些痛点，提升了开发的效率。所以说尽管目前仍存在一些问题，但是kotlin还是一个非常有前途的语言，可以在一些新的业务或者项目尝试使用。

2、深度学习AI目前是热点，应用场景也逐渐丰富起来。例如优图最近的图像还原项目，[腾讯QQ空间超分辨率技术TSR：为用户节省3/4流量，处理效果和速度超谷歌RAISR](https://www.leiphone.com/news/201710/c0GICjRacVyzHKIM.html) 。Android 8.1提供了神经网络API，深度学习还是未来比较重要的方向，但是它真正入门与进阶的门槛还是比较高的。Rogue可以尝试使用tensorflow/caffe这些主流框架实现简单的相册分类，语义识别等应用。

___

![](https://images.xiaozhuanlan.com/photo/2017/694e619e0e48160fb4dc99634993dc65.png)

> 我明年准备面试，工作 3 年了想面个大厂，现在那面试官会更注重问哪方面呢？是算法还是那些框架原理？还是Android源码？还是看你做过的项目经验？我知道肯定都会问到，那么问的那一方面偏多呢？因为精力有限，想在一方面深入了解一下，突出自己的一技之长。

**张绍文：**对于大公司来说，特别是工作3年内候选人，我们关心这个人当前能力的同时，更期待他的可塑性，即未来这个人可能达到什么样的高度。当然这不能一概而论，不同的公司面试的侧重点不太一样，跟面试官的个人喜好也有比较大的关系。建议可以找到相关的熟人，了解一下具体感兴趣的公司、职位的面试流程与侧重点。对于微信或者我个人来说，主要考察以下三点：

1、基础与算法；候选人是否可以写出高质量的代码，对于常用算法的熟悉情况与整个思维过程。对于T3以下的候选人基础与算法尤为关键。

2、项目经验；这一块主要挖掘候选人过去的工作情况，主要看这个人在过去项目中考虑是否深入、全面，是否有一些令人眼前一亮的点。一般来说，我们比较期待候选人有非常擅长的一个点，在这个点做过大量的工作与深入研究。

3、主动性；这里例如开源项目、文章积累还有对社区的一些贡献等。我们希望候选人在完成自己日常工作之外，可以主动承担更多的挑战，去做更多的尝试。

___

![](https://images.xiaozhuanlan.com/photo/2017/6af5b557a94c05d5fe9c238743236948.png)

> 自我介绍：应届毕业研究生，目前刚参加完秋招找到工作，也是 Android 岗位，我目前的状态是：学习过Android基础知识，进阶的知识也看过，比如 Handler 机制、事件分发机制等，跟着视频教程和书籍做过两个较为完整的小demo，但都是跟着视频一点点敲得，我目前的困惑主要有两个：
> 
> 1、如果让我单独实现一个功能，而不参考别人的代码，感觉会无从下手，不知道该从何写起，该使用哪些组件、哪些API来完成这个功能。这一阶段要如何度过呢，很多人给的建议是多些代码，可写些什么代码呢？看视频教程和基础书籍这一步我已经做了，都是跟着作者一步步写，感觉如果单独让自己来实现还是困难。
> 
> 2、第二个问题是关于职业规划的，先说说我的理解吧：  
> 我觉得Android开发者在技术上主要有以下几个方向：其一是在APP开发这个方向上不断进阶，不断学习应用层开发的各种技术，包括原生开发、ReactNative、前端技术等等，能够写出性能较好、UI酷炫的功能代码，然后结合某一业务方向，将来可以往产品经理这个方向发展；
> 
> 其二是往Android底层发展，可以做系统的定制优化相关的工作，这方面对应用层的开发要求就不是特别大，而且可以专注底层，深入下去，不用再去学习前端、Reactnative等一系列新的开发方式，专注深度而不是广度。而且这一领域也可以结合相关业务，比如手机，或者对性能要求非常高的APP。
> 
> 其三是结合前景比较好的AR/VR，或者深度学习等技术，将来移动端肯定都会使用上这些技术的，而且现在也有一些产品已经在用了，不过这个是不是应该属于后端或者其它专业领域的范畴？
> 
> 以上是我之前思考过的一些内容，因为还没出校门，眼界也很窄，所以希望前辈们能指点一下，因为我觉得能在一开始有个较为明确的发展方向，不会等到三年之后再迷茫，而且可以从现在就开始积累相关的技术和业务，我觉得能够对某一领域的业务非常熟悉，再结合技术，应该发展还不错。而且我目前求职的公司也做手机，所以想往系统这个方向发展，不知前景如何，毕竟手机在移动领域的地位仍然很重要，唯一担心的就是将来会出现新型的智能手机，出现新的操作系统把Android给颠覆了，就像当年塞班倒下一样。
> 
> 写的有点多，在此深表感谢！！！

**张绍文：**  
首先刚毕业的学生来说，核心在于基础能力的锻炼，而且更加无需担心 Android 系统是否会被颠覆。在微信中，之前负责塞班平台的同事现在依然活跃于微信的很多核心岗位中。

对于第一个问题，事实上我也经常会遇到这种情况，写代码的时候也会忘记一些 API 的用法，记不清一些看过代码的具体实现方式。但是其实关键是我们能掌握学习的方式，即使是暂时忘记了一些细节，遇到类似的问题时解决的速度也会快很多。这里我的建议是除了多看，更重要的是真正的去实践，学会去用，去优化(不仅仅是star，更要学习pr)。

对于第二个问题，在微信，我们比较期待候选人是属于T型人才。即在某一方面钻研比较深，同时广度也不错。也推荐北方的郎看看文章[谈谈腾讯的技术价值观与技术人才修炼](https://mp.weixin.qq.com/s/Vn0eKvY5AU1DEOrxbOxABQ)。对于Android来说，虽然平台技术发展相对缓慢，但是大前端跟精细化的运营还有许多需要解决的问题。另一方面，Android与音视频、[AR技术](https://zhuanlan.zhihu.com/p/30370146?from=timeline&isappinstalled=0)、AI的结合未来的想象力更大。但这这一块无论入门还是深入门槛相对较高，涉及个人的基础以及所在平台等因素。

![](https://images.xiaozhuanlan.com/photo/2017/4a435b932e6639bf6b3296c2ec7cbc58.png)

___

> 基本情况：创业小公司工作了 2.5 年，技术水平不上不下，现在杭州，打算年后换城市回广州或深圳……几个问题：
> 
> 1、首先想大概了解下广深 Android 就业的行情（3年左右的薪资水平，Android 招聘需求情况等），在广深的作者有了解的话，可以回答下；
> 
> 2、对于大公司金三银四的社招招聘流程（笔试、面试流程、招聘应聘规模，和校招区别等）有了解的作者，麻烦可以介绍下；(其实这个专栏讲到大公司社招的内容太少了) [@宅男潇涧](https://xiaozhuanlan.com/u/undefined) 有了解腾讯的话，可以多介绍点；
> 
> 3、各位大佬对于自己面试的大招 (技术亮点) 或者说作为面试官希望看到的大招，能不能举2-3个栗子并附带一下实践的方法？
> 
> 4、各位大佬，对于面试中说看过 Android 源码的话，必须要读懂哪几个模块？  
> 麻烦各位大佬有空可以指点下，谢谢！:)

**张绍文：**  
1、 从腾讯或者微信的一些招聘职位来说，移动开发的岗位的确减少了很多，但是有还是有的。薪资这块工作2.5年，在腾讯职级对应的大约在2.1-2.2之间，具体的数目不同人之间差距较大，不太多对比。

2、社招规模这个不太好说，这个都是根据项目的需要动态调整。面试的流程各个公司都不太一样，一般都需要笔试、2-4轮面试。如果对于大公司来说，寻找熟人内推的成功率会相对高一些。

3、对于微信的招聘来说，我们主要考察以下三点：

a. 基础与算法；候选人是否可以写出高质量的代码，对于常用算法的熟悉情况与整个思维过程。对于T3以下的候选人基础与算法尤为关键。

b. 项目经验；这一块主要挖掘候选人过去的工作情况，主要看这个人在过去项目中考虑是否深入、全面，是否有一些令人眼前一亮的点。一般来说，我们比较期待候选人有非常擅长的一个点，在这个点做过大量的工作与深入研究。

c. 主动性；这里例如开源项目、文章积累还有对社区的一些贡献等。我们希望候选人在完成自己日常工作之外，可以主动承担更多的挑战，去做更多的尝试。

___

![](https://images.xiaozhuanlan.com/photo/2017/ab9e8f28619396860673362d1c2c2c70.png)

> 我是15年毕业的，因为个人原因回杭州发展，校招进了现在的国企，从事Android开发，基本处于没人带的状态，自己熟悉项目代码，做需求，渐渐的变成项目Android端的负责人，但是自觉技术不够深入，比如没写过开源库，仍然有很多不明白的东西，希望能在大牛的带领下得到更好的成长。
> 
> 另外，自己心里明白目前的项目没有发展，想去大厂做些确确实实在解决一些实实在在的问题的事情。请问在大公司工作的大牛会怎么看待这种求职者？

**张绍文：**  
事实上，大厂不是都一定比创业公司强，我们需要看项目组的产品、技术氛围等比较多的因素。但是以你现在的情况来看，如果个人长期得不到发展，的确需要尝试为未来做考虑。可以多咨询，尝试找一些产品高速发展或是技术氛围比较不错的地方。

社招一般不太care学历与背景，当然在多个候选人水平差距不多的时候，我们还是会优选选择背景相对较好的。所以这边我们需要表现的更好，打铁还需自身硬。个人建议多看，多实践，多总结，快速提升自身实力才是硬道理。

___

![](https://images.xiaozhuanlan.com/photo/2017/861665ce5afda70e62abc584f116a37e.png)

> 先介绍下自己的情况，普通二本 计算机 13年毕业，到17年底算是4年半 Android 经验，一直在二线城市一家创业公司做智能家居方面的开发，对Android应用层开发掌握的还行，特别深入的 framework 层知道点皮毛。准备18年中旬去北京，看过很多 BAT TMD 的面试题和经验，感觉对于 Android 方面的知识自己都能答上来，但是算法，JVM 等方面很欠缺。所以我想问下，现在的一线大厂面试时掌握哪些知识，掌握到什么程度才能有把握，还有现在的薪资行情怎么样。

**张绍文：**  
薪资这块不同人差距很大，各个一线大厂都给得起钱，关键是候选人可以值多少钱。面试的题目随机性比较多，以微信来说主要面试的点有以下三个:

1、 基础与算法；候选人是否可以写出高质量的代码，对于常用算法的熟悉情况与整个思维过程。对于T3以下的候选人基础与算法尤为关键。

2、 项目经验；这一块主要挖掘候选人过去的工作情况，主要看这个人在过去项目中考虑是否深入、全面，是否有一些令人眼前一亮的点。一般来说，我们比较期待候选人有非常擅长的一个点，在这个点做过大量的工作与深入研究。

3、主动性；这里例如开源项目、文章积累还有对社区的一些贡献等。我们希望候选人在完成自己日常工作之外，可以主动承担更多的挑战，去做更多的尝试。
