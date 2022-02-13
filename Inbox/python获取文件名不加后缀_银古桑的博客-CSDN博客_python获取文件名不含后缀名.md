# [python获取文件名不加后缀_银古桑的博客-CSDN博客_python获取文件名不含后缀名](https://blog.csdn.net/weixin_43886133/article/details/90784002)

文件名 test.py  
使用os.path  
方法一：

```
import os 
file_name = os.path.basename(__file__)
print(file_name)
# 输出为 test.py
file_name = file_name.split('.')[0]
print(file_name)
# 输出为 test
```

方法二：

```
stem, suffix = os.path.splitext(filename)
print(stem, suffix) # test   .py
```

使用 pathlib包

```
p = Path(file_name) # test.py
print(p.suffix)  # .py
print(p.stem)# test

```
