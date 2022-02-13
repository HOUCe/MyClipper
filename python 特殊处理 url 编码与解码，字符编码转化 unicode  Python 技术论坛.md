# [python 特殊处理 url 编码与解码，字符编码转化 unicode | Python 技术论坛](https://learnku.com/articles/44727)

```
# 中文解析
import urllib
from urllib import parse
# URL编码与解码
wd4={"wd4":"客"}
pw4=parse.urlencode(wd4)
print(pw4)

# url解码
s='%E6%A4%8D%E7%89%A9%E5%85%B1%E4%BA%AB'
s=urllib.parse.unquote(s)
print(s)

# Python字符编码转换Unicode和str
ddd=u'\u9a71\u868a\u6c34\u6237\u5916\u55b7\u96fe\u6301\u4e45\u9632\u868a\u866b\u53ee\u54ac\u513f\u7ae5\u5b9d\u5b9d\u9a71\u868a\u6db2\u5bb6\u7528\u6cf0\u56fd\u539f\u6599\u82b1\u9732\u6c34'
ddd=ddd.encode("utf-8").decode("utf-8")
print(ddd)
```
