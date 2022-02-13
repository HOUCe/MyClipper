# [Mac配置crontab - 简书](https://www.jianshu.com/p/5ee2457b83de)

[![](https://cdn2.jianshu.io/assets/default_avatar/13-394c31a9cb492fcb39c27422ca7d2815.jpg)](https://www.jianshu.com/u/32e5e579b066)

[adu追风](https://www.jianshu.com/u/32e5e579b066)

0.1442018.07.17 10:33:16字数 61阅读 740

1.首先查看/etc/crontab文件是否存在

```
cat /etc/crontab
```

2.如果不存在，创建该文件

```
touch /etc/crontab
```

3.cron表达式

  

![](https://upload-images.jianshu.io/upload_images/7032863-a00a43d562bf3aed.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/png)

image.png

-   例子

```
0 0 * * *从左到右代表：分钟（0-59） 小时（0-23） 日（0-31） 月（1-12） 周（0-7，0也是周日）

```

30 11,17 \* \* \* /Users/clarence/bin/daily-backup  
50 21 \* \* \* /Users/duguangquan/shell/file.sh(每天21点50执行)

4.crontab命令

```
crontab -e //以编辑模式打开crontab配置文件
crontab -l //查看执行任务列表
crontab -r //删除没个用户的cron服务

```

1人点赞

[日记本](https://www.jianshu.com/nb/14656617)

更多精彩内容，就在简书APP

"小礼物走一走，来简书关注我"

还没有人赞赏，支持一下

[![  ](https://cdn2.jianshu.io/assets/default_avatar/13-394c31a9cb492fcb39c27422ca7d2815.jpg)](https://www.jianshu.com/u/32e5e579b066)

[adu追风](https://www.jianshu.com/u/32e5e579b066 "adu追风")

总资产1共写了1168字获得5个赞共1个粉丝

### 被以下专题收入，发现更多相似内容

收入我的专题
