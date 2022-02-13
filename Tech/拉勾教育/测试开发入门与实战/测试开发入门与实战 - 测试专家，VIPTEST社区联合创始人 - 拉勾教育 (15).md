---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=482#/detail/pc?id=4660
author: 
---

# [测试开发入门与实战 - 测试专家，VIPTEST社区联合创始人 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=482#/detail/pc?id=4660)


通过上一节课的学习，你已经迈入了测试开发的大门，能够自己搭建起一套融合 API 测试和 UI 测试的自动化框架，并且能把这套测试框架按照需要，快速部署到新项目中去，非常了不起。

本节课开始，我将带你探寻经典的本质，以下是这节课的框架供你参考。

![Lark20201021-171331.png](https://s0.lgstatic.com/i/image/M00/61/92/Ciqc1F-P-9eAUYldAAI9I7f4yvA883.png)

在正式的工作中数据驱动非常重要。可以说，我们测试一半的写代码时间都在准备数据、清理数据。既然数据驱动如此重要，那么我们要不要了解下什么是数据驱动？数据驱动的原理是什么？以及如何徒手实现呢？

### 什么是数据驱动？

数据驱动，指在自动化测试中处理测试数据的方式。

通常测试数据与功能函数分离，存储在**功能函数的外部位置**。在自动化测试运行时，数据驱动框架会读取数据源中的数据，把**数据作为参数传递到功能函数**中，并会根据数据的条数**多次运行同一个功能函数**。

数据驱动的数据源可以是函数外的数据集合、CSV 文件、Excel 表格、TXT 文件，以及数据库等。

### 数据驱动的好处有哪些？

#### 1.数据驱动能够减少重复代码

下面我们通过一个例子来看下数据驱动是如何减少代码重复的。

```
def book_order(user, product, num):
pass
book_order('张三', '前端自动化测试框架Cypress从入门到精通', 1)
book_order('李四', '测试开发入门与实战', 1)
book_order('王五', '[测试开发入门与实战,前端自动化测试框架Cypress从入门到精通]', 50)
```

没有数据驱动时，并且同一个功能函数存在多个测试数据，你只能多次调用这个功能函数；另外一旦某一个测试数据有更改/删除，你需要在函数调用里去更改相应的测试数据，非常不方便。

但有了测试驱动时，你的代码可能是下面这个样子。

```
data_book = './tests/data/testdata.csv'
@dataDrivenDecorator(data_book)
def book_order(user, product, num):
pass
```

这种情况下， 你无须进行多次调用，而且当你的测试数据发生改变时， 你仅需要更改数据源文件的数据就可以了。

#### 2.数据所属的测试用例失败，不会影响到其他测试数据对应的测试用例

同样举一个例子，没有数据驱动之前，假设我们有这样的一个函数：

```
test_data = [0, 1, 0, 1]
def test_without_data_driven(records):
for x in records:
assert x > 0
test_without_data_driven(test_data)
```

当你运行这段代码时，因为 test\_data 的第一个值是 0， 它不大于 0。所以断言失败，所有 test\_data 这个函数 0 后面的测试数据都没有执行。

如果有了数据驱动，则数据驱动会把这一个测试按照测试数据分解成多个测试，所有第一个测试数据失败不也会影响到后面的测试结果。

**了解了数据驱动的众多好处，我们来看下在 Python 中，应用比较广泛的两个数据驱动的框架。一个是 DDT（Data-Driven Tests），它是 unittest 框架中实现数据驱动的不二之选；另外一个是 parameterized，它是 pytest 能够实现数据驱动的秘诀。**

> 这一课时我先介绍 DDT，下一课时我将介绍 parameterized。

### DDT 含有哪些装饰器

#### 1.一个类装饰器

ddt 这个类装饰器必须装饰在 TestCase 的子类上，TestCase 是 unittest 框架中的一个基类，它实现了 Test Runner 驱动测试运行所需的接口（interface）。

#### 2.两个方法装饰器

分别是 data 和 file\_data。其中 data 装饰器，直接提供测试数据；file\_data 装饰器则从 JSON 或 YAML 文件加载测试数据。

**DDT 的使用步骤如下：**

-   使用 @ddt 装饰你的测试类；
    
-   使用 @data 或者 @file\_data 装饰你需要数据驱动的测试方法；
    
-   如一组测试数据有多个参数，则需 unpack，使用 @unpack 装饰你的测试方法。
    

### DDT 使用详解

**先安装 DDT**：

然后我以 lagouAPITest 框架里，tests 文件夹下的 test\_baidu.py 这个文件为例，来讲解下 ddt 的使用。

#### **1.ddt 直接提供数据**

```
from ddt import ddt, data, file_data, unpack
from selenium import webdriver
import unittest
import time
@ddt
class Baidu(unittest.TestCase):
def setUp(self):
        self.driver = webdriver.Chrome()
        self.driver.implicitly_wait(30)
        self.base_url = "http://www.baidu.com/"
    @data(['iTesting', 'iTesting'], ['helloqa.com', 'iTesting'])
    @unpack
def test_baidu_search(self, search_string, expect_string):
        driver = self.driver
        driver.get(self.base_url + "/")
        driver.find_element_by_id("kw").send_keys(search_string)
        driver.find_element_by_id("su").click()
        time.sleep(2)
        search_results = driver.find_element_by_xpath('//*[@id="1"]/h3/a').get_attribute('innerHTML')
        print(search_results)
        self.assertEqual(expect_string in search_results, True)
def tearDown(self):
        self.driver.quit()
if __name__ == "__main__":
    unittest.main(verbosity=2)
```

在这个例子中，我直接使用了 @data 装饰器。在这个装饰器中，我给出了测试的 2 组数据，分别是 **\['iTesting', 'iTesting'\]** 和 **\['helloqa.com', 'iTesting'\]**；然后我使用 @unpack 装饰器把每一组数据的数据 unpack 成一个个的参数传给我的函数 test\_baidu\_search。

直接运行这个文件，结果如下：

![Drawing 1.png](https://s0.lgstatic.com/i/image/M00/60/84/Ciqc1F-NdceAHdJAAABwa3neOXM327.png)

你注意下，虽然我们只有一个测试用例 test\_baidu\_search。但在生成的测试报告里，显示“Run 2 tests in 17.172s”，也就是 test\_baidu\_search 运行了 2 次，这就是 DDT 在起作用。

这是多组参数，每组多个数据的情况，如果每组仅有一个数据呢？你仅需要更改如下：

#### **2.ddt 使用函数提供数据**

ddt 直接提供数据，除去上述的直接把数据写在 @data() 的参数中外，还有一个情况，即数据先从函数获取，然后再写入 @data() 的参数中。

```
from ddt import ddt, data, file_data, unpack
from selenium import webdriver
import unittest
import time
def get_test_data():
    results = ['iTesting', 'iTesting'], ['helloqa.com', 'iTesting']
return results
@ddt
class Baidu(unittest.TestCase):
def setUp(self):
        self.driver = webdriver.Chrome()
        self.driver.implicitly_wait(30)
        self.base_url = "http://www.baidu.com/"
    @data(*get_test_data())
    @unpack
def test_baidu_search(self, search_string, expect_string):
        driver = self.driver
        driver.get(self.base_url + "/")
        driver.find_element_by_id("kw").send_keys(search_string)
        driver.find_element_by_id("su").click()
        time.sleep(2)
        search_results = driver.find_element_by_xpath('//*[@id="1"]/h3/a').get_attribute('innerHTML')
        print(search_results)
        self.assertEqual(expect_string in search_results, True)
def tearDown(self):
        self.driver.quit()
if __name__ == "__main__":
    unittest.main(verbosi
```

在本例中，我创建了一个函数 get\_test\_data() 用于获取我的测试数据。这个函数可以带参数，也可以不带参数，具体需要根据你的业务逻辑来。

**注意：get\_test\_data() 的返回值，一定需要遵守 ddt.data() 可接受的数据格式。即：**

**一组数据，每个数据为单个的值；多组数据，每组数据为一个列表或者一个字典。**

#### **3.ddt 使用文件提供数据 — JSON 和 YAML**

除了使用 @ddt 直接提供数据，DDT 还支持通过文件加载数据。

不过默认只支持两种格式 YAML 和 JSON，只有以“.yml” 或者“.yaml” 结尾的会被认作 YAML 文件，其他格式都将被认为是 JSON 文件。

-   **使用 JSON 文件**
    

如果把上述用例改成使用 JSON 文件，则我们的用例看起来是这样的：

```
|--lagouAPITest
    |-- .....
    |--tests
        |--test_baidu.py
        |--test_baidu.json
        |--__init__.py
```

首先，我们创建一个跟 test\_baidu.py 同名的文件 test\_baidu.json，内容如下：

```
{ "case1": {
"search_string": "itesting",
"expect_string": "iTesting"
  },
"case2": {
"search_string": "itesting",
"expect_string": "iTesting"
  }
}
```

然后更新 test\_baidu.py，更新后的代码如下所示：

```
from ddt import ddt, data, file_data, unpack
from selenium import webdriver
import unittest
import time
@ddt
class Baidu(unittest.TestCase):
def setUp(self):
        self.driver = webdriver.Chrome()
        self.driver.implicitly_wait(30)
        self.base_url = "http://www.baidu.com/"
    @file_data('test_baidu.json')
def test_baidu_search(self, search_string, expect_string):
        driver = self.driver
        driver.get(self.base_url + "/")
        driver.find_element_by_id("kw").send_keys(search_string)
        driver.find_element_by_id("su").click()
        time.sleep(2)
        search_results = driver.find_element_by_xpath('//*[@id="1"]/h3/a').get_attribute('innerHTML')
        print(search_results)
        self.assertEqual(expect_string in search_results, True)
def tearDown(self):
        self.driver.quit()
if __name__ == "__main__":
    unittest.main(verbosity=2)
```

可以看到，使用 @file\_data 这个装饰器，与使用 @data 的装饰器有一点不同：  
（1）@file\_data 这个装饰器里，文件的路径是相对于这个测试类本身来说的。在本例中为 Baidu 这个测试类所处的文件的相对位置；

（2）使用 @file\_data 无须使用 unpack，即使同一组数据的参数有多个。

-   **使用 YAML 文件**
    

如果想在 python 中使用 yaml 文件，则需要安装 PyYAML。

安装好后，我们在test\_baidu.json的同级目录下，创建一个文件test\_baidu.yaml，内容如下：

```
"case1":
  "search_string": "itesting"
  "expect_string": "iTesting"
"case2": 
  "search_string": "itesting"
  "expect_string": "iTesting"
```

然后，我们更改 test\_baidu.py，更改后的内容如下：

```
from ddt import ddt, data, file_data, unpack
from selenium import webdriver
import unittest
import time
try:
import yaml
except ImportError:
    have_yaml_support = False
else:
    have_yaml_support = True
needs_yaml = unittest.skipUnless(
    have_yaml_support, "Need YAML to run this test"
)
@ddt
class Baidu(unittest.TestCase):
def setUp(self):
        self.driver = webdriver.Chrome()
        self.driver.implicitly_wait(30)
        self.base_url = "http://www.baidu.com/"
    @needs_yaml
    @file_data('test_baidu.yaml')
def test_baidu_search(self, search_string, expect_string):
        driver = self.driver
        driver.get(self.base_url + "/")
        driver.find_element_by_id("kw").send_keys(search_string)
        driver.find_element_by_id("su").click()
        time.sleep(2)
        search_results = driver.find_element_by_xpath('//*[@id="1"]/h3/a').get_attribute('innerHTML')
        print(search_results)
        self.assertEqual(expect_string in search_results, True)
def tearDown(self):
        self.driver.quit()
if __name__ == "__main__":
    unittest.main(verbosity=2)
```

你可以看到，与使用 JSON 文件不同， 使用 YAML 文件必须要先安装 PyYaml。然后为了防止 yaml 导入失败，我定义了 needs\_yaml 这个装饰器，用来给我的程序加个安全判断。如果导入失败，则所有以 needs\_yaml 装饰的测试用例将不会执行。

#### 4.ddt 使用文件提供数据 — 其他格式数据文件

因为 ddt 默认只支持 JSON 和 YAML 格式的数据。但是我想使用其他数据格式怎么办？

常用的方式有如下两种：

-   先读取其他格式的文件（例如 Excel 格式），然后创建 ddt 支持的 JSON 或者 YAML 文件，最后把获取到的数据写入这个文件，再使用 @file\_data() 即可；
    
-   创建一个函数，在函数中读取其他格式的文件并获取数据，将数据直接返回为 @ddt.data() 支持的格式调用即可。
    

### DDT 的原理解析

了解了 ddt 的使用，不知你有没有想过如下问题：

-   ddt 是如何把你的测试数据转给你的测试用例的？
    
-   当你的一组数据有多个参数时，ddt 是如何 unpack 的？
    
-   当你有多组数据时，ddt 拆分测试用例是如何命名的？
    

下面我们就来一一揭晓 ddt 实现数据驱动的秘密。

其实 ddt 的实现核心就是\*\*@ddt(cls)**这个装饰器，而这个装饰器的**核心代码是 wrapper\*\*这个内函数，下面我直接把 wrapper 的源码贴上来，我们一起看看：

```
def wrapper(cls):
for name, func in list(cls.__dict__.items()):
if hasattr(func, DATA_ATTR):
for i, v in enumerate(getattr(func, DATA_ATTR)):
                test_name = mk_test_name(
                    name,
                    getattr(v, "__name__", v),
                    i,
                    fmt_test_name
                )
                test_data_docstring = _get_test_data_docstring(func, v)
if hasattr(func, UNPACK_ATTR):
if isinstance(v, tuple) or isinstance(v, list):
                        add_test(
                            cls,
                            test_name,
                            test_data_docstring,
                            func,
                            *v
                        )
else:
                        add_test(
                            cls,
                            test_name,
                            test_data_docstring,
                            func,
                            **v
                        )
else:
                    add_test(cls, test_name, test_data_docstring, func, v)
            delattr(cls, name)
elif hasattr(func, FILE_ATTR):
            file_attr = getattr(func, FILE_ATTR)
            process_file_data(cls, name, func, file_attr)
            delattr(cls, name)
return cls
```

来分析下这段代码， 对于每一个被 **@ddt 装饰**的测试类，ddt 首先去**遍历**测试类的自有属性，从而得出这个测试类**有哪些测试方法**，这部分主要靠这条语句：

```
for name, func in list(cls.__dict__.items()):
```

然后，ddt 去判断所有的 func（即类函数）里，有没有装饰器 @data 或者 @file\_data，主要靠这两条语句：

```
if hasattr(func, DATA_ATTR):
elif hasattr(func, FILE_ATTR):
```

接着程序会进入两条分支：被 @data 装饰，即由 ddt 直接提供数据；被 @file\_data 装饰，即数据由外部文件提供。

#### 1.被 @data 装饰，即由 ddt 直接提供数据

如果数据是直接通过 @data 提供的，那么为每一组数据新生成一个测试用例名称。

```
for i, v in enumerate(getattr(func, DATA_ATTR)):
    test_name = mk_test_name(
        name,
        getattr(v, "__name__", v),
        i,
        fmt_test_name
    )
```

test\_name 生成使用的是函数 mk\_test\_name。

**注意：ddt 在此时实现了把你的测试数据转给你的测试用例。 其实不是通过传递，而是通过把测试数据拆分，并且生成新测试用例的方式来达成的。**

而在函数 mk\_test\_name 里，ddt 更是把原来的测试函数通过特定的规则，拆分成不同的测试函数。

```
test_name = mk_test_name(name,getattr(v, "__name__", v),i,fmt_test_name)
```

mk\_test\_name 的参数里：

-   name 是原测试函数的名字
    
-   v 是我们的一组测试数据
    
-   i 是这组数据的 index
    
-   fmt\_test\_name 指定新的 test 函数的名字的格式，这个格式是按照原来测试函数名 _index_ 第一个测试数据\_第二个测试数据这样的格式。
    

例如，我们的测试数据 **\['iTesting'，'iTesting'\]** 会被转换成**test\_baidu\_search\_1\_\['iTesting'， 'iTesting'\]'**，但是由于符号 **'\['** 和 **''** 以及 **'，'** 是不合法的字符，故会被 **'\_'** 替换，故最终新生成的测试用例名是**test\_baidu\_search\_1\_\_\_iTesting\_\_\_\_iTesting\_\_** 这块的逻辑在函数 mk\_test\_name 的最后两行：

```
test_name = "{0}_{1}_{2}".format(name, index, value)
return re.sub(r'\W|^(?=\d)', '_', test_name)
```

紧接着，ddt 又去查找你的测试类函数，看它有没有被 @unpack 装饰。如果有，就意味着我们的测试类函数有多个参数，这个时候就需要把我们的测试数据 unpack，这样我们的测试类函数的各个参数才能接收到传入的值。

这样，ddt 把上一步生成的 test\_name 和刚刚 unpack 的值（数据是 list、tuple，还是 dictionary，决定了 unpack 采用 \*v 还是 \*\*v），通过 add\_test 来新生成一个测试用例，注册到我们的测试类下面，所有这些动作是在下面这段代码里完成的。

```
if hasattr(func, UNPACK_ATTR):
if isinstance(v, tuple) or isinstance(v, list):
        add_test(
            cls,
            test_name,
            test_data_docstring,
            func,
            *v
        )
else:
        add_test(
            cls,
            test_name,
            test_data_docstring,
            func,
            **v
        )
else:
    add_test(cls, test_name, test_data_docstring, func, v)
```

**注意：**

-   这个时候测试类中是多了测试函数的，多了多少个，要取决于 ddt 提供的测试数据的组数，**有几组就生成几个测试用例**，并且都注册到原测试类中去；
    
-   unpack 其实就是为了把一个测试用例的**多个测试数据全部传入新生成的测试函数中**去，这些测试数据和测试函数的参数一一对应。
    

最后，ddt 会把最初的那个原始测试类方法给删除（因为原测试函数已经根据各组数据变成了新的测试函数）。

```
# wrapper源码45行
delattr(cls, name)
```

通过这样的方式，ddt 根据测试数据的组数，通过函数 mk\_test\_name 生成多组测试用例，并通过 add\_test 函数注册到 unittest的TestSuite 里去。

#### 2.被 @file\_data 装饰，即数据由外部文件提供

如果测试函数被 @file\_data 装饰，ddt 则会先获取 file\_data 里的数据文件名称，然后通过函数 process\_file\_data 里进行下一步处理。

```
file_attr = getattr(func, FILE_ATTR)
process_file_data(cls, name, func, file_attr)
```

看起来只有短短的两行，其实 ddt 在函数 process\_file\_data 内部做了很多操作。  
首先 ddt 会先拿到我们提供的数据文件的绝对地址，并通过后缀名判断它是 yaml 文件还是 json 文件，然后分别调用 yaml 或者 json 的 load 方法拿到文件里提供的数据。

拿到数据后，最终也是通过 mk\_test\_name 函数和 add\_test 函数，生成多条测试用例，并且注册到 unittest 的 TestSuite 里去。

最后一样是删除原来的测试函数：

```
# wrapper源码54行
delattr(cls, name)
```

这就是 ddt 的整个实现逻辑了。

### 总结

我来总结下今天所讲的内容。

今天我们了解了 unittest 里数据驱动 DDT 的安装、使用，以及实现原理。通过对其源代码的解析，我们掌握**DDT 是如何实现按照数据组数生成测试用例、更新测试方法名，以及根据数据类型 unpack 测试数据的。**

DDT 的源代码非常经典，代码行数又不多，值得我们深读。仔细琢磨并研究透 DDT 的源码，有助于你的测试开发技术突飞猛进。

**我希望你能用单步调试的方式，结合本节课所讲，边执行测试代码边走读 DDT 代码，这样有助于你加深理解。**

**在此留一个课后作业给你：**

在本课时“ddt 使用文件提供数据——其他格式数据文件”这一小节中，我提及了使用其他数据格式进行数据驱动的方法，但是没有给出代码示例。

希望你结合本节所讲内容，以 Excel 格式的数据为例， 将 Excel 中的数据作为数据源提供给 DDT 使用

> Tips：读写 Excel 可以使用相关的 Library，例如“读”可以选择 xlrd、“写”可以选择 xlwt。

好，本节课就到这里，下一节课，我将带你探寻 pytest 里的数据驱动以及数据模块 parameterized。

关于更多测试框架的知识，请关注我公众号iTesting，回复“测试框架”查看。

___

[课程评价入口，挑选 5 名小伙伴赠送小礼品～](https://wj.qq.com/s2/7506053/9b01)
