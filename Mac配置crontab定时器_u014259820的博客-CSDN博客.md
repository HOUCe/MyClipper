# [Mac配置crontab定时器_u014259820的博客-CSDN博客](https://blog.csdn.net/u014259820/article/details/114972318)

Mac自带有[crontab](https://so.csdn.net/so/search?q=crontab)，直接操作终端即可。  
1、通过su切换到root权限

```
$ sudo su
Password:

The default interactive shell is now zsh.
To update your account to use zsh, please run `chsh -s /bin/zsh`.
For more details, please visit https://support.apple.com/kb/HT208050.
bash-3.2
```

2、然后通过crontab -e进入编辑设置

```
bash-3.2
```

设置格式：  
分 时 日 月 星期几 执行命令  
\* \* \* \* \*  
\- - - - -  
| | | | |  
| | | | ±---- day of week (0 - 7) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat  
| | | ±---- month (1 - 12) OR jan,feb,mar,apr …  
| | ±------ day of month (1 - 31)  
| ±------- hour (0 - 23)  
±---------minute (0 - 59)  
具体可参考：[https://tool.lu/crontab/](https://tool.lu/crontab/)

3、如何查看已设置的crontab

```
bash-3.2
0 10 * * 1-5 /usr/local/Cellar/python@3.8/3.8.6_2/bin/python3.8 test.py
```

表示周一到周五每天10点执行一遍test.py脚本
