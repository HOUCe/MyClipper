# [Linux服务器如何关闭crontab定时任务 - SegmentFault 思否](https://segmentfault.com/q/1010000018593125)

[![segmentfault](https://cdn.segmentfault.com/r-7e338484/static/logo-b.d865fc97.svg)![segmentfault](https://cdn.segmentfault.com/r-7e338484/static/sf-icon-small.82a498f6.svg)](https://segmentfault.com/)

[](https://segmentfault.com/search)[注册登录](https://segmentfault.com/user/login)

[问答](https://segmentfault.com/questions)[专栏](https://segmentfault.com/blogs)[课程](https://ke.segmentfault.com/)[招聘](https://segmentfault.com/jobs)[活动](https://segmentfault.com/events)

发现

[注册登录](https://segmentfault.com/user/login)

1.  [首页](https://segmentfault.com/)
2.  [问答](https://segmentfault.com/questions)
3.  [linux](https://segmentfault.com/t/linux/questions)
4.  问答详情

[**大吉**](https://segmentfault.com/u/dingwu)

-   16

发布于 2019-03-20

1、阿里云服务器，被黑客植入病毒，目前发现在我后台有一个定时任务，但是用crontab -r关闭不了。

![clipboard.png](https://segmentfault.com/img/bVbqa4E?w=1638&h=194 "clipboard.png")

2、对于这个任务，无法进行删除,用vim修改也不生效。

![clipboard.png](https://segmentfault.com/img/bVbqa4T?w=828&h=210 "clipboard.png")

![clipboard.png](https://segmentfault.com/img/bVbqa4P?w=1612&h=138 "clipboard.png")

目前虽然能重新安装服务器解决问题，但是不甘心，又不知道怎么解决。

[linux](https://segmentfault.com/t/linux)[linux运维](https://segmentfault.com/t/linux%E8%BF%90%E7%BB%B4)

[回复](https://segmentfault.com/q/1010000018593125###)

阅读

6.3k

如果对你有帮助，记得点赞支持

**3 个回答**

[得票数](https://segmentfault.com/q/1010000018593125?sort=votes)[最新](https://segmentfault.com/q/1010000018593125?sort=newest)

[**乌啦啦**](https://segmentfault.com/u/actors315)

-   1.3k

[发布于 2019-03-20](https://segmentfault.com/q/1010000018593125/a-1020000018593270)

service crond stop 

直接把 crond 服务关掉

crontab -r 删除后还在考虑是有自动程序或者服务器漏洞在删除后不断的添加任务。可以排查下可疑进程。

另外没看懂用 vim 是做什么操作，编辑crontab 可以用 crontab -e 命令

[回复](https://segmentfault.com/q/1010000018593125###)

[**user\_Yqo3PQdJ**](https://segmentfault.com/u/user_yqo3pqdj)

-   3

[发布于 2021-07-06](https://segmentfault.com/q/1010000018593125/a-1020000040297744) 新手上路，请多包涵

1.看到你的定时任务,估计你删除了,也有脚本会自动开启定时任务  
2.通过ps -aux 查看和你定时任务有关的进程通通杀掉  
3.清理完后删除定时任务，通过top -c 查看是否还在恶意执行脚本(很占cpu)  
4.如果还在执行,买个安全服务。  
5.最后换个高难度的密码，你root秘密都被别人破解了，说明你的密码弱。

[回复](https://segmentfault.com/q/1010000018593125###)

[**guanhui07**](https://segmentfault.com/u/guanhui07)

-   792

[发布于 2019-03-21](https://segmentfault.com/q/1010000018593125/a-1020000018594942)

[https://blog.csdn.net/shuaige...](https://link.segmentfault.com/?enc=GgeBUaxrVIqAIs2uyEyamg%3D%3D.9hKFzBWheq4pHfZDWEDy0yzZMY7Mn2QnH5WluUK84Hb%2B4yUm2u8EvI6fwS83Um3KMWW7%2BMhsRk4YqD2ArcwIbA%3D%3D) centos7看这个就好了

[回复](https://segmentfault.com/q/1010000018593125###)

**撰写回答**

###### 你尚未登录，登录后可以

-   和开发者交流问题的细节
-   关注并接收问题和回答的更新提醒
-   参与内容的编辑和改进，让解决方法与时俱进

[注册登录](https://segmentfault.com/q/1010000018593125###)

**相似问题**

-   [
    
    ##### linux crontab定时任务与flock
    
    由于开发在写模块的时候，忽视一个比较不常见的错误，所以现在需要我们运维配合开发写一个暂时性脚本,脚本名叫log499.sh。脚本执行下来大约需要4分钟左右。但是crontab的频率还是一分钟，为了防止出现脚本运行冲突的问题。我就效仿\[链接\]使用了flock，在crontab里写的语句是：
    
    3 回答5.8k 阅读✓ 已解决
    
    ](https://segmentfault.com/q/1010000008039907?utm_source=sf-similar-question)
-   [
    
    ##### linux 定时任务？
    
    虚拟机老是不定时网卡停用，非运维人员，想到是不是可以在 /etc/crontab中写一个定时任务：每隔15分钟 ping一下数据库，如果没回应则重启网卡
    
    3 回答1.9k 阅读✓ 已解决
    
    ](https://segmentfault.com/q/1010000012538978?utm_source=sf-similar-question)
-   [
    
    ##### Linux定时任务
    
    我想每周一凌晨两点执行一个PHP文件（程序会导出一个excel），然后将这个文件自动通过邮件发送
    
    2 回答1.5k 阅读
    
    ](https://segmentfault.com/q/1010000004969488?utm_source=sf-similar-question)
-   [
    
    ##### linux crontab定时任务不执行
    
    在crontab中设置05 17 \* cd /root/crawler/test &amp;&amp; scrapy crawl test 不能执行但是在命令行手动执行cd /root/crawler/test&amp;&amp; scrapy crawl test这条命令却能执行成功
    
    1 回答3.4k 阅读
    
    ](https://segmentfault.com/q/1010000016761451?utm_source=sf-similar-question)
-   [
    
    ##### crontab定时任务问题
    
    在Ubuntu系统中使用crontab，设置定时任务，文件名称test.cron 文件内容：1,30,45,59 echo "run" &gt;~/log.log 测试每分钟将内容run写到文件log.log中，也重启了crontab。查看定时任务列表：
    
    1 回答2.5k 阅读
    
    ](https://segmentfault.com/q/1010000020440744?utm_source=sf-similar-question)
-   [
    
    ##### linux crontab 执行多个定时任务
    
    需求： 可以在linux执行多个定时任务，不同的时间节点执行不同的shell脚本; 有大神知道吗？可以这么写吗？ {代码...}
    
    2 回答6.9k 阅读✓ 已解决
    
    ](https://segmentfault.com/q/1010000020045109?utm_source=sf-similar-question)
-   [
    
    ##### crontab定时任务不执行
    
    系统是centos，发现crontab定时任务不执行 登录系统用手工执行脚本就可以 其它如crontab配置文件没有对用户做限制， 脚本权限也赋予执行，用root账户执行，但是最后还是不行， 请各位大神指点一下，下面是Log和脚本内容
    
    5 回答38.6k 阅读
    
    ](https://segmentfault.com/q/1010000000404191?utm_source=sf-similar-question)
-   [
    
    ##### TP定时任务
    
    请教各位大神，ThinkPHP3.2.3里面的定时任务如何配置，是被动式的吗？（windows环境下）
    
    5 回答4.7k 阅读✓ 已解决
    
    ](https://segmentfault.com/q/1010000008509186?utm_source=sf-similar-question)

找不到问题？[创建新问题](https://segmentfault.com/ask)

**宣传栏**

6317[](https://segmentfault.com/q/1010000018593125#comment-area)
