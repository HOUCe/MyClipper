# python读paper - 知乎

source:: https://zhuanlan.zhihu.com/p/350553046

前面跟大家简单介绍过[Python提取多个pdf首页合并输出](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649209174&idx=1&sn=dc9d5c966be7bfda6fc8b26d788f1d3c&chksm=f3d1f323c4a67a3577d2ff67dfa3adb55924c3bbbd28e018b5a1043ad868b192f83aa83de358&scene=21#wechat_redirect)，还有[Python轻松处理Excel](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649209111&idx=1&sn=50e6057dad3c76245b4368702b08e101&chksm=f3d1f362c4a67a743e7c41d79303227553c111bb9e1ec5793b51861c5f45e866ed1591fc40b0&scene=21#wechat_redirect)。有位粉丝留言python能不能从文献中提取特定的数字，希望能出一个教程，那么今天我们就来聊一聊如何用python读paper，提取特定的数字。

![](https://pic3.zhimg.com/v2-5212ba968545e15fb7d1e37bc9d565aa_b.jpg)

我们先来捋一捋思路：

1.  利用python打开pdf文件，提取其中的文本
2.  将每一行的文字分成单个词语
3.  利用正则表达式来匹配每一个词语，看是不是数字
4.  将文本写入到word文档中，如果是数字用黄色高亮
5.  保存word文档

接下来我们用python代码来实现

```
#加载pdf,word和正则表达式模块
import PyPDF2
import docx
from docx.enum.text import WD_COLOR_INDEX
import re

#打开要读的pdf文件
pdfFileObj = open('meetingminutes.pdf', 'rb')
#生成pdf对象
pdfReader = PyPDF2.PdfFileReader(pdfFileObj)

#获取pdf文件中的文本信息
lines = []
for i in range(pdfReader.numPages):
    pageObj = pdfReader.getPage(i)
    text = pageObj.extractText()
    lines += text.split("\n")

#匹配所有数字的正则表达式
regx = re.compile(r'[-+]?([0-9]+(\.[0-9]*)?|\.[0-9]+)([eE][-+]?[0-9]+)?')
#新建一个word对象,用来保存pdf文件的内容
doc = docx.Document()
#循环处理pdf文件中每一行文本
for line in lines:
    #在word文档中添加段落
    para = doc.add_paragraph('')
    #对pdf文件中每一行文字，分成单词来处理
    words = line.split(" ")
    for word in words:
        #在word文档的每一个段落中再添加run
        run = para.add_run(word+" ")
        #如果单词是数字就用黄色来高亮显示
        mo = regx.findall(word)
        if len(mo) > 0:
            run.font.highlight_color = WD_COLOR_INDEX.YELLOW
#最后保存word文档
doc.save('highlighted_pdf_number.docx')

```

关于python处理word涉及到的两个概念paragraph和run在《[python让繁琐工作自动化](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649209362&idx=1&sn=df07544912641e8621efe5dcf44124ad&chksm=f3d1f067c4a67971fc8566294b9d8c78d7649334fc1b06759ef4ef22bd3da81cd399155338de&scene=21#wechat_redirect)》这本书中有详细介绍，大家感兴趣可以下去仔细读一下。

![](https://pic3.zhimg.com/v2-6dec5168d57d5eb4f40633f326c43c6e_b.jpg)

下图展示的试pdf文件中的本分内容

![](https://pic1.zhimg.com/v2-628a9c64381f726d8ee60178c5b40a2c_b.jpg)

下图展示的是高亮之后的word文档。这里的格式可能和原来pdf文件的格式不太一样，但是内容是一样的。

![](https://pic1.zhimg.com/v2-daf99380866f404d6259012ab97c2b38_b.jpg)

这个任务中用到的代码均出自于我前面提到《[python让繁琐工作自动化](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649209362&idx=1&sn=df07544912641e8621efe5dcf44124ad&chksm=f3d1f067c4a67971fc8566294b9d8c78d7649334fc1b06759ef4ef22bd3da81cd399155338de&scene=21#wechat_redirect)》这本书。为方便大家交流学习，小编为大家准备了本书的pdf版本，参考[python让繁琐工作自动化](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649209362&idx=1&sn=df07544912641e8621efe5dcf44124ad&chksm=f3d1f067c4a67971fc8566294b9d8c78d7649334fc1b06759ef4ef22bd3da81cd399155338de&scene=21#wechat_redirect)就可以获取。如果喜欢纸质版也可以购买一本，当做工具书，需要的时候随手翻一下。

参考资料：  

1.  [Python提取多个pdf首页合并输出](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649209174&idx=1&sn=dc9d5c966be7bfda6fc8b26d788f1d3c&chksm=f3d1f323c4a67a3577d2ff67dfa3adb55924c3bbbd28e018b5a1043ad868b192f83aa83de358&scene=21#wechat_redirect)
2.  [python让繁琐工作自动化](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649209362&idx=1&sn=df07544912641e8621efe5dcf44124ad&chksm=f3d1f067c4a67971fc8566294b9d8c78d7649334fc1b06759ef4ef22bd3da81cd399155338de&scene=21#wechat_redirect)

发布于 2021-02-12 20:14，编辑于 2021-08-08 11:11

### 文章被以下专栏收录

[

![python](https://pic1.zhimg.com/4b70deef7_xs.jpg?source=172ae18b)



](https://www.zhihu.com/column/c_1291486684983230464)

Drag to outliner or Upload

Close
