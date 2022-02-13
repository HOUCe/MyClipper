# [如何停止crontab中正在运行的job_leys123的专栏-CSDN博客_停止cron](https://blog.csdn.net/leys123/article/details/52937175)

当你发现crontab定时的某个shell运行有问题，但此shell需要运行很长时间时，该如何让此定时任务停止呢？

1\. 查到你要停止的那个定时job任务的进程号

ps aux |grep xx\_batch.sh 

2.kill-9 进程号。

3.如果此shell为单任务时，立马ok，搞定，但如果此shell里又调用了其他子shell时，

则你需要去查到子shell的进程号，再kill掉，这样才能彻底将此定时停止掉。
