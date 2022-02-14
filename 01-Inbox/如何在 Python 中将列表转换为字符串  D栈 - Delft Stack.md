# [如何在 Python 中将列表转换为字符串 | D栈 - Delft Stack](https://www.delftstack.com/zh/howto/python/how-to-convert-a-list-to-string/)

1.  [](https://www.delftstack.com/zh)
2.  [贴士文章](https://www.delftstack.com/zh/howto/)
3.  [Python 贴士](https://www.delftstack.com/zh/howto/python/)
4.  [如何在 Python 中将列表转换为字符串](https://www.delftstack.com/zh/howto/python/how-to-convert-a-list-to-string/)

创建时间: December-26, 2019 | 更新时间: July-18, 2021

1.  [`str` 在 Python 中将列表转换为字符串](https://www.delftstack.com/zh/howto/python/how-to-convert-a-list-to-string/#str-%25E5%259C%25A8-python-%25E4%25B8%25AD%25E5%25B0%2586%25E5%2588%2597%25E8%25A1%25A8%25E8%25BD%25AC%25E6%258D%25A2%25E4%25B8%25BA%25E5%25AD%2597%25E7%25AC%25A6%25E4%25B8%25B2)
2.  [`str` 在 Python 中将非列表转换为字符串](https://www.delftstack.com/zh/howto/python/how-to-convert-a-list-to-string/#str-%25E5%259C%25A8-python-%25E4%25B8%25AD%25E5%25B0%2586%25E9%259D%259E%25E5%2588%2597%25E8%25A1%25A8%25E8%25BD%25AC%25E6%258D%25A2%25E4%25B8%25BA%25E5%25AD%2597%25E7%25AC%25A6%25E4%25B8%25B2)

## [`str` 在 Python 中将列表转换为字符串](https://www.delftstack.com/zh/howto/python/how-to-convert-a-list-to-string/#str-%E5%9C%A8-python-%E4%B8%AD%E5%B0%86%E5%88%97%E8%A1%A8%E8%BD%AC%E6%8D%A2%E4%B8%BA%E5%AD%97%E7%AC%A6%E4%B8%B2)

我们可以使用 `str.join()` 方法将具有 `str` 数据类型元素的列表转换为字符串。

例如，

 pythonCopy`A = ["a", "b", "c"]
StrA = "".join(A)
print(StrA)` 

`join` 方法连接任意数量的字符串，被调用该方法的字符串被插入每个给定的字符串之间。如示例中所示，字符串 `""` （一个空字符串）被插入列表元素之间。

如果要在元素之间添加空格，则应使用

 pythonCopy`StrA = " ".join(A)` 

## [`str` 在 Python 中将非列表转换为字符串](https://www.delftstack.com/zh/howto/python/how-to-convert-a-list-to-string/#str-%E5%9C%A8-python-%E4%B8%AD%E5%B0%86%E9%9D%9E%E5%88%97%E8%A1%A8%E8%BD%AC%E6%8D%A2%E4%B8%BA%E5%AD%97%E7%AC%A6%E4%B8%B2)

`join` 方法需要将 `str` 数据类型作为给定参数。因此，如果你尝试转换 `int` 类型列表，你将获得 `TypeError`。

 pythonCopy`>>> a = [1,2,3]
>>> "".join(a)
Traceback (most recent call last):
  File "<pyshell#1>", line 1, in <module>
    "".join(a)
TypeError: sequence item 0: expected str instance, int found` 

`int` 类型应该先转换为 `str` 类型，然后再执行结合操作。

### [](https://www.delftstack.com/zh/howto/python/how-to-convert-a-list-to-string/#%E5%88%97%E8%A1%A8%E6%8E%A8%E5%AF%BC%E5%BC%8F-https-docs-python-org-3-tutorial-datastructures-html-list-comprehensions)[列表推导式](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)

 pythonCopy`>>> a = [1,2,3]
>>> "".join([str(_) for _ in a])
"123"` 

### [](https://www.delftstack.com/zh/howto/python/how-to-convert-a-list-to-string/#map-%E5%87%BD%E6%95%B0-http-book-pythontips-com-en-latest-map-filter-html-map)[`map` 函数](http://book.pythontips.com/en/latest/map_filter.html#map)

 pythonCopy`>>> a = [1,2,3]
>>> "".join(map(str, a))
'123'` 

`map` 函数将函数 `str` 应用于列表 `a` 中的所有元素，并返回一个可迭代的 `map` 对象。

`"".join()` 迭代 `map` 对象中的所有元素，并将连接的元素作为字符串返回。

## [相关文章 - Python String](https://www.delftstack.com/zh/tags/python-string/)

-   [在 Python 中创建一个多行字符串](https://www.delftstack.com/zh/howto/python/python-multi-line-string/ "在 Python 中创建一个多行字符串")
-   [用 Python 将每个单词的首字母大写](https://www.delftstack.com/zh/howto/python/python-capitalize-each-word/ "用 Python 将每个单词的首字母大写")

## [相关文章 - Python List](https://www.delftstack.com/zh/tags/python-list/)

-   [删除 Python 字符串中的前导零](https://www.delftstack.com/zh/howto/python/python-remove-leading-zeros/ "删除 Python 字符串中的前导零")
-   [Python 中的等效 toString()函数](https://www.delftstack.com/zh/howto/python/tostring-equivalent-python/ "Python 中的等效 toString()函数")
