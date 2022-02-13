# [美国计算机名校例如MIT ，CMU ，有哪些公认的好课并且有课程讲义的，适合国内学生自学的？ - 知乎](https://www.zhihu.com/question/57532048/answer/153255177)

谢邀。MIT的老师其实非常重视上课，准备很充分，都想更好地把知识传授给学生。下面列几个我上过的非常好而且课程资料也公开的课程。

[6.824](https://pdos.csail.mit.edu/6.824/) Distributed System: 系统方向非常好的一门课程，每堂课都讲一个新的分布式系统模型，没有教材，每堂课都是直接讲论文。老师是MIT PDOS的神牛Robert Morris (不错，这人就是当年因为发明蠕虫病毒而蹲监然后回MIT当教授的神人)和Frans Kaashoek。这些分布式系统都是实际用在各个大公司里的系统，比如说Spark, GFS，PNUTS。当年我修这门课的时候感觉课程压力非常大，有期中期末考试，有lab作业，有reading work, 还有course project，但是整个课程设计得非常好。lab要用Golang实现，硬生生地学了门新的语言。最后我的course project是用Go实现了一个Spark原型系统（[metalbubble/GoSpark](https://github.com/metalbubble/GoSpark)），那个还是2013年的时候，Spark还刚开始崭露头角：）。

6.830 [Database Systems](http://db.csail.mit.edu/6.830/): 数据库系统的一门核心课程。由数据库的一大山头Samuel Madden教授。前半部分比较基础的数据库的知识，后半段主要在讲Distributed Databases的东西，各种consistency挺有意思，也是database比较火的研究方向。

[18.409 Algorithmic Aspects of Machine Learning, Spring 2015](http://people.csail.mit.edu/moitra/409.html): Ankur Moitra教的machine learning课程。课程切入点跟一般的机器学习课程都不同，Ankur自己是做theory背景的（攻FOCS, STOC之类的会），所以这个课程有深厚的理论根基。对sparse coding, topic model, tensor decompositions等会有脑洞大开的认识。

6.869 Advances in Computer Vision ([Fall 2016](http://6.869.csail.mit.edu/fa16/) [Fall 2015](http://6.869.csail.mit.edu/fa15/)) 我TA过的一门计算机视觉的课程。课件不错，过了一遍CV的传统内容，也增加了很多deep learning的内容，适合初学者入门，也适合除了deep learning就不懂computer vision其他东西的朋友。。。Final Project我设计了一个Mini Places Challenge, 让学生可以组队比赛，训练深度模型。

其他我想到了再补充。

推荐一门工具课。工欲善其事，必先利其器。

[计算机教育中缺失的一课](https://missing-semester-cn.github.io/)

内容：ssh, git, tmux, vim ,debug, 加密工具等等。

简单说下：

### shell

-   介绍shell下常见命令
-   简单的shell脚本、工具

## Vim

-   配置插件
-   宏操作
-   特别多的vim练习资料推荐
-   多用多用多用

## 正则表达式

-   shell下的sed -E
-   练习网站很简单有趣

## Tmux

-   配置文件
-   dotfiles

## Git

-   数据模型角度下的git
-   推荐资料很多

## Debug

-   pdb
-   cProgile和line\_Profiler
-   htop
-   shellcheck
-   wireshark

## Metaprogramming

-   make
-   版本号
-   测试
-   github actions

## Security

-   散列函数
-   密匙生成函数
-   对称加密 文件加密
-   非对称加密 PGP

## 杂

-   键位修改
-   窗口管理

谢邀。前面的答主们列了很多有用的链接，我来说说怎么自己找到这些链接。

CMU的课程讲义基本都是开放的，一般只要google搜索CMU+课号+年份就能找到。在CMU 计算机系的课以15开头。想知道什么课比较流行，google一下 CMU 15，看下面的提示就知道了。

![](https://pic2.zhimg.com/50/v2-255dbaf0558a99da2c3e22510f337459_720w.jpg?source=1940ef5c)

值得注意的是，在CMU，machine learning自己是一个系，所以关于机器学习的课程都是10开头，10xxx。(谢评论提醒！）

在CMU，每个课都有一个5个数字组成的课号，其中第3个数字粗略表示了适用年纪。比如，15213，第三个数字是2，表示主要针对本科二年级的学生。第三个数字1-5的课主要针对本科生，6主要针对硕士生，7以上主要针对博士生或者偏研究类的项目。

但是，不要以为本科生的课简单。有的本科生的课甚至比研究生的课要难。毕竟CMU的小本们实力不容小觑。
