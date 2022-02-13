# 图片素材管理软件：Eagle VS Billfish - 知乎

source:: https://zhuanlan.zhihu.com/p/412280288

## **一、我的观点：**

> **_不吹不黑，用数据和事实说话，各产品都有其优劣，没有完美的。请大家按需选择。_**

中秋放假因为疫情不能乱跑，宅在家实在无聊，码点字陶冶一下情操。本文主要对比Eagle和另外一个免费产品Billfish，这两个产品我使用了一年多，评价一下这两个产品的一些区别和这期间的进展。本文将介绍本人亲测使用这两个软件的一些心得，希望提供给各位一些参考。

## 二、内容声明：

不接受反驳特别是错别字，_eagle本人一直在付费使用，主要用来整理电脑图片。billfish是去年6月份才出现的软件，本文是深度体验研究后进行对比的总结_

。我的eagle购买记录

![](https://pic4.zhimg.com/v2-d07c62e2622d1005f7f864682c99bc27_b.jpg)

## 一、EAGLE对比Billfish

## 1、价格费用对比

billfish

eagle

免费

199元

## 2、软件技术方案对比

eagle

billfish

Electron+json

C++&Qt+SQLit

-   Electron属于前端开发技术，上手容易。JSON一般是用来做数据传输，判断eagle使用JSON作为数据库主要是考虑第三方软件文件同步的便利性，使用Electron可以在界面方面有优势。
-   C++&QT开发难度高，对性能要求高的产品通常使用这套方案，如网易云音乐，WPS等。SQLite是关系数据库，技术成熟，可以处理非常复杂的数据关系，可管理大批量的数据。判断billfish使用这套技术方案可能主要以性能为主，方便后续tob业务。

### 3、文件存储结构对比

billfish

eagle

非侵占式：和资源管理器保持一致，且保持同步更改。存储方式可以选择备份，剪切，索引。脱离软件后，文件夹结构信息仍然保留。

侵占式：只能使用其特有存储方式，存储方式为完全复制，脱离软件后，文件结构信息不保留，素材库无法继续使用。

优势：文件结构清晰，灵活度高

优势：无数据库,文件信息写入json可直接读取

劣势：使用同步工具同步时，必须关闭billfish软件。

劣势：产生更多额外的碎片文件，素材库资源结构不清醒，绑定用户

> 原本有录制一个视频，但是没有审核通过！！！

![](https://pic1.zhimg.com/v2-f15cc6e5c47808cf858654e12e722fdc_b.gif)

## 4、导入性能对比

实测对比，电脑配置如下：

![](https://pic4.zhimg.com/v2-8e21d2f239dd41d956e257078a5f6bbf_b.jpg)

**测试结果如下：**

导入文件

eagle

billfish

导入 1.8G，360个素材文件

16:41-17:27 = 46分钟

17:28-17:34 = 6分钟

### 5、支持格式对比

> 两者目前差异支持的格式已在行尾>>> 显示，音视频支持格式billfish强于eagle，源文件和照片eagle强于billfish，**_但是billfish支持了自定义格式文件的管理_**

类型

eagle支持90种

billfish支持61种

图片

bmp, eps, gif, heic, ico, jpeg, jpg, png, svg, tif, tiff, webp >>> base64,icns,

jpg,jpeg,png,bmp,eps,heic,ico,svg,tif,tiff,webp,gif

3D

dds, exr, hdr, tga >>> blend,fbx, obj,

dds,tga,hdr,exr,

音频

aac, flac, m4a, mp3, ogg, wav

mp3,wav,flac,acc,aiff,ogg >>> amr,ape,m4a

视频

m4v, mp4, webm, wmv, avi, mov

avi,wmv,mov,mp4,webm,m4v >>> rm,rmvb,trp,ts,vob,mpg,mkv,3g2p,asf,dv,f4v,m2ts,mxf,ogv,mt2s,flv

字体

ttf, otf, ttc, woff

ttf,otf,ttc

文档

pdf, ppt, pptx, xls, xlsx, doc, docx >>> eddx, emmx,txt,potx,

pdf,ppt,pptx,doc,docx,xls,xlsx

源文件

ai, cdr, dwg, psb, psd >>> skp, xd, xmind,afdesign\*3,c4d,clip,graffle, idml, indd, indt,

swf,ai,psd,psb,cdr

raw

3fr, arw, cr2, cr3, crw, dng, erf, mrw, nef, nrw, orf, otf, pef, raf, raw, rw2, sr2, srw, x3f

N/A

自定义格式

不支持

支持

## 6、浏览器采集对比

见视频实测对比，插件收藏功能差异不大，录制了两者批量导入花瓣画板的对比视频

eagle

billfish

14:26:47-14:28:34=1分57秒

14:28:34-14:29:39=1分5秒

### 7、界面体验对比

关于美观相对这种比较主观，发表一下个人看法billfish的精细程度不如eagle，还有很多界面细节需要调整。billfish皮肤目前只有黑色但是可以调整透明度，eagle有7套皮肤不能调整透明度。

### 8、浏览器插件对比

支持浏览器

eagle

billfish

chrome

✔

✔

edge

✔

✔

QQ

✔

✔

360

✔

✔

firefox

✔

×

搜狗

×

✔

遨游

×

✔

Safari

✔

×

## 三、关于硬盘炸弹

为什么要写这篇文章，之前看到eagle是硬盘炸弹的言论，加之自己是eagle的用户，想要搞清楚这个问题到底是什么问题，于是乎做了仔细的研究。eagle的管理方式：截取eagle官方的描述，看得出来作者主要是为同步工具考虑。

![](https://pic4.zhimg.com/v2-9d0d5d2682a8713d8bcab47cf237b55b_b.jpg)

放弃了成熟的本地数据库方案，那么就会必然衍生另外一个核心问题，注意截图内容中提到的0.5-1KB的文件。eagle 的存储方式是一张图（文件）会生成一个自定义文件夹名称的文件夹，其中存储如下图的三个信息：

![](https://pic3.zhimg.com/v2-7fc43b77d996b804c641c42f6f8e90a2_b.jpg)

至于是不是硬盘炸弹先不予置评，但是有些共识想必大家都需要清楚，提供给大家参考判断：

-   用过硬盘测速的朋友应该知道，小于4k的文件读写是速度最慢的，那么如此大量的小文件的读写，势必会造成更大的磁盘压力。

![](https://pic2.zhimg.com/v2-2b1833ee5e2822201847cfe05be6afc9_b.jpg)

-   大量的小文件将占用更多的空间，这里分享一下硬盘存储的原理：硬盘格式有NTFS、 FAT 或 exFAT等，就以常见的NTFS格式为例，NTFS 的簇大小为 4KB。大家可以理解为簇就是磁盘的一系列最小单位的容器，文件存储时磁盘将文件放入容器。有两种情况，大于4kb和小于4kb的情况，大于4kb时会选择连续的存储区间，然后将剩余部分放入最后一个簇，就会导致始终会有剩余，这就是大家常常遇见的，明明文件大小比如是50MB占用空间却大于50MB。那么大量的小于4kb的文件需要存储时，每个文件就会分别单独占用一个簇，导致存储空间的浪费。文字对照下图说明：

![](https://pic2.zhimg.com/v2-3438cc380dd6d00c59a2efd9dd734e11_b.jpg)

## 总结：

_billfish在这发布后的一年中，进展很快，奋起直追eagle，大有逐步赶超之势。目前虽有部分不足，但仍可见其在不断的完善，其升级迭代进度比较快，目前见官网更新频率是周更新。如果其研发的智能化的成果能够在billfish稳定后接入的话，相信对于eagle来说是不小的挑战。对于用户来说，喜闻乐见有更优秀的产品能够使用。_

_eagle在Electron+json的框架下，技术提升空间有限，但好在有先发优势，占领了市场先机，如果能够持续服务好已获得的用户群，市场空间也不小。_
