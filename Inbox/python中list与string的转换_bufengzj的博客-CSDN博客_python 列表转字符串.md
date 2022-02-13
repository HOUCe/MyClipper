# [python中list与string的转换_bufengzj的博客-CSDN博客_python 列表转字符串](https://blog.csdn.net/bufengzj/article/details/90231555)

### 1.list转string

命令：**''.join(list)**

其中，引号中是字符之间的分割符，如“,”，“;”，“\\t”等等

如：

list = \[1, 2, 3, 4, 5\]

''.join(list) 结果即为：12345

','.join(list) 结果即为：1,2,3,4,5

### 2.string转list

命令：list(str)

```
#输出：['a', 'b', 'c', 'd', 'e']
```

![](https://img-blog.csdnimg.cn/2019051511233646.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2J1ZmVuZ3pq,size_16,color_FFFFFF,t_70)

这里在jupyter报错，在pycharm上没有出错，网上说是命名成list问题，但改过也如此。母鸡了！

值得注意的是，比如“abs"这种字符串，要想分隔开，只能用list("abs"),才能得到\['a','b','c'\]

或者使用string的函数

**str.split()**

这个内置函数实现的是将str转化为list。其中str=""是分隔符。

```
>>> line = "Hello.I am qiwsir.Welcome you."['Hello', 'I am qiwsir', 'Welcome you', '']['Hello', 'I am qiwsir.Welcome you.']    >>> name = "Albert Ainstain"  
```

值得注意的是：  
 

python里字符串数组转化为整型， 用**list(map(type,arr))**函数

```
>>> arr = ['22','44','66','88']>>> arr = ['22','44','66','88']>>> arr = list(map(int,arr))
```

### 3.为什么要转来转去？

肯定是各有好处了！

list是列表，其特点是不定长，所以可以list.append随时增加，也可以insert插入

```
list1=['a','b','c','d','e']#输出：['a', 'b', 'c', 'c', 'd', 'e']
```

下标索引【详见[https://www.runoob.com/python/python-lists.html](https://www.runoob.com/python/python-lists.html)】：

```
list1 = ['physics', 'chemistry', 1997, 2000]list2 = [1, 2, 3, 4, 5, 6, 7 ]print "list1[0]: ", list1[0]print "list2[1:5]: ", list2[1:5]list1 = ['physics', 'chemistry', 1997, 2000]
```

![](https://img-blog.csdnimg.cn/20190515124327459.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2J1ZmVuZ3pq,size_16,color_FFFFFF,t_70)

这里附上list类常用函数与方法：

_作者：张xxxxxx   
来源：CSDN   
原文：https://blog.csdn.net/weixin\_38284096/article/details/80409856_ 

Python列表操作的函数和方法  
列表操作包含以下函数:  
1、cmp(list1, list2)：比较两个列表的元素   
2、len(list)：列表元素个数   
3、max(list)：返回列表元素最大值   
4、min(list)：返回列表元素最小值   
5、list(seq)：将元组转换为列表   
列表操作包含以下方法:  
1、list.append(obj)：在列表末尾添加新的对象  
2、list.count(obj)：统计某个元素在列表中出现的次数  
3、list.extend(seq)：在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表）  
4、list.index(obj)：从列表中找出某个值第一个匹配项的索引位置  
5、list.insert(index, obj)：将对象插入列表  
6、list.pop(obj=list\[-1\])：移除列表中的一个元素（默认最后一个元素），并且返回该元素的值  
7、list.remove(obj)：移除列表中某个值的第一个匹配项  
8、list.reverse()：反向列表中元素  
9、list.sort(\[func\])：对原列表进行排序  
\---------------------   
list容易实现队列queue的操作【出队，入队】，还有栈stack的操作，出栈入栈；

[https://blog.csdn.net/w571523631/article/details/55099385](https://blog.csdn.net/w571523631/article/details/55099385 "垃圾中文技术性网站")

```
raise Exception("The queue is empty.")        self._items.append(newItem)def __getitem__(self, item):raise Exception('The stack is empty.')        self._items.insert(0, newItem)def __getitem__(self, item):def refresh(self, newItems):
```

python2里加了object就可以有更多的继承属

来自：https://blog.csdn.net/w571523631/article/details/55099385

```
if __name__ == "__main__":Person ['__doc__', '__module__', 'name']Animal ['__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'name']Person类很明显能够看出区别，不继承object对象，只拥有了doc , module 和 自己定义的name变量, 也就是说这个类的命名空间只有三个对象可以操作. Animal类继承了object对象，拥有了好多可操作对象，这些都是类中的高级特性。对于不太了解python类的同学来说，这些高级特性基本上没用处，但是对于那些要着手写框架或者写大型项目的高手来说，这些特性就比较有用了，比如说tornado里面的异常捕获时就有用到class来定位类的名称，还有高度灵活传参数的时候用到dict来完成.最后需要说清楚的一点， 本文是基于python 2.7.10版本，实际上在python 3 中已经默认就帮你加载了object了（即便你没有写上object）。
```

那么string呢？

资料来自：菜鸟编程 [https://www.runoob.com/python/python-strings.html](https://www.runoob.com/python/python-strings.html)

## Python字符串运算符

下表实例变量 a 值为字符串 "Hello"，b 变量值为 "Python"：

操作符

描述

实例

+

字符串连接

\>>>a + b 'HelloPython'

\*

重复输出字符串

\>>>a \* 2 'HelloHello'

\[\]

通过索引获取字符串中字符

\>>>a\[1\] 'e'

\[ : \]

截取字符串中的一部分

\>>>a\[1:4\] 'ell'

in

成员运算符 - 如果字符串中包含给定的字符返回 True

\>>>"H" in a True

not in

成员运算符 - 如果字符串中不包含给定的字符返回 True

\>>>"M" not in a True

## Python 字符串格式化

Python 支持格式化字符串的输出 。尽管这样可能会用到非常复杂的表达式，但最基本的用法是将一个值插入到一个有字符串格式符 %s 的字符串中。

在 Python 中，字符串格式化使用与 C 中 sprintf 函数一样的语法。

python字符串格式化符号:

    符   号

描述

      %c

 格式化字符及其ASCII码

      %s

 格式化字符串

      %d

 格式化整数

      %u

 格式化无符号整型

      %o

 格式化无符号八进制数

      %x

 格式化无符号十六进制数

      %X

 格式化无符号十六进制数（大写）

      %f

 格式化浮点数字，可指定小数点后的精度

      %e

 用科学计数法格式化浮点数

      %E

 作用同%e，用科学计数法格式化浮点数

      %g

 %f和%e的简写

      %G

 %f 和 %E 的简写

      %p

 用十六进制数格式化变量的地址

 字符串可以很方便的下标索引，连接，复制

## Python转义字符

在需要在字符中使用特殊字符时，python用反斜杠(\\)转义字符。如下表：

转义字符

描述

\\(在行尾时)

续行符

\\\\

反斜杠符号

\\'

单引号

\\"

双引号

\\a

响铃

\\b

退格(Backspace)

\\e

转义

\\000

空

\\n

换行

\\v

纵向制表符

\\t

横向制表符

\\r

回车

\\f

换页

\\oyy

八进制数，yy代表的字符，例如：\\o12代表换行

\\xyy

十六进制数，yy代表的字符，例如：\\x0a代表换行

\\other

其它的字符以普通格式输出

___

## Python字符串运算符

下表实例变量 a 值为字符串 "Hello"，b 变量值为 "Python"：

操作符

描述

实例

+

字符串连接

\>>>a + b 'HelloPython'

\*

重复输出字符串

\>>>a \* 2 'HelloHello'

\[\]

通过索引获取字符串中字符

\>>>a\[1\] 'e'

\[ : \]

截取字符串中的一部分

\>>>a\[1:4\] 'ell'

in

成员运算符 - 如果字符串中包含给定的字符返回 True

\>>>"H" in a True

not in

成员运算符 - 如果字符串中不包含给定的字符返回 True

\>>>"M" not in a True

r/R

原始字符串 - 原始字符串：所有的字符串都是直接按照字面的意思来使用，没有转义特殊或不能打印的字符。 原始字符串除在字符串的第一个引号前加上字母"r"（可以大小写）以外，与普通字符串有着几乎完全相同的语法。

\>>>print r'\\n' \\n >>> print R'\\n' \\n

%

格式字符串

请看下一章节

##  

## Python 字符串格式化

Python 支持格式化字符串的输出 。尽管这样可能会用到非常复杂的表达式，但最基本的用法是将一个值插入到一个有字符串格式符 %s 的字符串中。

在 Python 中，字符串格式化使用与 C 中 sprintf 函数一样的语法。

python字符串格式化符号:

 符   号

描述

 %c

 格式化字符及其ASCII码

 %s

 格式化字符串

 %d

 格式化整数

 %u

 格式化无符号整型

 %o

 格式化无符号八进制数

 %x

 格式化无符号十六进制数

 %X

 格式化无符号十六进制数（大写）

 %f

 格式化浮点数字，可指定小数点后的精度

 %e

 用科学计数法格式化浮点数

 %E

 作用同%e，用科学计数法格式化浮点数

 %g

 %f和%e的简写

 %G

 %f 和 %E 的简写

 %p

 用十六进制数格式化变量的地址

格式化操作符辅助指令:

符号

功能

\*

定义宽度或者小数点精度

\-

用做左对齐

+

在正数前面显示加号( + )

<sp>

在正数前面显示空格

#

在八进制数前面显示零('0')，在十六进制前面显示'0x'或者'0X'(取决于用的是'x'还是'X')

0

显示的数字前面填充'0'而不是默认的空格

%

'%%'输出一个单一的'%'

(var)

映射变量(字典参数)

m.n.

m 是显示的最小总宽度,n 是小数点后的位数(如果可用的话)

___

## python的字符串内建函数

字符串方法是从python1.6到2.0慢慢加进来的——它们也被加到了Jython中。

这些方法实现了string模块的大部分方法，如下表所示列出了目前字符串内建支持的方法，所有的方法都包含了对Unicode的支持，有一些甚至是专门用于Unicode的。

**方法**

**描述**

[string.capitalize()](https://www.runoob.com/python/att-string-capitalize.html)

把字符串的第一个字符大写

[string.center(width)](https://www.runoob.com/python/att-string-center.html)

返回一个原字符串居中,并使用空格填充至长度 width 的新字符串

**[string.count(str, beg=0, end=len(string))](https://www.runoob.com/python/att-string-count.html)**

返回 str 在 string 里面出现的次数，如果 beg 或者 end 指定则返回指定范围内 str 出现的次数

[string.decode(encoding='UTF-8', errors='strict')](https://www.runoob.com/python/att-string-decode.html)

以 encoding 指定的编码格式解码 string，如果出错默认报一个 ValueError 的 异 常 ， 除非 errors 指 定 的 是 'ignore' 或 者'replace'

[string.encode(encoding='UTF-8', errors='strict')](https://www.runoob.com/python/att-string-encode.html)

以 encoding 指定的编码格式编码 string，如果出错默认报一个ValueError 的异常，除非 errors 指定的是'ignore'或者'replace'

**[string.endswith(obj, beg=0, end=len(string))](https://www.runoob.com/python/att-string-endswith.html)**

检查字符串是否以 obj 结束，如果beg 或者 end 指定则检查指定的范围内是否以 obj 结束，如果是，返回 True,否则返回 False.

[string.expandtabs(tabsize=8)](https://www.runoob.com/python/att-string-expandtabs.html)

把字符串 string 中的 tab 符号转为空格，tab 符号默认的空格数是 8。

**[string.find(str, beg=0, end=len(string))](https://www.runoob.com/python/att-string-find.html)**

检测 str 是否包含在 string 中，如果 beg 和 end 指定范围，则检查是否包含在指定范围内，如果是返回开始的索引值，否则返回-1

**[string.format()](https://www.runoob.com/python/att-string-format.html)**

格式化字符串

**[string.index(str, beg=0, end=len(string))](https://www.runoob.com/python/att-string-index.html)**

跟find()方法一样，只不过如果str不在 string中会报一个异常.

[string.isalnum()](https://www.runoob.com/python/att-string-isalnum.html)

如果 string 至少有一个字符并且所有字符都是字母或数字则返

回 True,否则返回 False

[string.isalpha()](https://www.runoob.com/python/att-string-isalpha.html)

如果 string 至少有一个字符并且所有字符都是字母则返回 True,

否则返回 False

[string.isdecimal()](https://www.runoob.com/python/att-string-isdecimal.html)

如果 string 只包含十进制数字则返回 True 否则返回 False.

[string.isdigit()](https://www.runoob.com/python/att-string-isdigit.html)

如果 string 只包含数字则返回 True 否则返回 False.

[string.islower()](https://www.runoob.com/python/att-string-islower.html)

如果 string 中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True，否则返回 False

[string.isnumeric()](https://www.runoob.com/python/att-string-isnumeric.html)

如果 string 中只包含数字字符，则返回 True，否则返回 False

[string.isspace()](https://www.runoob.com/python/att-string-isspace.html)

如果 string 中只包含空格，则返回 True，否则返回 False.

[string.istitle()](https://www.runoob.com/python/att-string-istitle.html)

如果 string 是标题化的(见 title())则返回 True，否则返回 False

[string.isupper()](https://www.runoob.com/python/att-string-isupper.html)

如果 string 中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是大写，则返回 True，否则返回 False

**[string.join(seq)](https://www.runoob.com/python/att-string-join.html)**

以 string 作为分隔符，将 seq 中所有的元素(的字符串表示)合并为一个新的字符串

[string.ljust(width)](https://www.runoob.com/python/att-string-ljust.html)

返回一个原字符串左对齐,并使用空格填充至长度 width 的新字符串

[string.lower()](https://www.runoob.com/python/att-string-lower.html)

转换 string 中所有大写字符为小写.

[string.lstrip()](https://www.runoob.com/python/att-string-lstrip.html)

截掉 string 左边的空格

[string.maketrans(intab, outtab\])](https://www.runoob.com/python/att-string-maketrans.html)

maketrans() 方法用于创建字符映射的转换表，对于接受两个参数的最简单的调用方式，第一个参数是字符串，表示需要转换的字符，第二个参数也是字符串表示转换的目标。

[max(str)](https://www.runoob.com/python/att-string-max.html)

返回字符串 _str_ 中最大的字母。

[min(str)](https://www.runoob.com/python/att-string-min.html)

返回字符串 _str_ 中最小的字母。

**[string.partition(str)](https://www.runoob.com/python/att-string-partition.html)**

有点像 find()和 split()的结合体,从 str 出现的第一个位置起,把 字 符 串 string 分 成 一 个 3 元 素 的 元 组 (string\_pre\_str,str,string\_post\_str),如果 string 中不包含str 则 string\_pre\_str == string.

**[string.replace(str1, str2,  num=string.count(str1))](https://www.runoob.com/python/att-string-replace.html)**

把 string 中的 str1 替换成 str2,如果 num 指定，则替换不超过 num 次.

[string.rfind(str, beg=0,end=len(string) )](https://www.runoob.com/python/att-string-rfind.html)

类似于 find()函数，不过是从右边开始查找.

[string.rindex( str, beg=0,end=len(string))](https://www.runoob.com/python/att-string-rindex.html)

类似于 index()，不过是从右边开始.

[string.rjust(width)](https://www.runoob.com/python/att-string-rjust.html)

返回一个原字符串右对齐,并使用空格填充至长度 width 的新字符串

[string.rpartition(str)](https://www.runoob.com/python/att-string-rpartition.html)

类似于 partition()函数,不过是从右边开始查找

[string.rstrip()](https://www.runoob.com/python/att-string-rstrip.html)

删除 string 字符串末尾的空格.

**[string.split(str="", num=string.count(str))](https://www.runoob.com/python/att-string-split.html)**

以 str 为分隔符切片 string，如果 num 有指定值，则仅分隔 num+ 个子字符串

[string.splitlines(\[keepends\])](https://www.runoob.com/python/att-string-splitlines.html)

按照行('\\r', '\\r\\n', \\n')分隔，返回一个包含各行作为元素的列表，如果参数 keepends 为 False，不包含换行符，如果为 True，则保留换行符。

[string.startswith(obj, beg=0,end=len(string))](https://www.runoob.com/python/att-string-startswith.html)

检查字符串是否是以 obj 开头，是则返回 True，否则返回 False。如果beg 和 end 指定值，则在指定范围内检查.

**[string.strip(\[obj\])](https://www.runoob.com/python/att-string-strip.html)**

在 string 上执行 lstrip()和 rstrip()

[string.swapcase()](https://www.runoob.com/python/att-string-swapcase.html)

翻转 string 中的大小写

[string.title()](https://www.runoob.com/python/att-string-title.html)

返回"标题化"的 string,就是说所有单词都是以大写开始，其余字母均为小写(见 istitle())

**[string.translate(str, del="")](https://www.runoob.com/python/att-string-translate.html)**

根据 str 给出的表(包含 256 个字符)转换 string 的字符,

要过滤掉的字符放到 del 参数中

[string.upper()](https://www.runoob.com/python/att-string-upper.html)

转换 string 中的小写字母为大写

[string.zfill(width)](https://www.runoob.com/python/att-string-zfill.html)

返回长度为 width 的字符串，原字符串 string 右对齐，前面填充0

```
 返回长度，都可以用len函数。len(list)或者len(string)。这些性质类似，是因为他们都是序列型数据。所谓序列类型的数据，就是说它的每一个元素都可以通过指定一个编号，行话叫做“偏移量”的方式得到，而要想一次得到多个元素，可以使用切片。偏移量从0开始，总元素数减1结束。
```

[https://www.jb51.net/article/55418.htm](https://www.jb51.net/article/55418.htm "垃圾中文技术性网站")这篇文章讲了另一些区别：

1.string的位置上只能放字符，而list是可以放多种类型信息，甚至多维数据

2.string不能在原来位置上，直接改动，而list可以。

![](https://img-blog.csdnimg.cn/20190515125219212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2J1ZmVuZ3pq,size_16,color_FFFFFF,t_70)

**多维list**

这个也应该算是两者的区别了，虽然有点牵强。在str中，里面的每个元素只能是字符，在list中，元素可以是任何类型的数据。前面见的多是数字或者字符，其实还可以这样：

1

2

3

4

5

6

7

8

9

10

11

`>>> matrix` `=` `[[``1``,``2``,``3``],[``4``,``5``,``6``],[``7``,``8``,``9``]]`

`>>> matrix` `=` `[[``1``,``2``,``3``],[``4``,``5``,``6``],[``7``,``8``,``9``]]`

`>>> matrix[``0``][``1``]`

`2`

`>>> mult` `=` `[[``1``,``2``,``3``],[``'a'``,``'b'``,``'c'``],``'d'``,``'e'``]`

`>>> mult`

`[[``1``,` `2``,` `3``], [``'a'``,` `'b'``,` `'c'``],` `'d'``,` `'e'``]`

`>>> mult[``1``][``1``]`

`'b'`

`>>> mult[``2``]`

`'d'`

以上显示了多维list以及访问方式。在多维的情况下，里面的list也跟一个前面元素一样对待。

### 4.既然各有好处，可否合起来呢？string\[\]与list<string>

参考这两篇文章

[https://blog.csdn.net/zxf1242652895/article/details/83345573](https://blog.csdn.net/zxf1242652895/article/details/83345573 "垃圾中文技术性网站")

[https://blog.csdn.net/qq\_33774822/article/details/83188727](https://blog.csdn.net/qq_33774822/article/details/83188727 "垃圾中文技术性网站")

1.参考文章：[https://www.cnblogs.com/huwang-sun/p/6972990.html](https://www.cnblogs.com/huwang-sun/p/6972990.html)
