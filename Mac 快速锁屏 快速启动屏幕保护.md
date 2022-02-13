# Mac 快速锁屏 快速启动屏幕保护

[www.chadou.me](https://www.chadou.me/p/213)

使用Windows系统的朋友，如果经常使用快捷键锁屏的话，使用 `Win键 + L` 是很爽的。但是换到Mac系统上来，它就不起作用了。

那我们在Mac上如果操作才能达到同样的效果呢？

### 1.设置触发角

打开「系统偏好设置」-「桌面与屏幕保护程序」，点击“屏幕保护程序”标签，右下角有个“设置触发角”，可以设置当你的鼠标移动到屏幕的四角中任意一个角的时候激活屏保。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.chadou.me%2Fcontent%2F1%2FBfUR9X0YyLIsvNoL.png)-
![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.chadou.me%2Fcontent%2F1%2FBfUXfdlRFqeynwJ6.png)

### 2.Finder 锁屏功能

Finder锁屏实际上是利用了“钥匙串访问”应用的锁定屏幕功能。打开「应用程序」-「钥匙串访问」-「偏好设置」，勾选“在菜单栏中显示钥匙串状态”，菜单栏中会出现一个“锁”的标志。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.chadou.me%2Fcontent%2F1%2FBfU6jQIbNhBFhTrL.png)-
![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.chadou.me%2Fcontent%2F1%2FBfUlNHat3xoDuVkw.png)

### 3.Automator 制作服务

上面两个方法虽然实现了锁屏，但都不是快捷键方式操作，如果你依然习惯于使用快捷方式，那么仔细看看下面介绍的方法。

首先，打开「应用程序」-「Automator」，在弹出的文稿类型对话框中选择“服务”，点击“选取”。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.chadou.me%2Fcontent%2F1%2FBfUVZ6v5hKm1trmI.png)

接着，在左侧资源库中搜索并找到“启动屏幕保护程序”，将它拖拽添加到右侧工作流程栏，将“"服务"收到”设置为“没有输入”。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.chadou.me%2Fcontent%2F1%2FBfUUVFTUz4tlTi4K.png)

然后，保存并命名为“启动屏幕保护”。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fwww.chadou.me%2Fcontent%2F1%2FCihDQaIRC7a0ZVaK.png)-
打开「设置」-「键盘」-「快捷键」，点击左侧的“服务”，在右侧找到刚刚创建的服务“启动屏幕保护”，并设置快捷键即可。

[查看原网页: www.chadou.me](https://www.chadou.me/p/213)