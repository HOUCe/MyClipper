# [python中四种获取文件后缀名的方法【附源码】_Python热爱者_51CTO博客](https://blog.51cto.com/u_14246112/2678341)

获取文件的后缀名有好几种方法：

**第一种：splittext()方法**

```
os.path.splittext(path)[-1]
```

**第二种：endswith()方法**

```
path = "test_user_info.py"
bool = path.endswith(".py")
print(bool)
```

**第三种：判断后缀名是否在字符串中（这种会存在误判，若是.pyx后缀，一样会打印True，前面两种不会）**

```
'''
遇到问题没人解答？小编创建了一个Python学习交流QQ群：531509025
寻找有志同道合的小伙伴，互帮互助,群里还有不错的视频学习教程和PDF电子书！
'''
path = "test_user_info.py"
if ".py" in path:
    print(True)
```

**第四种：用split方法切割，但是吧这种只是拿到了py没有点，所以再加上点也是可以的**

```
path = "test_user_info.py"
suffix = path.split(".")[1]
print("suffix: {}".format(suffix))
```

举报文章

请选择举报类型

内容侵权 涉嫌营销 内容抄袭 违法信息 其他

上传截图

格式支持JPEG/PNG/JPG，图片不超过1.9M

已经收到您得举报信息，我们会尽快审核

 [![](https://ucenter.51cto.com/images/noavatar_middle.gif)](https://blog.51cto.com/) 

## 相关文章
