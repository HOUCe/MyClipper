# OKR + GTD + Note => Logseq

[zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/369386414)Dawei Ma关注他

> 原文首发自我的博客：[OKR + GTD + Note => Logseq](https://www.bmpi.dev/self/okr-gtd-note-logseq/)

在 [我的人生管理系统](https://www.bmpi.dev/self/life-in-plain-text/) 这篇文章中我分享了如何从个人愿景出发设定个人 OKR ，将 OKR 使用 [GTD](https://www.bmpi.dev/tags/%E6%97%B6%E9%97%B4%E7%AE%A1%E7%90%86/) 的方法拆解成多个 TODO ，这些 TODO 所产生的笔记通过反向链接关联起来。

[Logseq](https://logseq.com/) 是一个注重隐私的开源数据库工具，由于底层是个图数据库，所以支持多种 [使用场景](https://github.com/logseq/awesome-logseq)。比如你可以用来记笔记，也可以做时间管理，具体怎么玩取决于你的想象力。

本文分享如何通过 [Logseq](https://logseq.com/) 完成 [OKR](https://www.bmpi.dev/tags/okr/) + [GTD](https://www.bmpi.dev/self/gtd-tools-i-used/) + [Note](https://www.bmpi.dev/self/note-system/) 的功能，最终实现一个可以管理你的目标、时间及知识的 [人生管理系统](https://www.bmpi.dev/series/%E4%BA%BA%E7%94%9F%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F/)。

## OKR

    Mission -> Vision -> Strategy -> Objectives -> Key Results

![](https://image.cubox.pro/article/2021111820391826340/94058.jpg)

OKR 是一个从个人愿景到设定个人目标的方法论，它能帮助组织成员/使用者聚焦最重要的事。OKR 是确保将整个组织/个人的力量都聚焦于完成对所有人/个人都同样重要的事项的一套管理方法。正是多了 Objectives，才能让参与者先思考最重要的事，然后再去拆解任务。而 KPI 却是直接确定任务。

本文不会对 OKR 做过多介绍，具体可以看孟岩的这篇关于 OKR 的好文 [找到最重要的事](https://mp.weixin.qq.com/s/1T8r7HIX8NAQqUowFOV0rg)。

![](https://image.cubox.pro/article/2021111820391846632/29207.jpg)

如上图所示，OKR 的流程是将个人愿景拆解成多个 Objectives，之后将每个 Objective 拆解成多个 Key Result，最终每个 Key Result 变成了一个个 TODO 事项（从 OKR 到 GTD）。

让我们先把这个流程转变为一些问题域，之后再寻找这些问题域的 Logseq 的解空间。这个流程需要解决以下几个问题：

*   如何确定个人愿景？
*   个人愿景如何拆解为 Objectives？
*   Objective 如何拆解为 Key Result？
*   Key Result 如何拆解为 TODO？
*   如何追踪 Objective 耗费的时间与资金？

### 如何确定个人愿景

个人愿景的确立是个不断寻找的过程，我在很长时间都没意识到我需要一个个人愿景。直到某一天我从公司企业文化中意识到：企业需要文化来将公司的很多决策简单化，那么个人的愿景也可以将很多个人决策简单化。我们的工作和生活中会遇到很多机会和需要决策的地方，但我们的精力有限，如何筛选那些值得自己去做的事情是一件很难且耗费时间的事情。

我在 [构建终身学习体系进行自我提升](https://www.bmpi.dev/self/build-personal-knowledge-system/#%E4%B8%AA%E4%BA%BA%E6%84%BF%E6%99%AF%E7%9A%84%E8%AE%BE%E5%AE%9A) 这篇文章中分享了我的个人愿景的设定过程。感兴趣的读者可以参考，此处不再赘述。

### 个人愿景如何拆解为 Objectives

从个人愿景出发我设定了四个长期关注的纬度：健康、能力、收入与影响力。这四个纬度又可以设定以下的 Objectives：

*   身体提升：拥有一个健康的身体；
*   能力提升：提升技术到价值的转化效率；
*   收入提升：家庭财务状况健康；
*   影响力提升：在技术和投资领域建立专业影响力。

Objectives 相当于以期望未来能达成的结果而设定的大的目标，此阶段并不需要具体化。

### Objective 如何拆解为 Key Result

Objective 设定后需要拆解为多个 Key Result。Key Result 需要以目前能实现的程度而设定，如果设定的太庞大，根本实现不了，那就没有意义了。以下是我根据当前我的能力范围而设定的一些 Key Result：

*   2021-OKR-O1-KRs

*   每月熬夜不超过4次 #2021-OKR-O1-KR-1
*   减肥5斤 #2021-OKR-O1-KR-2

*   2021-OKR-O2-KRs

*   打造一个流量站，月PV达 100K #2021-OKR-O2-KR-1
*   打造一个英文内容权威站，月PV达 10K，月盈利达 $500 #2021-OKR-O2-KR-2
*   参加五次头马活动并至少演讲一次，发表3篇英文文章 #2021-OKR-O2-KR-3

*   2021-OKR-O3-KRs

*   被动收入投资组合年复合收益率超 15% #2021-OKR-O3-KR-1

*   2021-OKR-O4-KRs

*   在博客技术与投资领域分别输出超过10篇文章 #2021-OKR-O4-KR-1

类似 2021-OKR-O1-KRs 这种在 Logseq 是一个独立的页面，#2021-OKR-O1-KR-1 是一个标签（Tag），点击这个标签进入的就是一个 2021-OKR-O1-KR-1 的页面。这个页面就是此 Key Result 的页面，在这个页面中可以查询到所有用此页面标题作为标签的 TODO 任务。

如何用 Logseq 实现这些流程呢？先建立一个 2021 的页面，然后用 [Markdown](https://www.bmpi.dev/dev/what-markdown-can-do/) 表格实现 OKR 的内容：

    | [[四维提升]] | Objectives | Key Results |
    |-|-|-|
    | 身体提升 | 拥有一个健康的身体 | {{embed [[2021-OKR-O1-KRs]]}} |
    | 能力提升 | 提升技术到价值的转化效率 | {{embed [[2021-OKR-O2-KRs]]}} |
    | 收入提升 | 家庭财务状况健康 | {{embed [[2021-OKR-O3-KRs]]}} |
    | 影响力提升 | 在技术和投资领域建立专业影响力 | {{embed [[2021-OKR-O4-KRs]]}} |

在 Logseq 中效果如下：

![](https://image.cubox.pro/article/2021111820391876979/45090.jpg)

![](https://image.cubox.pro/article/2021111820391898421/62763.jpg)

### Key Result 如何拆解为 TODO

得益于 Logseq 的标签（Tag）能力，我们可以将任何 TODO 任务关联到某个 Key Result 上，比如以 2021-OKR-O1-KR-2 为例：

![](https://image.cubox.pro/article/2021111820391884006/42678.jpg)

如上图所示，每日日记页面中记录的 TODO 任务都通过标签关联起来了，这样我们在这个 Key Result 页面就可以看到所有关联的 TODO 任务。你可以将 TODO 放在日记页面，也可以放在某个特定的页面，比如某些和阅读相关的 TODO 都可以放在某个阅读的页面，然后通过标签关联不同的 Key Result。

### 如何追踪 Objective 耗费的时间与资金

OKR 落地到一个个 TODO 后，如何追踪某个 Objective 耗费的时间或资金是个很重要的问题。搞清楚所有 Objective 耗费的时间与资金，我们就可以在此 OKR 结束的时候知道总的收益与总的投入，甚至可以计算 ROI。于我来说，我只想大体了解下我的时间与资金有没有被浪费，能否通过一些措施提高我的单位时间收益。

Logseq 有 [极为强大的查询能力](https://logseq.github.io/#/page/Advanced%20Queries)，这也是为何说它本质是个数据库的原因。我对每个 Key Result 都计算了它的所有关联 TODO 的总时间或者打卡次数（对于某些固定时间的任务如跑步就不需要计算总时间了，只需要统计完成次数即可）。

1.  通过 Key Result 标题获取其具体内容。因为我们每个 Key Result 的具体内容都在其 Objective 页面存放，所以需要使用 Logseq 的高级查询实现：

    #+BEGIN_QUERY
    {:title "KR内容"
     :query [:find (pull ?b [*])
             :in $ ?current-page
             :where
             [?p :page/name ?current-page]
             [?b :block/ref-pages ?p]
             [?b :block/page ?bp]
             [?bp :page/name ?bp-name]
             [(not= "okr" ?bp-name)]
             (not [?b :block/marker ?marker]
             [(contains? #{"TODO" "DOING" "DONE" "LATER" "NOW"} ?marker)])]
     :inputs [:current-page]}
    #+END_QUERY

1.  统计 Key Result 关联的所有完成的 TODO 任务的总耗费时间。

    #+BEGIN_QUERY
    {:title [:code "花费时间 (分钟)"]
    :query [:find (sum ?spent-min)
                 :in $ ?current-page
                              :where
                              [?p :page/name ?current-page]
                              [?b :block/ref-pages ?p]
                              [?b :block/marker ?m]
                              [(contains? #{"DOING" "DONE"} ?m)]
                              [?b :block/properties ?prop]
                              [(get ?prop "doing") ?doing]
                              [(get ?prop "done") ?done]
                              [(- ?done ?doing) ?spent]
                              [(/ ?spent 60000) ?spent-min]]
                  :inputs [:current-page]
    }
    #+END_QUERY

1.  统计 Key Result 关联的所有完成的 TODO 任务的总打卡次数。

    #+BEGIN_QUERY
    {:title "打卡（次数）"
     :query [:find (count ?b)
             :in $ ?current-page
             :where
             [?p :page/name ?current-page]
             [?b :block/marker ?marker]
             [?b :block/ref-pages ?p]
             [(= "DONE" ?marker)]]
     :inputs [:current-page]}
    #+END_QUERY

1.  统计资金

资金使用 `Beancount` 统计，将每笔关联的账目用 Tag 关联即可统计出该 Tag 所涉及的总资金。

最终的效果如下：

![](https://image.cubox.pro/article/2021111820391853044/14670.jpg)

## GTD

我有多年使用 GTD 来管理时间的经验。一开始我使用 OmniFocus 来实践 GTD，但逐渐这种复杂的 GTD 流程让我疲惫不堪。逐渐我开始使用自己开发的工具去管理每日待办事项，每天我会通过邮件来接收正在做或者重要未做的事项。我对时间管理的使用经验见 [我的时间管理工具](https://www.bmpi.dev/self/gtd-tools-i-used/) 这篇文章。本来以为我会一直使用 V3 版的时间管理工具，直到我遇到了 Logseq，一个可以统一 OKR、GTD 与 Note 的工具，所以是时候升级我的时间管理工具了，可以理解为 Logseq 的 GTD 就是我的时间管理工具 V4 版。

一个简单的 GTD 系统可以让我直观的了解到时间四象限的待办事项，如下图：

![](https://image.cubox.pro/article/2021111820391865572/23103.jpg)

*   在做计划事项。标记为`重要`的或者和任何 `OKR 相关`的`在做`任务。
*   待做计划事项。标记为`重要`的或者和任何 `OKR 相关`的`待做`任务。
*   ⏰ 在做其他事项。任何`不重要`的或者和 `OKR 不相关`的`在做`任务。
*   待做其他事项。任何`不重要`的或者和 `OKR 不相关`的`待做`任务。

如何用 Logseq 实现如上图所示的效果？主要用到 Logseq 三个特性来完成这个功能：

*   TODO 任务支持；
*   用 Logseq 自定义 CSS 定制界面；
*   用 Logseq 高级查询获取每个象限的 TODO 任务。

Logseq 新建一个 TODO 很简单，比如：

    TODO 读《毛选》
    DOING 读《毛选》
    DONE 读《毛选》

这就完成了一个 TODO 任务从待办 -> 在做 -> 完成 的全流程。

我先参考了这篇 [\[css+template\] eisenhower matrix](https://discuss.logseq.com/t/css-template-eisenhower-matrix/526) 帖子实现了整个框架，之后修改了它的 CSS 样式，我的自定义 CSS 版本见 [custom.css](https://gist.github.com/madawei2699/61cf9601a443df21a9fabb282723936c)。

最后需要高级查询获取每个象限的 TODO 任务。

*   在做计划事项。

    #+BEGIN_QUERY
    {:query [:find (pull ?b [*])
             :where
             [?b :block/marker ?marker]
             (not (not [?b :block/ref-pages ?p]
             [?p :page/name ?page-name]
             [(clojure.string/includes? ?page-name "okr")])
             (not [?b :block/priority ?priority]
             [(contains? #{"A" "B" "C"} ?priority)]))
             [(contains? #{"DOING" "NOW"} ?marker)]]
     }
    #+END_QUERY

*   待做计划事项。

    #+BEGIN_QUERY
    {:query [:find (pull ?b [*])
             :where
             [?b :block/marker ?marker]
             (not (not [?b :block/ref-pages ?p]
             [?p :page/name ?page-name]
             [(clojure.string/includes? ?page-name "okr")])
             (not [?b :block/priority ?priority]
             [(contains? #{"A" "B" "C"} ?priority)]))
             [(contains? #{"TODO" "LATER"} ?marker)]]
     }
    #+END_QUERY

*   ⏰ 在做其他事项。

    #+BEGIN_QUERY
    {:query [:find (pull ?b [*])
             :where
             [?b :block/marker ?marker]
             (not [?b :block/ref-pages ?p]
             [?p :page/name ?page-name]
             [(clojure.string/includes? ?page-name "okr")])
             (not [?b :block/priority ?priority]
             [(contains? #{"A" "B" "C"} ?priority)])
             [(contains? #{"DOING" "NOW"} ?marker)]]
     }
    #+END_QUERY

*   待做其他事项。

    #+BEGIN_QUERY
    {:query [:find (pull ?b [*])
             :where
             [?b :block/marker ?marker]
             (not [?b :block/ref-pages ?p]
             [?p :page/name ?page-name]
             [(clojure.string/includes? ?page-name "okr")])
             (not [?b :block/priority ?priority]
             [(contains? #{"A" "B" "C"} ?priority)])
             [(contains? #{"LATER"} ?marker)]]
     }
    #+END_QUERY

最后将这个页面设置为首页就可以很方便看到今日待办事项。Logseq 对移动端的支持也可以，所以移动端的问题也解决了。

## Note

在 GTD 中我会不断的建立一些 TODO 任务，这些 TODO 任务也可以关联很多页面，在这些页面里我放入了相关的笔记，方便我快速找到此任务相关的上下文背景知识。**任务与知识在这一刻通过链接建立了关联，知识不再是孤立静态的东西，而是有了和某件事关联的能力，有了时间的属性。**

在 Logseq 中建立一个页面是很简单而高频的操作，通过 `[[Logseq]]` 即可建立一个 Logseq 标题的页面。

我在 [我的笔记系统](https://www.bmpi.dev/self/note-system/) 中分享过我的公开笔记系统，一个基于 [TiddlyWiki](https://wiki.bmpi.dev/) 的笔记系统。但是私有笔记系统我一直用 Markdown 格式的文本并存储至 GitHub，这种方式比较简陋，没法对私有笔记做进一步的利用。甚至一度我想用 Notion 来记录私有笔记，但是因其商业化的问题，考虑到早期使用 Evernote 的痛点，我放弃了这个想法。当找到 Logseq 后，我发现这就是我要用的私有笔记工具。

![](https://image.cubox.pro/article/2021111820391839100/43313.jpg)

如上图可以看出 Logseq 借鉴了很多笔记工具的特性，比如它拥有：

*   Roam Research 的图数据库、每日笔记、反向链接、右边栏；
*   Workflowy 的大纲视图；
*   Tiddlywiki 的页面嵌入、高级查询；
*   Emacs Org Mode 的属性设置、时间记录。

由于本文不是一个详细介绍 Logseq 功能的文章，想了解更多功能可以参考 [Logseq 的帮助手册](https://logseq.github.io/#/page/Changelog)。

这是我在使用 Logseq 几个月后，知识图谱的形状，它像宇宙中的某个星座：

![](https://image.cubox.pro/article/2021111820391850897/78232.jpg)

知识点由一座座孤岛通过反向链接建立了关联，我主要研究的几个领域在图谱中如恒星一样闪闪发光，这是我的知识宫殿。你的呢？

发布于 2021-05-02 13:28

[查看原网页: zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/369386414)