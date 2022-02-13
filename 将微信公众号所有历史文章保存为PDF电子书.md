# [将微信公众号所有历史文章保存为PDF电子书](https://mp.weixin.qq.com/s?__biz=MzI4NjQyMTM2Mw==&mid=2247483674&idx=1&sn=feced0fa9d6b87d8932d99abc8f85f07&chksm=ebdc7906dcabf010d83c554636bb66156d4d46f24be4efe54e369b65130019cde35cb0e5db74#rd)

相信在你所关注的微信公众号中，总会有一些作者在持续地输出优质内容。或许这些内容是你所处领域的精华干货，或许文章内的语句令人赏心悦目，以至于时不时你的脑中冒出这么一个想法：_我要将此微信公众号所有的文章都保存下来。_

但当看到众多的历史文章时，想到要一篇一篇地查看然后另存为PDF，会立即让人望而生畏。

So，本文为你提供一个较为极客的解决方案。

## 明确目的

**快速地**将某一微信公众号发布的所有历史文章都保存在一个PDF文件中。

## 了解笔者编写程序的逻辑

在下文中，笔者将通过Python语言编写代码，来实现将微信公众号所有历史文章保存下来的功能。

在这里需要先搞明白程序的逻辑：

1.  获取某一微信公众号所有历史文章的url链接
    
2.  编写可重复调用的函数，达到输入文章url链接，输出文章正文HTML内容的功能
    
3.  通过循环语句，将第一步获取到的所有url都调用第二步的函数
    
4.  将以上所有输出的正文内容拼接为一个HTML文件
    
5.  通过Chrome浏览器将HTML转为PDF文件即可
    

## 程序的具体实现步骤

### 1\. 通过Fiddler抓包获取查看历史文章的url链接

微信设置了限制，使得正常的浏览器不能打开微信查看历史文章的url链接。如下图所示，会显示**请在微信客户端打开链接**的字样：

![图片](http://mmbiz.qpic.cn/mmbiz_png/JEQ7aHgc61lO1rhN3xN0vIhxxvH8Syc1iaCyzduYVHqFVbnfVGw4hPt6hJqujIpETIDicT6RuibAgichBRJCUp9ZQw/640?wx_fmt=png&tp=png&wxfrom=5&wx_lazy=1&wx_co=1)

此时便需要借助网络抓包工具Fiddler来获取真正可在浏览器中打开的url。另外还需登录电脑端微信。

当用电脑端微信查看某一公众号历史消息时，Fiddler会记录到请求的https链接地址，复制此url在正常浏览器中打开即可。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如下图所示，此时便可通过正常浏览器查看某一公众号所有历史消息记录。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 2\. 从上述网页中提取所有文章的url链接

此时需注意，上图的网页只会缓存前几篇文章，你需要做的，就是不断将网页滚动条下拉至最底端，让所有历史文章缓存在此网页中。

接下来通过右键审查元素，复制并保存包含所有历史文章url的HTML源代码。

最后通过BeautifulSoup或者正则表达式提取出所有的正文链接，保存在数组中。

下面代码用**正则表达式**来提取所有历史文章的url链接：

```
# 引入正则库
import re
# 1.html为包含所有历史文章url的HTML源码文件
f = open('1.html','r')
c = f.read()
f.close()
# 正则表达式，提取所有符合正文格式的链接
links_b = re.findall('hrefs="(http://mp.weixin.qq.com.+?)"',c,re.S)
# 最终所有的url链接将会保存在数组links中
links = []
for i in links_b:
    # 保证数组正文链接不重复
if i not in links:
links.append(i)
```

### 3\. 循环语句合并且保存所有文章正文内容为HTML文件

首先我写了一个爬虫的类，创建对象时输入url便可：

```
import requests,re
from bs4 import BeautifulSoup
class Spider:
'网络爬虫'
def __init__(self,url):
self.url = url
self.content = self.getContent()
self.bs = self.toBs()

def getContent(self):
r = requests.get(self.url)
c = r.content
return c

def toBs(self):
b = BeautifulSoup(self.content,'lxml')
return b

def getGoalContent(self,tag,id,attr):
g = self.bs.find(tag,{id:attr})
return g
```

通过分析微信公众号的HTML代码，只需下面二行代码，便可获得返回文章正文内容（配合上面`class`使用）：

```
# 对象实例化
weixin = Spider(links[0])
# 调用类中的getGoalContent方法
content = str(weixin.getGoalContent('div','id','img-content'))
```

剩下的便是循环和保存为HTML文件的代码（以下代码有所删减，但不影响主要功能）：

```
# 最终所有历史文章正文合并后的HTML代码将保存在strs中
strs = ''
# 循环调用
for i in range(len(links)):
weixin = Spider(links[i])
content = str(weixin.getGoalContent('div','id','img-content'))
strs += content
# 保存为同一目录中的out.html文件
f = open('out.html','w')
f.write(strs)
f.close()
```

### 4\. 将HTML转换为PDF

将以上代码保存为`main.py`文件，然后在终端运行，便会在同一目录生成`out.html`文件。

用Chrome浏览器将`out.html`文件打开，右键**打印**，选择**导出PDF**即可。

以下是生成的PDF文件示例截图：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 后记

-   程序的逻辑部分较为重要，具体的代码方面上文有所删减
    
-   抓取到正文中的图片还需经过一定的程序处理才能显示
    
-   Fiddler抓包工具笔者也是刚上手，所以本文中没能详细介绍
    
-   欢迎扫码关注笔者的微信公众号，与笔者进行交流
    

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
