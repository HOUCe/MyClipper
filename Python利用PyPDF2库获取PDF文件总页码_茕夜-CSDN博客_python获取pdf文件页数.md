# [Python利用PyPDF2库获取PDF文件总页码_茕夜-CSDN博客_python获取pdf文件页数](https://blog.csdn.net/u012561176/article/details/104021130)

[Python](https://so.csdn.net/so/search?q=Python)中可以利用PyPDF2库来获取该pdf文件的总页码，可以根据下面的方法一步步进行下去：

1、首先，要安装PyPDF2库，利用以下命令即可：

```
pip install PyPDF2
```

2、接着，就是直接编写代码了，其中我新建了一个py文件，名为file\_utils.py，代码如下：

```
from PyPDF2 import PdfFileReaderdef get_num_pages(file_path):"""    获取文件总页码    :param file_path: 文件路径    :return:    """    reader = PdfFileReader(file_path)# 不解密可能会报错：PyPDF2.utils.PdfReadError: File has not been decryptedif reader.isEncrypted:        reader.decrypt('')    page_num = reader.getNumPages()return page_num
```

3、这样就可以获得该pdf文件的总页数了，但是需要传递文件路径进去，因为需要读取这个文件。

4、以上内容仅供学习参考，谢谢！
