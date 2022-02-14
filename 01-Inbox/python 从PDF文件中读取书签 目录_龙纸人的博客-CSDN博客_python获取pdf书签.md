# [python 从PDF文件中读取书签/目录_龙纸人的博客-CSDN博客_python获取pdf书签](https://blog.csdn.net/qq_41248959/article/details/110097464)

**有时候需要将PDF文件中的书签读取出来进行处理,因此写这篇博客记录具体的处理过程(某些pdf文件会出现打不开报错的情况,目前暂无处理办法)**

> 使用python3 , 需导入PyPDF2模块

## 需要使用到的函数

getOutlines()检索文档中存在的文本大纲,返回的对象是一个嵌套的列表  
举个例子 ,加入你的pdf文档的目录如下图所示  
![pdf文档目录](https://img-blog.csdnimg.cn/20201124214743789.PNG#pic_center)  
那么使用getOutlines()读取出来的嵌套列表为

```
[

{'/Title': '第1章 绪论', '/Page': IndirectObject(16, 0), '/Type': '/Fit'},

 [{'/Title': '1.1模块与接口', '/Page': IndirectObject(16, 0), '/Type':    '/Fit'},
 {'/Title': '1.2 工具和软件', '/Page': IndirectObject(18, 0), '/Type': '/Fit'}, 
 {'/Title': '1.3树语言的数据结构', '/Page': IndirectObject(18, 0), '/Type': '/Fit'}, 
 {'/Title': '程序设计：直线式程序解释器', '/Page': IndirectObject(22, 0), '/Type': '/Fit'}, 
 {'/Title': '推荐阅读', '/Page': IndirectObject(23, 0), '/Type': '/Fit'},
 {'/Title': '习题', '/Page': IndirectObject(24, 0), '/Type': '/Fit'}],
 
{'/Title': '第2章 词法分析', '/Page': IndirectObject(25, 0), '/Type': '/Fit'}, 

[{'/Title': '2.1词法单词', '/Page': IndirectObject(25, 0), '/Type': '/Fit'}, 
{'/Title': '2.2正则表达式', '/Page': IndirectObject(26, 0), '/Type': '/Fit'}, 
{'/Title': '2.3有限自动机', '/Page': IndirectObject(28, 0), '/Type': '/Fit'}, 

{'/Title': '2.4非确定有限自动机', '/Page': IndirectObject(30, 0), '/Type': '/Fit'}, 
[{'/Title': '2.4.1将正则表达式转换为NFA', '/Page': IndirectObject(31, 0), '/Type': '/Fit'}, 
{'/Title': '2.4.2将NFA转换为DFA', '/Page': IndirectObject(33, 0), '/Type': '/Fit'}],

{'/Title': '2.5 Lex：词法分析器的生成器', '/Page': IndirectObject(35, 0), '/Type': '/Fit'}, 
{'/Title': '程序设计：词法分析', '/Page': IndirectObject(37, 0), '/Type': '/Fit'}, 
{'/Title': '推荐阅读', '/Page': IndirectObject(38, 0), '/Type': '/Fit'}, {'/Title': '习题', '/Page': IndirectObject(38, 0), '/Type': '/Fit'}]

]
```

总结一下就是书签是一级目录时, 其在列表中是一个字典;二级目录是在列表中再嵌套一个列表;三级目录是在二级目录的列表中再嵌套一个列表,以此类推。

我们的目的是读出所有字典中的‘/Title’键所对应的值，可以使用递归函数实现该功能。当列表中的值不是列表变量而是字典变量时，取出字典中‘/Title’键所对应的值，存储起来

```
from PyPDF2 import PdfFileReader as pdf_read

#每个书签的索引格式
#{'/Title': '书签名', '/Page': '指向的目标页数', '/Type': '类型'}

directory_str = ''
def bookmark_listhandler(list):
    global directory_str
    for message in list:
        if isinstance(message, dict):
            directory_str += message['/Title'] + '\n'
            # print(message['/Title'])
        else:
            bookmark_listhandler(message)

with open('现代编译原理.pdf', 'rb') as f:
    pdf = pdf_read(f)
    #检索文档中存在的文本大纲,返回的对象是一个嵌套的列表
    text_outline_list = pdf.getOutlines()
    bookmark_listhandler(text_outline_list)

with open('现代编译原理目录.txt', 'w', encoding='utf-8') as f:
    f.write(directory_str)
```
