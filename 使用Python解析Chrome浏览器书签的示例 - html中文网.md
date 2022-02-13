# [使用Python解析Chrome浏览器书签的示例 - html中文网](https://m.html.cn/script/python/36590.html)

Chrome 浏览器的书签如果可以导出，并转换为我们需要的格式时，我们就可以编写各种插件来配合书签的使用。

答案显然是可以的，接下来我们以 Python 为例写一个遍历打印书签的例子

**书签地址**

先来说下获取书签的方法

Chrome 浏览器的书签存放位置在各个平台的区别

-   Mac

 ~/Library/Application Support/Google/Chrome/Default/Bookmarks 

-   Linux

 ~/.config/google-chrome/Default/Bookmarks 

-   Windows

 %LOCALAPPDATA%"\\Google\\Chrome\\User Data\\Default\\Bookmarks" 

**书签结构  
**

书签内容为 JSON 格式，结构如下

 { "checksum":"b196f618a9166d56dc6c98cfe9a98d45", "roots":{ "bookmark\_bar":{ "children":\[ { "date\_added":"13246172853099058", "guid":"83431411-157f-45f8-a9a4-d9af26c71bce", "id":"1944", "name":"blog local 温欣爸比的博客", "type":"url", "url":"http://localhost:4000/" }, { "children":\[ { "date\_added":"13246172853099058", "guid":"83431411-157f-45f8-a9a4-d9af26c71bce", "id":"1944", "name":"blog local 温欣爸比的博客", "type":"url", "url":"http://localhost:4000/" } \], "date\_added":"13246172844427649", "date\_modified":"13246172865895702", "guid":"6aa4ecce-a220-4689-9239-7df10965748b", "id":"1943", "name":"Blog", "type":"folder" } \], "date\_added":"13242060909278534", "date\_modified":"13246172853099058", "guid":"00000000-0000-4000-a000-000000000002", "id":"1", "name":"书签栏", "type":"folder" }, "other":{ "children":\[ \], "date\_added":"13242060909278616", "date\_modified":"0", "guid":"00000000-0000-4000-a000-000000000003", "id":"2", "name":"其他书签", "type":"folder" }, "synced":{ "children":\[ \], "date\_added":"13242060909278621", "date\_modified":"0", "guid":"00000000-0000-4000-a000-000000000004", "id":"3", "name":"移动设备书签", "type":"folder" } }, "sync\_metadata":"", "version":1 }

清晰了这个结构在写代码就很简单了，以书签栏为例，只需要将 data\['roots'\]\['bookmark\_bar'\]\['children'\] 进行循环遍历即可，代码详情可见 [demo](https://github.com/wxnacy/study/blob/master/python/simple/single_line_progress.py)

**完整demo**

 #!/usr/bin/env python # -\*- coding:utf-8 -\*- # Author: wxnacy(wxnacy@gmail.com) # Description: 打印不换行进度条 # 预览 https://raw.githubusercontent.com/wxnacy/image/master/blog/python\_progress.gif import time def get\_progress(progress, total): '''获取进度条''' progress\_ratio = progress / total progress\_len = 20 progress\_num = int(progress\_ratio \* 20) pro\_text = '\[{:-<20s}\] {:.2f}% {} / {}'.format( '=' \* progress\_num, progress\_ratio \* 100, progress, total) return pro\_text def print\_progress(total): '''模拟打印进度条''' progress = 0 step = 30 while progress \= total: end = '\\n' progress = total print(get\_progress(progress, total), end = end) if \_\_name\_\_ == "\_\_main\_\_": print\_progress(100)

以上就是使用Python解析Chrome浏览器书签的示例的详细内容，更多关于Python解析Chrome浏览器书签的资料请关注html中文网其它相关文章！
