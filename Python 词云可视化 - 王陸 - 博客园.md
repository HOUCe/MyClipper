# [Python 词云可视化 - 王陸 - 博客园](https://www.cnblogs.com/wkfvawl/p/11585986.html)

最近看到不少公众号都有一些词云图，于是想学习一下使用Python生成可视化的词云，上B站搜索教程的时候，发现了一位UP讲的很不错，UP也给出了GitHub上的源码，是一个很不错的教程，这篇博客主要就是搬运UP主的教程吧，做一些笔记，留着以后看。

_**B站视频链接：[https://www.bilibili.com/video/av53917673/?p=1](https://www.bilibili.com/video/av53917673/?p=1)**_

_**Github源码：[https://github.com/TommyZihao/zihaowordcloud](https://github.com/TommyZihao/zihaowordcloud)**_

词云是文本大数据可视化的重要方式，可以将大段文本中的关键语句和词汇高亮展示。

从四行代码开始，一步步教你做出高大上的词云图片，可视化生动直观展示出枯燥文字背后的核心概念。进一步实现修改字体、字号、背景颜色、词云形状、勾勒边框、颜色渐变、分类填色、情感分析等高级玩法。

学完本课之后，你可以将四大名著、古典诗词、时事新闻、法律法规、政府报告、小说诗歌等大段文本做成高大上的可视化词云，还可以将你的微信好友个性签名导出，看看你微信好友的“画风”是怎样的。

[![三国演艺词云](https://camo.githubusercontent.com/7685b271a8b63f423ee43c85fdcf08263551112b/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d653661316334633862666266303437312e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/7685b271a8b63f423ee43c85fdcf08263551112b/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d653661316334633862666266303437312e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

从远古山洞壁画到微信表情包，人类千百年来始终都是懒惰的视觉动物。连篇累牍的大段文本会让人感到枯燥乏味。在这个“颜值即正义”的时代，大数据更需要“颜值”才能展现数据挖掘的魅力。

对于编程小白，学会此技可以玩转文本，入门中文分词、情感分析。对于编程高手，通过本课可以进一步熟悉Python的开源社区、计算生态、面向对象，自定义自己专属风格的词云。

**词云的应用场景**

-   会议记录
-   海报制作
-   PPT制作
-   生日表白
-   数据挖掘
-   情感分析
-   用户画像
-   微信聊天记录分析
-   微博情感分析
-   Bilibili弹幕情感分析
-   年终总结

### 一行命令安装（推荐，适用于99.999%的情况）

打开命令行，输入下面这行命令，回车执行即可。

pip install numpy matplotlib pillow wordcloud imageio jieba snownlp itchat -i https://pypi.tuna.tsinghua.edu.cn/simple

### 如果安装过程中报错（0.001%会发生）

> 如果报错：`Microsoft Visual C++ 14.0 is required.`
> 
> 解决方法：
> 
> 到 [http://www.lfd.uci.edu/~gohlke/pythonlibs/#wordcloud](http://www.lfd.uci.edu/~gohlke/pythonlibs/#wordcloud) 页面下载所需的wordcloud模块的.whl文件，再用pip安装下载的文件。
> 
> 比如，对于64位windows操作系统，python版本为3.6的电脑，就应该下载
> 
> `wordcloud-1.4.1-cp36-cp36m-win_amd64.whl`这个文件
> 
> 下载后打开命令行，使用cd命令切换到该文件的路径，执行`pip install wordcloud-1.4.1-cp36-cp36m-win_amd64.whl`命令，即可安装成功。

### 1号词云：《葛底斯堡演说》黑色背景词云（4行代码上手）

import wordcloud
w \= wordcloud.WordCloud()
w.generate('and that government of the people, by the people, for the people, shall not perish from the earth.')
w.to\_file('output1.png')

运行完成之后，在代码所在的文件夹，就会出现`output.png`图片文件。可以看出，wordcloud自动将`and that by the not from`等废话词组过滤掉，并且把出现次数最多的`people`大号显示。

[![1号词云：葛底斯堡演说黑色背景词云](https://camo.githubusercontent.com/069bb87c311933b46d0230f0b62d59bc8cc9acff/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d343638643266383933616232306462622e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/069bb87c311933b46d0230f0b62d59bc8cc9acff/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d343638643266383933616232306462622e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

### 子豪兄带你逐行读代码

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 1号词云：葛底斯堡演说黑色背景词云 # B站专栏：同济子豪兄 2019-5-23
# 导入词云制作第三方库wordcloud
import wordcloud # 创建词云对象，赋值给w，现在w就表示了一个词云对象
w = wordcloud.WordCloud() # 调用词云对象的generate方法，将文本传入
w.generate('and that government of the people, by the people, for the people, shall not perish from the earth.') # 将生成的词云保存为output1.png图片文件，保存出到当前文件夹中
w.to\_file('output1.png')

![复制代码](https://common.cnblogs.com/images/copycode.gif)

`wordcloud`库为每一个词云生成一个WordCloud对象（注意，此处的W和C是大写）

也就是说，`wordcloud.WordCloud()`代表一个词云对象，我们将它赋值给`w`。

现在，这个`w`就是词云对象啦！我们可以调用这个对象。

我们可以在`WordCloud()`括号里填入各种参数，控制词云的字体、字号、字的颜色、背景颜色等等。

wordcloud库会非常智能地按空格进行分词及词频统计，出现次数多的词就大。

### 2号词云：面朝大海，春暖花开（配置词云参数）

增加宽、高、字体、背景颜色等参数

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 2号词云：面朝大海，春暖花开 # B站专栏：同济子豪兄 2019-5-23

import wordcloud # 构建词云对象w，设置词云图片宽、高、字体、背景颜色等参数
w = wordcloud.WordCloud(width=1000,height=700,background\_color='white',font\_path='msyh.ttc') # 调用词云对象的generate方法，将文本传入
w.generate('从明天起，做一个幸福的人。喂马、劈柴，周游世界。从明天起，关心粮食和蔬菜。我有一所房子，面朝大海，春暖花开') # 将生成的词云保存为output2-poem.png图片文件，保存到当前文件夹中
w.to\_file('output2-poem.png')

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![2号词云：面朝大海，春暖花开](https://camo.githubusercontent.com/8fd70d681b80ec5c853757a56b1d4f8818bde539/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d303938346538396234613664393438622e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/8fd70d681b80ec5c853757a56b1d4f8818bde539/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d303938346538396234613664393438622e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

如果参数过多，第二行写成长长的一行不好看，可以写成多行，让代码更工整

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 2号词云：面朝大海，春暖花开 # B站专栏：同济子豪兄 2019-5-23

import wordcloud # 构建词云对象w，设置词云图片宽、高、字体、背景颜色等参数
w = wordcloud.WordCloud(width=1000,
                        height\=700,
                        background\_color\='white',
                        font\_path\='msyh.ttc')

w.generate('从明天起，做一个幸福的人。喂马、劈柴，周游世界。从明天起，关心粮食和蔬菜。我有一所房子，面朝大海，春暖花开')

w.to\_file('output2-poem.png')

![复制代码](https://common.cnblogs.com/images/copycode.gif)

### 常用参数

-   width 词云图片宽度，默认400像素
    
-   height 词云图片高度 默认200像素
    
-   background\_color 词云图片的背景颜色，默认为黑色
    
    `background_color='white'`
    
-   font\_step 字号增大的步进间隔 默认1号
    
    font\_path 指定字体路径 默认None，对于中文可用`font_path='msyh.ttc'`
    
-   mini\_font\_size 最小字号 默认4号
    
-   max\_font\_size 最大字号 根据高度自动调节
    
-   max\_words 最大词数 默认200
    
-   stop\_words 不显示的单词 `stop_words={"python","java"}`
    
-   Scale 默认值1。值越大，图像密度越大越清晰
    
-   prefer\_horizontal：默认值0.90，浮点数类型。表示在水平如果不合适，就旋转为垂直方向，水平放置的词数占0.9？
    
-   relative\_scaling：默认值0.5，浮点型。设定按词频倒序排列，上一个词相对下一位词的大小倍数。有如下取值：“0”表示大小标准只参考频率排名，“1”如果词频是2倍，大小也是2倍
    
-   mask 指定词云形状图片，默认为矩形
    
    通过以下代码读入外部词云形状图片（需要先`pip install imageio`安装imageio）
    

import imageio
mk \= imageio.imread("picture.png")
w \= wordcloud.WordCloud(mask=mk)

也就是说，我们可以这样来构建词云对象w，其中的参数均为常用参数的默认值，供我们自定义：

![复制代码](https://common.cnblogs.com/images/copycode.gif)

w = wordcloud.WordCloud(      
    width\=400,
    height\=200,
    background\_color\='black',
    font\_path\=None, 
    font\_step\=1,
    min\_font\_size\=4,
    max\_font\_size\=None,
    max\_words\=200,
    stopwords\={},
    scale\=1,
    prefer\_horizontal\=0.9,
    relative\_scaling\=0.5,
    mask\=None) 

![复制代码](https://common.cnblogs.com/images/copycode.gif)

### 3号词云：乡村振兴战略中央文件（句子云）

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 3号词云：乡村振兴战略中央文件 # B站专栏：同济子豪兄 2019-5-23

import wordcloud # 从外部.txt文件中读取大段文本，存入变量txt中
f = open('关于实施乡村振兴战略的意见.txt',encoding='utf-8')
txt \= f.read() # 构建词云对象w，设置词云图片宽、高、字体、背景颜色等参数
w = wordcloud.WordCloud(width=1000,
                        height\=700,
                        background\_color\='white',
                        font\_path\='msyh.ttc') # 将txt变量传入w的generate()方法，给词云输入文字
w.generate(txt) # 将词云图片导出到当前文件夹
w.to\_file('output3-sentence.png')

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![3号词云：乡村振兴战略中央文件](https://camo.githubusercontent.com/97d91604ae489bddf0512e8fda30fa4a256edb5c/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d643163353238633234633662353534662e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/97d91604ae489bddf0512e8fda30fa4a256edb5c/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d643163353238633234633662353534662e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

### 中文分词第三方模块`jieba`

#### 中文分词-小试牛刀

安装中文分词库jieba：在命令行中输入`pip install jieba`

打开python的`交互式shell`界面，也就是有三个大于号`>>>`的这个界面，依次输入以下命令。

![复制代码](https://common.cnblogs.com/images/copycode.gif)

\>>> import jieba \>>> textlist = jieba.lcut('动力学和电磁学') \>>> textlist
\['动力学', '和', '电磁学'\] \>>> string = " ".join(textlist) \>>> string '动力学 和 电磁学'

![复制代码](https://common.cnblogs.com/images/copycode.gif)

以上代码将一句`完整的中文字符串`转换成了`以空格分隔的词组成的字符串`，而后者是绘制词云时`generate()`方法要求传入的参数。

#### 中文分词库`jieba`的常用方法

`精确模式（最常用，只会这个就行）`：每个字只用一遍，不存在冗余词汇。`jieba.lcut('动力学和电磁学')`

`全模式`：把每个字可能形成的词汇都提取出来，存在冗余。`jieba.lcut('动力学和电磁学',cut_all=True)`

`搜索引擎模式`：将全模式分词的结果从短到长排列好。`jieba.lcut_for_search('动力学和电磁学')`

以下命令演示了三种分词模式及结果，精确模式是最常用的。

![复制代码](https://common.cnblogs.com/images/copycode.gif)

\>>> import jieba \>>> textlist1 = jieba.lcut('动力学和电磁学') \>>> textlist1
\['动力学', '和', '电磁学'\] \>>> textlist2 = jieba.lcut('动力学和电磁学',cut\_all=True) \>>> textlist2
\['动力', '动力学', '力学', '和', '电磁', '电磁学', '磁学'\] \>>> textlist3 = jieba.lcut\_for\_search('动力学和电磁学') \>>> textlist3
\['动力', '力学', '动力学', '和', '电磁', '磁学', '电磁学'\]

![复制代码](https://common.cnblogs.com/images/copycode.gif)

一键执行的详细脚本文件详见[github代码库-zihaowordcloud](https://github.com/TommyZihao/zihaowordcloud)中的`test1-jieba.py`文件。

### 4号词云：同济大学介绍词云（中文分词）

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 4号词云：同济大学介绍词云 # B站专栏：同济子豪兄 2019-5-23

# 导入词云制作库wordcloud和中文分词库jieba
import jieba import wordcloud # 构建并配置词云对象w
w = wordcloud.WordCloud(width=1000,
                        height\=700,
                        background\_color\='white',
                        font\_path\='msyh.ttc') # 调用jieba的lcut()方法对原始文本进行中文分词，得到string
txt = '同济大学（Tongji University），简称“同济”，是中华人民共和国教育部直属，由教育部、国家海洋局和上海市共建的全国重点大学，历史悠久、声誉卓著，是国家“双一流”、“211工程”、“985工程”重点建设高校，也是收生标准最严格的中国大学之一' txtlist \= jieba.lcut(txt)
string \= " ".join(txtlist) # 将string变量传入w的generate()方法，给词云输入文字
w.generate(string) # 将词云图片导出到当前文件夹
w.to\_file('output4-tongji.png')

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![4号词云：同济大学介绍词云](https://camo.githubusercontent.com/a31dc4dd854f622c36dfbfb21771d346a302dda7/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d383235333336643332613566333531612e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/a31dc4dd854f622c36dfbfb21771d346a302dda7/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d383235333336643332613566333531612e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

### 5号词云：乡村振兴战略中央文件（词云）

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 5号词云：乡村振兴战略中央文件（词云） # B站专栏：同济子豪兄 2019-5-23

# 导入词云制作库wordcloud和中文分词库jieba
import jieba import wordcloud # 构建并配置词云对象w
w = wordcloud.WordCloud(width=1000,
                        height\=700,
                        background\_color\='white',
                        font\_path\='msyh.ttc') # 对来自外部文件的文本进行中文分词，得到string
f = open('关于实施乡村振兴战略的意见.txt',encoding='utf-8')
txt \= f.read()
txtlist \= jieba.lcut(txt)
string \= " ".join(txtlist) # 将string变量传入w的generate()方法，给词云输入文字
w.generate(string) # 将词云图片导出到当前文件夹
w.to\_file('output5-village.png')

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![5号词云：乡村振兴战略中央文件(词云)](https://camo.githubusercontent.com/e850adf8756088df9a50471c33fafa4ec4f3e2d9/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d333764326266633562303438393433642e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/e850adf8756088df9a50471c33fafa4ec4f3e2d9/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d333764326266633562303438393433642e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

通过以下代码读入外部词云形状图片（需要先`pip install imageio`安装imageio）

import imageio
mk \= imageio.imread("picture.png")
w \= wordcloud.WordCloud(mask=mk)

### 6号词云：乡村振兴战略中央文件（五角星形状）

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 6号词云：乡村振兴战略中央文件（五角星形状） # B站专栏：同济子豪兄 2019-5-23

# 导入词云制作库wordcloud和中文分词库jieba
import jieba import wordcloud # 导入imageio库中的imread函数，并用这个函数读取本地图片，作为词云形状图片
import imageio
mk \= imageio.imread("wujiaoxing.png")
w \= wordcloud.WordCloud(mask=mk) # 构建并配置词云对象w，注意要加scale参数，提高清晰度
w = wordcloud.WordCloud(width=1000,
                        height\=700,
                        background\_color\='white',
                        font\_path\='msyh.ttc',
                        mask\=mk,
                        scale\=15) # 对来自外部文件的文本进行中文分词，得到string
f = open('关于实施乡村振兴战略的意见.txt',encoding='utf-8')
txt \= f.read()
txtlist \= jieba.lcut(txt)
string \= " ".join(txtlist) # 将string变量传入w的generate()方法，给词云输入文字
w.generate(string) # 将词云图片导出到当前文件夹
w.to\_file('output6-village.png')

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![6号词云：乡村振兴战略中央文件(五角星)](https://camo.githubusercontent.com/68bf3b4c033c11afb83a617703b184072d69308d/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d356530343238613939613938383435342e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/68bf3b4c033c11afb83a617703b184072d69308d/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d356530343238613939613938383435342e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

### 7号词云：新时代中国特色社会主义（中国地图形状）

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 7号词云：新时代中国特色社会主义（中国地图形状） # B站专栏：同济子豪兄 2019-5-23

# 导入词云制作库wordcloud和中文分词库jieba
import jieba import wordcloud # 导入imageio库中的imread函数，并用这个函数读取本地图片，作为词云形状图片
import imageio
mk \= imageio.imread("chinamap.png")
w \= wordcloud.WordCloud(mask=mk) # 构建并配置词云对象w，注意要加scale参数，提高清晰度
w = wordcloud.WordCloud(width=1000,
                        height\=700,
                        background\_color\='white',
                        font\_path\='msyh.ttc',
                        mask\=mk,
                        scale\=15) # 对来自外部文件的文本进行中文分词，得到string
f = open('新时代中国特色社会主义.txt',encoding='utf-8')
txt \= f.read()
txtlist \= jieba.lcut(txt)
string \= " ".join(txtlist) # 将string变量传入w的generate()方法，给词云输入文字
w.generate(string) # 将词云图片导出到当前文件夹
w.to\_file('output7-chinamap.png')

![复制代码](https://common.cnblogs.com/images/copycode.gif)

加scale参数为15的效果

[![7号词云：新时代中国特色社会主义(中国地图形状)](https://camo.githubusercontent.com/6a6b7f83555da7bb2450f8a084ddd22d11a14dac/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d363663373734633330313362643764632e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/6a6b7f83555da7bb2450f8a084ddd22d11a14dac/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d363663373734633330313362643764632e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

不加scale参数的效果，稍显模糊

[![中国地图词云](https://camo.githubusercontent.com/86863617950eca7a6e18da89603de6d393eb639a/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d303032653662633030383563306264302e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/86863617950eca7a6e18da89603de6d393eb639a/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d303032653662633030383563306264302e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

### 8号词云：《三国演义》词云（stopwords参数去除词）

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 8号词云：《三国演义》词云（stopwords参数去除“曹操”和“孔明”两个词） # B站专栏：同济子豪兄 2019-5-23

# 导入词云制作库wordcloud和中文分词库jieba
import jieba import wordcloud # 导入imageio库中的imread函数，并用这个函数读取本地图片，作为词云形状图片
import imageio
mk \= imageio.imread("chinamap.png") # 构建并配置词云对象w，注意要加stopwords集合参数，将不想展示在词云中的词放在stopwords集合里，这里去掉“曹操”和“孔明”两个词
w = wordcloud.WordCloud(width=1000,
                        height\=700,
                        background\_color\='white',
                        font\_path\='msyh.ttc',
                        mask\=mk,
                        scale\=15,
                        stopwords\={'曹操','孔明'}) # 对来自外部文件的文本进行中文分词，得到string
f = open('threekingdoms.txt',encoding='utf-8')
txt \= f.read()
txtlist \= jieba.lcut(txt)
string \= " ".join(txtlist) # 将string变量传入w的generate()方法，给词云输入文字
w.generate(string) # 将词云图片导出到当前文件夹
w.to\_file('output8-threekingdoms.png')

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![三国演艺词云](https://camo.githubusercontent.com/d672fb167bfd1863e7fdeb09157c9d3c45635f7f/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d393634346234393661663264383734622e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/d672fb167bfd1863e7fdeb09157c9d3c45635f7f/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d393634346234393661663264383734622e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

### 9号词云：《哈姆雷特》（勾勒轮廓线）

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 9号词云：哈姆雷特（勾勒轮廓线） # B站专栏：同济子豪兄 2019-5-23

# 导入词云制作库wordcloud
import wordcloud # 将外部文件包含的文本保存在string变量中
string = open('hamlet.txt').read() # 导入imageio库中的imread函数，并用这个函数读取本地图片，作为词云形状图片
import imageio
mk \= imageio.imread("alice.png") # 构建词云对象w，注意增加参数contour\_width和contour\_color设置轮廓宽度和颜色
w = wordcloud.WordCloud(background\_color="white",
                        mask\=mk,
                        contour\_width\=1,
                        contour\_color\='steelblue') # # 将string变量传入w的generate()方法，给词云输入文字
w.generate(string) # 将词云图片导出到当前文件夹
w.to\_file('output9-contour.png')

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![8号词云：哈姆雷特(勾勒轮廓线)](https://camo.githubusercontent.com/a56ab17604695f013b7ac41b75203b30a87c11a7/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d343337323031356135663538383831322e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/a56ab17604695f013b7ac41b75203b30a87c11a7/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d343337323031356135663538383831322e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

### 10号词云：《爱丽丝漫游仙境》词云（按模板填色）

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 10号词云：《爱丽丝漫游仙境》词云（按模板填色） # B站专栏：同济子豪兄 2019-5-23

# 导入绘图库matplotlib和词云制作库wordcloud
import matplotlib.pyplot as plt from wordcloud import WordCloud,ImageColorGenerator # 将外部文件包含的文本保存在text变量中
text = open('alice.txt').read() # 导入imageio库中的imread函数，并用这个函数读取本地图片queen2.jfif，作为词云形状图片
import imageio
mk \= imageio.imread("alice\_color.png") # 构建词云对象w
wc = WordCloud(background\_color="white",
               mask\=mk,) # 将text字符串变量传入w的generate()方法，给词云输入文字
wc.generate(text) # 调用wordcloud库中的ImageColorGenerator()函数，提取模板图片各部分的颜色
image\_colors = ImageColorGenerator(mk) # 显示原生词云图、按模板图片颜色的词云图和模板图片，按左、中、右显示
fig, axes = plt.subplots(1, 3) # 最左边的图片显示原生词云图
axes\[0\].imshow(wc) # 中间的图片显示按模板图片颜色生成的词云图，采用双线性插值的方法显示颜色
axes\[1\].imshow(wc.recolor(color\_func=image\_colors), interpolation="bilinear") # 右边的图片显示模板图片
axes\[2\].imshow(mk, cmap=plt.cm.gray) for ax in axes:
    ax.set\_axis\_off()
plt.show() # 给词云对象按模板图片的颜色重新上色
wc\_color = wc.recolor(color\_func=image\_colors) # 将词云图片导出到当前文件夹
wc\_color.to\_file('output10-alice.png')

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![10号词云：《爱丽丝漫游仙境》词云(勾勒轮廓线)](https://camo.githubusercontent.com/da60e9b2139ab420202340fe421341ec1a26f72d/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d386638386533663934656234336233322e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/da60e9b2139ab420202340fe421341ec1a26f72d/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d386638386533663934656234336233322e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

[![image.png](https://camo.githubusercontent.com/6e2aa9ce1c4facc54c4514cb7d2056b4ac22cae2/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d626232343336623833663630336231392e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/6e2aa9ce1c4facc54c4514cb7d2056b4ac22cae2/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d626232343336623833663630336231392e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

[![加模板图片颜色的词云](https://camo.githubusercontent.com/3c78d0cbb90f1d80bb3ce790f6f1cd5730d439d8/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d363235393763363063653566343432662e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/3c78d0cbb90f1d80bb3ce790f6f1cd5730d439d8/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d363235393763363063653566343432662e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

### 11号词云：绘制你的微信好友个性签名词云

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 11号词云：绘制你的微信好友个性签名词云 # B站专栏：同济子豪兄 2019-05-23

# 导入微信库ichat，中文分词库jieba
import itchat import jieba # 先登录微信，跳出登陆二维码
itchat.login()
tList \= \[\] # 获取好友列表
friends = itchat.get\_friends(update=True) # 构建所有好友个性签名组成的大列表tList
for i in friends: # 获取个性签名
    signature = i\["Signature"\] if 'emoji' in signature: pass
    else:
        tList.append(signature)
text \= " ".join(tList) # 对个性签名进行中文分词
wordlist\_jieba = jieba.lcut(text, cut\_all=True)
wl\_space\_split \= " ".join(wordlist\_jieba) # 导入imageio库中的imread函数，并用这个函数读取本地图片，作为词云形状图片
import imageio
mk \= imageio.imread("chinamap.png") # 导入词云制作库wordcloud
import wordcloud # 构建并配置词云对象w，注意要加scale参数，提高清晰度
my\_wordcloud = wordcloud.WordCloud(background\_color='white',
                                   width\=1000,
                                   height\=700,
                                   font\_path\='msyh.ttc',
                                   max\_words\=2000,
                                   mask\=mk,
                                   scale\=20)
my\_wordcloud.generate(wl\_space\_split)

nickname \= friends\[0\]\['NickName'\]
filename \= "output11-{}的微信好友个性签名词云图.png".format(nickname)
my\_wordcloud.to\_file(filename) # 显示词云图片
import matplotlib.pyplot as plt
plt.imshow(my\_wordcloud)
plt.axis("off")
plt.show() print('程序结束')

微信好友个性签名词云

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![微信好友个性签名词云](https://camo.githubusercontent.com/6650cbba9ebcc33591da6229842a1a28d1919c98/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d663063363332373762656436653333362e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/6650cbba9ebcc33591da6229842a1a28d1919c98/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d663063363332373762656436653333362e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

### Python中文语言处理第三方库snownlp小试牛刀

安装中文文本分析库snownlp：在命令行中输入`pip install snownlp`。

打开python的`交互式shell`界面，也就是有三个大于号`>>>`的这个界面，依次输入以下命令。

![复制代码](https://common.cnblogs.com/images/copycode.gif)

\>>> import snownlp \>>> word = snownlp.SnowNLP("中华民族伟大复兴") \>>> feeling = word.sentiments \>>> feeling 0.9935086411278989
>>> word = snownlp.SnowNLP("快递慢到死，客服态度不好，退款！") \>>> feeling = word.sentiments \>>> feeling 0.00012171645785852281

![复制代码](https://common.cnblogs.com/images/copycode.gif)

> snownlp的语料库是淘宝等电商网站的评论，所以对购物类的文本情感分析准确度很高。

一键执行的详细脚本文件详见[github代码库-zihaowordcloud](https://github.com/TommyZihao/zihaowordcloud)中的`test2-snownlp.py`文件。

### 12号词云：《三体Ⅱ黑暗森林》情感分析词云

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 12号词云：《三体Ⅱ黑暗森林》情感分析词云 # B站专栏：同济子豪兄 2019-5-23

# 导入词云制作库wordcloud和中文分词库jieba
import jieba import wordcloud # 导入imageio库中的imread函数，并用这个函数读取本地图片，作为词云形状图片
import imageio
mk \= imageio.imread("chinamap.png") # 构建并配置两个词云对象w1和w2，分别存放积极词和消极词
w1 = wordcloud.WordCloud(width=1000,
                        height\=700,
                        background\_color\='white',
                        font\_path\='msyh.ttc',
                        mask\=mk,
                        scale\=15)
w2 \= wordcloud.WordCloud(width=1000,
                        height\=700,
                        background\_color\='white',
                        font\_path\='msyh.ttc',
                        mask\=mk,
                        scale\=15) # 对来自外部文件的文本进行中文分词，得到积极词汇和消极词汇的两个列表
f = open('三体黑暗森林.txt',encoding='utf-8')
txt \= f.read()
txtlist \= jieba.lcut(txt)
positivelist \= \[\]
negativelist \= \[\] # 下面对文本中的每个词进行情感分析，情感>0.96判为积极词，情感<0.06判为消极词
print('开始进行情感分析，请稍等，三国演义全文那么长的文本需要三分钟左右') # 导入自然语言处理第三方库snownlp
import snownlp for each in txtlist:
    each\_word \= snownlp.SnowNLP(each)
    feeling \= each\_word.sentiments if feeling > 0.96:
        positivelist.append(each) elif feeling < 0.06:
        negativelist.append(each) else: pass
# 将积极和消极的两个列表各自合并成积极字符串和消极字符串，字符串中的词用空格分隔
positive\_string = " ".join(positivelist)
negative\_string \= " ".join(negativelist) # 将string变量传入w的generate()方法，给词云输入文字
w1.generate(positive\_string)
w2.generate(negative\_string) # 将积极、消极的两个词云图片导出到当前文件夹
w1.to\_file('output12-positive.png')
w2.to\_file('output12-negative.png') print('词云生成完成')

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![《三体Ⅱ黑暗森林 积极词汇词云和消极词汇词云》](https://camo.githubusercontent.com/47ec86cae03fdc2bfd6d0b5e0fd0d369c949d721/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d656164613562633430316532316239362e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/47ec86cae03fdc2bfd6d0b5e0fd0d369c949d721/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d656164613562633430316532316239362e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

# 13号词云：三国人物阵营分色词云 # B站专栏：同济子豪兄 2019-5-23

# 导入wordcloud库，并定义两个函数
from wordcloud import (WordCloud, get\_single\_color\_func) class SimpleGroupedColorFunc(object): """Create a color function object which assigns EXACT colors
       to certain words based on the color to words mapping

       Parameters
       ----------
       color\_to\_words : dict(str -> list(str))
         A dictionary that maps a color to the list of words.

       default\_color : str
         Color that will be assigned to a word that's not a member
         of any value from color\_to\_words. """

    def \_\_init\_\_(self, color\_to\_words, default\_color):
        self.word\_to\_color \= {word: color for (color, words) in color\_to\_words.items() for word in words}

        self.default\_color \= default\_color def \_\_call\_\_(self, word, \*\*kwargs): return self.word\_to\_color.get(word, self.default\_color) class GroupedColorFunc(object): """Create a color function object which assigns DIFFERENT SHADES of
       specified colors to certain words based on the color to words mapping.

       Uses wordcloud.get\_single\_color\_func

       Parameters
       ----------
       color\_to\_words : dict(str -> list(str))
         A dictionary that maps a color to the list of words.

       default\_color : str
         Color that will be assigned to a word that's not a member
         of any value from color\_to\_words. """

    def \_\_init\_\_(self, color\_to\_words, default\_color):
        self.color\_func\_to\_words \= \[
            (get\_single\_color\_func(color), set(words)) for (color, words) in color\_to\_words.items()\]

        self.default\_color\_func \= get\_single\_color\_func(default\_color) def get\_color\_func(self, word): """Returns a single\_color\_func associated with the word"""
        try:
            color\_func \= next(
                color\_func for (color\_func, words) in self.color\_func\_to\_words if word in words) except StopIteration:
            color\_func \= self.default\_color\_func return color\_func def \_\_call\_\_(self, word, \*\*kwargs): return self.get\_color\_func(word)(word, \*\*kwargs) # 导入imageio库中的imread函数，并用这个函数读取本地图片，作为词云形状图片
import imageio
mk \= imageio.imread("chinamap.png")

w \= WordCloud(width=1000,
              height\=700,
              background\_color\='white',
              font\_path\='msyh.ttc',
              mask\=mk,
              scale\=15,
              max\_font\_size\=60,
              max\_words\=20000,
              font\_step\=1) import jieba # 对来自外部文件的文本进行中文分词，得到string
f = open('三国演义.txt',encoding='utf-8')
txt \= f.read()
txtlist \= jieba.lcut(txt)
string \= " ".join(txtlist) # 将string变量传入w的generate()方法，给词云输入文字
w.generate(string) # 创建字典，按人物所在的不同阵营安排不同颜色，绿色是蜀国，橙色是魏国，紫色是东吴，粉色是诸侯群雄
color\_to\_words = { 'green': \['刘备','刘玄德','孔明','诸葛孔明', '玄德', '关公', '玄德曰','孔明曰', '张飞', '赵云','后主', '黄忠', '马超', '姜维', '魏延', '孟获', '关兴','诸葛亮','云长','孟达','庞统','廖化','马岱'\], 'red': \['曹操', '司马懿', '夏侯', '荀彧', '郭嘉','邓艾','许褚', '徐晃','许诸','曹仁','司马昭','庞德','于禁','夏侯渊','曹真','钟会'\], 'purple':\['孙权','周瑜','东吴','孙策','吕蒙','陆逊','鲁肃','黄盖','太史慈'\], 'pink':\['董卓','袁术','袁绍','吕布','刘璋','刘表','貂蝉'\]
} # 其它词语的颜色
default\_color = 'gray'

# 构建新的颜色规则
grouped\_color\_func = GroupedColorFunc(color\_to\_words, default\_color) # 按照新的颜色规则重新绘制词云颜色
w.recolor(color\_func=grouped\_color\_func) # 将词云图片导出到当前文件夹
w.to\_file('output13-threekingdoms.png')

13号词云：《三国演义》人物阵营分色词云

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![13号词云：《三国演义》人物阵营分色词云](https://camo.githubusercontent.com/297376338faefc1c191263a575564d88da9c68d5/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d316632643561363035346165666165642e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)](https://camo.githubusercontent.com/297376338faefc1c191263a575564d88da9c68d5/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31333731343434382d316632643561363035346165666165642e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)
