# Python处理PDF及生成多层PDF - 掘金

source:: https://juejin.cn/post/6963434397928259621

Python提供了众多的PDF支持库，本文是在Python3环境下，试用了两个库来完成PDF的生成的功能。PyPDF对于读取PDF支持较好，但是没找到生成多层PDF的方法。Reportlab看起来更成熟，能够利用Canvas很方便的生成多层PDF，这样就能够实现图片扫描上来的内容也可以进行内容搜索的目标。

### Reportlab

#### 生成双层PDF

双层PDF应用PDF中的Canvas概念，先画文字，最后将图片画上去，这样就是两层的PDF。

```
import os

import time
from reportlab import platypus
from reportlab.lib.pagesizes import letter
from reportlab.lib.units import inch
from reportlab.platypus import SimpleDocTemplate, Image
from reportlab.pdfgen import canvas

image_file = "./42.png"


c = canvas.Canvas('reportlab_canvas.pdf', pagesize=letter)
width, height = letter

c.setFillColorRGB(0,0.77,0.77)

c.drawString( 3*inch, 3*inch, "Hello World")
c.drawImage(image_file, 0 , 0)
c.showPage()
c.save()
复制代码
```

### PyPDF2

#### 读取PDF

```
from PyPDF2 import PdfFileWriter, PdfFileReader

output = PdfFileWriter()
input1 = PdfFileReader(open("jquery.pdf", "rb"))


print(input1.getDocumentInfo())


print ("pdf_document.pdf has %d pages." % input1.getNumPages())


page_content = input1.getPage(0).extractText()
print( page_content )


output.addPage(input1.getPage(0))


output.addPage(input1.getPage(1).rotateClockwise(90))


outputStream = open("PyPDF2-output.pdf", "wb")
output.write(outputStream)
复制代码
```

但是PyPDF获取PDF内容有很多问题，可以看这个[问题列表](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmstamy2%2FPyPDF2%2Fissues%3Fpage%3D2%26q%3Dis%253Aissue%2Bis%253Aopen "https://github.com/mstamy2/PyPDF2/issues?page=2&q=is%3Aissue+is%3Aopen")。文档中也有说明。

> | extractText(self) | ## | # Locate all text drawing commands, in the order they are provided in the | # content stream, and extract the text. This works well for some PDF | # files, but poorly for others, depending on the generator used. This will | # be refined in the future. Do not rely on the order of text coming out of | # this function, as it will change if this function is made more | # sophisticated. | #  
> | # Stability: Added in v1.7, will exist for all future v1.x releases. May | # be overhauled to provide more ordered text in the future. | # @return a unicode string object

参考资料：  
1、[PDF 1.0](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fjaraco%2FPDF "https://github.com/jaraco/PDF")  
2、[PyPDF 2](https://link.juejin.cn/?target=https%3A%2F%2Fpypi.python.org%2Fpypi%2FPyPDF2 "https://pypi.python.org/pypi/PyPDF2")  
3、[PyPDF2 Homepage](https://link.juejin.cn/?target=http%3A%2F%2Fmstamy2.github.io%2FPyPDF2%2F "http://mstamy2.github.io/PyPDF2/")  
4、[PyPDF2 Documentation](https://link.juejin.cn/?target=https%3A%2F%2Fpythonhosted.org%2FPyPDF2%2F "https://pythonhosted.org/PyPDF2/")  
5、[python name 'file' is not defined的解决办法](https://link.juejin.cn/?target=http%3A%2F%2Fblog.csdn.net%2Fmenuconfig%2Farticle%2Fdetails%2F8672118 "http://blog.csdn.net/menuconfig/article/details/8672118")  
6、[ReportLab](https://link.juejin.cn/?target=http%3A%2F%2Fwww.reportlab.com%2Fopensource%2F "http://www.reportlab.com/opensource/")  
7、[用Python/reportlab生成PDF](https://link.juejin.cn/?target=https%3A%2F%2Fwww.dup2.org%2Fnode%2F1202 "https://www.dup2.org/node/1202")  
8、[Writing Pdf with Python: Add image](https://link.juejin.cn/?target=http%3A%2F%2Fwww.tylerlesmann.com%2F2009%2Fjan%2F28%2Fwriting-pdfs-python-adding-images%2F "http://www.tylerlesmann.com/2009/jan/28/writing-pdfs-python-adding-images/")
