# Mac os下定时启动一个脚本

[blog.sina.com.cn](http://blog.sina.com.cn/s/blog_60b45f2301011hqp.html)

最近任务需求，要让服务器定时触发一个事件。windows下面用计划任务就可以了。但是苹果的的确还不明白着呢么搞，搜索下用crontab命令可以实现。摸索会，搞不定原因是我的VI语法不会，弄不好。

下面帖点VI的知识，稍后介绍crontab的用法(太长了，自己去看好了）

http://www.eefocus.com/czzheng/blog/10-03/185656\_bee47.html

先写下我的操作吧：

sudo crontab -e//回车后输入密码

//进入VI编辑，输入

\* \* \* \* \* say hello//这个地方可以放脚本的路径

保存即可。

这样每分钟都会听到hello了

五个星星表示

minute — 分钟，从 0 到 59 之间的任何整数 

hour — 小时，从 0 到 23 之间的任何整数 

day — 日期，从 1 到 31 之间的任何整数（如果指定了月份，必须是该月份的有效日期） 

month — 月份，从 1 到 12 之间的任何整数（或使用月份的英文简写如 jan、feb 等等） 

dayofweek — 星期，从 0 到 7 之间的任何整数，这里的 0 或 7 代表星期日（或使用星期的英文简写如 sun、mon 等等）

crontab -l显示目前所有的任务

crontab -r删除所有的任务

crontab -e编辑任务

[查看原网页: blog.sina.com.cn](http://blog.sina.com.cn/s/blog_60b45f2301011hqp.html)