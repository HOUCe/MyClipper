# 1万字详解 python logging日志模块

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MjM5MzgyODQxMQ==&mid=2650378137&idx=1&sn=0db47ea066c8edcc70bfafa1e77fd141&chksm=be9c34cd89ebbddb0cda616be94c1b4f7dca25cbe55bd8a05c93306f391963391e8fd955e454&mpshare=1&scene=1&srcid=1223tak8zdTbp7ayIVVADBUs&sharer_sharetime=1640227622679&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)志军100 Python之禅

**前言**

这篇文章熬了一周，终于写完了。一个知识点自己理解可能只需要花半个小时，而要想把它写出来让别人理解，要花十倍甚至更多的时间。所以说写技术文是真的不容易。而它的价值在于它的生命力更长久。即使三五年后给别人看依然会有收获。对写作者自己而言，写的过程也是对知识的一次更通透的理解。

以下为正文

说到日志，无论是写框架代码还是业务代码，都离不开日志的记录，他能给我们定位问题带来极大的帮助。

记录日志最简单的方法就是在你想要记录的地方加上一句 print ， 我相信无论是新手还是老鸟都经常这么干。在简单的代码中或者小型项目中这么干一点问题都没有。但是在一些稍大一点的项目，有时候定位一个问题，需要查看历史日志定位问题，用print就不合时宜了。

print 打印出来的日志没有时间，不知道日志记录的位置，也没有可读的日志格式， 还不能把日志输出到指定文件。。。。除非这些你都全部自己重复造一遍轮子。

最佳的做法是使用内置的logging模块， 因为 logging 模块给开发者提供了非常丰富的功能。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FrO1ibUkmNGMlyASvLaJQHMlkiaft6WlvZaYJowyDIIUPicTlnx0iazicaFrL6dE1oLuFuC6wuTkLznt1Y3QaicYVial6A%2F640%3Fwx_fmt%3Dpng)

比如上图就是用标准库logging模块记录生成的日志，有日志的具体时间、日志发生的模块、有日志级别和日志的具体内容等等

怎么用呢，来看个例子

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FrO1ibUkmNGMlyASvLaJQHMlkiaft6WlvZaV55zYeHR4lj1zaT8vk2tpqjTf7jORKia7fYhSCLuNQU82BC7oGOo3yA%2F640%3Fwx_fmt%3Dpng)

导入logging模块，然后直接使用logging提供的日志消息记录方法就可以。

### 日志级别

日志级别分为以下5个级别

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FrO1ibUkmNGMlyASvLaJQHMlkiaft6WlvZaV5CnknP1mfsWa2u5osKUCvKoOHXQRw8LZFGZH9eAdbvQaK5iaX0RoMg%2F640%3Fwx_fmt%3Dpng)

日志级别

使用场景

DEBUG

debug级别用来记录详细的信息，方便定位问题进行调试，在生产环境我们一般不开启DEBUG

INFO

用来记录关键代码点的信息，以便代码是否按照我们预期的执行，生产环境通常会设置INFO级别

WARNING

记录某些不预期发生的情况，如磁盘不足

ERROR

由于一个更严重的问题导致某些功能不能正常运行时记录的信息

CRITICAL

当发生严重错误，导致应用程序不能继续运行时记录的信息

日志级别重要程度逐次提高，python提供了5个对应级别的方法。默认情况下日志的级别是WARGING, 低于WARING的日志信息都不会输出。

从上面代码中可以看到loging.warging以后的日志内容都打印在标准输出流，也就是命令行窗口，但是logging.debug和info记录的日志不会打印出来。

#### 修改日志级别

如何让debug级别的信息也输出？

当然是修改默认的日志级别，在开始记录日志前可以使用`logging.basicConfig`方法来设定日志级别

`import logging-
logging.basicConfig( level=logging.DEBUG)-
logging.debug("this is debug")-
logging.info("this is info")-
logging.error("this is error")-
`

设置为debug级别后，所有的日志信息都会输出

`DEBUG:root:this is debug-
INFO:root:this is info-
ERROR:root:this is error-
`

#### 日志记录到文件

前面的日志默认会把日志输出到标准输出流，就是只在命令行窗口输出，程序重启后历史日志没地方找，所以把日志内容永久记录是一个很常见的需求。同样通过配置函数logging.basicConfig可以指定日志输出到什么地方

`import logging-
logging.basicConfig(filename="test.log", level=logging.INFO)-
logging.debug("this is debug")-
logging.info("this is info")-
logging.error("this is error")-
`

这里我指定日志输出到文件test.log中，日志级别指定为了 INFO，最后文件中记录的内容如下：

`INFO:root:this is info-
ERROR:root:this is error-
`

每次重新运行时，日志会以追加的方式在后面， 如果每次运行前要覆盖之前的日志，则需指定 filemode='w'， 这个和 `open` 函数写数据到文件用的参数是一样的。

#### 指定日志格式

默认输出的格式包含3部分，日志级别，日志记录器的名字，以及日志内容，中间用“:”连接。如果我们想改变日志格式，例如想加入日期时间、显示日志器名字，我们是可以指定format参数来设置日志的格式

`import logging-
logging.basicConfig(format='%(asctime)s %(levelname)s %(name)s %(message)s')-
logging.error("this is error")-
`

输出

`2021-12-15 07:44:16,547 ERROR root this is error-
`

日志格式化输出提供了非常多的参数，除了时间、日志级别、日志消息内容、日志记录器的名字外，还可以指定线程名，进程名等等

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FrO1ibUkmNGMlyASvLaJQHMlkiaft6WlvZacscI6guCux5okSs2Z8DSprv0WiciayjmanWOsROGNgej3OlexzgZCqyA%2F640%3Fwx_fmt%3Dpng)

到这里为止，日志模块的基本用法就这些了，也能满足大部分应用场景，更高级的方法接着往下看，可以帮助你更好的处理日志

### 记录器（logger）

前面介绍的日志记录，其实都是通过一个叫做日志记录器（Logger）的实例对象创建的，每个记录器都有一个名称，直接使用logging来记录日志时，系统会默认创建 名为 root 的记录器，这个记录器是根记录器。记录器支持层级结构，子记录器通常不需要单独设置日志级别以及Handler（后面会介绍），如果子记录器没有单独设置，则它的行为会委托给父级。

记录器名称可以是任意名称，不过最佳实践是直接用模块的名称当作记录器的名字。命名如下

`logger = logging.getLogger(__name__)-
`

默认情况下，记录器采用层级结构，上句点作为分隔符排列在命名空间的层次结构中。层次结构列表中位于下方的记录器是列表中较高位置的记录器的子级。例如，有个名叫 foo 的记录器，而名字是 foo.bar，foo.bar.baz，和 foo.bam 的记录器都是 foo 的子级。

`├─foo-
│ │ main.py-
│ │ __init__.py-
│ │ -
│ ├─bam-
│ │ │ __init__.py-
│ │ │ -
│ │ -
│ ├─bar-
│ │ │ __init__.py-
│ │ │ -
│ │ ├─baz-
│ │ │ │ __init__.py-
│ │ │ │ -
`

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FrO1ibUkmNGMlyASvLaJQHMlkiaft6WlvZa4xxz9h6g8aIfWyvkBA7fico0xWlia3W65ibj2JzicRNramsNkmXM1pKl2A%2F640%3Fwx_fmt%3Dpng)

main.py

`import foo-
from foo import bar-
from foo import bam-
from foo.bar import baz

if __name__ == '__main__':-
 pass

`

foo.py

`import logging

logging.basicConfig()-
logger = logging.getLogger(__name__)-
logger.setLevel(logging.INFO)

logger.info("this is foo")

`

这里我只设置foo这个记录器的级别为INFO

bar.py

`import logging

logger = logging.getLogger(__name__)-
logger.info("this is bar")

`

其它子模块都是像bar.py一样类似的代码，都没有设置日志级别，最后的输出结果是

`INFO:foo:this is foo-
INFO:foo.bar:this is bar-
INFO:foo.bam:this is bam-
INFO:foo.bar.baz:this is baz-
`

这是因为 foo.bar 这个记录器没有设置日志级别，就会向上找到已经设置了日日志级别的祖先，这里刚好找到父记录器foo的级别为INFO，如果foo也没设置的话，就会找到根记录器root，root默认的级别为WARGING。

### 处理器（Handler）

记录器负责日志的记录，但是日志最终记录在哪里记录器并不关心，而是交给了另一个家伙--处理器（Handler）去处理。

例如一个Flask项目，你可能会将INFO级别的日志记录到文件，将ERROR级别的日志记录到标准输出，将某些关键日志（例如有订单或者严重错误）发送到某个邮件地址通知老板。这时候你的记录器添加多个不同的处理器来处理不同的消息日志，以此根据消息的重要性发送的特定的位置。![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FrO1ibUkmNGMlyASvLaJQHMlkiaft6WlvZabI9IOs9bs5opAWp0UFabfGMwGnyL7iaJlFY983xxPbCkiakD8eZXZGSg%2F640%3Fwx_fmt%3Dpng)

Python内置了很多实用的处理器，常用的有：

1、StreamHandler 标准流处理器，将消息发送到标准输出流、错误流-
2、FileHandler 文件处理器，将消息发送到文件-
3、RotatingFileHandler 文件处理器，文件达到指定大小后，启用新文件存储日志-
4、TimedRotatingFileHandler 文件处理器，日志以特定的时间间隔轮换日志文件

#### 处理器操作

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FrO1ibUkmNGMlyASvLaJQHMlkiaft6WlvZa06gtdvTLibGqME9V5Um33hxNmMzx4vq3F6tT6Buct2G9FKlWTAKmtfw%2F640%3Fwx_fmt%3Dpng)

Handler 提供了4个方法给开发者使用，细心的你可以发现了，logger可以设置level，Handler也可以设置Level。通过setLevel可以将记录器记录的不同级别的消息发送到不同的地方去。

`import logging-
from logging import StreamHandler-
from logging import FileHandler

logger = logging.getLogger(__name__)

# 设置为DEBUG级别-
logger.setLevel(logging.DEBUG)

# 标准流处理器，设置的级别为WARAING-
stream_handler = StreamHandler()-
stream_handler.setLevel(logging.WARNING)-
logger.addHandler(stream_handler)

# 文件处理器，设置的级别为INFO-
file_handler = FileHandler(filename="test.log")-
file_handler.setLevel(logging.INFO)-
logger.addHandler(file_handler)

logger.debug("this is debug")-
logger.info("this is info")-
logger.error("this is error")-
logger.warning("this is warning")

`

运行后，在命令行窗口输出的日志内容是：

`this is error-
this is warning-
`

输出在文件的日志内容是：

`this is info-
this is error-
this is warning-
`

尽管我们将logger的级别设置为了DEBUG，但是debug记录的消息并没有输出，因为我给两个Handler设置的级别都比DEBUG要高，所以这条消息被过滤掉了。

### 格式器（formatter）

格式器在文章的前面部分其实已经有所介绍，不过那是通过logging.basicConfig来指定的，其实格式器还可以以对象的形式来设置在Handler上。格式器可以指定日志的输出格式，要不要展示时间，时间格式什么，要不要展示日志的级别，要不要展示记录器的名字等等，都可以通过一个格式器对消息进行格式化输出。

`import logging-
from logging import StreamHandler

logger = logging.getLogger(__name__)

# 标准流处理器-
stream_handler = StreamHandler()-
stream_handler.setLevel(logging.WARNING)

# 创建一个格式器-
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')-
# 作用在handler上-
stream_handler.setFormatter(formatter)-
# 添加处理器-
logger.addHandler(stream_handler)

logger.info("this is info")-
logger.error("this is error")-
logger.warning("this is warning")

`

注意，格式器只能作用在处理器上，通过处理器的`setFromatter`方法设置格式器。而且一个Handler只能设置一个格式器。是一对一的关系。而 logger 与 handler 是一对多的关系，一个logger可以添加多个handler。handler 和 logger 都可以设置日志的等级。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FrO1ibUkmNGMlyASvLaJQHMlkiaft6WlvZaot0HWBa0NP7ibaGBG9GyibRrSFnHPkxfPPtu2xjN6dkqQoXzAf3n5yuA%2F640%3Fwx_fmt%3Dpng)

### logging.basicConfig

回到最开始的地方，logging.basicConfig() 方法为我们干了啥？现在你大概能猜出来了。来看python源码中是怎么说的

> Do basic configuration for the logging system.
> 
> This function does nothing if the root logger already has handlers configured. It is a convenience method intended for use by simple scripts to do one-shot configuration of the logging package.
> 
> The default behaviour is to create a StreamHandler which writes to sys.stderr, set a formatter using the BASIC\_FORMAT format string, and add the handler to the root logger.
> 
> A number of optional keyword arguments may be specified, which can alter the default behaviour.

1、创建一个root记录器-
2、设置root的日志级别为warning-
3、为root记录器添加StreamHandler处理器-
4、为处理器设置一个简单格式器

`logging.basicConfig()-
logging.warning("hello")-
`

这两行代码其实就等价于：

`import sys-
import logging-
from logging import StreamHandler-
from logging import Formatter

logger = logging.getLogger("root")-
logger.setLevel(logging.WARNING)-
handler = StreamHandler(sys.stderr)-
logger.addHandler(handler)-
formatter = Formatter(" %(levelname)s:%(name)s:%(message)s")-
handler.setFormatter(formatter)-
logger.warning("hello")

`

logging.basicConfig 方法做的事情是相当于给日志系统做一个最基本的配置，方便开发者快速接入使用。它必须在开始记录日志前调用。不过如果 root 记录器已经指定有其它处理器，这时候你再调用basciConfig，则该方式将失效，它什么都不做。

### 日志配置

日志的配置除了前面介绍的将配置直接写在代码中，还可以将配置信息单独放在配置文件中，实现配置与代码分离。

日志配置文件 logging.conf

`[loggers]-
keys=root

[handlers]-
keys=consoleHandler

[formatters]-
keys=simpleFormatter

[logger_root]-
level=DEBUG-
handlers=consoleHandler

[handler_consoleHandler]-
class=StreamHandler-
level=DEBUG-
formatter=simpleFormatter-
args=(sys.stdout,)

[formatter_simpleFormatter]-
format=%(asctime)s - %(name)s - %(levelname)s - %(message)s

`

加载配置文件

`import logging-
import logging.config

# 加载配置-
logging.config.fileConfig('logging.conf')

# 创建 logger-
logger = logging.getLogger()

# 应用代码-
logger.debug("debug message")-
logger.info("info message")-
logger.warning("warning message")-
logger.error("error message")

`

输出

`2021-12-23 00:02:07,019 - root - DEBUG - debug message-
2021-12-23 00:02:07,019 - root - INFO - info message-
2021-12-23 00:02:07,019 - root - WARNING - warning message-
2021-12-23 00:02:07,019 - root - ERROR - error message-
`

到这里算是对logging的一次比较完整的介绍，当然，还有很多细节并没有涉及到，因此我给了几个链接供参考。

参考链接：

https://docs.python.org/3/library/logging.html#

https://docs.python.org/3/howto/logging.html#logging-advanced-tutorial

https://awaywithideas.com/python-logging-a-practical-guide/

https://rmcomplexity.com/article/2020/12/01/introduction-to-python-logging.html

温馨提示：文章为有偿阅读，单篇1元即可支持

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MjM5MzgyODQxMQ==&mid=2650378137&idx=1&sn=0db47ea066c8edcc70bfafa1e77fd141&chksm=be9c34cd89ebbddb0cda616be94c1b4f7dca25cbe55bd8a05c93306f391963391e8fd955e454&mpshare=1&scene=1&srcid=1223tak8zdTbp7ayIVVADBUs&sharer_sharetime=1640227622679&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)