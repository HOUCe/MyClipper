# 跟波利亚学解题(rev#3) – 刘未鹏 | Mind Hacks

source:: http://mindhacks.cn/2008/04/18/learning-from-polya/

**一些故事**

[波利亚](http://en.wikipedia.org/wiki/Polya)在他著名的[《How To Solve It》](http://www.douban.com/subject/1456890/)中讲了这么一个有趣的心理学实验：

> 用一个缺了一条边的正方形围栏围住一只动物（狗、黑猩猩、母鸡、人类婴儿），在围栏的另一侧放上一个被试很想要的物体（对动物来说是食物，对人类婴儿来说是有趣的玩具），然后观察他们各自的行为。发现，狗在扒着围栏吠了几声发现无法通过的时候，不久便学会了从围栏的缺口的那一边绕出去，母鸡则朝着围栏一个劲的扑腾，不会想到绕弯子。此外，人类婴儿很快就学会了绕过障碍；而黑猩猩也学得很快（黑猩猩是和人类最近的灵长类亲属）。这个实验有力的证明了，动物解决问题的能力是进化而来的、天生的、硬编码在大脑的神经元网络里面的。[![clip_image001](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image001-thumb1.jpg "clip_image001")](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image0011.jpg)

事实上，不仅解决问题方面是如此，人类整个认知系统中绝大部分功能从本质上都是硬编码的，能在后天习得的只是“程度”的不同，而不是“本质”的不同。[《动机心理学》](http://www.douban.com/subject/1712350/)中有一个令人印象深刻的一个例子：

> 先给小鼠喝某种甜味水（称为“可口水”），然后用X射线促使其产生反胃感，能使小鼠形成对这种味道的水的厌恶和回避（经典条件反射）。但如果不是在水里面加味道，而是在它喝水的时候伴随强光刺激（即让它喝“光噪水”），然后同样刺激其反胃，却无法使它养成对“光噪水”的厌恶。另一方面，如果不是促使其反胃（身体不适），而是用电击惩罚，则它无法形成对“可口水”的厌恶，而是形成对“光噪水“的厌恶。显然，小鼠对事件之间的关联的归因也具有着某种硬编码好了的倾向。在这个例子中，老鼠的大脑里面硬编码了“将身体不适（内部事件）归因于食物而不是闪光”、”将电击（外部事件）归因于闪光而非食物” 这种逻辑。

而人类也有类似的归因倾向。金出武雄在[《像外行一样思考，像专家一样实践》](http://www.douban.com/subject/1867455/)中也提到，他认为人类的直觉实际上也是计算，捷径式的计[![clip_image002](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image002-thumb.jpg "clip_image002")](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image002.jpg)算，只不过由于我们目前还不了解人类大脑内神经元的全部结构（或者说“感性”的物质基础）这才把“感性”当成人类所特有的；金出武雄的这种观点跟心理学中的认知捷径不谋而合。实际上，越是高等的动物，大脑中用于处理特定问题的硬编码神经元回路就越是多和复杂。例如，达尔文早在[《人类和动物的情绪表达》](http://www.douban.com/subject/1049670/)中就先知先觉的提出了动物情绪的适应价值；[《Mean Genes》](http://www.douban.com/subject/1128662/)列出了用于解决生存繁衍问题的特定认知倾向；[《决策与判断》](http://www.douban.com/subject/1193621/)里面则列出了人类在解决更具一般性的决策问题中的一些系统性的、可预测的认知偏差；而[《Predictably Irrational》](http://www.douban.com/subject/2990015/)更是把这个认识提高到方法论的层面，主张人类的非理性实际上是完全可预知的。事实上，所有这些观点都建立在一个基本事实的基础上，即人类大脑中的千亿神经元是由在漫长的进化过程中被塑造出来的分工明确的、[ad hoc](http://en.wikipedia.org/wiki/Ad_hoc)的一组子系统构成的。

越是高等的动物，解题能力越高，猩猩能够进行某种顿悟，在脑子里就构想出通过堆放墙角的箱子来帮助获取高高吊着的香蕉；而出于进化之树 顶端的人类则具有非比寻常的大脑，在人类整个进化的过程中，解决问题的能力一直在进化，所以说人脑中的神经元最重要的部分是为了解题而存在的也不为过。不同的人只是在解题能力程度上不同，并没有本质上能与不能的差异。

波利亚在《How To Solve It》中另外还举了下面这个例子：

一个原始人站在一条小溪前，他想要越过这条小溪，但溪水经过昨天一夜，已经涨了上来；因此他面临一个问题：如何越过这条小溪。他联想起以前曾经从一棵倒下并横在河上的树木上走过去，于是他的问题变成了如何找到这样一颗倒下并横在溪流上的树木。他环顾四周，发现溪流上没有这样的横着的树木，但他发现周围倒是有不少生长着的树木；于是问题再次变成了：如何使这些树木躺到溪流上。

在这个想像的故事中我们看到了一个问题是如何被一步步[归约](http://en.wikipedia.org/wiki/Reduction_%28complexity%29)的：首先，原始人通过对一个已知的类似问题的联想认识到一个重要的性质：如[![clip_image003](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image003-thumb.jpg "clip_image003")](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image003.jpg)果有一棵树横在河上，我就可以借助这棵树过河。这就将一个无法直接解决的问题转化为了一个新的、已知的、并容易解决的问题。值得注意的是这里“联想”是极其重要的一个环节，联想可以将手上的问题与已知的类似问题联系起来，并从后者中吸取能够利用的方法。联想也能够将与问题有关的定理或性质从大脑的知识系统中提取出来。基本上，如果一个联想能够得到某个性质，而这个性质能够或者将问题往上归约一层，或者将条件往下推导一层，这个联想就是有用的。事实上，如果你仔细注意以下解题的过程，你也许会发现，所有的启发式思维方法（[heuristics](http://en.wikipedia.org/wiki/Heuristics)）实质上都是为了联想服务的，而联想则是为了从我们大脑的知识系统中提取出有价值的性质或定理，从而补上从条件到结论、从已知到未知之间缺失的链环。

**一段历史**

实际上，人类自从进入理性文明以来，不仅在不断的解题，还在不断的对自身的解题方法进行反省和总结。在这条路上，有一个真正光荣与辉煌的梦想，那就是发现人类解题的所有[一般性法则](http://en.wikipedia.org/wiki/Heuristic)，并借此建造出一台能够解决人类能够解决的所有问题的[一般解题机](http://en.wikipedia.org/wiki/General_Problem_Solver)。与物理中的建造永动机不一样，这个梦想并非遥不可及的，自从古希腊哲学家对人类心智的反省思考以来，许多著名的数学和哲学家为此建造了阶梯，[Pappus](http://en.wikipedia.org/wiki/Pappus_of_Alexandria)，亚历山大学派最后一位伟大的几何学家，就曾在他恢弘的八卷本《数学汇编》中描述了其中的一种法则，他将它称为“分析与综合”，大意如下：

首先我们把需要求解的问题本身当成条件，从它推导出结论，再从这个结论推导出更多的结论，直到某一个点上我们发现已经出现了真正已知的条件。这个过程称为分析。有了这条路径，我们便可以从已知条件出发，一路推导到问题的解。

波利亚在他的三卷本中把这种做法叫做Working Backwards（倒过来解）。

[笛卡尔](http://en.wikipedia.org/wiki/Ren%C3%A9_Descartes)也曾经试图将人类思维的规则总结为36条（最终完成了[21条](http://en.wikisource.org/wiki/Rules_for_the_Direction_of_the_Mind)）。[莱布尼兹](http://en.wikipedia.org/wiki/Gottfried_Leibniz)，现代计算机实质上的发明者，也说到：

在我看来，没有什么能比探索发明的源头还要重要，它远比发明本身更重要。

再后来，捷克数学家[波尔查诺](http://en.wikipedia.org/wiki/Bernard_Bolzano)也试图总结人类思维的本质规律，他在他的著作《科学的理论》中写道：

我根本不奢望自己能够提供任何超于其他天才所使用过的科学探索方法之外的新方法，从这个意义上，你别指望能在书中看到什么新的东西。但是，我会尽我的全力去总结所有伟大的思想者们共有的、思维的原则和方法，我认为即便是他们自己在思考的时候也未必全都意识到自己在使用什么方法。

再后来，就到了近代，随着科学技术的进步，心理学最活跃的子学科——[认知科学](http://en.wikipedia.org/wiki/Cognitive_science)——开始辉煌起来，人类开始向思维乃至自我意识的物质基础发起进攻。两位多才多艺的计算机科学家兼认知科学家，[Herbert Simon](http://en.wikipedia.org/wiki/Herbert_Simon)（另外还是经济学家）和[Allen Newell](http://en.wikipedia.org/wiki/Allen_Newell)写出了世界上第一个[一般性解题机](http://en.wikipedia.org/wiki/General_Problem_Solver)的程序（GPS），虽然GPS只能解决很狭窄的一类问题，但这是第一个将“问题解决策略”和“知识”分离开来的程序。显然，在知识之外，人类的思维是有着一些一般性的指导规则的。事实上，波利亚在《数学与猜想》中写道，欧拉是最重数学思维的教学的，欧拉认为如果不能把解决数学问题背后的思维过程教给学生的话，数学教学就是没有意义的。

**一些方法**[![clip_image004](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image004-thumb1.jpg "clip_image004")](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image0041.jpg)

这些一般性的思维方法，就是波利亚用了整整三本书，五卷本（[《How To Solve It》](http://www.douban.com/subject/1456890/)、[《数学的发现》](http://www.douban.com/subject/1850407/)、[《数学与猜想》](http://www.douban.com/subject/1134230/)）来试图阐明的。波利亚的书是独特的，从小到大，我们看过的数学书几乎无一不是欧几里德式的：从定义到定理，再到推论。是属于“顺流而下”式的。这样的书完全而彻底的扭曲了数学发现的真实过程。举个例子，[《证明与反驳：数学发现的逻辑》](http://www.douban.com/subject/2124368/)在附录一中讲了一个非常有趣的例子：[柯西](http://en.wikipedia.org/wiki/Cauchy)当年试图将函数的连续性从单个函数推广到无穷级数上面去，即证明由无穷多个连续函数构成的收敛级数本身也是一个连续的函数，柯西给出了一个巧妙的证明，似乎漂亮地解决了这个问题。然而傅立叶却给出了一个噩梦般的三角函数的收敛级数，它的和却并不是连续的。这令柯西大为头疼，以至于延迟了他的数学分析教程的出版好些年。后来，赛德尔解决了这个问题：原来柯西在他看似无懈可击的证明中非常隐蔽（他自己也不知觉的情况下）引入了一个潜在的假设，这个假设就是后来被称为的“一致收敛”条件。当时我看到这里就去翻我们的数学分析书，发现“一致收敛”这个概念第一次出现的时候是这样写的：定义：一致收敛…

所以说，从这个意义上，[《数学，确定性的丧失》](http://www.douban.com/subject/1049136/)从历史的角度再现了真实的数学发展过程，是一本极其难得的好书。而事实上，[从真实的数学 历史发展的角度去讲授数学](http://www.math.nmsu.edu/~history/)，也是[数学教学法](http://en.wikipedia.org/wiki/Mathematics_education#Methods)的最佳方法。不过，《数学，确定性的丧失》的弱点是并没有从思维的角度去再现数学发现的思维过程，而这正是波利亚所做的。

总结波利亚在书中提到的思维方法，尤其是《How To Solve It》中的启发式思考方法，有这样一些：[![clip_image005](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image005-thumb.jpg "clip_image005")](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image005.jpg)

以上是我认为最重要的，也是最具一般性的、放之四海都可用的思维法则。一些更为“问题特定”的，或更为现代的启发法，可以参见[《如何解题：现代启发式方法》](http://www.douban.com/subject/1232071/)以及所有的[算法书](http://www.douban.com/people/pongba/booktags/%E7%AE%97%E6%B3%95)。不过，在结束这一节之前，还有两个有趣的启发法值得一提：

-   **下意识孵化法**。这个方法有点像老母鸡孵小鸡的过程：我们先把问题的吃透，放在脑子里，然后等着我们的下意识把它解出来。不过，不宜将这个方法的条件拉伸过远，实际上，除非能够一直保持一种[思索的状态](http://blog.csdn.net/pongba/archive/2007/05/24/1624382.aspx "垃圾中文技术性网站")（金出武雄所谓“思维体力”），或者问题很简单，否则一转头去做别的事情之后，你的下意识很容易就把问题丢开了。据说庞加莱有一次在街上，踏上一辆马车的那一瞬间，想出了一个重要问题的解。其他人也像仿效，结果没一个人成功。实际上，非但马车与问题无关，更重要的是，庞加莱实际上在做任何事的时候除了投入有限的注意力之外，其他思维空间都让给了那个问题了。同样，阿基米德从浴缸里面跳出来也是如此；如若不是经过了极其痛苦和长时间的思索，也不会如此兴奋。如果你也曾经花过几天的时间思考一个问题，肯定也是会有类似的经历的。
-   **烫手山芋法**。说白了，就是把问题扔给别人解决。事实上，在这个网络时代，这个方法有着无可比拟的优越性。几乎任何知识性的问题，都可以迅速搜索或请教到答案。不过，如何在已知知识之外发掘出未知知识，如何解决未知问题，那就还是要看个人的能力了。数学界流传一个与此有关的笑话：如果你有一个未解决问题，你有两个办法，一，自己解决它。二，让[陶哲轩](http://en.wikipedia.org/wiki/Terence_Tao)对它感兴趣。[![clip_image009](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image009-thumb.jpg "clip_image009")](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image009.jpg)

除了波利亚的书之外，陶哲轩的[《Solving Mathematical Problems》](http://www.douban.com/subject/1859573/)也对解题的启发式思路作了极有意义的介绍，他在书的第一章遵循波利亚的思路从一个具体的题目出发，介绍了如何运用波利亚在书中提到的各种启发式方法来对解题进行尝试。

总而言之，充分挖掘题目中蕴含的知识，是解题的最关键步骤。本质上，所有启发式方法某种意义上都是为此服务的。这些知识，有些时候以联想的方式被挖掘出来，此时启发式方法充当的便是辅助联想的手段。有时候则以演绎和归纳的手法被挖掘出来，此时启发式方法则充当助探（辅助探索）工具。

**一点思考**

**1\. 联想的法则**

人类的大脑是一个复杂而精妙的器官，然而某种程度上，人类的大脑也是一个愚蠢的器官。如果你总结过你解过的一些有意义的好题目，你会发现它们有一个共同点：没有用到你不知道的知识，然而那个最关键的、攸关成败的知识点你就是想不到。所以你不禁要问，为什么明明这个知识在我脑子里（也就是说，明明我是“能够” 解决这个问题的），但我就是没法想到它呢？“你是怎么想到的？”这是问题解决者最常问的一个问题。甚至对于熟练的解题者来说，这个问题的答案也并不总是很明确的，很可能他们自己也不清楚那个关键的想法是怎么“蹦”出来的。我们在思考一个问题的时候，自己能意识到的思维部分似乎是很少的，绝大多数时候我们能感知到的就是一个一个的转折点在意识层面显现，我们的意识就像一条不连续的线，在其上的每一段之间那个空档内发生了什么我们一无所知，往往我们发现被卡在一个地方，我们苦思冥想，然后一个知识（也许是一个性质，也许是一个定理）从脑子里冒了出来，或者说，被我们意识到，然后我们沿着这条路走一段，然后又卡住，然后又等待一个新的关键知识的出现。而至于这些知识是怎么冒出来的？我们可以对它们的“冒出来”提供怎样的帮助？我们可以在意识层面做一些工作，帮助我们的下意识联想到更多重要的知识吗？那些灵光一现的瞬间，难道只能等待它们的出现？难道我们不能通过一些系统化的步骤去“捕获”或“生成”它们？又或者我们能不能至少做些什么工作以使得它们更容易发生呢？

正如金出武雄在《像外行一样思考，像专家一样实践》中所说的，人类的灵感一定是有规律的，认知科学目前至少已经确认了人类思维的整个物质基础——神经元。而既然它们是物质，自然要遵循物质的运行规律。只不过我们目前还没有窥破它们，但至少我们可以确信的是，它们在那里。事实上，不需要借助于认知科学，单单是通过对我们自己思维过程的自我观察，也许就已经能够总结出一些重要的规律了，也许，对自身思维过程的反观真的是人有别于其它动物的本质区别。

[《专注力》](http://www.douban.com/subject/2296845/)当中有这样一个例子：一天夜里，你被外面的吵闹声叫醒了，你出去一看，发现有一群人，其中有一个人开着很名贵的轿车，他跟你说他们正在玩一个叫“拾荒者”的游戏，由于一些原因，他必须要赢这个游戏，现在他需要一块1.5m\*1m的木板，如果你能帮忙的话，愿以一万美元酬报。你怎么办？被测试的大多数人都没有想到，只要把门拆给他就可以了（如果你想到了，祝贺你:-)），也许你会说现在的门都是钢的，没关系，那你有没有想到床板、立柜的门、大桌子的桌面之类的？这个问题测试的就是心理学上所谓的“范畴陷阱”，“木板”这个名词在你脑子里的概念中如果是指“那些没有加工的，也许放在木材厂门口的，作为原材料的木板”的话，那么“木板”就会迅速在你的下意识里面建立起一个搜索范畴，你也会迅速的反应到“这深更半夜叫我上哪去找木板呢？”如果你一下就想到了，那么很大的可能性是“木板”这个概念在你脑子里的范畴更大，更抽象，也许包含了所有“木质的、板状的东西”。

这就是联想的法则。

我们的大脑无时无刻不在对事物进行归类，实际上，不仅是事物，一切知识，都在被自动的归类。在有关对世界的认知方面，被称为[认知图式](http://en.wikipedia.org/wiki/Schema_%28psychology%29)，我们根据既有的知识结构来理解这个世界，会带来很大的优势。实际上，[模块化](http://www.sciam.cn/article.php?articleid=334)是一个重要的降低复杂性的手段。然而，**知识是一把双刃剑，一方面，它们提供给了我们解决问题的无以伦比的捷径优势**，“砖头是砌墙的”，于是我们遇到砌墙这个问题的时候就可以迅速利用砖头。然而**另一方面，知识却也是思维的桎梏**。思维定势就是指下意识遵循既有知识框架思考的过程。上面的那个木板的例子也是思维定势的例子。每一个知识都是一个优势，同时又是一个束缚。著名的科幻作家[阿瑟·克拉克](http://en.wikipedia.org/wiki/Arthur_C._Clarke)有一句名言：如果一位德高望重的老科学家说某个事情是不可能的，那么他很可能是错的。所以，如何在获取知识优势的同时，防止被知识束缚住，是一门技术。

掌握这门技术的钥匙，就是抽象。在吸收知识的时候进行抽象，同时在面对需要用到知识的新问题时也要对问题进行抽象。就以大家都知道的“砖头”有多少种用途为例，据说这道题目是用于测试人的发散思维的，能联想到的用途越多，思维定势就越小。实际上，借助于抽象这个利器，这类题目（乃至更广的一类问题）是可以系统性的进行求解的，我们只需对砖头从各个属性维度进行抽象。譬如，砖头是——长方形的（长方形的东西有什么用途？还有哪些东西也是长方形的，它们都有什么用途？）、有棱角的（问题同上）、坚硬的、固体、有一定大小的体积的、红色的、边界线条平直的、有一定重量的… 对于每一个抽象，我们不妨联想还有其他什么物体也是具有同样抽象性质的，它们具有同样的用途吗？当然，除了抽象之外，还有“修改”，我们可以在各个维度上对砖头的属性进行调整，以期得到新的属性：譬如大小可以调整、固体可以调整为碎末、棱角可以打磨、重量也可以调整、形状也可以调整… 然后看看新的属性可以如何联想开去。

除了这个简单的例子之外，我们也不妨看一看一些算法上的例子，同样一个算法，不同的人来理解，也许你脑子里记得的是某个特定的巧妙技巧（也许这个技巧在题目的某步关键的地方出现，从而带来了最令人意外的转折点），然而另一人个记得得也许是“递归”这种手法，还有另外一个人记得的也许是“分治”这种更一般化的解题思路。从不同的抽象层面去掌握这道题目的知识信息，以后遇到类似的问题，你能够想起这道题所提供的知识的可能性是有极大的差异的。《Psychology of Problem Solving》的第11章举了这样一个例子：先让被试（皆为大学生）阅读一段军事材料，这个材料是说一小撮军队如何通过同时从几个不同方向小规模攻击来击溃一个防守严实的军事堡垒的。事实上这个例子的本质是对一个点的同时的弱攻击能够集聚成强大的力量。然后被试被要求解决一个问题：一个医生想要用X射线杀死一个恶性肿瘤，这个肿瘤只可以通过高强度的X射线杀死，然而那样的话就会伤及周围的良好组织。医生应该怎么办呢？在没有给出先前的军队的例子的被试中只有10%想到答案，这是控制基线。然后，在先前学习了军队例子的被试中，这个比例也仅仅只增加到30%，也就是说只有额外20%的人“自动”地将知识进行了转移。最后一组是在提醒之下做的，达到了75%，即比“自动”转移组增加了45%之多。这个例子说明，知识的表象细节会迷惑我们的眼睛，阻碍我们对知识的运用，在这个例子中是阻碍问题之间的类比。

而抽象，则正是对非本质细节去枝减叶的过程，抽象是我们在掌握知识和解决问题时候的一把有力的[奥卡姆剃刀](http://en.wikipedia.org/wiki/Occam%27s_Razor)。所以，无论是在解题还是在学习的过程中，问自己一个问题“**我是不是已经掌握了这个知识最深刻最本质的东西**”是非常有益的。

**2\. 知识，知识**

如果你是一个熟练的解题者，你也许会发现，除了一些非常一般性的、本质的思维法则之外，将不同“能力”的解题者区分开来的，实际上还是知识。知识是解[![clip_image010](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image010-thumb.jpg "clip_image010")](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image010.jpg)题过程中的[罗塞塔碑石](http://en.wikipedia.org/wiki/Rosetta_Stone)。一道几何题为什么欧几里德能够做出来我们不能，是因为欧几里德比我们所有人都更了解几何图形有哪些性质，借助于一个性质，他很容易就能抵达问题的彼岸；反之，对于不知道某个性质的我们，倒过来试图“发现”需要这样的性质有时几乎是不可能的。有人说数学是在黑暗中摸索的学科，是有道理的。并不是所有的问题都能够通过演绎、归纳、类比等手法解出来的。这方面，费马大定理就是一个绝好的例子，[《费马大定理：一个困惑了世间智者358年的谜》](http://www.douban.com/subject/1322358/)一书描述了费马大定理从诞生到被解决的整个过程，事实上，通过对费马大定理本身的考察，几乎是毫无希望解决这个问题的，我们根本不能推导出“好，这里我只需要这样一个性质，就可以解决它了”，也许大多数时候我们可以，但那或者是因为我们有已知的知识，或者这样的归约很显然。而对于一些致命的问题，譬如费马大定理，最重要的归约却是由别人在根本不是为了解决费马大定理的过程中得出来的。运气好的话，我们在既有的知识系统中会有这样的定理可以用于归约，运气不好的话，就得去摸索了。

所幸的是，绝大多数问题并不像费马大定理这样难以解决。而且绝大多数问题需要用到的知识，在现有的知识系统里面都是存在的。我们只要掌握得足够好，就有希望联想起来，并用于解题。

当然，也有许多题目，求解它们的那个关键的知识可以通过考察题目本身蕴涵的条件来获得，这类题目就是测试思维本身的能力的好题目了。而如果这个性质根本无法通过对题目本身的考察得出来，那么这个题目测试的就是知识储备以及联想能力。

**3\. 好题目、坏题目**

在我看来，好题目即测试一个人思维的习惯的题目（因为知识性的东西是更容易弥补的，尤其是在这样一个年代；而好习惯不是一朝一夕养成的），它应有这样一些性质：

-   不需要用到未知的知识，或者
-   需要用到未知的知识，但一个敏锐的解题者可以通过对题目的分析自行发现这些所需的知识。
-   考察解题的一般性思路，而不是特定（[ad hoc](http://en.wikipedia.org/wiki/Ad_hoc)）的解题技巧，尤其是当这个技巧几乎不可能在短时间内通过演绎和试错发现的时候。譬如题目需要用到某种性质，而这个性质对于不知道它的人来说几乎是无法从对题目的考察中得出来的。
-   考察思维能力：联想能力、类比能力、抽象能力、演绎能力、归纳能力、观察能力、发散能力（思维不落巢臼的能力）。
-   考察一般性的思维方法：通过特例启发思考、通过试错寻找规律、通过泛化试探更一般性命题、通过倒过来推导将问题进行归约、通过调整（分解、删除、增加等等）题目的条件来感知它们之间的联系以及和结论的联系、通过系统化的分类讨论来覆盖每种可能性。
-   好题目举例：烙饼排序问题（考察特例启发法以及观察能力）、Nim问题（还有简单版本的取火柴问题）（烙饼排序问题和Nim问题可参见[《编程之美》](http://www.douban.com/subject/3004255/)）、9公升4公升水桶倒6公升水的问题（考察倒过来思考问题的能力）、[9点连线问题](http://en.wikipedia.org/wiki/Thinking_outside_the_box)、6根火柴搭出4个面的问题、“木板”问题（考察思维定势，此外《心理学与生活》的第九章也有好几个经典的问题）、许多数论问题（观察能力、演绎能力、归纳能力）。此外，[我们](http://groups.google.com/group/pongba)最近也在[讨论好题目](http://blog.csdn.net/pongba/archive/2008/04/09/2270171.aspx "垃圾中文技术性网站")。

而坏题目呢：

-   好题目各有各的好，坏题目都是相似的。
-   坏题目基本上就是指那些所谓的 unfair questions，什么是unfair，举个例子：一个人住在一栋非常高的楼上，每天早晨他乘电梯下到一楼，出门上班。但晚上回来之后却最多只能坐到一半高度的楼层，剩下一半只能走楼梯上去，除非是下雨天。问为什么。这个例子据说不少人小时候在脑筋急转弯里面做过，但我很怀疑基本上任何正常人是不是可能想出来。这个问题的问题在于他需要用到千百个有可能与问题有关的性质中的一个，而且这个性质还根本无法通过对题目本身的考察得出来，只可能某天我们碰巧遇到类似的场景也许才能想到。知道答案的人也许会说答案很显然，但别忘了心理学上的**事后偏见****——****一旦知道结果之后，所有指向结果的证据看上去都那么显然和充分，而同时所有反结果的证据看起来都那么不显然和不充分**。譬如这题关键是要想到这人是矮子和雨天要带伞，也许你会说“只要考虑一下电梯的按钮面板就会发现了”，或者“看到下雨，那还不想到带伞么？”，然而这只是事后的合情推断。在不知道答案的情况下，这个故事中有数不清的因素可能会成为问题的解释，除非某天我们碰到类似的问题，否则大致也只能一个个穷举了去使劲往上凑，譬如除了身高之外还有：是不是瞎子、是不是聋子、是不是哑子、男人女人、什么牌子的电梯、大厦是哪种大厦？这些因素重要吗？不重要吗？最令人头疼的是，在不知道答案的时候，我们也根本不知道他们重不重要，一个出谜语的人可能从任何一个微小的地方引申出某个谜语来；更头疼的是，我们不知道我们不知道的那些因素是不是也可能与题目的解有关，譬如这样一个问题：一个人走进酒吧，问酒保要一杯水，酒保掏出一只枪，拉上扳机；这人说声“谢谢”，走了出去。这些题目固然有趣，但几乎没有价值。
-   值得注意的是，这样的问题跟著名的[9点连线问题](http://en.wikipedia.org/wiki/Thinking_outside_the_box)和6根火柴搭出4个面的问题（还有《如何解题：现代启发式方法》里面那个经典的“小球在盒内碰撞何时回到原轨迹”的问题）不同，后者的条件都在眼前，并且解的搜索空间无论如何很小，就看思维能不能突破某一个框框。而上面这些问题则是要人进行根本不可能的联想。9点问题实际上是可以系统化思考解决的，但unfair question则像许多谜语一样，随便哪个人都可以出一个另一个人根本无法想出来的谜语，因为从谜语隐含的信息加上人可能从谜语中联想出来的信息，加起来也不足以构成解题的充分条件；这种情况下除非你遇到出题人在出题时的心理或所处情况，否则是无法解的。
-   最后，发散性思维其实是可以系统化的，参见前文“联想的规则”。

出题的误区：

-   最大的误区就是把知识性的题目误当成能力型的题目。如果题目中需要用到某个重要的定理或性质，而对于一个原本不知道这个定理或性质的人来说是无法通过题目本身到达这个性质的，那这就属于知识性的题目。
-   虽然几乎所有题目归根到底都是知识性的，但有些题目更为知识性，尤其是当解题中需要用到的定理或性质并不那么trivial的时候。
-   一个最好的题目就是问题明明白白，而且最终的解也没有用到什么神秘的定理，但要想获知到解，取决于你会不会思考一个问题（参见“好问题”）。譬如烙饼问题和Nim问题，还有许许多多问题简洁明确但很锻炼思考的算法问题。

**4\. 一个好习惯**

在解题的过程中，除了必要条件——知识储备——之外，对于一些并不涉及什么你不知道的定理的题，很大程度上就要看思维能力或者习惯了。而在思考一个问题的时候，最容易犯的一类错误就是忘了考虑某种可能性，不管这种可能性是另一种做法（譬如只顾着构造一个能一步得出结果的算法，没记得还可以从错误情况逼近。譬如只顾着正着推导，却忘了可以反过来推。只顾着反过来推，居然忘了可以考察简单特例。试了各种手法，却发现忘了考虑题目的某个条件。觉得试遍了所有可能性，已经走不下去了，然后其实在思维的早些时候就已经落入了思维陷阱。等等）事实上，即便是一个熟练的解题者也容易犯顾此失彼的问题，因为我们一旦意识到一个看似能够得到结论的解法，整个注意力就容易被吸引过去，而由于推导的路径是很长的，所以很容易在一条路上走到黑，试图再往下走一步就得出解。却忘了回过头来看看再更高的层面上还有没有其它手法，思路上有没有其他可能性。

而对于像我这样目前尚不谙熟所有思维方法的人来说，则更容易犯这样的错误。为了避免这样的错误，一个有效的办法就是将自己的思考过程（中的重要环节）清晰的写在纸上（称为“看得见的思考”），这有如下几个好处：

-   人在思考一个问题的时候，就像是在黑暗中打着电筒往前走（事实上，我们的[工作记忆](http://en.wikipedia.org/wiki/Working_memory)资源是有限的，有研究证明我们只能在工作记忆里面持有[7加减2个项目](http://en.wikipedia.org/wiki/The_Magical_Number_Seven%2C_Plus_or_Minus_Two)；此外[认知负荷](http://en.wikipedia.org/wiki/Cognitive_load)也是有极限的），每一步推导，每一步逻辑或猜测都将我们往前挪一步，然而电筒的光亮能找到的范围是有限的，我们走了几步发现后面又黑了。有时候，我们是如此努力地试图一下就走出很远，同时又老是怕忘记目前已经取得的进展和重要结论，结果意识的微光就在一个很小的范围内打转，始终无法往前走出很远。而将思维过程记录下来，则给了我们完全的回顾机会。如果你是经常做笔记的人，你肯定会发现，有时候一个在脑子里觉得两句话就能说完不需要记下来的东西，一旦开始往纸上写下来，你就自然而然能得出更多的结论和东西，越写越多，最终关于你的问题的所有方面都被推导出来展现在你面前。
-   思考问题时的注意力是[自上而下控制大脑的神经处理过程](http://en.wikipedia.org/wiki/Attention#Neural_correlates_of_attention)的，当我们集中注意力在某一个过程上时，其它的过程就会受到抑制。我们平常都遇到过一些时候，由于集中注意力从而忽略了周围发生的事情的时候（处理环境输入的神经回路受到抑制）。所以，当我们竭尽全力将一些非常重要的因素控制在工作记忆里面的时候，实际上很大程度上抑制了其余的思考——可以想见如果科比在跳投的瞬间集中注意力思考跳投的各种技术要素的话会发生怎样的灾难。此外，这么做还占用了宝贵的工作记忆空间，从这个意义上，借助于纸笔，将思考的东西写下来实际上就是扩充了我们的工作记忆，增大了思维的缓存。注意，这倒不是说思考问题不需要集中注意力，而是说由于将项目维持在工作记忆中需要很大的认知精力，使得我们的注意力无法暂时移开去思考其它相关的子问题，而写到纸上的话我们就减轻了工作记忆的负担，可以转移注意力去集中思考某个子问题；同时我们又可以随时回过头来，重新将以前想过的结论装入记忆（内存），完全不用费心去阻止它们被我们的工作记忆遗忘。
-   一句话从嘴里说出来，或者写到纸上，被视觉或听觉模块接收，再认知；跟在心里默念所产生的神经兴奋程度是不一样的。我们都有过这样的经历：一句令人不愉快的话，我们心里清楚，但就是不愿意自己也不愿意别人从嘴里说出来。同样，将思考的过程写到纸上，能够激起潜在的更多的联想。为什么会这样的另一个可能的原因是我们大脑中思维过程的呈现形式和纸上的表现形式是不一样的，既没有那么严格、详细也没有那么多的符号（如数学符号）——再一次，工作记忆资源是很有限的——而后者，作为视觉线索，可能激起更多对既有知识的回忆。
-   我们在思考问题的过程中容易落入思维定势，不知不觉就走上来某条“绝大部分时候是如此”的思维捷径，对于一些问题而言这固然能够让我们快速得到解，但对于另一些问题而言却是致命的。我们容易在逻辑的路径上引入想当然的假设，从而排除某种不该排除的可能性或做法。通过将思路过程写到纸上，我们便能够回头细细考察自己的思考过程，觉察到什么地方犯了想当然的毛病。
-   我们在思维过程中的每一个关键的一步也许都有另一种可能性，一个问题越复杂，需要推导的步骤就越多，我们就越容易忽视过程中的其它可能性，容易一条路走到黑。而将思维过程写下来，在走不下去的时候可以回过头看看，也许会发现另一种可能性，另一条“少有人走的路”。
-   最后，通过将思维过程写下来，我们就能够在解题完毕之后完整的回顾自己的整个思维过程，并从中再次体悟那些关键的想法背后所发生的心理活动过程，总结思考中的重要的一般原则，分析思维薄弱的环节，等等。就算是最终发现并没有到达结果的无效思路，也未必就没有意义，因为不是因为错误的思路，也不会知道正确的思路，况且对一道题目用不上的思路，对其它题目未必用不上。通过对自己思维过程的彻底反思，就能从每次解题中获得最多的收获。

**5\.** **练习，练习**

本质上，练习并不产生新能力。然而练习最重要的一个作用就是将[外显记忆](http://en.wikipedia.org/wiki/Explicit_memory)转化为[内隐记忆](http://en.wikipedia.org/wiki/Implicit_memory)。用大白话来说就是将平时需要用脑子去想（参与）的东西转化为内在的习惯。譬如我们一开始学骑自行车的时候需要不断提醒自己注意平衡，但随着不断的联系，这种技能就内化成了所谓的[程序式记忆](http://en.wikipedia.org/wiki/Procedural_memory)（内隐记忆的一种），从而就算你一边骑车一边进行解题这样需要消耗大量脑力的活动，也无需担心失去平衡（不过撞树是完全可能的，但那是另一回事）。

同样，对于解题中的思维方法来说，不断练习这些思维方法就能做到无意识间就能运用自如，大大降低了意识的负担和加快了解题速度。

不过，并非所有的练习方法都是等效的，有些练习方法肯定要比另一些更有效率。譬如就解题来说，解题是一项涉及到人类最高级思维机制的活动，其中尤其是推理（归纳和演绎）和联想。而后者中又尤数联想是最麻烦的，前面提到，绝大多数时候启发式方法实质上都是在为联想服务——为了能像晃筛子那样把你脑袋里那个关键的相关知识抖落出来。并且，为了方便以后能够联想，在当初吸收知识的时候就需要做最恰当的加工才行，譬如前面提到的“抽象”加工，除此之外还有将知识与既有的知识框架整合，建立最多的思维连接点（或者说“钩子”）。对于知识的深浅加工所带来的影响，[《找寻逝去的自我》](http://www.douban.com/subject/1315575/)里面有精彩的介绍（里面也提到了提取线索对回忆的影响——从该意义上来说运用启发式思维方法来辅助联想，其实就是进行策略性记忆提取的过程）。最后，人类的无意识思维天生有着各种各样的坏习惯，譬如前面提到的范畴陷阱就是创新思维的杀手，譬如根据表面相似性进行类比也是知识转移的一大障碍。更遑论各种各样的[思维捷径](http://www.douban.com/doulist/127649/)了（我们平常进行的绝大多数思考和决策，都是通过认知捷径来进行的）。所以说，如果任由我们天生的思维方式发展，也许永远都避不开无意识中的那些陷阱，好在我们除了无意识之外还多出了一层监督机制——意识。通过不断反省思维本身，时时纠正不正确的思考方式，我们就能够对其进行淬炼，最终养成良好的思维习惯。反之被动的练习虽然也能熟能生巧，但势必花的时间更多，而且对于涉及复杂的思维机制的解题活动来说，远远不是通过钱眼往油壶里面倒油这样简单的活动所能类比的，倒油不像思维活动那样有形形色色的陷阱，倒油不需要联想和推理，倒油甚至几乎完全不需要意识的辅助性参与，除了集中注意力（而解题活动就算对于极其熟练的人来说也不断需要大量的意识参与）。所以对于前者，良好的思维习惯至关重要，而反省加上运用正确的思维方法则是最终养成良好思维习惯的途径。

练习还有另外一个很重要的作用，就是增加领域知识（关于知识在问题解决中的作用，前面已经提到过）。我们看到很多人，拿到一道题目立即脑子里就反应出解法，这个反应快到他自己都不能意识到背后有什么逻辑。这是因为既有的知识（我们常说的“无他，实在是题做得太多了”）起到了极大的作用，通过对题目中几个关键元素或结构的感知，大脑中的相关知识迅速被自动提取出来。而对于知道但不熟悉相应知识（譬如很早我们就知道归纳法，但是很久以后我们才真正能够做到面对任何一道可能用归纳法的题目就立即能够想到运用归纳法），或者干脆就不知道该知识的人来说，就需要通过启发法来辅助联想或探索了。后者可以一定程度上代偿对知识的不够熟悉，但在一些时候知识的缺失则是致命的（参见上面第2点）。不过要注意的是，那种看到题目直接反应出答案的或许也不是纯粹的好事，因为这样的解题过程严重依赖于既有知识，尤其是做过的类似的题目，其思维过程绝大部分运用的是联想或类比，而非演绎或归纳。更重要的是，联想也分两种，被动联想和策略性联想（参考《找寻逝去的自我》），这里用的却是被动联想。所以，能直接反应出答案并不代表遇到真正新颖的题目的时候的解决能力，后者由于不依赖于既有领域知识，就真正需要看一个人的思维能力和习惯究竟如何了。

**6\.** **启发法的局限性**

首先肯定的是，启发法一定（也许很大）程度上是可以代偿知识的不足的（这里的知识主要是指大脑中的“联系”，下面还会提到另一种知识，即hard knowledge）。譬如，一道题目，别人直接就能通过类比联想到某道解过的题目，并直接使用了其中的一个关键的性质把题目给解出来了。你并没有做过那道题目，这导致两种可能的结果：一，你就是不知道那个性质。二，你虽然“知道”那个性质，但并没有在以前的解题经历中将那个性质跟你手头的这个问题中的“线索”联系起来，所以你还是“想不到”。后一种可以称为soft knowledge，即你“知道”，但就是联想（联系）不起来。所谓不能活学活用，某些时候就是这种情况，即书本上提供什么样的知识联系，脑子里也记住什么，而没有事后更广泛地去探索知识之间的本质联系（总结的作用）。前一种则可以称为hard knowledge，即你就是不知道，它不在你的脑子里。

而启发式方法在两个层面上起作用：

-   **辅助联想起soft knowledge**：譬如，特例法是一种启发式思考方法，它通过引入一个简单的特例，特例中往往蕴含有更多的“线索”，通过这些线索，有可能就会激发起对既有的知识的联想。另外一种强大的辅助联想办法就是对题目进行变形，变形之后就产生了新的视觉和语意线索，比如式子的对称性、从直角坐标到极坐标从而引发对后者的知识的联想等等。大量的启发式方法实际上的作用就是辅助联想，通过对题目中的线索的发掘，激起大脑中已知相关知识的浮现。在这个意义上，相对于那些能够直接联想到某个性质的人，那些不知道但可以通过启发式思维联想到的，启发式思维就提供了一种“曲径通幽”的策略性联想。还是以经典的例子来说：砖头的用途。有人立即能够直接联想到“敲人”。有人也许不能。然而启发式联想策略“抽象”就能够帮助后者也能够联想到“敲人”，因为“抽象”策略启发人去考虑砖头的各个性质维度，如“质地”，“形状”，当你考察到“质地坚硬”，“棱角”，离“敲人”的功能还会远么？本质上，能够直接联想到“敲人”功能的人是因为大脑中从砖头到敲人这两个概念之间的神经通路被走过了很多遍（譬如由于经常拿砖头敲人），神经元之间的联系相当“粗”（形象的说法，严格的事实请参考《追寻记忆的痕迹》），而不经常拿砖头敲人的人呢，这个联系就非常的弱，乃至于根本激不起一次神经冲动。那么为什么通过启发式方法又能联想到呢？因为启发式方法相当于带入了一种新的神经调控回路，首先它增加你联系到砖头的属性维度上的可能性，使得“质地坚硬”、“棱角”这两个语意概念被激活起来（注意，如果没有启发式方法的参与，这是不会发生的），一旦后者被激活起来，从后者到“敲人”的联系就被激活起来了。从本质上，解题中的启发联想方法做的也就是这个工作。而越是一般性的启发式方法就越是能对广泛的问题有帮助（譬如《How to Solve It》中介绍的那些，譬如分类讨论、分治、乃至我认为很重要的一个——写下自己的思维过程，详细分解各个环节，考察思维路径中有无其它可能性（我们很容易拿到一道题目便被一种冲动带入到某一条特定的思路当中，并且遵循着“最可能的”推导路径往下走，往往不自觉的忽略其它可能性，于是那些可能性上的联想就被我们的注意力“抑制”了。））。
-   **辅助探索出hard knowledge**：倒推法是一种启发式思考方法，它将你的注意力集中到问题的结论中蕴含的知识上，一旦你开始关注可能从结论中演绎出来的知识，你就可能得到hard knowledge，即并不是早先就存在你脑子里，但是可以通过演绎获得的。上文中的最小和子序列中的倒推方法就是一个例子。

而启发式方法的局限性也存在于这两个方面：

-   **有些联系是不管怎样“****启发”****也想不起来的。**譬如“当布被刺破了，干草堆就重要了”，你怎么解释这句话？如果有人提示一下“降落伞”，每个人都会恍然大悟。这是因为从“布”到“降落伞”之间的单向联系是近乎不存在的。而且就算运用启发法，譬如，考虑所有布做的东西，也基本绝无可能想到降落伞，因为同样，从“布做的东西”到“降落伞”之间的关联也是极其微弱的。我们脑子里只能保留那些最最重要的联系。（如果一提到布，“降落伞”和“衣服”、“被单”、“窗帘”等日常物品以同等重要级别闪现，就乱套了。）那为什么从降落伞我们能想到布呢？我们实际上不能，我们为什么有些时候能，是因为譬如有人叫你“考虑降落伞的材料”，后者就激发了“降落伞之材料”这个语意，后者又指导了我们去考察降落伞的材料构成，于是我们想到是布。否则“布”是不会直接被激发起来的。那为什么在我们的这个问题中，一旦有人提到降落伞，我们就能建立从布到降落伞的关联呢？这是因为“降落伞”和“布”这两个语意单元的同时兴奋增大了它们之间关联的可能性，就好比是加大另一端的电压从而发生了“击穿”一样。从本质上，解数学题也是如此，费马大定理的求解过程是一个很好的例子，谷山志村猜想，就相当于那个“降落伞”的提示。我们还听到很多这样的故事（或者自己经历）：苦思冥想一个问题不得要领，某一天在路上走，看到某个东西或听到某句话，然后忽然，一道闪电划破长空，那个问题解开了（阿基米德是因为躺在浴缸里从而想到浮力原理的吗？）。我敢保证，如果一个人早就把那个问题从脑海里扔到九霄云外去了（不再处于兴奋状态了），那么就算线索出现，也是不可能发生顿悟的。我们都知道，带着一个问题（使其在大脑中处于兴奋状态）去寻找答案更可能找到，即便不是有意去寻找，只要问题还在脑子里，任何周围的有可能与它相关的线索都不会被大脑漏掉，因为“问题”和“周围的其他线索”同时的兴奋增大了关联的可能性。如果问题早就被从大脑（意识或者潜意识）中撤下了，即便周围出现提示也不会被捕捉到。
-   **许多hard knowledge****是不能被启发探索出来的。**至少是不能被“直接命中目标”地探索出来的。一个问题有可能跟三角函数有关，也许你只能带着问题去探索三角函数的所有性质，从而最终发现那个关键的性质。费马大定理与椭圆方程有关，也许只能去探索椭圆方程的所有性质，这个过程一定程度上是盲目的，试错的，遍历的。而不是直接面向目标的。再聪明的人也无法从费马大定理直接反推到谷山志村猜想。在这些时候，启发式方法最多只能提供一个探索的大致方向：譬如，探索三角函数的性质，并随时注意其中哪个可能对我这个问题有帮助。譬如，探索模运算的性质，看看哪些性质可能会有用。譬如，探索椭圆曲线的性质…等等。启发式方法并不能使我们的探索精准地命中目标。而只能划定一个大致的范围。也难怪有人说数学是盲目的。

但话说回来，启发式方法的局限性并不能否认在大量场合启发式方法的巨大帮助，许多时候，单靠启发式方法就能带来突破。而且，一旦知识性的东西掌握的是[![clip_image011](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image011-thumb.jpg "clip_image011")](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image011.jpg)一样多的，能否运用更优秀的思维方法就决定了能力的高下。有很多[介绍思维方法的书](http://www.douban.com/people/pongba/booktags/%E6%80%9D%E7%BB%B4)。

**7\.** **总结的意义**

解题练习的最重要目的不是将特定的题目解出来，而是在于反思解题过程中的一般性的，跨问题的思维法则。简单的将题目解出来（或者解不出来看答案，然后 “恍然大悟”），只能得到最少的东西，解出来固然能够强化导致解出来的那个思维过程和方法，但缺少反思的话便不能抽取出一般性的东西供更多的题目所用。而解不出来，看答案然后“哦”的一声更是等同于没有收获，因为“理解”和“运用”相差何止十万八千里。每个人都有过这样的经历：一道题目苦思冥想不得要领，经某个人一指点其中的关键一步，顿时恍然大悟——这是理解。但这个理解是因为别人已经将新的知识（那个关键的一步）放到你脑子里了，故而你才能理解。而要运用的话，则需要自己去想出那关键的一步。因此，去揣测和总结别人的思维是如何触及那关键的一步，而你自己的思维又为什么触及不到它，有一些一般性的原则可以指导你下次也能想到那个“关键的一步”吗，是很有意义的。我们很多时候会发现，一道题目，解不出来，最终在提示下面解出来之后，发现其中并没有用到任何自己不知道的知识，那么不仅就要问，既然那个知识是在脑子里的，为什么我们当时愣是提取不出来呢？而为什么别人又能够提取出来呢？我怎么才能像别人那样也提取出相应的知识呢？实际上这涉及到关于记忆的最深刻的[![clip_image012](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image012-thumb.jpg "clip_image012")](http://mindhacks.cn/wp-content/uploads/2009/02/clip-image012.jpg)原理，实际上文中已经提到了一些。（有兴趣的建议参考以下几本书：[《追寻记忆的痕迹》](http://www.douban.com/subject/1944205/)，[《找寻逝去的自我》](http://www.douban.com/subject/1315575/)，[《Synaptic Self》](http://www.douban.com/subject/2345245/)，[《Psychology of Problem Solving》](http://www.douban.com/subject/2845839/)）一般性的思维法则除了对于辅助联想（起关键的知识）之外，另一个作用就是辅助演绎/归纳（助探），一开始学解题的时候，我们基本上是先读懂题目条件，做可能的一些显然的演绎。如果还没推到答案的话，基本就只能愣在那里等着那个关键的步骤从脑子里冒出来了。而所谓的启发式思维方法，就是在这个时候可以运用一些一般性的，所有题目都适用的探索手法，进一步去探索问题中蕴含的知识，从而增大成功解题的可能性。启发式的思维方法有很多，从一般到特殊，最具一般性的，在波利亚的《How to Solve It》中已经基本全部都介绍了。一些更为特殊性的（譬如“如果全局搜索空间没有递归结构，那么考虑分割搜索空间”，譬如那些“看到XX，要想到YY”的联系），则需要自己在练习中不断抽象总结。

**一句结尾**

“我想我就在这里结束”——如果你知道我在说什么的话:-)
