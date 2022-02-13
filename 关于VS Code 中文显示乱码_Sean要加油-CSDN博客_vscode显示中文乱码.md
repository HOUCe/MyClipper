# [关于VS Code 中文显示乱码_Sean要加油-CSDN博客_vscode显示中文乱码](https://blog.csdn.net/gongxun1994/article/details/80356031)

[VSCODE](https://so.csdn.net/so/search?q=VSCODE)默认是UTF-8编码打开文件的。如果遇到了像GB18030 GBK等等的编码，就显示乱码了。

方法一：  
找到右下角的UTF-8，上面正中出现“reopen with encoding”，发现可以点击。输入gbk或者gb18030。选对了编码打开，就不会乱码了。

方法二：  
文件->首选项->设置 ->搜索  
“files.autoGuessEncoding”: false  
将其用户设置改为  
“files.autoGuessEncoding”: true
