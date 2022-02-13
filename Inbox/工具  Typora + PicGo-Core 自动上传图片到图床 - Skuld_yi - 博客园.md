# [工具 | Typora + PicGo-Core 自动上传图片到图床 - Skuld_yi - 博客园](https://www.cnblogs.com/skuld-yi/p/14533794.html)

## 0 前言[#](https://www.cnblogs.com/skuld-yi/p/14533794.html#0-%E5%89%8D%E8%A8%80)

Markdown 是现在十分流行的标记式语言，在博客等很多场景中应用十分广泛。众所周知，Markdown 中的图片是以链接的形式存在的，不像 Word 等传统文本编辑器直接把图片嵌入文档中。传统的做法是在写作过程中，先将所需图片上传至在线图床，然后将生成的链接插入文档中；或者先将图片保存在本地，再插入在磁盘中的路径（只能本地查看图片）。这两种方法显然都比较麻烦，体验欠佳。

[Typora](https://typora.io/) 在较早的版本中就支持了从剪贴板直接将图片粘贴至文档中，图片被将自动保存到指定的目录中。在本地写作的场景中（只在本地查看、或写完导出为PDF格式分享），插入图片的体验已经明显提升。但对于需要直接在线分享 md 的场景（例如本地写完粘贴到在线的博客 md 编辑器），本地图片链接就不起作用了，依然需要手动上传图片后再更新图片链接。

但在版本 `≥ 0.9.9.32 on macOS or 0.9.84 on Windows / Linux` 后，Typora 集成了通过第三方工具自动上传图片的功能。只需进行5分钟的设置，就能极大提升 md 写作体验。下面分享配置过程。

> 版本信息：
> 
>  Typora: 0.9.96
> 
>  PicGo-Core: 1.4.18

## 1 PicGo-Core[#](https://www.cnblogs.com/skuld-yi/p/14533794.html#1-picgo-core)

### 安装[#](https://www.cnblogs.com/skuld-yi/p/14533794.html#%E5%AE%89%E8%A3%85)

[PicGo-Core](https://picgo.github.io/PicGo-Core-Doc/zh/guide/) 是一个开源的命令行图片上传工具。虽然可以在 Typora 的设置中一键下载安装 PicGo-Core，但它是从 Typora 自己的 fork 库中安装的，并不是 PicGo-Core 的官方开源库；因此版本通常较为老旧，存在各种没有必要的 bug、也无法使用更新的 feature。

基于以上理由，强烈建议直接在本地自己安装 PicGo-Core。安装本身也很简单：

```
# 需要 Node.js 版本 >= 8
npm install picgo -g # 或 yarn global add picgo
```

### 配置[#](https://www.cnblogs.com/skuld-yi/p/14533794.html#%E9%85%8D%E7%BD%AE)

安装成功后，可以使用交互式命令行方便地进行配置并自动生成配置文件，无需手动生成和复制粘贴（有的终端支持不太好，可以多换几个试试）。配置好图床后使用 `picgo use uploader` 选择当前要使用的 `Uploader` 。

```
$ picgo set uploader
? Choose a(n) uploader (Use arrow keys)
  smms
❯ tcyun
  github
  qiniu
  imgur
  aliyun
  upyun
```

我使用的是默认的 SM.MS 图床，选择 `smms` 选项后要求输入 api token。在注册登录 SM.MS 后，可以在[个人中心](https://sm.ms/home/apitoken)获取到自己的 token，输入即可完成配置；其他图床也有对应的交互配置选项。

如果由于种种原因无法通过交互命令行完成配置，也可以手动生成配置文件。这里以默认的smms图床为例，更多图床的配置方法详见[配置文档](https://picgo.github.io/PicGo-Core-Doc/zh/guide/config.html#picbed)。

> picgo 的默认配置文件为`~/.picgo/config.json`。其中`~`为用户目录。不同系统的用户目录不太一样。
> 
> linux 和 macOS 均为`~/.picgo/config.json`。
> 
> windows 则为`C:\Users\你的用户名\.picgo\config.json`。

```
{
  "picBed": {
    "uploader": "smms", 
    "smms": {
      "token": "" 
    }
  },
  "picgoPlugins": {} 
}
```

### 测试[#](https://www.cnblogs.com/skuld-yi/p/14533794.html#%E6%B5%8B%E8%AF%95)

完成安装和配置后，就可以通过命令行上传图片到图床了。如果执行命令后返回了图床的URL，则说明配置成功。

```
# 上传具体路径图片
picgo upload /xxx/xxx.jpg

# 上传剪贴板里的第一张图片（上传时会将格式转成png）
picgo upload
```

[![image-20210314185507394](https://i.loli.net/2021/03/14/O1NLATjikolIusJ.png)](https://i.loli.net/2021/03/14/O1NLATjikolIusJ.png)

## 2 Typora 设置[#](https://www.cnblogs.com/skuld-yi/p/14533794.html#2-typora-%E8%AE%BE%E7%BD%AE)

配置好 PicGo-Core 后，在 Typora 的偏好设置-图像中进行设置。

### 上传服务设定[#](https://www.cnblogs.com/skuld-yi/p/14533794.html#%E4%B8%8A%E4%BC%A0%E6%9C%8D%E5%8A%A1%E8%AE%BE%E5%AE%9A)

由于我们自己安装了 PicGo-Core，没有使用它自己集成的，所以上传服务选择 `Custom Command` （自定义命令），命令使用 `picgo u` 即可。

[![image-20210314185815826](https://i.loli.net/2021/03/14/GWp93LNXmUenTJh.png)](https://i.loli.net/2021/03/14/GWp93LNXmUenTJh.png)

设置后点击下方的“验证图片上传选项”，他会自动上传两张图片测试图片上传服务。如果出现如下结果，说明配置成功。

[![image-20210314190322698](https://i.loli.net/2021/03/14/DfbLNXPZ4omTWS3.png)](https://i.loli.net/2021/03/14/DfbLNXPZ4omTWS3.png)

### 插入图片设置[#](https://www.cnblogs.com/skuld-yi/p/14533794.html#%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87%E8%AE%BE%E7%BD%AE)

[![image-20210314190940618](https://i.loli.net/2021/03/14/7EiVre23BR1KZSk.png)](https://i.loli.net/2021/03/14/7EiVre23BR1KZSk.png)

如果想在插入图片时直接上传，可以按照如上选项设置。然而这样设置存在一个问题，插入图片后需要等待上传完成返回URL才能在文档中看到图片的预览，感觉不够流畅。并且如果反复多次替换图片，图床中会出现很多冗余图片，造成空间浪费。

### 批量上传图片[#](https://www.cnblogs.com/skuld-yi/p/14533794.html#%E6%89%B9%E9%87%8F%E4%B8%8A%E4%BC%A0%E5%9B%BE%E7%89%87)

由于上述原因，个人建议在插入图片时先保存到本地以实现流畅的即时预览，在完成文章后统一批量上传文章中用到的所有本地图片。

在菜单中选择“格式-图像-上传所有本地图片（Format-Image-Upload All Local Images）即可完成批量上传。

## 结语&参考文献[#](https://www.cnblogs.com/skuld-yi/p/14533794.html#%E7%BB%93%E8%AF%AD%E5%8F%82%E8%80%83%E6%96%87%E7%8C%AE)

以上是我自己配置 Typora 图片上传的过程，希望能对你有所帮助。文中考虑的情况不够全面，可能存在疏漏，欢迎留言指正讨论！

参考文献：

[Typora support](https://support.typora.io/Upload-Image/)

[PicGo-Core 官方文档](https://picgo.github.io/PicGo-Core-Doc/zh/guide/)
