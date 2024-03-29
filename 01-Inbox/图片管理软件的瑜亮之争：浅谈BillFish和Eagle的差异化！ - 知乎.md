# 图片管理软件的瑜亮之争：浅谈BillFish和Eagle的差异化！ - 知乎

source:: https://zhuanlan.zhihu.com/p/151061991

这篇文章是我最近写的第N篇关于图片管理工具的介绍了，「Eagle」、「花瓣pro」，甚至是「天翼云盘」，期间也测试过了很多软件「Picasa」、「Bridge」、「digiKam」、「XnView」，作为个人使用习惯来说，只能说谁的功能更方便，谁的UI更舒服，谁的架构更略胜一筹。

本人非专业的后端开发人员精通架构，也非设计师有收集素材癖，不是摄影人员专注于后期，更非某个图片管理软件的水军，我只是需要一个轻量级的相片管理工具，多年来一直想找到一个图片管理工具，来解决历年来使用手机拍摄的照片、视频进行一个分类管理，但是很难一个软件可以解决所有的痛点，目前只能人工按时间进行排序建立文件夹：年份/设备名\_年-月-日\_年-月-日\_地点\_做了什么

![](https://pic3.zhimg.com/v2-11d1a6aaf845500de4f358f2fe40321a_b.jpg)

其实使用过的大部分还是有本质性的差别的，例如

_花瓣Pro：定位的更偏向社交属性，在线维护好个人画板，形成自己的素材库。_

_天翼云盘_

：定位是向个人及家庭用户推出的云存储服务产品，只是我个人将相册单独使用，实现了_时间分类、人物分类、地点分类、智能分类_，所以我也倾向于认为是照片管理工具，非大家常用的素材管理工具。

_Picasa_

：产品是好产品，特别是_人脸识别功能，另外自动查找电脑上的所有图片，甚至是那些已经遗忘的图片，找出的照片会按日期顺序放在可见的相册中，同时以您易于识别的名称命名文件夹，可惜随着谷歌的战略调整，被放弃了_，Google photos需要科学上网，甚至后续推出的离线图片管理工具 Gallery Go也只支持移动端。

_Digikam_

：在B乎上被吹上天了，带着试试看的心理，下载安装一气呵成，但是并没有想象中那么好用，甚至在操作体验比Eagel稍逊一筹。

_Bridge_

：作为摄影师后期软件处理，和Adobe软件的协同，达到资源管理互通，作为图片管理工具来说，管理功能上还是欠缺。

_XnView_

：第一次运行后差点被界面劝退，在使用了3个小时候，发现功能并没有亮点，真真正正被劝退。

_Eagle_

：是第一个让我眼前一亮的软件，无论从主题界面，操作功能，支持格式，还是其他方面都满足了预期，直接入手了付费版本，Eagle在单机图片管理软件领域也算是几无对手，但是_基于Electron+json开发模式还是有些担心硬盘的问题。_

今天重点介绍的就是_BillFish_

这个软件，理论上是今年6月份新推的，很是关注就简单与Eagle评测下

## _1、名字_

Eagle：不知道官方命名是什么意思，我的理解是「鹰」，被誉为飞禽之王，体态雄伟，性情凶猛。

BillFish：不知道官方命名是什么意思，我的理解是「旗鱼」，被誉为海洋杀手，海洋中一种大型的凶猛食肉鱼类，速度最快的海洋动物。

## _2、架构_  

_Eagle：Electron+json_

_BillFish：Qt+SQLit_

关于架构是个人更为关注的，架构决定了软件所能容纳数据的量级。

_SQLite是关系数据库，数据间有复杂的关系，操作需要用到复杂的增删改查。_

_JSON是「数据交互语言」，一种数据交换常用的格式，不是数据存储与处理用的格式。_

JSON可以作为数据仓库/数据库里边的一种存储格式，通常用来数据传输，短时间存储一些大小的数据，或者作为config文件，但是JSON本身无法作为数据库来使用，或者说JSON作为数据库是有局限性的，JSON查找数据是将数据全部读进来，然后再进行查找，这种做法在数据量小的时候没有问题，如果存储数据非常庞大，每次查找数据，都需要将整个加载进去差，系统处理就不堪负重。而SQLite则占据优势，他可以根据程序提出的要求，直接在数据里实现筛选出需求的数据，这样就大大提高效率，也减轻了系统的负担。

如果读写数据量小的时候，JSON速度会快一些，因为SQLite开始需要建立db connection之类的额外开销，但数据量到一定规模的时候SQLite的存储引擎应该会带来明显的优势，SQLite的存储效率和查询效率都很高，并且所支持的聚合、索引、高可用等需求都是JSON所不具备的，更重要的是数据库的存在只是为了持久性和容灾。

所以JSON适合处理是少量数据，如果只是几千张图片，用json能应付得来，数据量大时的效率太差，如果是上万/十万以上就受不了了，载入时间太长。SQLite是关系数据库，数据间有较复杂关系的，操作需要用到复杂的增删改查，无疑使用 SQLite 是最好的方案。

## 3、启动时间

测试环境：

使用iphone的秒表工具计算，从点击图标至界面加载完毕，默认软件的资源为空。

![](https://pic4.zhimg.com/v2-f7c13a96ec6e6b520a535fdeda8891f3_b.jpg)

**Billfish**

6.20秒-6.80秒 （6-7秒以下）

![](https://pic3.zhimg.com/v2-3c31513cefda3cc9964023e0262c2fc2_b.jpg)

**Eagle**

4.90秒-4.92秒 （4-5秒以下）

从测试结果来看，Eagle在启动软件方面，确实具备优势。

## 4、导入素材时间

**测试环境**

导入一个文件件，包括jpg、png、mp4、move等文件，文件数量274个，文件总体积449MB。

![](https://pic4.zhimg.com/v2-9c7f5365b4fbdb3b214ea9f2a9d66cf7_b.jpg)

**Billfish**

15.03秒

**Eagle**

30.54秒

## _5、导入素材后启动时间_

**Billfish**

6.92秒

**Eagle**

4.97秒

## 5、内存占用

![](https://pic2.zhimg.com/v2-c2f7fce3a1f46b216f7959635361f0ed_b.jpeg)

Billfish：63.4MB

![](https://pic1.zhimg.com/v2-3dcc80cf22f81b01dd16f85823ab16b8_b.jpeg)

Eagel：303.6MB

## 6、文件存储结构  

![](https://pic1.zhimg.com/v2-54a48d1b3a046ca6509954223517b620_b.jpg)

![](https://pic4.zhimg.com/v2-4f56a6611b65a55063f0f5932643aa4b_b.jpg)

Billfish首次导入文件会提示复制、剪切、索引，索引的文件结构和资源管理器一致，这个功能可以节省硬盘的存储空间，对应素材更加灵活，不存在软件对用户使用的绑架行为。

选择索引模式后，原始图片位置发生变化会造成软件无法读取素材，尽量不移动文件。

当然复制和剪切两种模式则不存在此种弊端，软件会监听素材库文件夹内的文件调整，软件和资源管理器两边调整都会同步变更。

![](https://pic2.zhimg.com/v2-a024fed573e5251e8f43175d0ae87b21_b.jpg)

Eagele的文件夹路径为 F:\\图库名字\\图库系统.library\\images\\随机产生13位文件夹名.info\\原始文件+thumbnail.png+json文件

相对来说，webp格式文件的压缩率比例更由于PNG格式。

两款软件在存储结构上最大的差别是：

Eagle采用的是封闭式的管理方式，将素材导入Eagle，会把素材复制到它的素材库里，想要节省空间必须删掉源文件，否则会造成磁盘空间的浪费。后续不使用软件，整理好的素材结构也会不复存在。

在导入素材后，Eagle会对这个素材解析生成一个缩略图，和一个配置的json文件，所有的图片信息都会写入这个json文件后续搜索文件的时候，都需要去读写所有的json文件，查找缓慢。

Billfish采用的是非侵占式的管理模式，可以直接读取用户在资源管理器上整理好的文件夹结构，也可以对其他路径的文件夹进行索引，管理素材的同时还能最大限度的节约存储空间。

无论是通过软件还是在资源管理器中，进行素材的增删都不会相互影响，即便后续不使用Billfish，整理好的文件结构也不会发生变动。

另外，在文件检索方面，Billfish采用数据库来管理素材信息的模式，查询时会通过数据库查询，再根据数据库的查询结果，给你返回搜索结果，所以相比Eagle，Billfish的检索速度会更快。

## 7、支持格式  

**Billfish：**61种

图像: jpg,jpeg,png,bmp,svg,eps,dds,exr,hdr,heic,tga,ico,tif, tiff,webp,gif

源文件：ai,psd,psb,cdr,afdesign,afphoto,afpub,c4d,clip,dwg,fig,skt,skp,prd,graffle,idml,indd,indt,mindnode,pxd,sketch,xd,eddx,emmx

视频:mp4, avi,flv,mov,rmvb,mkv

音频 mp3, ogg, wav,:aac, flac

字体:ttf, otf,ttc

办公:xmind,potx,txt,ppt,pptx,doc,docx,xls,xlsx

![](https://pic2.zhimg.com/v2-98911409944492f8d77a4e1cff05d0f9_b.jpg)

**Eagle：**65 种（windows)

图像: bmp, dds, eps, exr, gif, hdr, heic, ico, jpeg, jpg, png, svg, tga, tif, tiff, ttf, webp, base64

源文件:afdesign(Affinity Designer), afphoto(Affinity Photos), afpub(Affinity Publisher), ai(Illustrator), c4d(Cinema 4D), cdr(CorelDRAW), dwg(AutoCad), graffle(OmniGraffle), psb(Photoshop Large Document), psd(Photoshop), skp(SketchUp), xd(Adobe Xd), xmind

视频:m4v, mp4, webm

音频:aac, flac, m4a, mp3, ogg, wav

字体:ttf, otf

RAW:3fr, arw, cr2, crw, dng, erf, mrw, nef, nrw, orf, otf, pef, raf, raw, rw2, sr2, srw, x3f

办公:pdf, potx, ppt, pptx, xls, xlsx, doc, docx

![](https://pic4.zhimg.com/v2-09034c459252864ebecd04d873eee3ef_b.jpg)

## 8、筛选方式

![](https://pic1.zhimg.com/v2-4f9b5b8b53d38e54a16fa3fcab576194_b.png)

Billfish

![](https://pic1.zhimg.com/v2-5a0c3f29d525139920c8fc4d10d86b0c_b.jpeg)

Eagle

相比Eagle，Billfish的筛选方式多了备注筛选。

## 9、颜色筛选

对色值为#156950的图片进行筛选

Eagel匹配出50张图片

Billfilsh 匹配出30张图片

从色值的准确度来说，Billfilsh更为精确，Eagel的容错度大些。从用户使用角度来说，一般也不会精确到色值。

## 10、市场占有率

Eagel：目前得到阿里巴巴、腾讯、百度、字节跳动、网易、美团、饿了么、微软、大疆等设计团队的使用。

Billfish：Billfish：2020年6月份新推出的，依托于知乎种草、B站up主推荐、用户论坛安利等，目前用户量破十万。

## 11、扩展插件

Eagle：浏览器扩展：支持Chrome、Firefox、Edge、QQ、360等，通过拖拽、Alt+右键、右键进行网页图片收藏，支持批量收藏及尺寸筛选，支持将网页保存为图片（整页截图、区域截图、页面元件截图）

Billfish：完成Chrome浏览器插件研发，预计功能测试时间6月24日上线，具体功能支持多少还不得而知。

## 12：文件大小

随机找了一个图片名字，定位两个软件存储的位置，不计算原始文件大小，只记录增量文件大小。

**eagle**

png文件缩略图文件大小28.0 KB (28,672 字节) ；

metadata.json文件大小593字节

**billfish**

webp文件缩略图大小16.0 KB (16,384 字节)

## 13：个人需求

**AI人像识别**

通过AI对图片的人物进行面部识别，进行分类。目前主流相册APP基本都支持，相册管理软件支持的不多，Picasa和digiKam准确度都有待提升，但目前Eagle和Billfish都不支持此功能。

**重复文件检测**  

我的图片做的非增量备份，每次的都是全量备份，所以存在大部分重复的图片，而且有些时间比较久的文件，同样一个照片文件名不一致。  

Billfish支持md5识别，文件数据有由哈希识别提升准确率，Egale的判断方式不清楚。

**EXIF信息**

部分文件由于一些软件和数据的多次移动，文件的创建时间和修改时间已经出现混乱，如果通过EXIF进行读取，相对会更方便。

**地点分类**

如果实现了EXIF信息，理论上可以实现地点的定位，相册管理更方便。

**文件夹命名**

目前两个软件的文件夹命名都是基于随机生成的文件名，如果能通过EXIF的信息读取，建立「文件创建时间+文件名字」作为文件夹名字。极端情况下未来某天不再使用eagle或者billfish软件，用户也可以很快恢复。

**其他**

因为我个人主要用于照片的留存和管理，所以希望能实现「时间线」「人物」「地点」等功能，再通过「标签」定格你生活瞬间，记录你在那时那地的故事和心情。

所以很多需求是建立在照片的基础上，对于大部分用户的素材管理可能不太适用，仅仅是个人意见。

## 14：收费模式

Eagle：一次性买断，官方售价199元，支持坚果云的云同步。

Billfish：目前是免费策略，官方计划推出云同步功能，届时理论上会推出收费策略，可能会一次性买断，也可能通过云同步策略收费，目前我的相册+视频基本都是T级的，如果Billfish自己的云空间实行订阅机制，不是太划算，如果你能和第三方云存储实现更适用于大部分网友，毕竟天翼云盘我也买了几年的铂金会员。

互联网的收费模式已经成为主流，个人不排斥收费，但是前提是稳定性和价格的合理性。

## 14：尾语

Eagle这个软件的主要优势在于「文件支持格式」「主题界面」

Billfish这个软件的主要优势在于「软件架构」「稳定性」

以上仅是个人的使用习惯，尽量做到中立，不排除测试环境所引起的误差，所以不接受反驳，特别是文中的错别字。
