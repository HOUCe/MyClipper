# [mac定时任务launchctl list查看status是78？ - 知乎](https://www.zhihu.com/question/26889257)

[](https://www.zhihu.com/topic/19550264)

[Mac](https://www.zhihu.com/topic/19550264)

我用launchctl load了一个plist任务,自动执行foo.sh任务，但是launchctl list查看该任务的时候status一栏为78…

被浏览

**4,031**

创建时间:  2014-12-02  19:35:22

最后编辑:  2014-12-02  19:35:22

#### 4 个回答

[![不能更酷](https://pic1.zhimg.com/v2-f638372ea17a37512e30ebfdaa3dabfe_xs.jpg?source=1940ef5c)](https://www.zhihu.com/people/bu-neng-geng-ku)

[不能更酷](https://www.zhihu.com/people/bu-neng-geng-ku)

软件工程在读

[发布于 2018-04-03 15:16](https://www.zhihu.com/question/26889257/answer/357491343)

如我直言 我的78既不是plist语法错误也不是权限问题也不应该是找不到文件 很迷

[![Lei Yang](https://pic3.zhimg.com/d47eb3c4b_xs.jpg?source=1940ef5c)](https://www.zhihu.com/people/lei-yang-70)

[Lei Yang](https://www.zhihu.com/people/lei-yang-70)



Software Engineer

[发布于 2015-12-04 06:47](https://www.zhihu.com/question/26889257/answer/75155129)

78 应该是文件找不到

[发布于 2015-12-04 06:47](https://www.zhihu.com/question/26889257/answer/75155129)

[![HateTree](https://pic3.zhimg.com/v2-abed1a8c04700ba7d72b45195223e0ff_xs.jpg?source=1940ef5c)](https://www.zhihu.com/people/hatetree)

[HateTree](https://www.zhihu.com/people/hatetree)

[发布于 2015-11-10 21:21](https://www.zhihu.com/question/26889257/answer/71659908)

这个我来回答下吧，估计折腾的人并不多，78，可以去检查plist的语法了。。。

[发布于 2015-11-10 21:21](https://www.zhihu.com/question/26889257/answer/71659908)

[![小南书](https://pic2.zhimg.com/v2-774da68fd51e939fac0c411bebad15a3_xs.jpg?source=1940ef5c)](https://www.zhihu.com/people/woshizn)

[小南书](https://www.zhihu.com/people/woshizn)

加班加点，努力工作

[发布于 2015-02-01 11:58](https://www.zhihu.com/question/26889257/answer/38595551)

可以检查一下launchctl load 的配置文件里面的脚本权限。当前用户是否有可执行权限。尝试直接用当前用户运行一下制定脚本。

[发布于 2015-02-01 11:58](https://www.zhihu.com/question/26889257/answer/38595551)

相关问题

[Mac系统 怎么将F1、F2等键用作标准功能键？](https://www.zhihu.com/question/59118163) 5 个回答

[Mac m1芯片到底怎么才能用上win？](https://www.zhihu.com/question/470011029) 10 个回答

[大学计算机专业可以用搭载M1芯片的Mac吗？](https://www.zhihu.com/question/431299071) 19 个回答

[如何让mac电脑程序坞浮动于页面上方例如图1展示那样，跪求大神感激不尽？](https://www.zhihu.com/question/334576524) 8 个回答

[设计专业读研mac m1和thinkbook 15p选哪个？](https://www.zhihu.com/question/461230016) 16 个回答

相关推荐

[![live](https://pica.zhimg.com/90/v2-686c07c8f6ee66a66cef88d5f2c2f560_250x0.jpg?source=31184dd1)

A Little Book of Eternal Wisdom

0 人读过阅读





](https://www.zhihu.com/pub/book/120100431)[![live](https://pic3.zhimg.com/90/v2-c89e28d21151c8f3bb1f0cc51518b3e5_250x0.jpg?source=31184dd1)

苹果 Mac OS X El Capitan 10.11 完全手册

547 人读过阅读





](https://www.zhihu.com/pub/book/119583221)[![live](https://pic1.zhimg.com/90/v2-91b9a4bf645936af25f426ac74472a99_250x0.jpg?source=31184dd1)

苹果电脑玩全攻略 OS X 10.8 Mountain Lion

473 人读过阅读





](https://www.zhihu.com/pub/book/119590991)
