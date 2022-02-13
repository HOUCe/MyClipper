# [JSON — The Hitchhiker's Guide to Python](https://pythonguidecn.readthedocs.io/zh/latest/scenarios/json.html)

![https://farm5.staticflickr.com/4174/33928819683_97b5c6a184_k_d.jpg](https://farm5.staticflickr.com/4174/33928819683_97b5c6a184_k_d.jpg)

[json](https://docs.python.org/2/library/json.html) 库可以自字符串或文件中解析JSON。 该库解析JSON后将其转为Python字典或者列表。它也可以转换Python字典或列表为JSON字符串。

## 解析JSON[¶](https://pythonguidecn.readthedocs.io/zh/latest/scenarios/json.html#id2 "永久链接至标题")

创建下面包含JSON数据的字符串

json\_string \= '{"first\_name": "Guido", "last\_name":"Rossum"}'

它可以被这样解析：

import json
parsed\_json \= json.loads(json\_string)

然后它可以像一个常规的字典那样使用:

print(parsed\_json\['first\_name'\])
"Guido"

您可以把下面这个对象转为JSON：

d \= {
    'first\_name': 'Guido',
    'second\_name': 'Rossum',
    'titles': \['BDFL', 'Developer'\],
}

print(json.dumps(d))
'{"first\_name": "Guido", "last\_name": "Rossum", "titles": \["BDFL", "Developer"\]}'

## simplejson[¶](https://pythonguidecn.readthedocs.io/zh/latest/scenarios/json.html#simplejson "永久链接至标题")

json库是Python2.6版中加入的。如果您使用更早版本的Python， 可以通过PyPI获取 [simplejson](https://simplejson.readthedocs.io/en/latest/) 库。

simplejson类似json标准库，它使得使用老版本Python的开发者们可以使用json库中的最新特性。

如果json库不可用，您可以将simplejson取别名为json来使用：

import simplejson as json

在将simplejson当成json导入后，上面的例子会像您在使用标准json库一样正常运行。
