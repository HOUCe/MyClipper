---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


从今天开始，我们进入了一个新的部分：使用 pandas 进行数据处理。在上一个模块我们学习了爬虫技术，并学会了怎么将数据从网页中抓取出来保存成 csv 数据集。

在有了数据集之后，接下来我们就开始学习怎么把数据集的内容加载到 Python 中。虽然我们在上一个模块学过简单的读取 csv 的文件内容。

但是存在两个问题：

1.  只能读取 csv 文件，但数据分析的数据除了可能来自 csv，也可能来自 Excel，甚至可以来自 html 的表格。
    
2.  读取到的结果一般是字典列表，并不利于分析，比如虽然我们每个字典就代表一行记录，但一旦我们想拿某一列的数据的时候就会非常复杂。
    

Python 作为数据分析领域的头号种子选手，自然不会只有 csv 模块这样的初级工具。这个部分我们将会学习表格类型的大数据处理神器：pandas.

pandas 不仅可以从多种不同的文件格式读取数据，还有各种各样的数据处理的功能。可以说学好了pandas，就基本已经算踏上了数据分析之路。话不多说，我们这就开始 pandas 之旅。

### 课前准备：安装 pandas 工具包

和前面的课程一致，我们第一步需要安装 pandas 工具包。打开开始菜单 → Anaconda3 → Anaconda Prompt， 并输入 conda install pandas 回车执行， 如下图所示。

![Drawing 0.png](https://s0.lgstatic.com/i/image6/M01/3D/BE/CioPOWCWJUGAAuSkAAENUnrgPlY677.png)

输入 y 确认安装，安装完毕后关闭该窗口。

到课程目录，新建文件夹 chapter11。之后打开 VS code，选择文件 → 打开文件夹，选择打开刚才创建的 chapter11 文件夹。

打开文件夹之后，创建新的 Notebook，并保存到刚才我们创建的 chapter11 文件夹中，并命名为 chapter11.ipynb。

在第一个 Cell 中，导入 pandas。这次为了在后续调用 pandas 的函数方便，我们给它起一个别名：pd。import 语句中，在模块名后面 跟 as 就可以给导入的模块起一个别名，如下所示。

运行之后如果没有报错，说明 pandas 已经成功安装。

### 使用 pandas 读取 csv 文件

首先我们来学习如何使用 pandas 来读取 csv 文件。

pandas 模块提供了一个 read\_ csv 的方法，可以直接读取 csv 文件，并返回一个 DataFrame 对象。DataFrame 对象是 pandas 模块的核心，pandas 的所有表格都是通过 DataFrame 对象来存储的，并且 DataFrame 还提供了非常多查看数据、修改数据的方法。我们在之后的课程中会逐渐学习 DataFrame 的用法。

现在你只需要知道，pandas 可以直接从一个 csv 文件中，将数据读到 Python 中，并且以DataFrame 对象的形式返回，我们拿到这个对象就可以查看其中的数据就可以了。

#### 实战 read\_ csv

我们来具体演练一下这个过程。首先我们将上一讲收集到的国产电视剧数据集 tv\_rating. csv 拷贝到 chapter11 文件夹中。

新建 Cell， 输入如下的代码。

```
df_rating = pd.read_ csv("tv_rating. csv")
print("df_rating:")
print(df_rating)
print("df_rating type:")
print(type(df_rating))
```

运行之后输出如下所示。(为了表格类的数据看着好看一些，所以这类数据我都截图展示)。

![Drawing 1.png](https://s0.lgstatic.com/i/image6/M01/3D/B6/Cgp9HWCWJUyADMoLAAKPt-W-zEM315.png)

从上面的输出中可以看到，df\_rating 变量中包含了我们 csv 文件中的所有数据，并且还有形状的描述：3600 行 x 3列，这个也和我们下载的数据一致。通过打印 df\_rating 的类型，可以看到 df\_rating 的类型就是我们上文提到的 DataFrame。

#### 更好看的表格

DataFrame 很强大，它甚至针对 notebook 有专门优化。我们之前学习过，当一个 Cell 的最后一行是一个变量时，那 notebook 就会将这个变量打印出来。比如如下的代码：

或者

这样的代码，即便没有任何的 print 语句，但因为满足 cell 最后一行是一个变量条件，那 notebook 都会打印出这个变量，也就是 a 的值。

回到 DataFrame， 当我们不是直接用 print 打印它，而是把 DataFrame 变量放在 Cell 的末尾时， notebook 就会用一种更好看的格式来打印它。我们马上来试一下，直接新建一个 Cell，输入如下的代码运行。

输出

![Drawing 2.png](https://s0.lgstatic.com/i/image6/M01/3D/BE/CioPOWCWJViATNS6AAIUgxWwN2o307.png)

可以看到这一次的格式可比上一次好看多了，更像一个表格，对应的也更加整齐。DataFrame 的常见操作我们下一讲会具体展开介绍。

### 使用 pandas 读取 excel 文件

在 Python 还没有兴起之前，大量的数据分析是通过 Excel 完成的。这也造就了在很多传统行业中，还有大量的数据是保存在 Excel 中，所以读取 Excel 也是进行分析也是 Python 数据分析领域的常见任务。

Excel 的 .xls/.xlsx 文件格式是微软针对表格开发的，只能使用 Excel 来打开，比如用记事本打开往往会看到有乱码。那 Python 是怎么读取 Excel 里面的内容呢？那自然是我们本模块的 Super Star：pandas。

类似 csv 的读取，pandas 也提供了 read\_excel 函数来实现读取 excel 文件中的内容，但是使用方法比 read\_ csv 稍微复杂一些。

接下来我们通过一个小实战来学习 read\_excel 使用。

#### （1）准备测试数据

我们之前做的数据都是 csv 格式的。我们现在先来制作一个简单的 excel 文件，用来测试我们后面的代码。

打开 Excel ，将第一个表格（sheet）的名字改为：基本信息，并复制添加下述内容

![1.png](https://s0.lgstatic.com/i/image6/M00/3E/49/Cgp9HWCY25mAPoQKAABMElc0YNA061.png)

添加完后的 Excel 如下所示

![Drawing 3.png](https://s0.lgstatic.com/i/image6/M01/3D/B6/Cgp9HWCWJWWANsA3AADm8AXpScQ480.png)

然后，点击下面的 +号，点击新建一个表格，并命名为“绩效”，然后复制粘贴以下内容。

![2.png](https://s0.lgstatic.com/i/image6/M00/3E/49/Cgp9HWCY26eAALxwAABK5F5N0L8191.png)

添加之后的 Excel 界面如下所示。

![Drawing 4.png](https://s0.lgstatic.com/i/image6/M00/3D/BE/CioPOWCWJb6AEoVdAADv603fm-E192.png)

保存 Excel 到之前我们建的 chapter11 目录中，并命名为 info.xlsx。

#### （2）读取数据

数据准备完毕了，现在我们来读取我们刚才创建的表格。形式类似刚才的 read\_ csv， 你可以先尝试思考一下自己写，然后继续往下看。

新建 Cell ，输入如下的代码。

```
df_info = pd.read_excel("info.xlsx")
df_info
```

执行之后，输出如下。  
![Drawing 5.png](https://s0.lgstatic.com/i/image6/M00/3D/BE/CioPOWCWJcaANQkrAAAxyK7fFMY251.png)

可以看到，Excel 中的数据被成功的打印了出来，和 read\_ csv 一样，read\_excel 返回的也是一个 DataFrame。

不过内容虽然打印出来了，但是只打印出了第一个表格，也就是“基本信息”这个表格，我们后面添加的“绩效”表格并没有打印出来。这是 pandas 的机制导致的，read\_excel 默认读取 excel 文件中的一个表格。

一个 Excel 中包含多个表格还是很常见的，pandas 不可能坐视不理，如何读取更多的表格呢？我们进入下一个步骤。

#### （3）读取不同的表格

read\_excel 比 read\_ csv 复杂的地方就在于，read\_excel 支持非常多的参数。比如要实现读取后面的表格，我们只需要给 read\_excel 函数的 sheet\_name 参数赋值即可。

新建 Cell，输入下面的代码。

```
df_perf = pd.read_excel("info.xlsx", sheet_name="绩效")
df_perf
```

输出如下所示。

![Drawing 6.png](https://s0.lgstatic.com/i/image6/M01/3D/B6/Cgp9HWCWJc6ABaWsAAAz1xFmzUw736.png)

可以看到，这次输出的就是我们增加的“绩效”表格中的内容。

简单来说，**我们可以在 read\_excel 中，通过给 sheet\_name 赋值来决定要加载文件哪个表格，如果不指定，pandas 则默认加载第一个表格**。

#### （4）选择性读取

有时候， excel 文件的数据很多，全部加载到 Python 中可能会卡，而且有时候我们只对其中某几列感兴趣，全部加载显示也不容易看。read\_excel 提供了 usecols 参数，可以指定要加载哪几列。

举个例子，刚才的“绩效”表格，我们对上期考核结果不感兴趣，只想加载姓名和绩效考核这两列的内容。那我们可以这么做，新建 Cell，输入如下代码。

```
df_perf1 = pd.read_excel("info.xlsx", sheet_name="绩效", usecols="A,B")
df_perf1
```

执行之后，输出结果可以看到，我们已经成功实现了只加载前两列的内容。

![Drawing 7.png](https://s0.lgstatic.com/i/image6/M00/3D/BF/CioPOWCWJdmARpP5AAAlohezKM8738.png)

在上面的代码里，我们通过字母 A 和 B 指定了加载第一列和第二列。字母和列的对应关系其实就是 Excel 里面的对应关系。如下图所示。

![Drawing 8.png](https://s0.lgstatic.com/i/image6/M01/3D/B6/Cgp9HWCWJd6AIRR1AAEO8tMU3eU810.png)

至此，我们学习完了基本的读取 excel 文件的操作。

### 使用 pandas 读取 html 文件

有的时候数据并不是整理好的 csv 表格或者 Excel 表格，而是以网页中的表格的形式存在，最常见的就是各类股票财经网站，比如像同花顺的股票涨跌幅数据中心：[http://data.10jqka.com.cn/market/zdfph/](http://data.10jqka.com.cn/market/zdfph/?fileGuid=xxQTRXtVcqtHK6j8)。

![Drawing 9.png](https://s0.lgstatic.com/i/image6/M00/3D/B6/Cgp9HWCWJeiAZkmxAAbhDyca7tw465.png)

或者像招行银行的外汇行情页面，如下所示。

![Drawing 10.png](https://s0.lgstatic.com/i/image6/M00/3D/B6/Cgp9HWCWJe-AXZ3bAAY3rmneDJg833.png)

如果我们希望将这些网页中的表格“导入”到 Python 进行处理，那自然也是可以的。根据我们上一个部分学习的爬虫技术，我们只需要将这个页面的 html 下载下来，然后用 BeautifulSoup 分析表格的标签结构，然后把内容一行一行的提取出来，再一步一步拆分成列。

做是可以做，但只听这个流程，只看这个复杂的页面，都让人觉得工作量很大。而对于提取网页中的表格，其实存在一个非常简单的方法：使用强大的 pandas。

和 read\_ csv、read\_excel 类似，pandas 也提供了一个 read\_html 的方法，来**智能的提取网页中的所有表格**，并以 DataFrame 列表的形式返回，一个表格对应一个 DataFrame。看到这里，是否有感触到 pandas 的强大之处？

我们还是通过一个简单的小例子来学习 read\_html 方法的使用。我们以刚才举的招商银行外汇行情的页面为例，尝试在 Python 中加载该网页中表格的数据。

#### （1）准备网页

首先要做的第一件事，就是要拿到网页的内容。在第 07 讲中，我们已经写过了一个拿网页内容的函数。

新建 Cell，将第 07 讲的 download\_content 函数先搬过来，别忘记在开头加上导入 urllib3的语句，代码如下所示。

```
import urllib3
def download_content(url):
    http = urllib3.PoolManager()
    response = http.request("GET", url)
    response_data = response.data
    html_content = response_data.decode()
return html_content
html_content = download_content("http://fx.cmbchina.com/Hq/")
```

执行上述代码之后，网页内容就已经存储在 html\_content 变量中。

#### （2）读取数据

在准备好了网页的内容之后，我们就可以调用 read\_html 函数来获取表格了。

新建 Cell，输入如下代码。

```
cmb_table_list = pd.read_html(html_content)
print(len(cmb_table_list))
```

执行代码，输出结果为 2。这说明找到了两个表格，但我们回过头去看网页，主体应该只有一个表格。这种情况还是比较常见的，多半是网页在一些**不是表格的元素也使用了表格的标签**，导致 pandas 识别多了一个，遇到这样的情况我们只需要**逐一查看返回的 DataFrame 列表**，找到我们要的即可。

![Drawing 11.png](https://s0.lgstatic.com/i/image6/M00/3D/BF/CioPOWCWJf6AJeEpAAY3rmneDJg801.png)

现在我们来从列表中找出我们要的表格，首先查看第一个 DataFrame。

新建 Cell， 输入以下代码并运行。

执行后，输出如下：

![Drawing 12.png](https://s0.lgstatic.com/i/image6/M00/3D/B6/Cgp9HWCWJgyAQv1mAAAhvorP4NA771.png)

很明显，这不是我们要的表格，现在我们看第二个。

新建 Cell，输入如下的代码。

执行之后，输出如下所示。  
![Drawing 13.png](https://s0.lgstatic.com/i/image6/M00/3D/BF/CioPOWCWJiCAEwgHAAFrcarzvm4465.png)

很明显，这个就是我们要的表格了。可以看到我们在招行官网上看到的汇率表格已经完整的被加载到了 pandas 的 DataFrame 中，并且能够以表格的形式打印出来。是不是感觉到非常神奇？

如果我们用 BeautifulSoup 来解析这个网页然后提取出表格的内容，恐怕代码都要写大几十行，而 pandas 却一个函数搞定了。

### 小结

至此，我们从各种文件读取数据生成 DataFrame 的课程就学习完毕了。下一讲我们将会学习拿到 DataFrame 后，我们如何愉快的操作和查看里面的数据。

回顾一下今天学习的内容，我们可以：

-   通过 read\_ csv 函数将 csv 读取到 pandas 的 DataFrame 对象；
    
-   通过 read\_excel 函数将 excel 文件读取到 DataFrame，并且可以通过 cheet\_name 参数指定要读取哪个表，以及通过 use\_cols 参数来指定要读取哪几列；
    
-   通过 read\_html 函数将 html 内容中的表格提取为一个DataFrame 的列表，通过逐一查看来确定哪个是我们想要的。
    

**课后练习**

尝试加载人民银行的2020金融市场报表数据([http://www.pbc.gov.cn/eportal/fileDir/defaultCurSite/resource/cms/2021/01/2021011818262828764.htm](http://www.pbc.gov.cn/eportal/fileDir/defaultCurSite/resource/cms/2021/01/2021011818262828764.htm?fileGuid=xxQTRXtVcqtHK6j8))

提示：该网页需要使用动态内容下载技术 selenium。

快来动手操作下！操作完再看答案~

___

答案：

```
url= "http://www.pbc.gov.cn/eportal/fileDir/defaultCurSite/resource/cms/2021/01/2021011818262828764.htm"
from selenium import webdriver
brow = webdriver.Chrome()
brow.get(url)
eco_content = brow.page_source
df_list = pd.read_html(eco_content)
print(len(df_list))
```

输出 1，代表只发现一个表格。直接在新的 Cell 里面输出列表的第一个元素即可

输出:  
![Drawing 14.png](https://s0.lgstatic.com/i/image6/M01/3D/B6/Cgp9HWCWJi2ALDmEAALrDxDzE3Y335.png)
