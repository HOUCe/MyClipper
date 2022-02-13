# [Python 从路径获取不带扩展名的文件名 | D栈 - Delft Stack](https://www.delftstack.com/zh/howto/python/python-get-filename-without-extension-from-path/)

1.  [](https://www.delftstack.com/zh)
2.  [贴士文章](https://www.delftstack.com/zh/howto/)
3.  [Python 贴士](https://www.delftstack.com/zh/howto/python/)
4.  [Python 从路径获取不带扩展名的文件名](https://www.delftstack.com/zh/howto/python/python-get-filename-without-extension-from-path/)

创建时间: February-07, 2021 | 更新时间: February-21, 2021

1.  [在 Python 中使用 `pathlib.path().stem` 方法从路径中获取没有扩展名的文件名](https://www.delftstack.com/zh/howto/python/python-get-filename-without-extension-from-path/#%25E5%259C%25A8-python-%25E4%25B8%25AD%25E4%25BD%25BF%25E7%2594%25A8-pathlib.path.stem-%25E6%2596%25B9%25E6%25B3%2595%25E4%25BB%258E%25E8%25B7%25AF%25E5%25BE%2584%25E4%25B8%25AD%25E8%258E%25B7%25E5%258F%2596%25E6%25B2%25A1%25E6%259C%2589%25E6%2589%25A9%25E5%25B1%2595%25E5%2590%258D%25E7%259A%2584%25E6%2596%2587%25E4%25BB%25B6%25E5%2590%258D)
2.  [使用 Python 中的 `os.path.splitext()` 和 `string.split()` 方法从路径中获取不带扩展名的文件名](https://www.delftstack.com/zh/howto/python/python-get-filename-without-extension-from-path/#%25E4%25BD%25BF%25E7%2594%25A8-python-%25E4%25B8%25AD%25E7%259A%2584-os.path.splitext-%25E5%2592%258C-string.split-%25E6%2596%25B9%25E6%25B3%2595%25E4%25BB%258E%25E8%25B7%25AF%25E5%25BE%2584%25E4%25B8%25AD%25E8%258E%25B7%25E5%258F%2596%25E4%25B8%258D%25E5%25B8%25A6%25E6%2589%25A9%25E5%25B1%2595%25E5%2590%258D%25E7%259A%2584%25E6%2596%2587%25E4%25BB%25B6%25E5%2590%258D)
3.  [在 Python 中使用 `os.path.basename()` 和 `os.path.splitext()` 方法从路径中获取文件名](https://www.delftstack.com/zh/howto/python/python-get-filename-without-extension-from-path/#%25E5%259C%25A8-python-%25E4%25B8%25AD%25E4%25BD%25BF%25E7%2594%25A8-os.path.basename-%25E5%2592%258C-os.path.splitext-%25E6%2596%25B9%25E6%25B3%2595%25E4%25BB%258E%25E8%25B7%25AF%25E5%25BE%2584%25E4%25B8%25AD%25E8%258E%25B7%25E5%258F%2596%25E6%2596%2587%25E4%25BB%25B6%25E5%2590%258D)

本教程将演示在 Python 中从文件路径中获取不带扩展名的文件名的各种方法。假设我们的目标是从以字符串形式存在的文件路径列表中获取文件名，比如从路径 `Desktop/folder/myfile.txt` 中，我们只得到没有 `.txt` 扩展名的文件名 `myfile`。

## [在 Python 中使用 `pathlib.path().stem` 方法从路径中获取没有扩展名的文件名](https://www.delftstack.com/zh/howto/python/python-get-filename-without-extension-from-path/#%E5%9C%A8-python-%E4%B8%AD%E4%BD%BF%E7%94%A8-pathlib-path-stem-%E6%96%B9%E6%B3%95%E4%BB%8E%E8%B7%AF%E5%BE%84%E4%B8%AD%E8%8E%B7%E5%8F%96%E6%B2%A1%E6%9C%89%E6%89%A9%E5%B1%95%E5%90%8D%E7%9A%84%E6%96%87%E4%BB%B6%E5%90%8D)

`path().stem` 方法将文件路径作为输入，通过从文件路径中提取文件名来返回。例如，从路径 `Desktop/folder/myfile.txt` 中，它将返回不含 `.txt` 扩展名的 `myfile`。

下面的代码示例演示了如何使用 `path().stem` 从文件路径中获取不含文件扩展名的文件名。

 pythonCopy`from pathlib import Path

file_path = "Desktop/folder/myfile.txt"
file_name = Path(file_path).stem
print(file_name)` 

输出：

 textCopy`myfile` 

## [使用 Python 中的 `os.path.splitext()` 和 `string.split()` 方法从路径中获取不带扩展名的文件名](https://www.delftstack.com/zh/howto/python/python-get-filename-without-extension-from-path/#%E4%BD%BF%E7%94%A8-python-%E4%B8%AD%E7%9A%84-os-path-splitext-%E5%92%8C-string-split-%E6%96%B9%E6%B3%95%E4%BB%8E%E8%B7%AF%E5%BE%84%E4%B8%AD%E8%8E%B7%E5%8F%96%E4%B8%8D%E5%B8%A6%E6%89%A9%E5%B1%95%E5%90%8D%E7%9A%84%E6%96%87%E4%BB%B6%E5%90%8D)

`os` 模块的 `path.splitext()` 方法将文件路径作为字符串输入，并将文件路径和文件扩展名作为输出。

由于我们要从文件路径中获取文件名，所以可以先用 `os.path.splitext()` 方法将文件路径中的文件扩展名去掉。拆分结果的第一个元素就是没有扩展名的文件路径。这个结果使用`/`作为分隔符进一步分割它。最后一个元素是没有扩展名的文件名。下面的代码示例演示了如何使用 `path.splitext()` 和 `string.split()` 方法从文件路径中获取不带扩展名的文件名。

 pythonCopy`import os

file_path = "Desktop/folder/myfile.txt"

file_path = os.path.splitext(file_path)[0]
file_name = file_path.split('/')[-1]
print(file_name)` 

输出：

 textCopy`test` 

## [在 Python 中使用 `os.path.basename()` 和 `os.path.splitext()` 方法从路径中获取文件名](https://www.delftstack.com/zh/howto/python/python-get-filename-without-extension-from-path/#%E5%9C%A8-python-%E4%B8%AD%E4%BD%BF%E7%94%A8-os-path-basename-%E5%92%8C-os-path-splitext-%E6%96%B9%E6%B3%95%E4%BB%8E%E8%B7%AF%E5%BE%84%E4%B8%AD%E8%8E%B7%E5%8F%96%E6%96%87%E4%BB%B6%E5%90%8D)

在 Python 中，`os` 模块的 `path.basename()` 方法将文件路径作为输入，并返回从文件路径中提取的基名。例如，`Desktop/folder/myfile.txt` 的基名是 `myfile.txt`。

由于我们要从文件路径中获取文件名，可以使用 `path.basename()` 方法提取基名，使用 `path.splitext()` 提取文件名。下面的代码示例演示了如何使用 `path.basename()` 和 `path.splitext()` 方法从文件路径中获取文件名。

 pythonCopy`import os

file_path = "Desktop/folder/myfile.txt"

basename = os.path.basename(file_path)
file_name = os.path.splitext(basename)[0]
print(file_name)` 

输出：

 pythonCopy`myfile` 

警告

如果文件的名字是 `myfile.tar.gz`，则上述所有方法都会返回 `myfile.tar` 作为文件名。

假设我们需要从路径 `Desktop/folder/myfile.tar.gz` 中获取不带 `.` 后面的部分的文件名，比如 `myfile` 而不是 `myfile.tar`，可以使用 `string.index()` 方法从 `myfile.tar` 中只提取 `myfile`。但是这个方法的缺点是，如果 `.` 是文件名的一部分，比如 `my.file.tar.gz`，它将返回 `my` 作为文件名。

下面的代码示例，我们如何使用 `string.index()` 从上面解释的方法的输出 `myfile.tar` 中删除 `.tar`。

 pythonCopy`file_name = "myfile.tar"
index = file_name.index('.')
file_name = file_name[:index]
print(file_name)

file_name = "my.file.tar"
index = file_name.index('.')
file_name = file_name[:index]
print(file_name)` 

输出：

 textCopy`myfile
my` 

## [相关文章 - Python File](https://www.delftstack.com/zh/tags/python-file/)

-   [将 Python 变量保存到文件](https://www.delftstack.com/zh/howto/python/python-save-variable-to-file/ "将 Python 变量保存到文件")
-   [Python 在文件中查找字符串](https://www.delftstack.com/zh/howto/python/python-find-string-in-file/ "Python 在文件中查找字符串")
