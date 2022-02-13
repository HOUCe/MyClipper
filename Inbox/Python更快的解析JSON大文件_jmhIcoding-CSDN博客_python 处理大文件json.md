# [Python更快的解析JSON大文件_jmhIcoding-CSDN博客_python 处理大文件json](https://blog.csdn.net/jmh1996/article/details/78360375)

提出问题  
今天用python的simplejson库解析一个 >200MB 的JSON文件，发现一次[decode](https://so.csdn.net/so/search?q=decode)/encode都得要 >10s，这个在我开来，实在太慢了，有没有更快的库了？

___

先给出我的简单测试结果  
json大小：245MB  
测试方法：read文件内容，然后一次decode, 一次encode

解释器

simplejson

json

ujson

pypy

40s多

10s

无

cpython

12s多

17s多

10s多

不成熟的结论： pypy+json最快

___

方法一：pypy+json  
python自带的JSON库是用纯python代码实现的，而pypy对纯python代码的加速效果比较好。至于为什么，大家可以去google吧，很多文章解释的很好。

___

方法二：UltraJson  
我首先想到的用C库来做JSON的解析，原因你懂的，而C语言有个JSON库叫CJSON，于是用python+cjson在google里找到了UltraJson  
UltraJson是作者用C语言实现的JSON库，实际测试的效果是，整个encode的效率提升了2倍多。  
使用方法：  
安装：pip instal ujson

```
    >>> import ujson
    >>> ujson.dumps([{"key": "value"}, 81, True])
    '[{"key":"value"},81,true]'
    >>> ujson.loads("""[{"key": "value"}, 81, true]""")
    [{u'key': u'value'}, 81, True]
```

___

一些测试  
Test machine:  
Linux 3.13.0-66-generic x86\_64 #108-Ubuntu SMP Wed Oct 7 15:20:27 UTC 2015

Versions:  
CPython 2.7.6 (default, Jun 22 2015, 17:58:13) \[GCC 4.8.2\]  
blist : 1.3.6  
simplejson: 3.8.1  
ujson : 1.34 (0c52200eb4e2d97e548a765d5f089858c41967b0)  
yajl : 0.3.5  
Array with 256 UTF-8 strings

Object

ujson

yajl

simplejson

json

encode

3508.19

5742.00

3232.38

3309.09

decode

25103.37

11257.83

11696.26

11871.04

Array with 256 UTF-8 strings

Object

ujson

yajl

simplejson

json

encode

3189.71

2717.14

2006.38

2961.72

decode

1354.94

630.54

356.35

344.05

Array with 256 strings

Object

ujson

yajl

simplejson

json

encode

18127.47

12537.39

12541.23

20001.00

decode

23264.70

12788.85

25427.88

9352.36

Medium complex object

Object

ujson

yajl

simplejson

json

encode

10519.38

5021.29

3686.86

4643.47

decode

9676.53

5326.79

8515.77

3017.30

Array with 256 True values

Object

ujson

yajl

simplejson

json

encode

105998.03

102067.28

44758.51

60424.80

decode

163869.96

78341.57

110859.36

115013.90

Array with 256 dict{string, int} pairs

Object

ujson

yajl

simplejson

json

encode

13471.32

12109.09

3876.40

8833.92

decode

16890.63

8946.07

12218.55

3350.72

Dict with 256 arrays with 256 dict{string, int} pairs

Object

ujson

yajl

simplejson

json

encode

50.25

46.45

13.82

29.28

decode

33.27

22.10

27.91

10.43

Dict with 256 arrays with 256 dict{string, int} pairs, outputting sorted keys

Object

ujson

yajl

simplejson

json

encode

27.19

7.75

2.39

Complex object

Object

ujson

yajl

simplejson

json

encode

577.98

387.81

470.02

decode

496.73

234.44

151.00

145.16
