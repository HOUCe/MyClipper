# [python如何判断数据类型 - 知乎](https://zhuanlan.zhihu.com/p/99247743)

**![](https://pic1.zhimg.com/v2-56d4c1d25045e65869b062b001ec0d94_b.jpg)**

**python如何判断数据类型？**

在python中可以使用isinstance()函数来判断数据类型，isinstance()函数来判断一个对象是否是一个已知的类型，类似 type()。

**isinstance() 与 type() 区别：**

type() 不会认为子类是一种父类类型，不考虑继承关系。

isinstance() 会认为子类是一种父类类型，考虑继承关系。

如果要判断两个类型是否相同推荐使用 isinstance()。

**语法**

以下是 isinstance() 方法的语法:

![](https://pic4.zhimg.com/v2-9d96985e0ee89f86eac8b97d93e25bc3_b.png)

参数

object -- 实例对象。

classinfo -- 可以是直接或间接类名、基本类型或者由它们组成的元组。

返回值

如果对象的类型与参数二的类型（classinfo）相同则返回 True，否则返回 False。。

实例

以下展示了使用 isinstance 函数的实例：

![](https://pic2.zhimg.com/v2-722e5261b7ea3a6f689cf2a0a2afe9e5_b.jpg)

type() 与 isinstance()区别：

![](https://pic1.zhimg.com/v2-d9f28ef311b74e767612397f4d49ccec_b.jpg)

以上就是python如何判断数据类型的详细内容

**[如果大家如果在学习中遇到困难，想找一个Python学习交流环境，可以加入我们的Python学习圈，点击我加入吧，会节约很多时间，减少很多遇到的难题。](https://jq.qq.com/?_wv=1027&k=5VuOrF2)**

发布于 2019-12-25 10:06

### 文章被以下专栏收录

[

![Python](https://pica.zhimg.com/v2-c43bd683f83cff5fff6943417bc87e9d_xs.jpg?source=172ae18b)



](https://www.zhihu.com/column/c_1159848316581126144)

Drag to outliner or Upload

Close
