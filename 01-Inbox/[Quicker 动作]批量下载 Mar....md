# \[Quicker 动作\]批量下载 Markdown 中的外链图片并替换成本地离线图片链接 （主要应用场景：通过简悦导出 Markdown 到 Obsidian 中） #2241

[github.com](https://github.com/Kenshin/simpread/discussions/2241)Kenshin

(2021-10-14 更新) Obsidian 插件市场里有人放出了类似的插件：[aleksey-rezvov/obsidian-local-images](https://github.com/aleksey-rezvov/obsidian-local-images) 如果没什么特殊需求就请你们使用那个插件吧……-
(2021-10-09 更新) 本脚本进行了大幅度的更新，原有的说明不再适用，请按照新版进行下载和安装，并且按照新版的说明进行操作。原来的版本将作为存档放在最后。

简悦提供了阅读模式以及标注的 Markdown 格式输出和离线 Markdown 格式输出。 Markdown 格式输出的时候，文章中的图片是以外链的形式存在，如：

    ![img](https://www.gamespark.jp/imgs/zoom/488339.png)
    

这种形式并不适合于离线的文章保存，所以如果要离线保存，我们可能需要导出离线 Markdown 格式。而离线 Markdown 格式则把图片用 Base64 的形式写在文档中，在 Obsidian 等 Markdown 编辑器上编辑的时候此类的内嵌图片形式会影响二次编辑和文档加载的时间。

所以我试着通过用 [Quicker](https://getquicker.net/) 写个自动脚本来解决这个问题。

## 新版本的使用与说明

### 实现原理

1.  获取选中文本。没有选中文本的话从剪贴板读取内容。
2.  逐行判断有无外链图片的语法。
3.  如果找到了外链图片，提取图片外链地址，下载到本地。
4.  离线图片的位置为第一次运行脚本时指定的文件夹下 `assets` → `笔记名称` 的文件夹中。文件命名方式为 `yyyyMMddHHmmss_ffff.{图片文件扩展名}` 。`笔记名称` 通过模拟Obsidian的 `复制当前文件的路径` 的快捷键获取，若没有获取成功则弹出输入框让用户手动输入。
5.  把外链图片地址替换成本地离线图片文件的相对位置。
6.  合并生成新的文本。之前选中文本的话直接输出并覆盖原来选中的文本内容。如果没有选中文本的话输出到剪贴板。

### 前提

*   可不仅限于简悦导出的 Markdown 格式文档。
*   需要安装 [Quicker](https://getquicker.net/) ，最好是更新为最新版本。
*   Quicker 应该不需要专业版账户，免费版应该也能运行。（待验证）
*   需要预先在Obsidian里设置 `复制当前文件的路径` 快捷键。请设置为 `Ctrl+Shift+Y` 。（可以通过编辑脚本自己定制）
*   如果玩 Quicker 比较熟悉的用户，可根据自己需要适当修改脚本内容来满足自己的需要。

### 前期准备

#### 设置 Obsidian 快捷键

*   进入 Obsidian 的 `设置` → `快捷键`
*   搜索 `复制当前文件的路径` 并设置为 `Ctrl+Shift+Y` 。

#### 安装 Quicker 脚本

访问地址：[https://getquicker.net/sharedaction?code=16d0410b-2e88-4e30-91fb-08d982821308](https://getquicker.net/sharedaction?code=16d0410b-2e88-4e30-91fb-08d982821308) ，并点击右侧的 `复制到剪贴板` 。

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F396190%2F119298510-c10afc80-bc8f-11eb-8ccb-65bd33460a8d.png)](https://user-images.githubusercontent.com/396190/119298510-c10afc80-bc8f-11eb-8ccb-65bd33460a8d.png)

打开 Quicker 的面板，在任意空白按钮上右键，选择 `粘贴分享的动作` 。

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F396190%2F119298532-c9633780-bc8f-11eb-9788-a982bb44241a.png)](https://user-images.githubusercontent.com/396190/119298532-c9633780-bc8f-11eb-9788-a982bb44241a.png)

选择 `安装` 即可。

### 使用脚本

这个脚本有两种使用方式。

#### 选中文本→处理替换→覆盖

1.  选中含有外链图片的 Markdown 文本片段。
2.  打开 quicker 面板，执行脚本。
3.  外链图片地址将会覆盖成本地离线图片相对位置。
4.  修改好的 Markdown 文本片段将会自动覆盖选中的文本。

#### 复制文本到剪贴板→处理替换→手动粘贴

1.  复制 Markdown 文本片段。
2.  打开 Quicker 面板，执行脚本。
3.  外链图片地址将会覆盖成本地离线图片相对位置。
4.  修改好的 Markdown 文本片段将会自动覆盖剪贴板内容。
5.  在任意位置粘贴剪贴板中的 Markdown 文本片段。

### 第一次运行脚本

*   第一次运行脚本时会提示指定 Obsidian 的文件夹。请指定 **Obsidian的Vault文件夹** 。
*   第一次运行脚本之后会在本地保存状态数据（具体位置：`C:\User\yourusername\AppData\Local\Quicker\states\state_c2d4018d-1257-4b58-b5db-d253c988cda4.json`），该状态在各个设备上独立，也就是说每台设备第一次运行都要设置各自的 Obsidian Vault 文件夹。

### 如果没有设置快捷键

*   如果没有设置 Obsidian 的快捷键，则会提示用户手动输入 `笔记名称` 。因此本脚本其实也可以用于非 Obsidian 的编辑器，但是不保证脚本的正常运行。（主要是工作目录相关）

#### 使用效果

使用脚本之前：

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F396190%2F136645880-b9627931-dd82-451b-9211-cd7fb9cb5e6c.png)](https://user-images.githubusercontent.com/396190/136645880-b9627931-dd82-451b-9211-cd7fb9cb5e6c.png)

使用脚本之后：

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F396190%2F136645885-25fe80c4-e948-4ea5-9895-2b518ce4d6b8.png)](https://user-images.githubusercontent.com/396190/136645885-25fe80c4-e948-4ea5-9895-2b518ce4d6b8.png)

图片将会下载到该笔记目录的 `Assets\笔记名称` 文件夹下：

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F396190%2F136645903-b07bb739-4f4b-47c3-8303-b8b8af10b389.png)](https://user-images.githubusercontent.com/396190/136645903-b07bb739-4f4b-47c3-8303-b8b8af10b389.png)

### 使用上的注意

*   由于本地离线图床位置固定，所以当你移动 Markdown 文档至新的位置时，图片显示可能会失效。在 Obsidian 的 Vault中移动 Markdown 文档不会有影响，但是为了保证兼容其他 Markdown 的编辑器，尽量少移动 Markdown 文档。
*   为了方便迁移文档数据或是打包分享的情况，特地把图片文件夹设置成 `Assets\笔记名称` 的形式。
*   目前我只支持了 `jpg|jpeg|png|gif` 格式的图片。如果有其他格式需求请自行修改脚本添加。
*   有什么使用上的建议请积极反馈。

* * *

## 初始版本存档（不再更新维护支持）

### 实现原理

1.  获取选中文本。没有选中文本的话从剪贴板读取内容。
2.  逐行判断有无外链图片的语法。
3.  如果找到了外链图片，提取图片外链地址，下载到本地。
4.  离线图片的位置为**环境变量所指定的位置**下的 `assets` 文件夹中。文件命名方式为 `yyyyMMddHHmmss_ffff.{图片文件扩展名}` 。
5.  把外链图片地址替换成本地离线图片文件的相对位置。
6.  合并生成新的文本。之前选中文本的话直接输出并覆盖原来选中的文本内容。如果没有选中文本的话输出到剪贴板。

### 前提

*   可不仅限于简悦导出的 Markdown 格式文档。
*   需要安装 [Quicker](https://getquicker.net/) ，最好是更新为最新版本。
*   Quicker 应该不需要专业版账户，免费版应该也能运行。（待验证）
*   为支持多设备不同本地目录的需求，需要设置系统环境变量来指定下载目录。
*   如果玩 Quicker 比较熟悉的用户，可根据自己需要适当修改脚本内容来满足自己的需要。

### 前期准备

#### 设置系统环境变量

*   Win 键启动开始菜单 → `查看高级系统设置` → `环境变量`
*   创建新的用户变量或系统变量，变量名为 `OBWEB` ，变量值为**存放 Markdown 文档的路径** ，如下图。

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F396190%2F119298499-bc464880-bc8f-11eb-9cff-c9b97d70c380.png)](https://user-images.githubusercontent.com/396190/119298499-bc464880-bc8f-11eb-9cff-c9b97d70c380.png)

> 增加环境变量之后可能需要重启 Quicker 才生效。

#### 安装 Quicker 脚本

访问地址：[https://getquicker.net/sharedaction?code=c1799420-32c0-40a4-08aa-08d91c1f78c6](https://getquicker.net/sharedaction?code=c1799420-32c0-40a4-08aa-08d91c1f78c6) ，并点击右侧的 `复制到剪贴板` 。

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F396190%2F119298510-c10afc80-bc8f-11eb-8ccb-65bd33460a8d.png)](https://user-images.githubusercontent.com/396190/119298510-c10afc80-bc8f-11eb-8ccb-65bd33460a8d.png)

打开 Quicker 的面板，在任意空白按钮上右键，选择 `粘贴分享的动作` 。

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F396190%2F119298532-c9633780-bc8f-11eb-9788-a982bb44241a.png)](https://user-images.githubusercontent.com/396190/119298532-c9633780-bc8f-11eb-9788-a982bb44241a.png)

选择 `安装` 即可。

### 使用脚本

这个脚本有两种使用方式。

#### 选中文本→处理替换→覆盖

1.  选中含有外链图片的 Markdown 文本片段。
2.  打开 quicker 面板，执行脚本。
3.  外链图片地址将会覆盖成本地离线图片相对位置。
4.  修改好的 Markdown 文本片段将会自动覆盖选中的文本。

#### 复制文本到剪贴板→处理替换→手动粘贴

1.  复制 Markdown 文本片段。
2.  打开 Quicker 面板，执行脚本。
3.  外链图片地址将会覆盖成本地离线图片相对位置。
4.  修改好的 Markdown 文本片段将会自动覆盖剪贴板内容。
5.  在任意位置粘贴剪贴板中的 Markdown 文本片段。

#### 使用效果

使用脚本之前：

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F396190%2F119298545-d08a4580-bc8f-11eb-9e38-de3c25169c70.png)](https://user-images.githubusercontent.com/396190/119298545-d08a4580-bc8f-11eb-9e38-de3c25169c70.png)

使用脚本之后：

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fuser-images.githubusercontent.com%2F396190%2F119298557-d5e79000-bc8f-11eb-9acc-e8ee03c2957c.png)](https://user-images.githubusercontent.com/396190/119298557-d5e79000-bc8f-11eb-9acc-e8ee03c2957c.png)

### 使用上的注意

*   由于本地离线图床位置固定，所以当你移动 Markdown 文档至新的位置时，图片显示可能会失效。在 Obsidian 的 Vault中移动 Markdown 文档不会有影响，但是为了保证兼容其他 Markdown 的编辑器，尽量少移动 Markdown 文档。
*   增加环境变量之后运行脚本，提示没有设置环境变量的，请重启一下 Quicker 之后再试。
*   目前我只支持了 `jpg|jpeg|png|gif` 格式的图片。如果有其他格式需求请自行修改脚本添加。
*   有什么使用上的建议请积极反馈。

[查看原网页: github.com](https://github.com/Kenshin/simpread/discussions/2241)