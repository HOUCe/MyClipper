# [Python获取文件路径、文件名和扩展名_小龙在线-CSDN博客_python 获取路径文件名](https://blog.csdn.net/lilongsy/article/details/99853925)

这里用到了`os.path.splitext()`和`os.path.split()`。

函数

作用

返回值

`os.path.splitext()`

分割文件名和扩展名

元组

`os.path.split()`

分割路径和文件名

元组

例子：

```
import os

url = "http://file.iqilu.com/custom/new/v2016/images/logo.png"

(file, ext) = os.path.splitext(url)
print(file)
print(ext)

(path, filename) = os.path.split(url)
print(filename)
print(path)
```

![](https://img-blog.csdnimg.cn/20190820160958194.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpbG9uZ3N5,size_16,color_FFFFFF,t_70)  
参考：  
https://docs.python.org/3/library/os.path.html
