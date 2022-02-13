# [Python基础学习教程：Python玩转PDF各种骚操作大全_ITPUB博客](http://blog.itpub.net/69923331/viewspace-2648009/)

Python基础学习教程：Python玩转PDF各种骚操作大全！

Portable Document Format（可移植文档格式），或者PDF是一种文件格式，可以用于跨操作系统的呈现和文档交换。尽管PDF最初是由Adobe发明的，但它现在是由国际标准化组织（ISO）维护的开放标准。你可以通过使用PyPDF2包在Python中处理已先存在的PDF。

PyPDF2是一个纯Python包，可用于许多不同类型的PDF操作。

![Python基础学习教程：Python玩转PDF各种骚操作大全](http://p3.pstatp.com/large/pgc-image/1936a465aca446bbb70b6f0eb26da97c)

这里一篇 Python基础学习教程 带你了解如何执行以下操作：

-   从Python中提取PDF中的文档信息
-   旋转页面
-   合并PDF
-   拆分PDF
-   添加水印
-   加密PDF

PyPdf PyPDF2 PyPDF4的历史

最初的pyPdf软件包于2005年发布。pyPdf的最后一个正式版本是在2010年。大约一年后，一家名为Phasit的公司赞助了一个名为PyPDF2的pyPdf分支。该代码编写为向后与原始代码兼容，并且用了好多年，效果一直很好，其最后一个版本是在2016年。

有一个名为PyPDF3的软件包简短系列版本，然后该项目被重命名为PyPDF4。所有这些项目都完全相同，但pyPdf和PyPDF2 +之间的最大区别在于后者版本增加了Python 3支持。Python 3的原始pyPdf有一个不同的Python 3分支，但是这个分支已经多年没有维护了。

虽然最近放弃了PyPDF2，但新的PyPDF4与PyPDF2没有完全的向后兼容性。本文中的大多数示例都可以与PyPDF4完美配合，但也有一些不能，这就是为什么PyPDF4在本文中没有更多的特色。随意用PyPDF4替换PyPDF2的导入，看看它是如何工作的。

pdfrw：一个替代的PDF操作包

Patrick Maupin创建了一个名为pdfrw的软件包，它可以完成许多与PyPDF2相同的工作。除了加密的特殊情况外，本文后面提到PyPDF2的所有操作，pdfrw均可以实现。

pdfrw的最大区别在于它与ReportLab软件包集成，因此你可以使用一些或所有预先存在的PDF构建一个新的PDF。

PyPDF2的安装

如果使用Anaconda而不是常规Python，可以使用pip或conda安装PyPDF2。以下是使用pip安装PyPDF2的方法：

$ pip install pypdf2

由于PyPDF2没有任何依赖，因此安装非常快。

如何从Python\*\*\*\*中提取PDF文档信息

我们可以使用PyPDF2从PDF中提取元数据和一些文本，尤其是当在预先存在的PDF文件上执行某些类型的自动化时是非常有用的。

以下是当前可以提取的数据类型：

-   Author
-   Creator
-   Producer
-   Subject
-   Title
-   Number of page

可以在自己的电脑上随便找一个PDF文件进行尝试操作。下面是使用该PDF编写一些代码，并了解如何访问这些属性：

from PyPDF2 import PdfFileReader
def extract\_information(pdf\_path):
 with open(pdf\_path, 'rb') as f:
 pdf = PdfFileReader(f)
 information = pdf.getDocumentInfo()
 number\_of\_pages = pdf.getNumPages()
 txt = f"""
 Information about {pdf\_path}:
 Author: {information.author}
 Creator: {information.creator}
 Producer: {information.producer}
 Subject: {information.subject}
 Title: {information.title}
 Number of pages: {number\_of\_pages}
 """
 print(txt)
 return information
if \_\_name\_\_ == '\_\_main\_\_':
 path = 'xxxx.pdf'
 extract\_information(path)

首先从PyPDF2包导入PdfFileReader。PdfFileReader是一个具有多种与PDF文件交互的方法的类。在此示例中，我们调用了.getDocumentInfo()，它将返回DocumentInformation的实例,包含了我们感兴趣的大部分信息。我们还可以在reader对象上调用.getNumPages()，让它返回文档中的页数。

information这个变量具有多个实例属性，可以使用这些属性从文档中获取所需的其余元数据。我们可以打印出该信息并将其返回以备将来使用。

虽然PyPDF2具有.extractText()，可以在其页面对象上使用提取文本（本例中未显示），但它的效果不是很好。有些PDF会返回文本，有些会返回空字符串。如果要从PDF中提取文本，建议应该看一下PDFMiner项目。PDFMiner更加强大，专门用于从PDF中提取文本。

如何旋转页面？

有时候PDF是横向模式而不是纵向模式，甚至是颠倒的。当有人扫描文档为PDF或电子邮件时，很可能会发生这种情况。我们可以打印出文档并阅读纸质版本，也可以使用Python的强大功能来旋转有问题的页面。

下面看一下如何使用PyPDF2旋转文章的一些页面：

from PyPDF2 import PdfFileReader, PdfFileWriter
def rotate\_pages(pdf\_path):
 pdf\_writer = PdfFileWriter()
 pdf\_reader = PdfFileReader(path)
 # 顺时针旋转90度
 page\_1 = pdf\_reader.getPage(0).rotateClockwise(90)
 pdf\_writer.addPage(page\_1)
 # 逆时针旋转90度
 page\_2 = pdf\_reader.getPage(1).rotateCounterClockwise(90)
 pdf\_writer.addPage(page\_2)
 # 在正常方向上添加一页
 pdf\_writer.addPage(pdf\_reader.getPage(2))
 with open('rotate\_pages.pdf', 'wb') as fh:
 pdf\_writer.write(fh)
if \_\_name\_\_ == '\_\_main\_\_':
 path = '新路径.pdf'
 rotate\_pages(path)

上面除了pdfileReader之外，还导入了pdfileWriter，因为我们需要编写一个新的pdf。rotate\_pages()获取要修改的PDF的路径。在这个函数中，需要创建一个可以命名为pdf-writer的writer对象和一个名为pdf-reader的reader对象。

接下来，可以使用.get page()获取所需的页面。上面开始输入了第0页，也就是第一页，调用page对象的.rotateClockwise()顺时针旋转方法并输入90。然后同样地，对于第二页，调用.rotateCounterLockwise()逆时针旋转并输入90。

每次调用Rotation旋转方法后，都会调用.addPage()，这将向writer对象添加页面的旋转版本。最后一页是第3页，没有对其进行任何旋转。最后，使用.write()把所有新页写入新的PDF。

如何合并PDF？

在许多情况下，我们希望将两个或多个PDF合并到一个PDF中。例如，现在可能有一个标准的封面，需要转到许多类型的报告中。这时候就可以使用python来帮助完成这类工作。

下面是实现的代码，完成PDF合并的操作：

from PyPDF2 import PdfFileReader, PdfFileWriter
def merge\_pdfs(paths, output):
 pdf\_writer = PdfFileWriter()
 for path in paths:
 pdf\_reader = PdfFileReader(path)
 for page in range(pdf\_reader.getNumPages()):
 # 将每页添加到writer对象
 pdf\_writer.addPage(pdf\_reader.getPage(page))
 # 写入合并的pdf
 with open(output, 'wb') as out:
 pdf\_writer.write(out)
if \_\_name\_\_ == '\_\_main\_\_':
 paths = \['document1.pdf', 'document2.pdf'\]
 merge\_pdfs(paths, output='merged.pdf')

假如有一个要合并到一起的pdf列表时，可以直接使用merge\_pdf函数完成。此函数采用了输入路径和输出路径作为参数。

首先遍历输入的paths，并为每个输入创建一个PDF阅读对象。然后遍历PDF文件中的所有页面，并使用.addpage()将这些页面写入writer对象。当完成对列表中所有PDF的所有页面的写入后，将在末尾写入新的结果中。

如果不想合并每个PDF的所有页面，可以通过添加一系列要添加的页面来稍微增强这个脚本。挑战一点的话，也可以使用Python的argparse模块为这个函数创建一个命令行接口。

如何拆分PDF?

有时可能需要将PDF拆分为多个PDF，对于包含大量扫描内容的PDF来说尤其重要。以下是如何使用PyPDF2将PDF拆分为多个文件：

from PyPDF2 import PdfFileReader, PdfFileWriter
def split(path, name\_of\_split):
 pdf = PdfFileReader(path)
 for page in range(pdf.getNumPages()):
 pdf\_writer = PdfFileWriter()
 pdf\_writer.addPage(pdf.getPage(page))
 output = f'{name\_of\_split}{page}.pdf'
 with open(output, 'wb') as output\_pdf:
 pdf\_writer.write(output\_pdf)
if \_\_name\_\_ == '\_\_main\_\_':
 path = 'xxx.pdf'
 split(path, 'jupyter\_page')

这个函数中再次创建了PDF的reaer对象，并对其所读取的页面进行遍历。对于PDF中的每个页面，创建一个新的PDF的writer实例并向其添加单个页面。然后，将该页面写入一个唯一命名的文件。脚本运行完毕后，就可以将原始PDF的每个页面拆分为单独的PDF。

如何添加水印？

水印是纸质或者电子文档上的图像或图案，一些水印只能在特殊照明条件下才能看到。水印的重要性在于它可以保护你的知识产权，例如图像或PDF。

我们可以使用Python和PyPDF2为文档添加水印，而且是拥有仅包含水印图像或文本的PDF。下面是向PDF添加水印方法：

from PyPDF2 import PdfFileWriter, PdfFileReader

def create\_watermark(input\_pdf, output, watermark):

watermark\_obj = PdfFileReader(watermark)

watermark\_page = watermark\_obj.getPage(0)

pdf\_reader = PdfFileReader(input\_pdf)

pdf\_writer = PdfFileWriter()

\# 给所有页面添加水印

for page in range(pdf\_reader.getNumPages()):

page = pdf\_reader.getPage(page)

page.mergePage(watermark\_page)

pdf\_writer.addPage(page)

with open(output, 'wb') as out:

pdf\_writer.write(out)

if \_\_name\_\_ == '\_\_main\_\_':

create\_watermark(

input\_pdf='Jupyter\_Notebook\_An\_Introduction.pdf',

output='watermarked\_notebook.pdf',

watermark='watermark.pdf')

上面create\_watermark有三个参数：

-   input\_pdf：要加水印的PDF文件路径
-   output：要保存PDF的水印版本的路径
-   watermark：包含水印图像或文本的PDF

在代码中，打开水印PDF并从文档中抓取第一页，因为这是水印应该驻留的位置。然后使用input\_pdf和通用pdf\_writer对象创建PDF的writer对象，以写出带水印的PDF。

下一步是遍历input\_pdf中的页面，然后调用.mergePage()并以用上面读取的水印对象watermark\_page为参数，这样会将watermark\_page覆盖在当前页面的顶部，然后再将新合并的页面添加到pdf\_writer对象中。遍历完成后，最后将新加水印的PDF写入磁盘。

如何加密PDF？

PyPDF2目前仅支持将用户密码和所有者密码添加到预先存在的PDF。在PDF版本中，所有者密码会提供PDF的管理员权限，并允许设置文档的权限，而用户密码只允许打开文档。

实际上，PyPDF2是不允许设置文档的任何权限的，即使它允许设置所有者密码的情况下。但无论如何，这是可以加密的方式，也将固有地加密PDF：

from PyPDF2 import PdfFileWriter, PdfFileReader
def add\_encryption(input\_pdf, output\_pdf, password):
 pdf\_writer = PdfFileWriter()
 pdf\_reader = PdfFileReader(input\_pdf)
 for page in range(pdf\_reader.getNumPages()):
 pdf\_writer.addPage(pdf\_reader.getPage(page))
 pdf\_writer.encrypt(user\_pwd=password, owner\_pwd=None, 
 use\_128bit=True)
 with open(output\_pdf, 'wb') as fh:
 pdf\_writer.write(fh)
if \_\_name\_\_ == '\_\_main\_\_':
 add\_encryption(input\_pdf='reportlab-sample.pdf',
 output\_pdf='reportlab-encrypted.pdf',
 password='twofish')

add\_encryption以输入输出PDF路径和要添加到PDF的密码为参数。由于需要加密整个输入PDF，因此需要遍历其所有页面并将其添加到writer编写器。最后一步是调用.encrypt()，以用户密码，所有者密码以及是否应该添加128位加密为参数。默认情况下，要启用128位加密。如果将其设置为False，则将应用40位加密。

结论

PyPDF2包非常有用，可以使用PyPDF2自动执行脚本完成PDF文档的批量操作。本文介绍了如何从PDF中提取元数据，旋转页面，合并和拆分PDF，添加水印，以及添加加密的操作。

同时，还要关注较新的PyPDF4包，因为它很快就会取代PyPDF2。也可以看看pdfrw包，它也可以执行许多与PyPDF2相同的操作。

来自 “ ITPUB博客 ” ，链接：http://blog.itpub.net/69923331/viewspace-2648009/，如需转载，请注明出处，否则将追究法律责任。
