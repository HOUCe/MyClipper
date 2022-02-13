# [Mac下使用crontab来实现定时任务 - dreamhighqiu - 博客园](https://www.cnblogs.com/dreamhighqiu/p/11175433.html)

## [Mac下使用crontab来实现定时任务](https://www.cnblogs.com/dreamhighqiu/p/11175433.html)

1、Linux和Mac下操作crontab都是一致的

2、配置文件都在/etc/crontab下，如果没有就创建。

3、测试发现直接使用crontab -e命令创建的定时任务是放在临时文件夹的，重启会删除，并且与/etc/crontab文件无关联。

实际操作：

-   查看 crontab 是否启动
    
    sudo launchctl list | grep cron
    
-   检查需要的文件
    
    $  LaunchAgents  ll /etc/crontab
    ls: /etc/crontab: No such file or directory  #表示没有这个文件，需要创建一个
    
-   创建文件
    

**crontab的参数**

-   ![复制代码](https://common.cnblogs.com/images/copycode.gif)
    
    ![复制代码](https://common.cnblogs.com/images/copycode.gif)
    
    \-u user：用来设定某个用户的crontab服务；
    
    file：file是命令文件的名字,表示将file做为crontab的任务列表文件并载入crontab。如果在命令行中没有指定这个文件，crontab命令将接受标准输入（键盘）上键入的命令，并将它们载入crontab。
    
    -e：编辑某个用户的crontab文件内容。如果不指定用户，则表示编辑当前用户的crontab文件。
    
    -l：显示某个用户的crontab文件内容，如果不指定用户，则表示显示当前用户的crontab文件内容。
    
    -r：从/var/spool/cron目录中删除某个用户的crontab文件，如果不指定用户，则默认删除当前用户的crontab文件。
    
    -i：在删除用户的crontab文件时给确认提示。
    
    ![复制代码](https://common.cnblogs.com/images/copycode.gif)
    
    ![复制代码](https://common.cnblogs.com/images/copycode.gif)
    

> eg: `*/1 * * * * /bin/date >> /User/Username(你的用户名)/time.txt`表示每分钟输出当前时间到time.txt上.

-   如果出现以下问题
    
    crontab: no crontab for hayek - using an empty one
    crontab: "/usr/bin/vi" exited with status 1 
    
    1.  方法1：`EDITOR=vim crontab -e` 直接编辑，以后直接`crontab -e`直接打开就行。
    2.  方法2：`export EDITOR=vim`
    3.  方法3：向cron进程提交一个crontab文件之前，首先要设置环境变量EDITOR。cron进程根据它来确定使用哪个编辑器编辑crontab文件。9 9 %的UNIX和LINUX用户都使用vi，如果你也是这样，那么你就编辑$HOME目录下的. profile文件，在其中加入这样一行:   
        `EDITOR=vi; export EDITOR`

**crontab的文件格式**

```
    * 第1列分钟0～59
    * 第2列小时0～23（0表示子夜）
    * 第3列日1～31
    * 第4列月1～12
    * 第5列星期0～7（0和7表示星期天）
    * 第6列要运行的命令
```

**crontab服务的重启关闭，开启**

-   Mac系统下
    
    sudo /usr/sbin/cron start
    sudo /usr/sbin/cron restart
    sudo /usr/sbin/cron stop
    
-   Ubuntu:
    
    sudo /etc/init.d/cron start
    sudo /etc/init.d/cron stop
    sudo /etc/init.d/cron restart
    

总结：

1\. sudo touch /etc/crontab

2\. crontab -e 打开文件按照vim语法输入定时任务内容：

eg:  \*/10 \* \* \* \* /usr/bin/python /Users/qiuyunxia/yunxia.qiu/code/mtg-ios-automation/auto\_install\_ios\_app.py -d f028d43c1b8969ab5dce14d163af42105d3dc744 -P 4723 --country CN

 3. sudo /etc/init.d/cron start 启动服务

4\. crontab -l 查看任务的运行情况

posted on 2019-07-12 12:46  [dreamhighqiu](https://www.cnblogs.com/dreamhighqiu/)  阅读(4010)  评论()  [编辑](https://i.cnblogs.com/EditPosts.aspx?postid=11175433)  收藏  举报
