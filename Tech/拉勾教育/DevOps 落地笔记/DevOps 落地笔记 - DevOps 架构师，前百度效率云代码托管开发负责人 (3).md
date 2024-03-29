---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=561#/detail/pc?id=5743
author: 
---

# [DevOps 落地笔记 - DevOps 架构师，前百度效率云代码托管开发负责人 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=561#/detail/pc?id=5743)


你好，我是 DevOps 亮哥。上一讲主要跟你介绍了如何使用**影响地图**这个工具来进行产品定义、里程碑规划和用户需求分析。影响地图让我们始终以达到目标为核心，并让功能和需求不偏离该目标，从而让交付更有重点。可视化、结构化的思维导图为技术和业务人员创建了共享的整体视图，从而加强了彼此之间的协作。

那么，你有没有遇到这样的错乱情况？对于一个用户需求，产品、开发和测试对这个需求的理解完全不一样，最终交付的产品根本不是用户想要的，这种情况在实际开发中非常普遍。今天的课程内容就可以帮你解决这样的老大难问题。

今天介绍的**用户故事**也是一种将**需求可视化**的工具，它通过将需求拆分成一个一个的用户故事，来组织软件开发。每一个用户故事都是软件开发过程中相关角色沟通的媒介，所以用户故事也是一种通过合作沟通的方式来理解用户需求的工具。下面就让我们来开始吧。

### 软件开发是一项团体项目

我们都知道，软件是由一个又一个的**功能**组成的，而每个功能又都经历了从需求分析，到代码开发，到测试，到部署的整个开发流程。

在整个开发流程中，功能就像篮球比赛中的篮球一样，从一个队员（角色）的手里传递到下一个队员（角色）的手里，最后交给队伍里位置最好或投篮最准的队员（角色）投入篮筐。这个球在每次进攻的过程中，不管是控球后卫从后场带球到前场，中锋从篮板上抢球下来，得分后卫三分线的一处跳投......每个队员的共同目标就是将球投入篮筐。

回到软件开发这个流程里，**功能**从需求开始流经整个软件开发过程，其中会涉及多种角色，产品经理对功能的需求分析，开发人员对功能的代码实现，测试人员对功能的功能测试，最后运维人员将功能部署到生产环境交付给用户......同样，每个领域的人员都有一个共同的目标：最终将功能交付给用户。

![Drawing 0.png](https://s0.lgstatic.com/i/image/M00/72/C9/Ciqc1F_EzMyAeJvGABODTv-N57w609.png)

团队协作是个老生常谈的话题，但真正做到高效协作并非易事。在软件开发流程中涉及的各个人员，都有自己的工作规范，语言和节奏也各不相同。同时，软件开发中的功能与篮球不同，看不见摸不着，当功能在每个角色之间流转时，是将自己领域的产出交付下一个角色，产品经理交付开发功能需求，开发交付测试代码，测试交付运维是的制品。由于交付物不同，增加了彼此之间沟通协调的难度。

因此，我们需要一种能够让大家达成共识的方法。这就需要让软件开发过程中的每个角色都能理解用户的需求是什么，工作内容是什么。比如：产品经理提出“”**下订单时，送货地址从地址簿里选择**“”的需求，产品经理、开发人员、测试人员三者对这个需求的理解是不一样的。地址簿是下拉列表显示还是弹窗？这个隐含的需求决定了开发人员如何编写代码，测试人员如何编写测试用例。所以只有达成共识了，才能保证大家的工作方向是一致的。

### 共享文档并不代表达成共识

写文档，其实是软件开发过程中的通用动作，但是我先介绍一下**写文档存在的问题**。在传统开发模式中，产品经理通过先写一份用户需求说明书，然后传给开发、测试等相关人员，并期望他们能够按照这个文档去开发、测试，最终实现用户需求，但最终交付的软件常常并不能达到用户需求。

事实上，人们习惯性地根据经验见识在文档范围内去思考，难免对内容理解不到位，造成误解。比如，还是上面的例子，产品经理想说的是“送货地址从地址簿里选择”，而开发就认为下拉列表的形式也是满足要求的，但用户可能需要一个弹窗的形式。很明显，文档并不能让人们彼此之间达成共识。

这是2018年我在广州塔上拍的一张照片，鸟瞰广州夜景。当你看到这张照片时，你可能会说：“广州夜景好美啊”，但是当我看到这张照片的时候，我会**回想**起当天晚上人山人海排队上广州塔的情景，**回想**起女儿为了快点上去不耐烦的情景，**回想**起一边排队一边和朋友聊天的情景。当到了塔顶，坐到缆车里，整个广州，尽收眼底，非常漂亮。

![2.png](https://s0.lgstatic.com/i/image/M00/72/D4/CgqCHl_EzRuAJHUXAAcE2am_Jrc300.png)

**看文档，就像看旅游时拍的照片一样**。当我们在看文档时，重要的并不是文档里记录的内容，而是当我们阅读文档时，脑子里能够回忆起什么。所以只有面对基于讨论写出的文档，参与讨论的各方才能对文档有一致的理解。没有参与讨论的人，是无法通过仅阅读文档来了解这些细节的。

**因此，达成共识的唯一方法是各相关方坐到一起，用有效的方式进行讨论，最终寻求一致的理解。这种有效的方式就是讲用户故事。**

用户故事之所以叫“故事”，是因为它是用来“讲”，而不是用来“写”的。它从用户的角度来讲述如何使用软件的功能，带来怎样的收益。整个软件团队通过讲用户故事，让大家获得对软件产品的一致理解，然后共同创造出更符合用户需求的产品。

### 如何“讲”好用户故事

有很多人并不清楚什么是用户故事。

用户故事的概念比较抽象。我举个例子，用户说：“当我在输入‘送货地址’的时候，能立即显示出目前存在的地址。这样就可以从列表中选择一个，而不是每次都需要重新输入。”这是一个典型的用户使用场景，是一个用户故事。

就像上面提到的，**用户故事是用来“讲”的，而不只是“写”的**。上面的例子中，如果这个不讨论用户故事，你能知道【立即显示】是输入 1 个字、 2 个字，还是有光标就能显示呢？因此，“讲故事”是本课时的核心观点，用户故事的关键就是“讲”。那么，怎样才能“讲”好用户故事呢？

#### 聚焦全景

我们都知道，软件开发最终交付给用户的是一个可以工作的软件，这就需要先实现一个一个单独的功能。因此，我们需要将一个大需求进行合理拆分，拆分后的需求就称为“用户故事”。

我们可以通过拆分用户故事的方式来组织开发产品，每个用户故事完成后，都需要与整个软件进行集成。不过值得提醒注意的是，当我们在将大需求拆解为用户故事，或者在讨论单个用户故事时，都需要从整体的视角出发，和团队一起理解正在开发的产品，共同讨论每个故事在整个产品中的定位、优先级、共同权衡和取舍。杰夫·巴顿（Jeff Patton）将这种做事的方法称为“**用户故事地图**”。

马丁·福勒（Martin Fowler）在给杰夫·巴顿（Jeff Patton）的《用户故事地图》一书作序时说：“**故事地图是一门在需求拆分过程中保持全景图的技术。**”全景图为一致的用户体验提供了基准，可以帮助团队之间，以及团队与用户之间进行有效的沟通。下图是百度效率云产品中用户故事地图功能的示例图。

![图片2.png](https://s0.lgstatic.com/i/image/M00/72/CA/Ciqc1F_E0pOAVmLEAAEVRH5ysAE021.png)

在该用户故事地图的顶部是**主干**。主干通常分两级：

-   一级是组成该产品的基本骨架，如图中的**用户场景**层级，包含账号管理、产品首页、商品列表页等；
    
-   另一级是每个用户场景下包含的**用户任务**层级，如产品首页场景，包含首页轮播图、产品橱窗、产品搜索等。
    

从左到右看，整个用户故事地图的主干是一个所有用户和他们使用该软件产品的完整故事。从左到右的顺序，也是我们在讲故事时候的叙事主线，更容易被接受和理解。

再往下是具体的产品发布路线图，是团队根据**市场需求**和**用户要求**综合考虑后，决定要发布的用户故事。每个用户任务下的用户故事按照**从上到下**的顺序排列**优先级**。通常，选择那些对用户价值较大的用户故事进行发布。

有了这个用户故事全景图，团队中的每个角色都能清晰地知道本次迭代中需要交付的用户故事内容，用户是如何使用的，以及产品能够给用户带来什么帮助。有了对用户故事的一致理解，后续大家在交付每个用户故事的过程中就有了统一的目标，不容易产生误解。

#### 3C 原则

上面我们说到讲用户故事要基于全景视图，但这样并不能表示能讲好用户故事，因此下面为你介绍 3C 原则。

3C 原则是在由 Ron Jeffries（罗恩·杰弗里斯） 等人在《极限编程实施》一书中提出的。3C 原则描述的是为了更有效的使用用户故事进行需求分析时应该遵循的原则：

-   **卡片（Card）**：在卡片上写下所期望的软件特性。
    
-   **交谈（Conversation）**：聚在一起对要开发的软件进行深入讨论。
    
-   **确认（Confirmation）**：对完成的特性进行确认。  
    **1\. 卡片（Card）**
    

卡片是用户故事的载体，用来记录用户想要的每一个产品特性，上图中的每一个用户故事卡片都对此有所体现。

通过这种方式，可以很容易地组织这些卡片，比如通过拖拽的方式选择本次迭代需要交付的用户故事，排列用户故事的优先级等。

**2\. 交谈（Conversation）**

创建好卡片后，我们并不是直接将卡片甩给团队中的其他角色。而是请相关人员围绕卡片中的用户故事进行讨论。在讨论过程中：

-   每个参与者都可以通过提问来表达自己的疑惑，其他参与者则可以帮助解答。如果在某个问题上有分歧，则所有参与人员共同讨论决定。
    
-   除了使用创建好的卡片之外，还可以借助像思维导图、流程图、原型图等有助于理解的可视化工具，避免参与人员通过手势对着空气挥舞。
    
-   讨论中所涉及的问题都需要进行标记、并在事后进行修正整理。
    

总之，交谈的目的是让所有参与者对用户故事达成一致的理解，并一起努力找到解决问题的最佳方案。就像看到旅行照片一样，团队成员当看到这张卡片时就能回忆起当时讨论的细节。

**3\. 确认（Confirmation）**

现在，团队成员已经对用户故事达成了一致，这时就需要考虑如何判断这个用户故事是否已经完成开发，也就是明确验收条件。

一般情况下，当开发完成后需要通过单元测试、SIT 测试、UAT 测试，通过测试后该用户故事才算完成。剩下的就是根据业务需要随时发布该功能到生产环境中，完成最终的用户交付。

#### 使用故事模板

现在你了解了讲好用户故事需要注意的几个方面，但作为软件从业人员来说，这还不够。相对于产品和市场人员来说，很多开发人员不知道如何去讲用户故事，也不习惯讲用户故事。这时，你需要使用用户故事模板，这样就能够对卡片上的内容有一个指引，方便讨论。如下图所示：

![Drawing 3.png](https://s0.lgstatic.com/i/image/M00/72/D4/CgqCHl_EzU2ACfvbAAATfT4rGrM940.png)

也就是说，大家在创建卡片的时候主要考虑这几个问题：用户是谁？用户需要什么特性？用户需要这个特性是要解决什么问题？

在讨论之前，如果不能回答这几个问题，就无法知道我们交付这个用户故事的价值，也就不清楚这件事的意义，就无法交付对用户真正有用的东西。

通过统一的故事模板，就是为了统一团队成员之间沟通交流的语言。而基于故事模板内容来引发讨论，能提升讨论的效果。

为了达到更好的讨论效果，这里罗列几个讨论清单：

-   **讨论用户角色。** 不要单纯地将所有使用软件的人抽象为“用户”，要更加具体，比如企业用户还是个人用户，个人用户又可以根据年龄、性别进行细分。每一个用户群体对软件的诉求是不一样的。
    
-   **讨论要做的功能。** 不要只讨论用户能够看得见摸得着的界面功能，也要考虑界面后所调用的后台服务，其他服务与服务之间的调用关系、服务调用时的鉴权功能等也需要考虑。因为这些才是软件可工作的基础。
    
-   **讨论该功能的价值。** 讨论为什么用户需要这个功能。提问也是一门艺术，可以通过 5 Why 提问法，多问几个为什么，就可以发现隐藏在背后的真正原因。
    
-   **讨论异常情况。** 讨论可能出现的异常情况，以及出现异常后的解决方案。
    
-   **讨论开发周期。** 讨论用户故事什么时间完成开发，什么时间完成测试，什么时间最终交付给用户......为这些时间点提供一个预估的计划表。
    

用户故事及用户故事地图，很多企业的研发团队中都有使用。就像开头提到的那样，软件开发是一个团队项目，在团队中沟通协调是至关重要的。

大多数时候，软件本身的功能并不复杂，但团队之间若因为沟通不到位，理解不一致，最终导致开发的产品与用户想要的不一样，可能会错过最佳的市场机会。这种因为疏忽，大意失荆州的事情是我们所有工作中都应该尽力去避免的。

### 总结

今天，我从团队协作入手，强调了高效沟通在软件开发中对于达成目标共识的重要作用，分析了单纯使用文档来达成团队共识的问题所在，并提出通过对用户需求的讨论才能真正达成团队的一致理解，从而引出了敏捷软件开发中使用的用户故事这个绝佳工具。

用户故事是一个强大的工具，这里主要强调如何“讲”好用户故事，并列举了“讲”好用户故事需要注意的事项和遵循的原则。如果你的团队也经常出现对用户需求理解不一致的情况，不妨试一试！
