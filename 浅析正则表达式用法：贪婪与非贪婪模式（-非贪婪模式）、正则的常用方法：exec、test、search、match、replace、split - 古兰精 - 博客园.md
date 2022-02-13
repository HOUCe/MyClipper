# [浅析正则表达式用法：贪婪与非贪婪模式（?-非贪婪模式）、正则的常用方法：exec、test、search、match、replace、split - 古兰精 - 博客园](https://www.cnblogs.com/goloving/p/14004542.html)

### 一、匹配次数中的贪婪与非贪婪

 在使用修饰匹配次数的特殊符号时，有几种表示方法可以使同一个表达式能够匹配不同的次数，比如："{m,n}", "{m,}", "?", "\*", "+"，具体匹配的次数随被匹配的字符串而定。这种重复匹配不定次数的表达式在匹配过程中，总是尽可能多的匹配。比如，针对文本 "**dxxxdxxxd**"，举例如下：

表达式

匹配结果

[(d)(/w+)](http://www.regexlab.com/zh/workshop.asp?pat=%28d%29%28%5Cw%2B%29&txt=dxxxdxxxd)

"/w+" 将匹配第一个 "d" 之后的所有字符 "xxxdxxxd"

[(d)(/w+)(d)](http://www.regexlab.com/zh/workshop.asp?pat=%28d%29%28%5Cw%2B%29%28d%29&txt=dxxxdxxxd)

"/w+" 将匹配第一个 "d" 和最后一个 "d" 之间的所有字符 "xxxdxxx"。虽然 "/w+" 也能够匹配上最后一个 "d"，但是为了使整个表达式匹配成功，"/w+" 可以 "让出" 它本来能够匹配的最后一个 "d"

由此可见，"/w+" 在匹配的时候，总是尽可能多的匹配符合它规则的字符。虽然第二个举例中，它没有匹配最后一个 "d"，但那也是为了让整个表达式能够匹配成功。同理，带 "\*" 和 "{m,n}" 的表达式都是尽可能地多匹配，带 "?" 的表达式在可匹配可不匹配的时候，也是尽可能的 "要匹配"。这 种匹配原则就叫作 "贪婪" 模式 。

非贪婪模式：

在修饰匹配次数的特殊符号后再加上一个 "?" 号，则可以使匹配次数不定的表达式尽可能少的匹配，使可匹配可不匹配的表达式，尽可能的 "不匹配"。这种匹配原则叫作 "非贪婪" 模式，也叫作 "勉强" 模式。如果少匹配就会导致整个表达式匹配失败的时候，与贪婪模式类似，非贪婪模式会最小限度的再匹配一些，以使整个表达式匹配成功。举例如下，针对文本 "dxxxdxxxd" 举例：

表达式

匹配结果

[(d)(/w+?)](http://www.regexlab.com/zh/workshop.asp?pat=%28d%29%28%5Cw%2B%3F%29&txt=dxxxdxxxd)

"/w+?" 将尽可能少的匹配第一个 "d" 之后的字符，结果是："/w+?" 只匹配了一个 "x"

[(d)(/w+?)(d)](http://www.regexlab.com/zh/workshop.asp?pat=%28d%29%28%5Cw%2B%3F%29%28d%29&txt=dxxxdxxxd)

为了让整个表达式匹配成功，"/w+?" 不得不匹配 "xxx" 才可以让后边的 "d" 匹配，从而使整个表达式匹配成功。因此，结果是："/w+?" 匹配 "xxx"

更多的情况，举例如下：

[举例1：表达式 "<td>(.\*)</td>" 与字符串 "<td><p>aa</p></td> <td><p>bb</p></td>" 匹配时](http://www.regexlab.com/zh/workshop.asp?pat=%3Ctd%3E%28%2E%2A%29%3C%2Ftd%3E&txt=%3Ctd%3E%3Cp%3Eaa%3C%2Fp%3E%3C%2Ftd%3E%3Ctd%3E%3Cp%3Ebb%3C%2Fp%3E%3C%2Ftd%3E)，匹配的结果是：成功；匹配到的内容是 "<td><p>aa</p></td> <td><p>bb</p></td>" 整个字符串， 表达式中的 "</td>" 将与字符串中最后一个 "</td>" 匹配。

[举例2：相比之下，表达式 "<td>(.\*?)</td>" 匹配举例1中同样的字符串时](http://www.regexlab.com/zh/workshop.asp?pat=%3Ctd%3E%28%2E%2A%3F%29%3C%2Ftd%3E&txt=%3Ctd%3E%3Cp%3Eaa%3C%2Fp%3E%3C%2Ftd%3E%3Ctd%3E%3Cp%3Ebb%3C%2Fp%3E%3C%2Ftd%3E)，将只得到 "<td><p>aa</p></td>"， 再次匹配下一个时，可以得到第二个 "<td><p>bb</p></td>"。

 首先我们以简单的例子来说说什么是正则表达式的贪婪与非贪婪匹配？

 比如假定匹配字符串和正则表达式为：

1、贪婪匹配

let res = /ab.\*c/
'abcdefc'.match(res) // \["abcdefc", index: 0, input: "abcdefc", groups: undefined\]

 正则表达式一般趋向于最大长度匹配，总是尝试匹配尽可能多的字符，也就是所谓的贪婪匹配。如上面使用模式p匹配字符串str，结果就是匹配到：**abcdefc。**当出现c时，它还是继续向后找，又找到c，它就把cdef当做是(.\*)的匹配

2、非贪婪匹配

let res = /ab.\*?c/
'abcdefc'.match(res) // \["abc", index: 0, input: "abcdefc", groups: undefined\]

 非贪婪匹配就是匹配到结果就好，总是尝试匹配尽可能少的字符。如上面使用模式p匹配字符串str，结果就是匹配到：**abc**。当它遇见c后，它就停止查找，此时把空字符作为(.\*)的匹配。

3、贪婪匹配与非贪婪匹配如何区分

 **默认是贪婪模式；在量词后面直接加上一个问号？就是非贪婪模式。**

 我们熟知的量词有：

\*

任意多个

+

至少一个

？

0或1个

{m,n}

m到n个

 比如在去除HTML中的标签时，我们使用 '<.+>' 去匹配得到的却是一堆'\\n'，我们来看看原因。拿其中的一行 <p><br/></p> 来看，为什么输出'\\n'：

![复制代码](https://common.cnblogs.com/images/copycode.gif)

let \_html1 = \`<p><br/></p>
<p><br/></p>
<p><br/></p>\`
let res \= /<.+>/g
\_html1.replace(res, '') " 
"

![复制代码](https://common.cnblogs.com/images/copycode.gif)

 在匹配操作时，首先匹配 <（左尖括号），<p>的<就已经匹配到，当匹配到<p>的>时，匹配未结束，它继续往后匹配。当匹配到<br/>的>时，仍然未结束，贪婪的向后继续匹配，直到匹配到</p>的>，再继续去匹配，字符串后面有个‘\\n',结束匹配，它就把  p><br/></p 这些内容都当做 .+ 来处理。因此匹配到这一行内容除了'\\n'，并进行替换操作，替换为空字符''，因此输出'\\n'。因此，在 .+ 后面加上?表示非贪婪模式，当碰到<p>的 > 就停止此次匹配。

![复制代码](https://common.cnblogs.com/images/copycode.gif)

let res = /<.+>/
'<p><br/></p>'.match(res) // \["<p><br/></p>", index: 0, input: "<p><br/></p>", groups: undefined\]
 let res \= /<.+?>/
'<p><br/></p>'.match(res) // \["<p>", index: 0, input: "<p><br/></p>", groups: undefined\]

![复制代码](https://common.cnblogs.com/images/copycode.gif)

### 二、常用方法

1、exec()：一个在字符串中执行查找匹配的RegExp方法，它返回一个数组（未匹配到则返回null）

var reg1= /^\\w+$/
var str = 'w111fafdd' console.log(reg1.exec(str)) // \["w111fafdd", index: 0, input: "w111fafdd", groups: undefined\]

1.  跟match的结果有点像，但是match匹配到多个时可以返回一个满足匹配规则所有字符串的数组
2.  第一个参数是返回能够匹配正则规则的字符串，则案例的非特殊字符
3.  input属性的值是原来的字符串，即str
4.  结果的index表示从第几个字符串开始匹配到的

2、test()：一个在字符串中测试是否匹配的RegExp方法，它返回true或false

// 接上面代码
console.log(reg1.test(str)) // true

3、search()：一个在字符串中测试匹配的String方法，它返回匹配到的位置索引，或者在失败时返回-1。

// 接着上面代码
console.log(str.search(reg1)) // 0

4、match()：一个在字符串中执行查找匹配的String方法，它返回一个数组或者在未匹配到时返回null。

console.log(str.match(reg1)) //\["w111fafdd", index: 0, input: "w111fafdd", groups: undefined\]

 跟上面exec除了调用的对象和参数不同，结果是一样，当一个字符串多个能匹配正则表达式规则时，match返回是不同的

 看一下这2个区别

![复制代码](https://common.cnblogs.com/images/copycode.gif)

var reg1= /\[a-z\]\\d{3}/g var str = 'w111faf222dd' reg1.exec(str) // \["w111", index: 0, input: "w111faf222dd", groups: undefined\]

var reg1 = /\[a-z\]\\d{3}/g var str = 'w111faf222dd' str.match(reg1) // (2) \["w111", "f222"\]

![复制代码](https://common.cnblogs.com/images/copycode.gif)

 **案例：提取工资**

 需求：var str = ‘张三：1000，李四：5000，王五：8000。’；提取他们三人的工资并返回一个数组

![复制代码](https://common.cnblogs.com/images/copycode.gif)

// 方式1：使用提取组的方法
var str = '张三：1000，李四：5000，王五：8000。'; // 写一个匹配它们的正则表达式
var reg = /(\\d+)/g // 使用数组用来装他们
var arr = \[\] while(reg.test(str)){
　　arr.push(RegExp.$1) //$1是提取第一个组的意思,如果上面的正则表达式的括号没加，则提取是空的字符串，因为没有分组
}
console.log(arr) //\["1000", "5000", "8000"\]

![复制代码](https://common.cnblogs.com/images/copycode.gif)

// 我们可以使用一个match就可以搞定了
var str = '张三：1000，李四：5000，王五：8000。'; var reg = /\\d+/g 
console.log(str.match(reg)) // \["1000", "5000", "8000"\] 

5、replace()：一个在字符串中执行查找匹配的String方法，并且使用替换字符串替换掉匹配到的子字符串

1）str.replace(parame1,parame2)；就是将parame1替换成parame2

2）**去字符串的所有空格**

 trim()这个方法只能去掉前后的空格，当要去掉字符串所有的空格时，使用replace替换掉所有的空格

var str = " 123AD  asadf     asadfasf  adf "
var str1 = str.replace(/\\s+/g,'')
console.log(str1) // 123ADasadfasadfasfadf

6、split()：一个使用正则表达式或者一个固定字符串分隔一个字符串，并将分隔后的子字符串存储到数组中的String方法

1）我们之前使用split()时大部分是将字符串切割为数组，括号跟着的是一个字符串

var dateStr = '2015-1-5'; var arr = dateStr.split('\-')
console.log(arr) // \["2015", "1", "5"\]

2）参数为正则表达式

var dateStr = '2015-12&23,78' dateStr.split(/\[-&,\]/) // (4) \["2015", "12", "23", "78"\]

 以上的所有方法并不会改变调用它的字符串 | 正则表达式，记得它们如何使用和返回值即可。
