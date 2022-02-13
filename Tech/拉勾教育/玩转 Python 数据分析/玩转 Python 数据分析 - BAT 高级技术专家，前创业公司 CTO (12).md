---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


今天我们来学习在数据分析领域最为常见一种文件格式：CSV 文件，然后我们再将上一讲抓取到的数据保存到 CSV 文件中。

### 什么是 CSV 文件

CSV（Comma-Separated Values） 是一种使用逗号分隔来实现存储表格数据的文本文件。

我们都知道表格有多种形式的存储，比如 Excel 的格式或者数据库的格式。CSV 文件也可以存储表格数据，并且能够被多种软件兼容，比如 Excel 就能直接打开 CSV 文件的表格，很多数据库软件也支持导入 CSV 文件。除了兼容性好之外，CSV 格式还是所有能存储表格的格式中最简单的一种。

下面，我们以一个例子来讲解 CSV 存储表格的原理。

假设有如下员工信息的表格。

![图片1.png](https://s0.lgstatic.com/i/image6/M01/3B/A8/Cgp9HWCGW3uAdw0gAAEO7lp53XM723.png)

要存储类似上面的表格，以往我们只能将其输入到 Excel 中并保存为 xlsx 格式，现在我们来尝试将其以 CSV 的格式保存。

在工作目录下，新建 chapter09 文件夹，作为我们第九讲的实验目录。然后打开文本编辑器，如 Windows 下的记事本，新建空白文件。

输入以下内容，并保存为 info.csv （编码选择 ANSI），文件夹的位置选择刚才创建的 chapter09/ 中。

```
姓名,年龄,籍贯,部门
小明,22,河北,IT部
小亮,25,广东,IT部
小E,23,四川,财务部
```

保存成功后，我们用 Excel 打开这个文件，可以看到我们存储的 CSV 文件成功地在 Excel 中展示为表格。

![图片2.png](https://s0.lgstatic.com/i/image6/M01/3B/A8/Cgp9HWCGW4iAfTk4AASFdtZ50e0408.png)

相信看到这里，你也发现 CSV 格式的奥秘了，总结一下：

-   表格中的一行，对应 CSV 文件中的一行；
    
-   一行中不同单元格的内容，在 CSV 文件中用**逗号分隔；**
    
-   务必保证每行的逗号数量是一致的（对应表格中每行的单元格一致）。
    

在我们后续的数据分析任务中，CSV 文件将会是我们存储数据的主要格式。

### Python 的 CSV 模块

学会了 CSV 文件的基本概念，今天我们来学习如何使用 Python 来操作 CSV 文件。因为对于数据分析场景而言，最常见的操作就是读取和写入。

#### 准备知识

在学习使用 Python 操作 CSV 文件之前，我们首先先来充充电，学习一个 Python 非常常见的循环技术：遍历循环。

要逐个打印一个列表的元素，按照以前我们学习的循环知识，方法如下：让循环变量 i 从 0 逐步叠加到列表的长度，然后在循环体中每次取列表的第 i 个数并打印。

这种逐个处理列表中元素的行为，我们称之为遍历。比如我们初始化一个数组，然后让变量 i 从 0 循环到列表的长度。

```
arr = [12,5,33,4,1]
for i in range(0,len(arr)):
    item = arr[i]
    print(item)
```

输出结果为数组的所有数值：

事实上，Python 还存在一个更加简便的方法来实现列表的遍历，即不用循环变量，而是直接对列表使用 for..in.. 语句，用法如下：

```
arr = [12,5,33,4,1]
for item in arr:
    print(item)
```

输出为：

可以看到，两种循环输出的结果是一样的，但是直接的遍历循环的代码量更少、更简单。遍历循环可以适用于任何类型的列表，以字符串列表为例：

```
string_arr = ["hi","hello", "你好", "aloha"]
for item in string_arr:
    print("本次循环 item 变量的值：", item)
```

上述列表中有四个字符串，使用遍历循环的方式遍历该列表，循环则会执行四次：第一次执行，item 变量存储的值是 "hi"，第二次是 "hello" 以此类推。  
上述代码输出为：

```
本次循环 item 变量的值： hi
本次循环 item 变量的值： hello
本次循环 item 变量的值： 你好
本次循环 item 变量的值： aloha
```

遍历循环相比较我们之前学习的循环，适用场景更广，比如后续会遇到一些“特殊列表”只能用此方案去访问每个元素。

#### 从 CSV 文件读取内容

现在我们来尝试读取刚才保存的 info.csv 。

打开 VS Code，新建 Notebook，并保存为 chapter09.ipynb。在第一个 cell 中，我们首先导入 csv 模块，输入如下代码：

要读取 CSV 文件，我们需要用到 CSV 模块中的 DictReader 类，DictReader 可以将每一行以字典的形式读出来，key 就是表头，value 就是对应单元格的内容。

具体用法如下所示，新建 Cell ，输入如下代码。

```
fo = open("info.csv ")
reader = CSV.DictReader(fo)
headers = reader.fieldnames
fo.close()
print(headers)
```

输出如下：

上面的例子中，我们学习了 DictReader 对象的创建方法以及通过 fileldnames 属性获取了 CSV 表格的表头。

接下来，我们尝试获取表格的实际内容。

新建 cell，输入如下代码：

```
fo = open("info.csv ")
reader = CSV.DictReader(fo)
row_list = []
for row in reader:
    row_list.append(row)
fo.close()
print(row_list[0])
```

打印的结果显示：

```
{'姓名': '小明', '年龄': '22', '籍贯': '河北', '部门': 'IT部'}
```

可以看到，我们拿到了第一行的内容，并且是以字典的形式，字典把每个单元格的内容和表头联系了起来，表头是 key，而具体内容就是 value。每行都是这样的一个字典，所有字典都存储在 row\_list 列表中。

接下来，我们来演示对于 row\_list 列表的常见操作：打印某一行、某一列的值。

```
print("打印年龄一列的内容：")
for d in row_list:
    print(d["年龄"])
print("")
print("打印第三行的内容：")
d = row_list[2]
print("姓名:", d["姓名"])
print("年龄：",d["年龄"])
print("籍贯：",d["籍贯"])
print("部门：",d["部门"])
```

输出如下：

```
打印年龄一列的内容
22
25
23
打印第三行的内容
姓名: 小E
年龄： 23
籍贯： 四川
部门： 财务部
```

#### 写入 CSV 文件

在之前的例子中，我们写入 CSV 文件是靠我们人肉写入的，现在我们来试着通过 Python 写入 CSV 文件。

与读取类似，Python 的 CSV 模块提供了 DictWriter 方法，使得我们可以将表格数据以字典的形式存在到 CSV 文件中。

具体用法如下：

```
fo = open("info2.CSV", "w", newline='')
header = ["姓名", "年龄", "籍贯", "部门"]
writer = CSV.DictWriter(fo, header)
writer.writeheader()
writer.writerow({"姓名": "小刚", "年龄":"28", "籍贯":"福建", "部门":"行政部"})
fo.close()
```

上述代码的关键点就在于，创建了 DictWriter 后，需要首先调用 writeheader 来写入表头，然后再调用 writerow 来写入行。

执行上述代码之后，并不会有内容输出，但是 chapter09 文件夹下会多出一个 Info2.csv， 用Excel 打开后，如下图所示。

![图片3.png](https://s0.lgstatic.com/i/image6/M01/3B/B1/CioPOWCGW5-AAHi1AAUBviplMwg738.png)

可以看到，我们的表头和记录已经成功写入 CSV 文件中。

DictWriter 除了提供 writerow 方法来将单个字典保存为 CSV 表格中的一行，还提供了 writerows 方法来一次性地保存多行的内容。

你还记得我们之前将我们手工建的 CSV 表格的内容存储在 row\_list 变量中吗？现在我们尝试使用 writerow 方法来一次性写入多条记录。

新建 Cell，输入以下代码：

```
fo = open("info3.CSV", "w", newline='')
header = ["姓名", "年龄", "籍贯", "部门"]
writer = CSV.DictWriter(fo, header)
row_list.append({"姓名": "小刚", "年龄":"28", "籍贯":"福建", "部门":"行政部"})
writer.writeheader()
writer.writerows(row_list)
fo.close()
```

执行完毕后，chapter09/ 下生成了新的 info3.csv，打开后如下图所示，包含了一开始的三条记录，以及我们插入的“小刚”的记录。

![图片4.png](https://s0.lgstatic.com/i/image6/M01/3B/B1/CioPOWCGW-WAP4BrAAU43clX1Mg547.png)

### 实现煎蛋新闻列表保存到 CSV 文件中

接下来，我们来将上一讲中过滤出来的新闻列表写入 CSV 文件中。在上一讲中，我们在课程内容中获取了煎蛋的新闻标题，在课后作业中获取了新闻发布的时间。

我们今天的内容就是将每篇新闻的这两个内容保存到 CSV 中，相当于一个新闻就是一行，每一行有两列，一个是新闻标题，一列是发布时间。对应的表头就是：标题、发布时间。

#### （1）数据准备

第一步，我们首先将 chapter08 文件夹中的 jiandan.html 拷贝到 chapter09 文件夹中。

第二步，将第 08 讲中的抽取标题的代码整理成几个函数，方便后续调用。这里我们再简单回顾一下上一讲中抓取新闻的步骤：

1.  打开文件网页，读出内容，并创建对应的 BeautifulSoup 对象；
    
2.  找到所有包含新闻的 div 元素列表（class=indexs 的 div）；
    
3.  从 2 中的 div 元素中抽取出标题；
    
4.  从 2 中的 div 元素中抽取出时间。
    

我们把上述四个操作整理为四个函数。

**1**. 首先我们来实现创建 BeautifulSoup 对象的函数。

```
from bs4 import BeautifulSoup
def create_doc_from_filename(filename):
    fo = open(filename, "r", encoding='utf-8')
    html_content = fo.read()
    fo.close()
    doc = BeautifulSoup(html_content)
return doc
```

函数的具体实现在 chapter08 都讲过，这里不再赘述。按 shift + enter 执行，这样后续我们就能够使用该函数。

**2**. 实现定位包含新闻的 div 元素的列表函数。

```
def find_index_labels(doc):
    index_labels = doc.find_all("div", class_="indexs")
return index_labels
```

继续按 shift + enter 执行。

**3**. 实现新闻标题的抽取函数。

这里我们直接复制上一讲中的 get\_title 函数即可。

```
def get_title(label_object):
    a_labels = label_object.find_all("a",target="_blank")
    my_label = a_labels[0]
return my_label.get_text()
```

**4**. 实现获取新闻发布时间的函数。

```
def get_pub_time(label_object):
    spans = label_object.find_all("span", class_="comment-link")
    span = spans[0]
return span["title"]
```

至此，我们四个基础函数已经准备好了，以上的 Cell 都需要注意按 shift + enter 执行，这样我们接下来才可以使用这些函数。

#### （2）获取新闻标题与列表

接下来，我们开始使用上面的函数来获得新闻的标题与新闻列表。

新建 Cell，输入如下代码：

```
doc = create_doc_from_filename("jiandan.html")
index_labels = find_index_labels(doc)
for label_object in index_labels:
    title = get_title(label_object)
    pub_time = get_pub_time(label_object)
    print("标题：", title)
    print("发布时间：", pub_time)
```

上述代码把我们刚才准备的四个函数都串了起来。大概的思路就是首先创建 BeautifulSoup 对象，之后针对该对象查询 class = indexs 的列表，然后使用遍历循环遍历该列表，对于每一个 div 元素，分别调用 get\_title 以及 get\_pub\_time 函数来获得标题与发布时间。

执行上述代码后，输出如下所示。可以看到，我们的新闻标题和时间都已经被成功打印了出来。

```
标题： 引发普通感冒的鼻病毒会将新冠病毒排挤出细胞！
发布时间： 1小时 ago
标题： 无厘头研究：植入虚假的记忆再抹去它们
发布时间： 4小时 ago
标题： 什么是仇恨犯罪？
发布时间： 8小时 ago
标题： 突发：LHCb发现了违背标准模型的现象
发布时间： 12小时 ago
标题： 今日带货 20210324
发布时间： 14小时 ago
标题： 舌战裸猿：IBM搞出了可以打辩论赛的AI
发布时间： 23小时 ago
标题： 大吐槽：「我没醉，醉的是世界」
发布时间： 1天 ago
标题： 今年世界总发电量的0.6%被用于挖比特币
发布时间： 1天 ago
标题： 接种疫苗后还是感染新冠？不要为此惊讶
发布时间： 1天 ago
标题： 今日带货：蛋友家的血橙
发布时间： 2天 ago
标题： 科学家首次在野外检测到抗多药的超级真菌
发布时间： 2天 ago
标题： 未在iPhone12盒中搭配充电器，苹果被巴西消协罚200万美元
发布时间： 2天 ago
标题： 工程师将解决城市陷坑的问题
发布时间： 2天 ago
标题： 今日带货：粉面专场
发布时间： 3天 ago
标题： 科学家在碟子里培育出了泪腺，并让它哭泣
发布时间： 3天 ago
标题： 疯狂实验进行时：把志愿者禁闭在黑暗的空间里40天
发布时间： 3天 ago
标题： 今日带货 20210321
发布时间： 4天 ago
标题： 我们已向外星人发送了哪些消息？
发布时间： 4天 ago
标题： 脑力小体操：石头+剪刀 VS 石头+布
发布时间： 4天 ago
标题： 发霉啦：今天，我终于向母亲摊牌了
发布时间： 5天 ago
标题： 普渡大学的经济学家计算出世界各地幸福的价格
发布时间： 5天 ago
标题： 人类首次观察到木星上极光黎明风暴的成形过程
发布时间： 5天 ago
标题： 为女儿出头，母亲编辑假裸照败坏高中啦啦队队员的名誉
发布时间： 5天 ago
标题： 今日带货：淘宝京东蛋友推荐
发布时间： 6天 ago
```

#### （3）将数据存储为字典的形式

要存储到 CSV，首先我们需要将我们的数据创建为字典的形式，我们可以在（2）的循环中将标题和时间存储为字典，然后使用一个字典列表来存储每个新闻对应的字典。最后直接使用 DictWriter 的 writerows 方法来将字典列表写入 CSV 文件即可。

我们直接修改刚才打印标题和发布时间的 Cell，删除原本的打印代码，并添加字典相关操作的代码。

添加完后的 Cell ，如下所示。

```
doc = create_doc_from_filename("jiandan.html")
index_labels = find_index_labels(doc)
news_dict_list = []
for label_object in index_labels:
    title = get_title(label_object)
    pub_time = get_pub_time(label_object)
    news = {"标题": title, "发布时间": pub_time}
    news_dict_list.append(news)
print(news_dict_list)
```

通过循环，我们将新闻以字典的形式逐个添加到了字典列表中，然后在最后打印出列表，输出如下所示。

```
[{'标题': '引发普通感冒的鼻病毒会将新冠病毒排挤出细胞！', '发布时间': '1小时 ago'}, {'标题': '无厘头研究：植入虚假的记忆再抹去它们', '发布时间': '4小时 ago'}, {'标题': '什么是仇恨犯罪？', '发布时间': '8小时 ago'}, {'标题': '突发：LHCb发现了违背标准模型的现象', '发布时间': '12小时 ago'}, {'标题': '今日带货 20210324', '发布时间': '14小时 ago'}, {'标题': '舌战裸猿：IBM搞出了可以打辩论赛的AI', '发布时间': '23小时 ago'}, {'标题': '大吐槽：「我没醉，醉的是世界」', '发布时间': '1天 ago'}, {'标题': '今年世界总发电量的0.6%被用于挖比特币', '发布时间': '1天 ago'}, {'标题': '接种疫苗后还是感染新冠？不要为此惊讶', '发布时间': '1天 ago'}, {'标题': '今日带货：蛋友家的血橙', '发布时间': '2天 ago'}, {'标题': '科学家首次在野外检测到抗多药的超级真菌', '发布时间': '2天 ago'}, {'标题': '未在iPhone12盒中搭配充电器，苹果被巴西消协罚200万美元', '发布时间': '2天 ago'}, {'标题': '工程师将解决城市陷坑的问题', '发布时间': '2天 ago'}, {'标题': '今日带货：粉面专场', '发布时间': '3天 ago'}, {'标题': '科学家在碟子里培育出了泪腺，并让它哭泣', '发布时间': '3天 ago'}, {'标题': '疯狂实验进行时：把志愿者禁闭在黑暗的空间里40天', '发布时间': '3天 ago'}, {'标题': '今日带货 20210321', '发布时间': '4天 ago'}, {'标题': '我们已向外星人发送了哪些消息？', '发布时间': '4天 ago'}, {'标题': '脑力小体操：石头+剪刀 VS 石头+布', '发布时间': '4天 ago'}, {'标题': '发霉啦：今天，我终于向母亲摊牌了', '发布时间': '5天 ago'}, {'标题': '普渡大学的经济学家计算出世界各地幸福的价格', '发布时间': '5天 ago'}, {'标题': '人类首次观察到木星上极光黎明风暴的成形过程', '发布时间': '5天 ago'}, {'标题': '为女儿出头，母亲编辑假裸照败坏高中啦啦队队员的名誉', '发布时间': '5天 ago'}, {'标题': '今日带货：淘宝京东蛋友推荐', '发布时间': '6天 ago'}]
```

#### （4）存储到 CSV 文件中

现在，我们已经将网页中抓取到的数据都保存在一个字典列表中：news\_dict\_list ，接下来就是将这个列表写入到 CSV 文件中即可。

代码如下所示：

```
fo = open("news.CSV", "w", newline='', encoding='utf_8_sig')
header = ["标题", "发布时间"]
writer = CSV.DictWriter(fo, header)
writer.writeheader()
writer.writerows(news_dict_list)
fo.close()
```

执行之后，在 chapter09 文件夹下会生成 news.CSV 文件，用 Excel 打开后如下图所示。可以看到，我们的数据已经成功以表格的形式呈现了。

![图片5.png](https://s0.lgstatic.com/i/image6/M00/3B/A9/Cgp9HWCGXFOAGwVfAAdzQElKjnA321.png)

### 小结

至此，本讲的内容我们就学习完毕了，简单的复习一下本讲所学习的内容：

-   使用 for..in.. 语句可以直接对列表进行**遍历循环**，每次循环时，循环变量存储的都是当前遍历到的列表元素；
    
-   使用 DictReader 可以读取 CSV 文件，主要方式是构建 reader 对象后，直接对 reader 对象用 for..in.. 循环来遍历；
    
-   使用 DictWriter 可以写入 CSV 文件，writerrow 方法用于写入一行，writerows 方法用于写入一个列表（对应多行）。
    

学完了本讲，我们就已经完整了学习了通过 Python 来实现爬虫的网页下载→数据抽取→数据保存三大环节。是不是感觉爬虫技术也没那么难呢？

下一讲，我们将会以一个实战案例的形式，通过爬虫技术来构建一个数据集。

课后练习：

读取刚才保存的 news.csv，添加一列“分级信息”。

分级信息的标准是：前三条的分级信息为“推荐”，后面为“普通”，添加之后保存为新的 news1.csv。

___

答案如下：

关键点：

-   将本讲学习的读取 csv 和写入 csv 的操作整理成两个函数，方便后续调用；
    
-   遍历读取到的字典列表，为每个元素都增加分级信息的键值对；
    
-   通过一个计算变量 i 来判断当前是第几次循环；
    
-   用 if 语句根据变量i 的值来决定当前字典的分级信息是推荐还是普通。
    

```
def write_dict_list_to_CSV(dict_list, filename, headers):
    fo = open(filename, "w", newline='', encoding='utf_8_sig')
    writer = CSV.DictWriter(fo, headers)
    writer.writeheader()
    writer.writerows(dict_list)
    fo.close()
def get_dict_list_from_CSV(filename):
    fo = open(filename, "r")
    reader = CSV.DictReader(fo)
    dict_list = []
for item in reader:
        dict_list.append(item)
return dict_list
news_list = get_dict_list_from_CSV("news.CSV")
header = ["标题","发布时间", "分级信息"]
i = 0
for item in news_list:
if i<=2:
        item["分级信息"] = "推荐"
else:
        item["分级信息"] = "普通"
    i = i + 1
write_dict_list_to_CSV(news_list, "news1.CSV", header)
```

执行后，用 Excel 打开新产生的 news1.csv，如下图所示。可以看到分级信息已经按照题目要求加上了。

![图片6.png](https://s0.lgstatic.com/i/image6/M00/3B/A9/Cgp9HWCGXF-AS6avAAgYOOiMiFQ649.png)
