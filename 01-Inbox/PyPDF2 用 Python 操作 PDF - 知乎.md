# [PyPDF2 用 Python 操作 PDF - 知乎](https://zhuanlan.zhihu.com/p/26647491)

最近需要处理一些 PDF, 简单来说，就是需要对一些 PDF 进行合并分割等一些操作，由于 PDF 的数量有点多，使用一些 PDF 的工具来处理这个事情，一个个去点击处理，对于一个 Pythoner 来说，简直不能更傻了。xchaoinfo 在知乎问题 [如何使用自己的编程能力提升生活质量？](http://xn--/?%20-%20-9l0mr8fhn912a59b5z0brgpefwdt0b3x3a0eal2zg4hl7mmh3a8dkfyfin9f1i0a/) 回答的说

> _简单可重复的工作，坚决不做第二次，通通写个脚本自动化_

果断直接去 awesome-Python 去找找有没有 Python 操作 PDF 的优秀的第三方模块，发现 PyPDF2 满足我的需求，但是我在网上搜的好多教程都是基于 PyPDF 的，但是 PyPDF 自 2010年 12月开始就不在更新了，PyPDF2 接棒 PyPDF, 并且支持 Py2 Py3 的版本。故写此文简单介绍下 PyPDF2，已期对诸君有所益。  
本文基于 Python3.4.4 PyPDF2==1.26.0

### 安装

直接使用 pip 安装就可以了  
pip install PyPDF2

PyPDF2 包含了 PdfFileReader PdfFileMerger PageObject PdfFileWriter 四个常用的主要 Class。

### 　简单读写 PDF

```
from PyPDF2 import PdfFileReader, PdfFileWriter
infn = 'infn.pdf'
outfn = 'outfn.pdf'
# 获取一个 PdfFileReader 对象
pdf_input = PdfFileReader(open(infn, 'rb'))
# 获取 PDF 的页数
page_count = pdf_input.getNumPages()
print(page_count)
# 返回一个 PageObject
page = pdf_input.getPage(i)

# 获取一个 PdfFileWriter 对象
pdf_output = PdfFileWriter()
# 将一个 PageObject 加入到 PdfFileWriter 中
pdf_output.addPage(page)
# 输出到文件中
pdf_output.write(open(outfn, 'wb'))
```

### 应用实例 合并分割 PDF

```
from PyPDF2 import PdfFileReader, PdfFileWriter

def split_pdf(infn, outfn):
    pdf_output = PdfFileWriter()
    pdf_input = PdfFileReader(open(infn, 'rb'))
    # 获取 pdf 共用多少页
    page_count = pdf_input.getNumPages()
    print(page_count)
    # 将 pdf 第五页之后的页面，输出到一个新的文件
    for i in range(5, page_count):
        pdf_output.addPage(pdf_input.getPage(i))
    pdf_output.write(open(outfn, 'wb'))

def merge_pdf(infnList, outfn):
    pdf_output = PdfFileWriter()
    for infn in infnList:
        pdf_input = PdfFileReader(open(infn, 'rb'))
        # 获取 pdf 共用多少页
        page_count = pdf_input.getNumPages()
        print(page_count)
        for i in range(page_count):
            pdf_output.addPage(pdf_input.getPage(i))
    pdf_output.write(open(outfn, 'wb'))

if __name__ == '__main__':
    infn = 'infn.pdf'
    outfn = 'outfn.pdf'
    split_pdf(infn, outfn)
```

应用实例源代码可以在 [https://github.com/xchaoinfo/Py\-example-by-xchaoinfo](https://github.com/xchaoinfo/Py-example-by-xchaoinfo) 找到。

Refer: [PyPDF2 Documentation](http://pythonhosted.org/PyPDF2/)
