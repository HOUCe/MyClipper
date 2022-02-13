# [列表推导式实现Python函数式编程的小案例 - 知乎](https://zhuanlan.zhihu.com/p/433259455)

> 本文使用 [Zhihu On VSCode](https://zhuanlan.zhihu.com/p/106057556) 创作并发布

![](https://pic2.zhimg.com/v2-0b02964663868483195bf2d3ab4bcb6d_b.png)

\[\[Python\]\]的\[\[列表推导式\]\]，用来实现\[\[函数式编程\]\]的map和filter，是一种非常不错的实践。

![](https://pic3.zhimg.com/v2-0e3483856834baef4cc817026d79b442_b.png)

我们可以在`output expression`中做map操作，在`if conditions`中做filter操作。

下面我用一个具体的例子来说明一下：平方列表中所有小于0的数

## 最原始的写法

```
x = range(-5, 5)
new_list=[]
for item in x:
    if item < 0:
        new_item=item*item
        new_list.append(new_item)
print(new_list)
```

## map和filter的函数式编程写法

```
x = range(-5, 5)
all_less_than_zero = list(map(lambda num: num * num, list(filter(lambda num: num < 0, x))))
print(all_less_than_zero)
```

## 列表推导式的写法

```
x = range(-5, 5)
all_less_than_zero = [num * num for num in x if num < 0]
print(all_less_than_zero)
```

## 3种写法的对比

我们可以看到，虽然map的写法只有一行了，但是完全被括号和lambda关键字淹没了，反而失去了简介易读的优点。

列表推导式则是非常简洁易懂，也同样只有一行。

## 列表推导式生成字典

和列表类似，区别是最外层使用{},而不是\[\]

```
DIAL_CODES = [
    (86, 'China'),
    (91, 'India'),
    (1, 'United States'),
    (7, 'Russia'),
    (81, 'Japan'),
    ]
country_code = {country: code for code, country in DIAL_CODES}
print(country_code)
```

## 参考资料

1.  [\[翻译\]10分钟快速入门Python函数式编程 - 知乎](https://zhuanlan.zhihu.com/p/42621241)
2.  [Python列表推导教程(List Comprehensions)\_neweastsun的专栏-CSDN博客](https://blog.csdn.net/neweastsun/article/details/98535850 "垃圾中文技术性网站")
3.  [列表推导式（ comprehensions） - Python 进阶 - 极客学院Wiki](https://wiki.jikexueyuan.com/project/interpy-zh/Comprehensions/list-comprehensions.html)

发布于 2021-11-14 23:58，编辑于 2021-11-15 00:00

Drag to outliner or Upload

Close
