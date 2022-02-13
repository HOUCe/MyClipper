# 静态网站不用购买服务器或虚拟空间阿里云OSS对象存储一键搞定_阿里云百科-CSDN博客

source:: https://blog.csdn.net/aliyunbaike/article/details/106047521

![](https://csdnimg.cn/release/blogv2/dist/pc/img/original.png)

[aliyunbaike](https://blog.csdn.net/aliyunbaike "垃圾中文技术性网站") 2020-05-11 09:45:22 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/articleReadEyes.png) 583 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/tobarCollect.png) 收藏  2 

版权声明：本文为博主原创文章，遵循 [CC 4.0 BY-SA](http://creativecommons.org/licenses/by-sa/4.0/) 版权协议，转载请附上原文出处链接和本声明。

静态网站不需要购买[云服务器](https://so.csdn.net/so/search?q=%E4%BA%91%E6%9C%8D%E5%8A%A1%E5%99%A8)、VPS或者虚拟主机，使用阿里云OSS对象存储即可搞定，稳定且访问速度快，你只需要准备好域名和静态网页代码，如果选择中国香港的区域还不需要备案，码笔记分享使用阿里云OSS对象存储实现静态网站托管教程：

## 阿里云OSS对象存储托管静态网站

[使用OSS托管静态网站](http://www.mabiji.com/aliyun/ossjingtai.html)的方法很简单，大致的流程是在OSS上创建Bucket，Bucket读写权限设置为公共读；创建完Bucket后，在基础设置中设置一下静态页面默认首页；然后将域名通过CNAME解析到外网访问的Bucket域名上。码笔记来详细说下教程：

### 一：创建OSS Bucket

-   1\. 打开OSS对象存储控制台；
-   2\. 创建Bucket

在左侧栏“Bucket列表”中点击“创建 Bucket”

![OSS对象存储创建Bucket](https://img-blog.csdnimg.cn/20200511094119630.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsaXl1bmJhaWtl,size_16,color_FFFFFF,t_70)

OSS对象存储创建Bucket

> Bucket 名称：Bucket名称会出现在你的OSS域名中，名称不支持中文  
> 区域：选择中国大陆地域你的网站域名需要备案，没有备案或不想备案可以选择中国（香港）节点  
> 存储类型：默认标准存储  
> 读写权限：这一步很重要，选择公共读

其他如版本控制、服务器端加密、实时日志查询、定时备份等选项根据实际情况选择，没有特殊要求默认即可。

### 二：设置静态页面

选择刚刚创建的Bucket，点击“基础设置”--“静态页面”，如下图：

![OSS Bucket静态页面设置](https://img-blog.csdnimg.cn/20200511094150218.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsaXl1bmJhaWtl,size_16,color_FFFFFF,t_70)

OSS Bucket静态页面设置

> 默认首页：index.html  
> 默认 404 页：404.html  
> 子目录首页：开通

至此OSS对象存储Bucket的创建和设置就完成了，下一步就是上传静态网站源码。

### 三：上传静态网站源码到Bucket

在“文件管理”中上传网页、新建目录等操作，如下图：

![上传静态网页到Bucket](https://img-blog.csdnimg.cn/20200511094212608.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsaXl1bmJhaWtl,size_16,color_FFFFFF,t_70)

上传静态网页到Bucket

  
根据静态网站目录结构，将静态源码上传到刚刚创建的Bucket中。

### 四：OSS Bucket绑定域名

选择“传输设置”--“域名管理”--“绑定用户域名”，填写你的网站域名，如果你的域名也在阿里云账号下，可以打开“自动添加 CNAME 记录”，阿里云域名解析系统会自动添加CNAME解析记录；如果域名不在阿里云，登录到域名解析平台，手动添加CANME记录即可。

### 五：手动添加域名CNAME记录

域名解析处添加CNAME解析到Bucket外网域名，登录到你的域名管理控制台，添加CNAME解析，记录值填写Bucket“概览”中外网访问的Bucket 域名，如下图所示：

![域名解析CNAME记录值](https://img-blog.csdnimg.cn/20200511094238899.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FsaXl1bmJhaWtl,size_16,color_FFFFFF,t_70)

域名解析CNAME记录值

填写CNAME记录值后，访问你的网站域名，应该可以正常访问了。

阿里云OSS对象存储价格很便宜，可是使用按量计费模式，后付费，费用会从阿里云账号下扣除，也可以通过购买OSS资源存储包来抵扣，本文转自码笔记，也可以参考OSS官方文档说明：[设置静态网站托管 - 阿里云](https://help.aliyun.com/document_detail/31899.html?source=5176.11533457&userCode=r3yteowb&type=copy)，文档中有详细说明。

其他云厂商也也有静态网站托管服务，阿里云OSS比较好的一点是可以选择中国香港地域，域名不需要备案，而且稳定访问速度也快。
