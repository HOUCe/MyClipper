---
created: 2021-11-10
source: https://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247483715&idx=1&sn=4101b28e6411698e57a69786b7d989f8&chksm=e9212322de56aa3436e9fd8679685949249d3adee2c0da0dd6d5b599c66a9eacfb0f6af39c72#rd
author: flomo
---

# [flomo weekly vol.008 - 「 批注」功能再设计](https://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247483715&idx=1&sn=4101b28e6411698e57a69786b7d989f8&chksm=e9212322de56aa3436e9fd8679685949249d3adee2c0da0dd6d5b599c66a9eacfb0f6af39c72#rd)


这是 flomo weekly 第八期。如果你想参与到 flomo 的内测中来，点击阅读全文，填写内测申请表即可加入。

### All in one 的是一个好产品么？

在回答这个问题的时候，我们其实是在问我们自己，是否需要贪心的把数据都归集在一处呢？

历史上不止一个产品有这样的想法，但是最终能如此使用的用户却为数不多。在历史的故纸堆里面，躺着微软的 Wallop ， Google 的 Wave 等，究其原因，是因为使用者本身的原因 —— **All in one 的场景，势必要求使用者本身更加谨慎，控制好信息的数量和关系之间的复杂度，而一旦失去了控制，使用者就会喜新厌旧的去寻找「下一个好工具」。**

### 我们究竟该记录什么

这就和我们的屋子一样。如果你希望有一个干净简洁的屋子，那么需要的是除了勤快的打扫，还需要审慎的添置物品，如果经常往家里带各种奇怪的东西，那么就算勤快也会被堆满。所以除了添加谨慎，还要去丢掉一些不常用，或者已经有替代品的东西，这样才能维持屋子的整洁。

思维工具就像是我们知识的房间，我们不断的从外界输入，摘录。这本身让我们空无一物的房间开始有了生活的气息，而随着时间的变化，如果没有没有审慎的添加和不断地回顾总结抽象以及删除，则会变成一片信息的垃圾场。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/wDNLH7zcd1MxibtibrjEuPT6BOFrgricYMoyOVrcoOlSm9m7tpmS8icZibNDexooQPPO8CQG3WV0OaByWthjEWZoHdg/640?wx_fmt=jpeg&tp=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)

那么在 flomo 里面该记录什么呢？虽然 flomo 鼓励大家迅速且持续的记录想法，但是记录后的回顾和整理同样重要。根据《How to take smart note》这本书中提到的，在使用卡片盒记忆法的时候，不能胡乱添加笔记，这样会破坏其中的质量。而除了质量之外，还需要足够的数量才能引起质变。要达到临界质量，需要区分三种情况：  

•Fleeting Notes：流水笔记，这种笔记只是信息的提醒，随便写但是很快就会被扔到垃圾桶•Permanent Notes：永久性笔记，永远不会被扔掉。本身就包含了必要的信息，以一种永久且可理解的方式存在，总是以同样的方式在同样的地方存储•Project Notes：项目笔记，只与一个特定的项目有关。他们被保存在特定的地方，在项目结束后即丢弃。

其实我的 flomo 在使用过程中已经出现了一些混乱，究其原因就是没有区分这三类笔记的性质。而书中也提到笔记的误区：

•不应该像做实验笔记一样事无巨细的记录。如果任何想法都被记录，那么真正好的想法就会被冲淡。严格的时间顺序并不能帮我们找到新的想法的组合方式 •只收集特定项目相关的笔记，但是项目一旦结束所有的内容即作废。如果这时候试图通过建立多任务来缓冲这种矛盾，那么 todo 列表就会越来越长。 •将所有的笔记当做昙花一现的笔记，不断的记录然后过一阵就试图大清洗。哪怕只有一些不清晰的东西在笔记本上出现，就会诱发从头开始的欲望。•如果没有永久性的想法库，你将无法在较长时间内发展任何重大的想法，因为你要么限制了自己单个项目的长度，要么限制了自己的记忆容量。 

所以我也花了两周时间将项目相关的记录挪到了专门的 todo 类产品中，而每周会定期的来删除查看和删除一些流水笔记。这样一来，flomo 中的留存下来的信息半衰期就足够长，能让其内容日后发挥更大的价值。  

笔记应该越多对你自己帮助越大， 而不是越多越混乱难以找出想要的东西

### 批注功能的更新

上周重要更新是，把上线许久的批注功能给重新改造了。其实许多同学可能没有用过批注功能，在设计之初，标签是希望围绕某些长期的「领域」来索引使用；而批注则是希望围绕某些「话题」来索引使用，比如「面试题库」对我来说就是一个很好的话题，一旦有新想法，就可以以批注的形式对之前的内容进行再补充。

**主要有以下几个改变：**

•批注的内容也会成为一个 MEMO 出现在主 Timeline 中•批注的内容会带上被批注对象的内容（及链接）•被批注的内容会自动的关联上批注的内容（及链接）

![图片](https://mmbiz.qpic.cn/mmbiz_png/wDNLH7zcd1MxibtibrjEuPT6BOFrgricYMoy6ibQ5HetiaIoFYmZS80VPibI4vJr7uXWZzXNWLq284sFwqtv2cfNHqJw/640?wx_fmt=png&tp=png&wxfrom=5&wx_lazy=1&wx_co=1)

**暂时还不支持**

•解除两者之间的关系•仅作为批注不出现在主 timeline

配合回顾 + 新改进的批注功能，可以更好地把过去的记录重新审视和组织。

### 10 月产品进度

发布框和排序重构：移动端会有比较大的改变（嗯，打脸了，放在上面了）批注功能重设计：批注的内容将会成为一条独立的 memo，并支持进一步关联和探索•支持 Pin ：可以 Pin 一个 tag 或一个 memo 在界面上•Tag 相关：支持自动完成及支持自动涌现（估计十月下旬到十一月初）•搜索改进：搜索功能重构，支持结构化搜索

### 往期内容

[flomo weekly vol.004](http://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247483670&idx=1&sn=f28848a349cfd46ce9265515b343ce71&chksm=e9212377de56aa61cd46d1aa192d701c71ed55bb37588f852916772856d7538f1bf1d1d40577&scene=21#wechat_redirect)  

[flomo weekly vol.005](http://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247483679&idx=1&sn=a4e98d2ebc689d4df0f450e8682911ec&chksm=e921237ede56aa6868fbcf46a465435cc31f84e1353ad9af03068dc052757b00eb9c4839b679&scene=21#wechat_redirect)  

[flomo weekly vol.006 - 「回顾」功能发布](http://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247483690&idx=1&sn=dcdc89cf9ed769993aa85dc759364ce3&chksm=e921234bde56aa5d40bf04eb94a8e909b75980064e88e470962a52ea17fcd69c3f13af68397f&scene=21#wechat_redirect)  

[flomo weekly vol.007 - PWA 版本发布及批注功能重构（计划）](http://mp.weixin.qq.com/s?__biz=MzI0MDA3MjQ2Mg==&mid=2247483700&idx=1&sn=ca579c578f1ed749d5ffaf182396420f&chksm=e9212355de56aa431cd71938e620e3c0400b240e2bfe4331915a324fb36f77132e4692fa67b2&scene=21#wechat_redirect)  

如果你想参与到 flomo 的内测中来，点击阅读全文，填写内测申请表即可加入
