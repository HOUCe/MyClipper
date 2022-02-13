# [linux操作提示：“Can't open file for writing”或“operation not permitted”的解决办法_ykblog.github.io-CSDN博客](https://blog.csdn.net/yangkai_hudong/article/details/30513837)

![](https://csdnimg.cn/release/blogv2/dist/pc/img/original.png)

[y\_keven](https://ykblog.blog.csdn.net/ "垃圾中文技术性网站") 2014-06-13 18:35:39 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/articleReadEyes.png) 122798 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/tobarCollect.png) 收藏  9 

版权声明：本文为博主原创文章，遵循 [CC 4.0 BY-SA](http://creativecommons.org/licenses/by-sa/4.0/) 版权协议，转载请附上原文出处链接和本声明。

 在linux上使用vi命令修改一个文件内容的时候，发现无法保存，每次写完使用“:q!”命令可以正常退出但是使用"：wq!"命令保存文件并退出时出现一下信息提示：

      E212: Can't open file for writing Press ENTER or type command to continue

      出现这个错误的原因可能有两个：  
    1.当前用户的权限不足  
    2.此文件可能正被其他程序或用户使用。  
      一般错误原因都是前者，解决方案是在使用vi命令打开文件时，前面加上sudo来临时提供管理员权限，比如使用命令“sudo vi hosts”打开编辑文件。  
     由此看来，sudo命令是很有用的，当我们执行某种操作系统提示诸如“operation not permitted”等权限不足信息时，我们很多时候都可以在命令前面加上sudo来解决权限不足问题。比如当我们从linux服务器上下载某一个文件或上传某一个文件有可能提示这个，也有肯能直接上传不成功但是什么都没提示；这时你就应该想想是不是账号的权限不足，加个sudo试试。
