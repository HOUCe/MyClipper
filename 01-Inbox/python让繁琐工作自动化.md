# python让繁琐工作自动化

source:: https://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649209362&idx=1&sn=df07544912641e8621efe5dcf44124ad&chksm=f3d1f067c4a67971fc8566294b9d8c78d7649334fc1b06759ef4ef22bd3da81cd399155338de&scene=21#wechat_redirect

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

 最近小编正在学习《Python编程快速上手—让繁琐工作自动化》，感觉确实能提高工作的效率，所以推荐给大家。后面也会跟大家分享我的学习心得。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

 大家都知道小编一直奉行的格言是“能让计算机做的事情，绝不手动去做”。前面我就给大家举过一些例子，比如

[☞R如何提取，合并pdf文件](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206361&idx=1&sn=c816bf319feaff4426de31804588ccba&chksm=f3d1e42cc4a66d3a1be8c72f42fef6eb09fa9c98776ed38e58e6e9f7a383137aa63706a02753&scene=21#wechat_redirect)

[](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649209174&idx=1&sn=dc9d5c966be7bfda6fc8b26d788f1d3c&chksm=f3d1f323c4a67a3577d2ff67dfa3adb55924c3bbbd28e018b5a1043ad868b192f83aa83de358&scene=21#wechat_redirect)[☞](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206361&idx=1&sn=c816bf319feaff4426de31804588ccba&chksm=f3d1e42cc4a66d3a1be8c72f42fef6eb09fa9c98776ed38e58e6e9f7a383137aa63706a02753&scene=21#wechat_redirect)[Python提取多个pdf首页合并输出](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649209174&idx=1&sn=dc9d5c966be7bfda6fc8b26d788f1d3c&chksm=f3d1f323c4a67a3577d2ff67dfa3adb55924c3bbbd28e018b5a1043ad868b192f83aa83de358&scene=21#wechat_redirect)  

[](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649200555&idx=1&sn=e02ee31ada276f00590563a8597ba2e9&chksm=f3d1dddec4a654c8d6057d658f39f38bc4c1bfa5f33545152c241b66b9ddbc9c4bdf456244f3&scene=21#wechat_redirect)[☞](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206361&idx=1&sn=c816bf319feaff4426de31804588ccba&chksm=f3d1e42cc4a66d3a1be8c72f42fef6eb09fa9c98776ed38e58e6e9f7a383137aa63706a02753&scene=21#wechat_redirect)[用python下载哔哩哔哩视频？](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649200555&idx=1&sn=e02ee31ada276f00590563a8597ba2e9&chksm=f3d1dddec4a654c8d6057d658f39f38bc4c1bfa5f33545152c241b66b9ddbc9c4bdf456244f3&scene=21#wechat_redirect)

[](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649200945&idx=1&sn=f3ce08fa9bfc04c4a0108c0e9261aaf2&chksm=f3d1d344c4a65a527edd7785837df6a4ac4cd51598b6777e9575998c1f82428cb48a26ccf43a&scene=21#wechat_redirect)[☞](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206361&idx=1&sn=c816bf319feaff4426de31804588ccba&chksm=f3d1e42cc4a66d3a1be8c72f42fef6eb09fa9c98776ed38e58e6e9f7a383137aa63706a02753&scene=21#wechat_redirect)[百度文库免费下载](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649200945&idx=1&sn=f3ce08fa9bfc04c4a0108c0e9261aaf2&chksm=f3d1d344c4a65a527edd7785837df6a4ac4cd51598b6777e9575998c1f82428cb48a26ccf43a&scene=21#wechat_redirect)  

[☞](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206361&idx=1&sn=c816bf319feaff4426de31804588ccba&chksm=f3d1e42cc4a66d3a1be8c72f42fef6eb09fa9c98776ed38e58e6e9f7a383137aa63706a02753&scene=21#wechat_redirect)[python自动播放网课](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649200499&idx=1&sn=c1e779b717339c3f26474283071ccf4d&chksm=f3d1dd06c4a654106cd6ae792215caf79853de0ddcdcd2bebd3d36186d6e908abc3981d15e2b&scene=21#wechat_redirect)  

[](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649207033&idx=1&sn=1208c4ab3f6c83898ebc2cd96a390a4c&chksm=f3d1fa8cc4a6739a115dd076506abcf4f3c9c5136bdca36d0daa750b9220913fbbfe5547aedd&scene=21#wechat_redirect)[☞](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206361&idx=1&sn=c816bf319feaff4426de31804588ccba&chksm=f3d1e42cc4a66d3a1be8c72f42fef6eb09fa9c98776ed38e58e6e9f7a383137aa63706a02753&scene=21#wechat_redirect)[R批量预测miRNA和靶基因之间的调控关系-TargetScan篇](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649207033&idx=1&sn=1208c4ab3f6c83898ebc2cd96a390a4c&chksm=f3d1fa8cc4a6739a115dd076506abcf4f3c9c5136bdca36d0daa750b9220913fbbfe5547aedd&scene=21#wechat_redirect)  

[](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206917&idx=1&sn=04aa8d70f07e0c6332acc39ce8272ae0&chksm=f3d1faf0c4a673e6f43569f5b5f65a0cbeef9dc24953164a9559b452d7b40a6ceac89e10f90a&scene=21#wechat_redirect)[☞](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206361&idx=1&sn=c816bf319feaff4426de31804588ccba&chksm=f3d1e42cc4a66d3a1be8c72f42fef6eb09fa9c98776ed38e58e6e9f7a383137aa63706a02753&scene=21#wechat_redirect)[R批量预测miRNA和靶基因之间的调控关系-ENCORI篇](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206917&idx=1&sn=04aa8d70f07e0c6332acc39ce8272ae0&chksm=f3d1faf0c4a673e6f43569f5b5f65a0cbeef9dc24953164a9559b452d7b40a6ceac89e10f90a&scene=21#wechat_redirect)  

[](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206400&idx=1&sn=e08d9b37bd8446f92cf1d34f98daecc9&chksm=f3d1e4f5c4a66de3dec9ec656412cece8200643f5eceb2cbda28e04b9fe7bbc2a57a51ab5d74&scene=21#wechat_redirect)[☞](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206361&idx=1&sn=c816bf319feaff4426de31804588ccba&chksm=f3d1e42cc4a66d3a1be8c72f42fef6eb09fa9c98776ed38e58e6e9f7a383137aa63706a02753&scene=21#wechat_redirect)[R批量下载B细胞和T细胞受体VDJ序列文件](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206400&idx=1&sn=e08d9b37bd8446f92cf1d34f98daecc9&chksm=f3d1e4f5c4a66de3dec9ec656412cece8200643f5eceb2cbda28e04b9fe7bbc2a57a51ab5d74&scene=21#wechat_redirect)  

 其实这些任务都是一些简单的重复劳动，手动去做当然可以完成，但是十分繁琐，耗时费力还容易出错。当小编看完这本书的目录的时候，就有一种相见恨晚的感觉，一口气就读了一百多页。虽然以前也学习过python，但是不成系统。这本书从python基础开始讲，后面就开始讲一些实用的提高工作效率的工具，每一章结束都会有配套的习题和一些小的项目，帮助理解和使用这一章中学到的知识点。这些小的项目并非纯粹的教科书式的习题，确实能够解决一些现实生活中和工作中遇到的问题。比如，我前面讲到的[☞](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649206361&idx=1&sn=c816bf319feaff4426de31804588ccba&chksm=f3d1e42cc4a66d3a1be8c72f42fef6eb09fa9c98776ed38e58e6e9f7a383137aa63706a02753&scene=21#wechat_redirect)[Python提取多个pdf首页合并输出](http://mp.weixin.qq.com/s?__biz=MzI4ODE0NTE3OA==&mid=2649209174&idx=1&sn=dc9d5c966be7bfda6fc8b26d788f1d3c&chksm=f3d1f323c4a67a3577d2ff67dfa3adb55924c3bbbd28e018b5a1043ad868b192f83aa83de358&scene=21#wechat_redirect)，就是从这本书中学到的。从这本书中你还可以学到Excel，word，pdf和网络应用相关的技巧。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/nZw6FoVHqJZmqTF3u2yfKfCM1Xpu88hvPbC3M1ovjfMvp2IIQf82ib3YeZWRCr3B7fN0dNkJibIOgXJLqyBU9lEw/640?wx_fmt=jpeg&tp=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)

**本书目录浏览**

第一部分 Python编程基础  
第1章　Python基础　  
第2章　控制流　  
第3章　函数  
第4章　列表  
第5章　字典和结构化数据  
第6章　字符串操作  
第二部分　自动化任务  
第7章　模式匹配与正则表达式  
第8章　读写文件  
第9章　组织文件  
第10章　调试  
第11章　从Web抓取信息  
第12章　处理Excel电子表格  
第13章　处理PDF和Word文档  
第14章　处理CSV文件和JSON数据  
第15章　保持时间、计划任务和启动  
第16章　发送电子邮件和短信  
第17章　操作图像  
第18章　用GUI自动化控制键盘和  
附录A　安装第三方模块　  
附录B　运行程序  
附录C　习题答案

为方便大家交流学习，小编为大家准备了本书的pdf版本，后台回复“自动化”就可以获取。如果喜欢纸质版也可以购买一本，当做工具书，需要的时候随手翻一下。下面这个链接包含三本，Python编程从入门到实践+快速上手+极客编程。后续我也会给大家分享相关的电子书。

**关注下方公众号，回复“**

**自动化**

**”**

**获取电子书**

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

长按二维码关注

**后台留言“生信交流群”入群**

往期精彩:
