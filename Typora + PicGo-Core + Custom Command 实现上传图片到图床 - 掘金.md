# [Typora + PicGo-Core + Custom Command 实现上传图片到图床 - 掘金](https://juejin.cn/post/6855469849637093390)

**教程参考**

[Typora+PicGo-Core(command line)+Gitee实现图片上传到图床](https://link.juejin.cn/?target=https%3A%2F%2Fwww.cnblogs.com%2FIT-cute%2Fp%2F13122302.html "https://www.cnblogs.com/IT-cute/p/13122302.html") 主要借鉴 `picgo 操作命令`

[Typora + PicGo + Gitee 实现图片自动上传到图床](https://link.juejin.cn/?target=https%3A%2F%2Fwww.cnblogs.com%2Fzui-ai-java%2Fp%2F13371348.html "https://www.cnblogs.com/zui-ai-java/p/13371348.html") 主要借鉴 `gitee 图床的搭建`

[使用 Gitee 搭建自己的图床](https://link.juejin.cn/?target=https%3A%2F%2Fblog.csdn.net%2Fu010654995%2Farticle%2Fdetails%2F88383995 "https://blog.csdn.net/u010654995/article/details/88383995") 主要借鉴 `Gitee Pages 服务的开启`

## 1\. node 环境准备

请自行百度解决...

## 2\. 使用 node 安装 PicGo-Core

```
// npm 命令执行速度过慢的话，我们可以使用一下淘宝的镜像 
npm install -g picgo --registry=https://registry.npm.taobao.org

// 安装完成以后测试一下是否安装成功
picgo -v
复制代码
```

## 3\. 使用 picgo 命令安装 gitee-uploader 插件

```
picgo install gitee-uploader
复制代码
```

## 4\. 使用 picgo 命令设置 uploader

```
C:\Users\Run\Desktop>picgo set uploader
? Choose a(n) uploader (Use arrow keys)
❯ gitee
  smms
  tcyun
  github
  qiniu
  imgur
  aliyun
  upyun
(Move up and down to reveal more choices)

? Choose a(n) uploader gitee
? repo: xxxx/image
? branch: master
? token: 5a34fa3f348d556...
? path: 2020
? customPath: 年月
? customUrl: https://gitee.com/xxxx/image/raw/master/
[PicGo SUCCESS]: Configure config successfully!
复制代码
```

## 5\. 配置 Typro 上传服务设定

**重点是 `自定义命令` 的组成部分：** `[your node path] [your picgo path] upload`

键

值

上传服务

Custom Command

自定义命令

D:\\nodejs\\node.exe D:\\nodejs\\node\_global\\node\_modules\\picgo\\bin\\picgo upload

**注意：配置完成后可以点击 `验证图片上传选项` 来测试是否配置成功**

## 6\. 完整的配置文件

**以下是参照 [PicGo-Core官方文档](https://link.juejin.cn/?target=https%3A%2F%2Fpicgo.github.io%2FPicGo-Core-Doc%2Fzh%2Fguide%2Fconfig.html "https://picgo.github.io/PicGo-Core-Doc/zh/guide/config.html") 的进行的配置**

首先我们需要找到我们的配置文件，**picgo** 的默认配置文件在不同系统的目录不太一样： linux 和 macOS 均为 `~/.picgo/config.json` windows 则为 `C:\Users\{你的用户名}\.picgo\config.json`

```
{
  "picBed": {
    "current": "gitee",
    "gitee": {
      "repo": "xxxx/image",
      "branch": "master",
      "token": "5a34fa3f348d556...",
      "path": "2020",
      "customPath": "yearMonth",
      "customUrl": "https://gitee.com/xxxx/image/raw/master/"
    },
    "uploader": "gitee",
    "transformer": "path"
  },
  "picgoPlugins": {
    "picgo-plugin-gitee-uploader": true
  },
  "picgo-plugin-gitee-uploader": {
    "lastSync": "2020-07-30 10:29:26"
  }
}
复制代码
```

## 7\. 解决 `文件大于1M，登录后可见` 的问题

> 按照步骤 1-6 我们确实成功地配置了一个免费好用的 `Gitee图床`，简单使用也没有什么问题。可是当我们上传的图片大小超过 1M 后：OMG，图片无法正常显示，在浏览器中打开图片的地址，直接跳转到 Gitee 登录界面，并且出现出现了很扎心的 `文件大于1M，登录后可见` 文字的提示。关键是这个文件大小限制还没有办法解决，凉凉！！！

**凉凉？不存在的！** 俗话说：办法总比困难多。我们访问 git 仓库中文件的方式并不是只有一种，更何况它只是一些`静态的资源`文件。所以是不是只要我们想办法配置一个简单的HTTP服务就可以了。问题迎刃而解：Gitee 官方给我们提供了一种供博客 / 门户 / 开源项目网站 / 开源项目静态效果演示用途的 `Git Pages`服务。

### 7.1 开启 Git Pages 服务

1.  进入到阁下 Gitee 图床 所在仓库的页面，找到 `服务` -> `Gitee Pages`

![开启 Git Pages 服务](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/114657378eb840218035638b34e62878~tplv-k3u1fbpfcp-watermark.awebp)

2.  无需修改任何配置。直接点击 `启动`按钮，等待服务启动完毕即可。

![image](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9c235fd884db47d29f88006328b6fe3d~tplv-k3u1fbpfcp-watermark.awebp)

### 7.2 更新图片访问的路径

**当我们的 图床仓库 开启 Git Pages 服务后，就会得到一个专属的网站地址，格式为：“ `个人空间地址`.gitee.io/`仓库名`”** 。

例如：`http://zi1.gitee.io/pic`，则我们访问该图床中的静态资源文件的路径为 `http://zi1.gitee.io/pic` + _仓库中文件的可见路径_。

比如：你的仓库中的 picture 目录下的 1.jpg 的图片文件： `picture/1.jpg`，则我们访问该图片的路径为：`http://zi1.gitee.io/pic/picture/1.jpg`

![image](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5d637191a0ff40b18e96fd90d38de98d~tplv-k3u1fbpfcp-watermark.awebp)

## 8\. 开启 Git Pages 后完整的配置文件

```
{
  "picBed": {
    "current": "gitee",
    "gitee": {
      "repo": "xxxx/image",
      "branch": "master",
      "token": "5a34fa3f348d556...",
      "path": "2020",
      "customPath": "yearMonth",
      "customUrl": "https://xxxx.gitee.io/image/"
    },
    "uploader": "gitee",
    "transformer": "path"
  },
  "picgoPlugins": {
    "picgo-plugin-gitee-uploader": true
  },
  "picgo-plugin-gitee-uploader": {
    "lastSync": "2020-10-12 09:23:39"
  }
}
复制代码
```
