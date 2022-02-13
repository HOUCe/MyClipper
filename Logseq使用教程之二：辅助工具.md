# Logseq使用教程之二：辅助工具

[zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/434690089)XuLei自以为是的图书馆员关注他

## **AHK—快速输入日期**

原始代码见：**[obsidian.ahk](https://gist.github.com/emisjerry/39e3dee49b4237267d12d45244991187)**。根据个人使用习惯，以及Logseq的双链符号自动补全功能。修改了部分代码，部分样例如下：

    :O*:'jt::         ;; 输入’jt就会自动生成当天日期
      ;; 0409 language code for USA
      FormatTime, vToday,L0x0409, yyyy-MM-dd
      send [[%vToday%     ;; 全文代码send语句中，将原先的[[]]，改成[[。Logseq会自动触发补全 
      return
    

如今天是2021-12-04，在Logseq中输入`’jt`后，则自动输入`[[2021-12-04]]`。输入`’mt`，则为`[[2021-12-05]]`。同理，其他的还有`’ht ’zt ’qt`

自用的完整代码见：**[logseq date.ahk](https://www.aliyundrive.com/s/9HNbJ5PUhwk)**

## **Quicker动作**

### **1\. Logseq内直接打开PDF**

*   Logseq内直接打开PDF的链接格式为`{{zotero-linked-file "attachments: }}`
*   其中`{{}}`内的取值有两部分组成：固定关键词`zotero-linked-file "attachments:` + `pdf本地路径`
*   因为格式固定，因此可以通过`Quicker`动作快速获取`pdf本地路径`，插入Logseq中。这里根据使用场景，提供了2个Quicker动作，具体描述见下。实现效果如图：

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic4.zhimg.com%2Fv2-58fbb3a15fbae6b2f7e7d00c910cf587_b.jpg)

*   **[ZPDF2LOG](https://getquicker.net/sharedaction?code=04add495-ccda-4221-f8ac-08d96304beb0)**

*   首先在zotero中右键`复制附件路径`
*   然后将光标定位到logseq中需要输入的位置
*   再点动作执行即可（也可以设置全局快捷键），自动写入完整的`{{}}`

*   **[PDF2LOG](https://getquicker.net/sharedaction?code=74ef40af-d7a4-4e06-f8ad-08d96304beb0)**

*   首先，直接在本地文件内点击需要的PDF
*   执行动作
*   ctrl+v复制进logseq即可
*   如果自用的话，此动作中有一个`文本处理-起始位置`需要根据本地文件夹地址修改，也就是Logseq中zotero设置的storage地址字符串长度

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic3.zhimg.com%2Fv2-db222d30354363905d9c74cd6eadbc8e_b.jpg)

### **2\. Logseq跳转到PDF高亮文本**

*   动作链接：**[从BookxNote复制文本与链接到Logseq](https://getquicker.net/Sharedaction?code=d61f19a1-364a-4d28-f714-08d96c99d246)**
*   Logseq原生的PDF高亮功能，会自动生成一个MD文件存储高亮文本，通过`(())`实现与文献笔记之间的链接。但在个人的使用场景中，更多时候只是需要一个页码标记可以跳转到PDF即可，复制的文本也可以任意修改，而不用额外生成MD文件。而BookxNote提供了高亮文本链接跳转功能（强烈推荐BookxNote ️ ️ ️）
*   原始的操作需要三部才能操作完成：选择文本后高亮，复制文本到Logseq，复制高亮文本链接到Logseq。高亮文本与链接还需要写成`[]()`格式。比较麻烦。
*   上述Quicker动作就实现了所有动作的整合。自用的版本只是稍微修改了下页码的显示。具体配置教程见：**[Logseq小白系列教程入门篇二 - 知乎](https://zhuanlan.zhihu.com/p/405764984?utm_source=wechat_session&utm_medium=social&utm_oi=41357590659072&utm_content=group3_article&utm_campaign=shareopn)**。这里向作者致敬 ️ ️ ️-
    

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic1.zhimg.com%2Fv2-9b432efe577e97613ed09bf632547370_b.jpg)

### **3\. PDF规范粘贴**

*   动作链接：**[PDF规范粘贴](https://getquicker.net/Sharedaction?code=023932c6-8c3a-4325-07b6-08d9864ddf23&fromMyShare=True)**
*   此动作是也是基于动作2修改得来。再次向原作者致敬 ️。
*   选中PDF文本后，执行此动作，会将多余的空格、换行去除，并自动复制进Logseq内。
*   动作2和3之间如果同时使用，并且设置全局快捷键的话，为避免冲突，需要将BookxNote和动作2中的快捷键进行修改。具体见下：
*   最终结果：动作2的全局快捷键是：`ctrl + shift + c`；动作3的全局快捷键是：`ctrl + c`

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic2.zhimg.com%2Fv2-016cc75e77a379ca2b4fcef4bf3f3695_b.jpg)

*   BookxNote快捷键修改如下图

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic1.zhimg.com%2Fv2-2e32c8fb3f8384ea4a7f5fc878220828_b.jpg)

*   动作2的修改

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic1.zhimg.com%2Fv2-5f6f7deb5cc963fcdbf3684feae54a34_b.jpg)

### 其他

*   **[AHK：PDF复制规范格式化粘贴](https://www.zhihu.com/question/41122422)**
*   **[AHK：TEXT选中文本后添加各种符号](https://gist.github.com/davebrny/088c48d6678617876b34f53571e92ee6)**
*   **[WinPopClip：开源的Windows平台版Popclip](https://github.com/millionart/WinPopclip)**

*   这个可以自行修改，增加各种自用的快捷方式。如沙拉查词，百度搜索，添加高亮、双链符号等等

* * *

关于我

*   个人博客：**[https://xulei.vercel.app](https://xulei.vercel.app/)**
*   学术主页：**[https://lei.vercel.app](https://lei.vercel.app/)**
*   个人公众号：圕言圕语

发布于 2021-11-18 11:13

[查看原网页: zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/434690089)