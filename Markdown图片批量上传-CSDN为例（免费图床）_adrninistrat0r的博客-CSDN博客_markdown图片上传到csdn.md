# [Markdown图片批量上传-CSDN为例（免费图床）_adrninistrat0r的博客-CSDN博客_markdown图片上传到csdn](https://blog.csdn.net/a82514921/article/details/104685564)

![](https://csdnimg.cn/release/blogv2/dist/pc/img/original.png)

[adrninistrat0r](https://blog.csdn.net/a82514921 "垃圾中文技术性网站") 2020-03-05 22:07:06 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/articleReadEyes.png) 995 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/tobarCollect.png) 收藏  1 

版权声明：本文为博主原创文章，遵循 [CC 4.0 BY-SA](http://creativecommons.org/licenses/by-sa/4.0/) 版权协议，转载请附上原文出处链接和本声明。

在将Markdown格式的文档导入博客或其他网站时，本地图片无法直接导入，需要使用互联网可以访问的图片。

通过以下方法，可以使用Markdown文档中引用的图片批量上传至互联网。

可以使用GitHub+jsDelivr作为免费图床，先将图片上传至GitHub的公共项目，再使用jsDelivr的CDN链接访问图片，最后替换Markdown引用的图片地址。

在GitHub中创建一个Public仓库，如下所示。

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL0Fkcm5pbmlzdHJhdG9yL3RtcF91cGxvYWRAMi9NYXJrZG93biVFNSU5QiVCRSVFNyU4OSU4NyVFNiU4OSVCOSVFOSU4NyU4RiVFNCVCOCU4QSVFNCVCQyVBMC1DU0ROJUU0JUI4JUJBJUU0JUJFJThCJUVGJUJDJTg4JUU1JTg1JThEJUU4JUI0JUI5JUU1JTlCJUJFJUU1JUJBJThBJUVGJUJDJTg5L3BpYy9hMDEuanBn?x-oss-process=image/format,png)

将创建的仓库克隆到本地，可使用TortoiseGit作为Git客户端，可参考 “TortoiseGit IDEA等配置Git服务器SSH密钥” [https://blog.csdn.net/a82514921/article/details/104661707](https://blog.csdn.net/a82514921/article/details/104661707 "垃圾中文技术性网站") 。

在本地将Markdown中引用的图片按目录保存好，提交至GitHub中，提交完毕后如下所示。

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL0Fkcm5pbmlzdHJhdG9yL3RtcF91cGxvYWRAMi9NYXJrZG93biVFNSU5QiVCRSVFNyU4OSU4NyVFNiU4OSVCOSVFOSU4NyU4RiVFNCVCOCU4QSVFNCVCQyVBMC1DU0ROJUU0JUI4JUJBJUU0JUJFJThCJUVGJUJDJTg4JUU1JTg1JThEJUU4JUI0JUI5JUU1JTlCJUJFJUU1JUJBJThBJUVGJUJDJTg5L3BpYy9hMDIuanBn?x-oss-process=image/format,png)

在GitHub中对上述仓库创建release，如下所示。

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL0Fkcm5pbmlzdHJhdG9yL3RtcF91cGxvYWRAMi9NYXJrZG93biVFNSU5QiVCRSVFNyU4OSU4NyVFNiU4OSVCOSVFOSU4NyU4RiVFNCVCOCU4QSVFNCVCQyVBMC1DU0ROJUU0JUI4JUJBJUU0JUJFJThCJUVGJUJDJTg4JUU1JTg1JThEJUU4JUI0JUI5JUU1JTlCJUJFJUU1JUJBJThBJUVGJUJDJTg5L3BpYy9hMDMuanBn?x-oss-process=image/format,png)

没有创建release时的界面：

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL0Fkcm5pbmlzdHJhdG9yL3RtcF91cGxvYWRAMi9NYXJrZG93biVFNSU5QiVCRSVFNyU4OSU4NyVFNiU4OSVCOSVFOSU4NyU4RiVFNCVCOCU4QSVFNCVCQyVBMC1DU0ROJUU0JUI4JUJBJUU0JUJFJThCJUVGJUJDJTg4JUU1JTg1JThEJUU4JUI0JUI5JUU1JTlCJUJFJUU1JUJBJThBJUVGJUJDJTg5L3BpYy9hMDQuanBn?x-oss-process=image/format,png)

已经创建过release时的界面：

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL0Fkcm5pbmlzdHJhdG9yL3RtcF91cGxvYWRAMi9NYXJrZG93biVFNSU5QiVCRSVFNyU4OSU4NyVFNiU4OSVCOSVFOSU4NyU4RiVFNCVCOCU4QSVFNCVCQyVBMC1DU0ROJUU0JUI4JUJBJUU0JUJFJThCJUVGJUJDJTg4JUU1JTg1JThEJUU4JUI0JUI5JUU1JTlCJUJFJUU1JUJBJThBJUVGJUJDJTg5L3BpYy9hMDUuanBn?x-oss-process=image/format,png)

指定版本号并发布，当提交了新的内容后，需要使用新的版本号（或删除仓库后创建同名仓库继续使用旧的版本号）。

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL0Fkcm5pbmlzdHJhdG9yL3RtcF91cGxvYWRAMi9NYXJrZG93biVFNSU5QiVCRSVFNyU4OSU4NyVFNiU4OSVCOSVFOSU4NyU4RiVFNCVCOCU4QSVFNCVCQyVBMC1DU0ROJUU0JUI4JUJBJUU0JUJFJThCJUVGJUJDJTg4JUU1JTg1JThEJUU4JUI0JUI5JUU1JTlCJUJFJUU1JUJBJThBJUVGJUJDJTg5L3BpYy9hMDYuanBn?x-oss-process=image/format,png)

查看 https://www.jsdelivr.com/?docs=gh ，通过jsDelivr访问GitHub资源的URL格式如下：

```
// load any GitHub release, commit, or branch
// note: we recommend using npm for projects that support it
https://cdn.jsdelivr.net/gh/user/repo@version/file
```

如果直接访问 https://cdn.jsdelivr.net/gh/ ，会提示 “ Invalid URL. The URL structure is /gh/user/repo@version/file.js ”。

以上URL需要进行以下替换：

参数

说明

https://cdn.jsdelivr.net/gh

固定URL

user

GitHub用户名

repo

GitHub仓库名

version

GitHub仓库发布的release版本号

file

GitHub仓库中的文件路径

可以打开 https://cdn.jsdelivr.net/gh/user/repo@version/ 链接（最后的“/”不能省略），会显示当前relase对应的文件信息，示例如下 。

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL0Fkcm5pbmlzdHJhdG9yL3RtcF91cGxvYWRAMi9NYXJrZG93biVFNSU5QiVCRSVFNyU4OSU4NyVFNiU4OSVCOSVFOSU4NyU4RiVFNCVCOCU4QSVFNCVCQyVBMC1DU0ROJUU0JUI4JUJBJUU0JUJFJThCJUVGJUJDJTg4JUU1JTg1JThEJUU4JUI0JUI5JUU1JTlCJUJFJUU1JUJBJThBJUVGJUJDJTg5L3BpYy9hMDcuanBn?x-oss-process=image/format,png)

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL0Fkcm5pbmlzdHJhdG9yL3RtcF91cGxvYWRAMi9NYXJrZG93biVFNSU5QiVCRSVFNyU4OSU4NyVFNiU4OSVCOSVFOSU4NyU4RiVFNCVCOCU4QSVFNCVCQyVBMC1DU0ROJUU0JUI4JUJBJUU0JUJFJThCJUVGJUJDJTg4JUU1JTg1JThEJUU4JUI0JUI5JUU1JTlCJUJFJUU1JUJBJThBJUVGJUJDJTg5L3BpYy9hMDguanBn?x-oss-process=image/format,png)

如果无法正常访问，需要等待一段时间，使jsDelivr对应GitHub的CDN链接可用。

假如在Markdown中通过“\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-YD7uYOUf-1583417166969)(pic/xx.jpg)\]”的形式引用图片，可将目录名替换为jsDelivr的CDN中对应目录的地址。

完成以上操作后，可在CSDN等博客，或其他网站上传Markdown文件，其中引用的图片会被批量获取。

也可当作免费图床使用。

若直接使用GitHub中的仓库的资源文件的raw形式，在CSDN中上传时，CSDN后台服务器无法获取到图片内容。

不直接使用GitHub或Gitee

若直接使用GitHub中的仓库的资源文件的raw形式，在CSDN中上传时，CSDN后台服务器无法获取到图片内容。

使用Gitee中的仓库的资源文件的raw形式，在CSDN中上传时，CSDN后台服务器可以获取到图片内容，但存在使用频率限制。
