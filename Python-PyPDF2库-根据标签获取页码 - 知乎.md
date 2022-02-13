# [Python-PyPDF2库-根据标签获取页码 - 知乎](https://zhuanlan.zhihu.com/p/385983610)

之前利用PyPDF2库完成过PDF的拆分、合并、页面重组的功能，主要利用了`PdfFileReader`、`PdfFileWriter`、`PdfFileMerger`。单纯论PDF页面级别的操作，PyPDF2库是比较灵活的，而且最近还发现PdfFileReader甚至可以读取PDF的标签内容。----环境Python3.8

正好当前有一个需求是**根据不同的一级标签，拆分PDF**。现有资料极少描述如何根据标签获取相应的页码，这里主要运用了`.getOutlines()`和`.getDestinationPageNumber()`。

1.  首先，利用`PdfFileReader`创建一个pdf对象，获取该pdf的标签的代码如下：

```
from PyPDF2 import PdfFileReader
pdf = PdfFileReader('PDF路径')
bookmarks = pdf.getOutlines()
```

返回值为代表所有标签对应类（PyPDF2库下的特定Destination类）的集合列表，顺序则按照pdf内的标签顺序排列，形如：

```
[{'/Title': 'Test 1', '/Page': IndirectObject(14997, 0), '/Type': '/FitH', '/Top': 846}, {'/Title': 'Test 2', '/Page': IndirectObject(7, 0), '/Type': '/FitH', '/Top': 846}, {'/Title': 'Test 3', '/Page': IndirectObject(18, 0), '/Type': '/FitH', '/Top': 846} ,......]
```

每个类均可以利用形如字典的方式提取该类的一些属性，其中'/Title': 'Test 1'中的'Test 1'即代表该标签下的内容。

2\. 接着，对于列表里的每个Destination类，可以用如下方式获取每个标签的页码，再进行拆分（此节不展开描述）：

```
page = [] #获得的标签页码的集合将被放入page列表
for i in range(len(bookmarks)):
    page.append([i['/Title'], pdf.getDestinationPageNumber(bookmarks[i])]) #这里放入了一个二维列表
```

3\. 实际上，题目中的例子较为简单，PDF中所有标签均为一级标签。如果存在二级标签，`.getOutlines()`将返回嵌套列表：

```
[{'/Title': 'Test 1', '/Page': IndirectObject(14997, 0), '/Type': '/FitH', '/Top': 846}, [{'/Title': 'Test 1-1', '/Page': IndirectObject(14998, 0), '/Type': '/XYZ', '/Left': -11, '/Top': 853, '/Zoom': <PyPDF2.generic.NullObject object at 0x0000017E1EF9E490>}, {'/Title': 'Test 1-2', '/Page': IndirectObject(1, 0), '/Type': '/XYZ', '/Left': -11, '/Top': 603, '/Zoom': <PyPDF2.generic.NullObject object at 0x0000017E1EF9E730>}], {'/Title': 'Test 2', '/Page': IndirectObject(7, 0), '/Type': '/FitH', '/Top': 846} ,......]
```

这时2中循环获取页码的方式可能出错，我们也可以再嵌套一个for循环。可是，如果存在更多级别的子级标签，而我们并不确定要加多少for循环。对于这类，可以用简单的递归方式实现。

```
page = []
def get_pagenum(page, lit):
    for i in range(len(lit):
        if len(i) == 1:
            page.append([i['/Title'], pdf.getDestinationPageNumber(lit[i])])
        else:
            get_pagenum(page, i)
    return page
get_pagenum(page, bookmarks)
```

> 现实中的难题是，手头PDF文件的标签实在是太不规范了……
