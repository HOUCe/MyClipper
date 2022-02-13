# 利用Python和webhook实现自动提交代码

[www.jianshu.com](https://www.jianshu.com/p/99d7b6ad9332)

> 最近在为公司书写项目的api文档，计划利用码云的wiki管理整个项目，公司自有git作为项目内容依托，这样全员都可参与，而我定期向码云推送就可以了。

## 问题

### 根据需求遇见了这样一个问题：我每次从git上拉取更新再向码云wiki推送，最后再通过钉钉向大家通知的过程，过于繁琐。于是计划用Python实现部分流程自动化

### 代码

#### 1.引入的头文件

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    # @Time    : 2018/9/25 上午10:52
    # @Author  : liuyonghu
    # @Site    : https://www.lyonghu.com
    # @File    : autoGitPush.py
    # @Software: PyCharm
    
    
    import subprocess
    import time
    from tools.sendMessageToDD import sendMessage
    

#### 2.add 方法

    def add():
        cmd = "git add ."
        process = subprocess.Popen(cmd, shell=True)
        process.wait()
        returnCode = process.returncode
    
        if returnCode != 0:
            print(" add returnCode", returnCode)
        else:
            commit()
    

#### 3.commit 方法

    commitMessage = ""
    def commit():
        global commitMessage
        commitMessage = time.strftime("%Y/%m/%d %H:%M")
        cmd = "git commit -m  '{}'".format(commitMessage)
    
        # print("cmd = " + cmd)
        process = subprocess.Popen(cmd, shell=True)
        process.wait()
        push()
    

#### 4.push 方法

    def push():
        cmd = "git push origin master"
        process = subprocess.Popen(cmd, shell=True)
        process.wait()
        returnCode = process.returncode
        if returnCode != 0:
            print("push returnCode", returnCode)
        else:
            sendMessage({
                "fileName": "api文档 : \n\n已更新，请注意查看！ \n" +"\n更新信息: {}".format(
                    commitMessage),
                "text": time.strftime("%Y/%m/%d %H:%M"),
                "error": False
            })
    

#### 5.最后调用

    add()
    

**注意**-
代码调用顺序

由此除了项目成员向wiki推送状态无法获取外其他事件基本可以省去很多手动操作。如果大家知道更好的办法欢迎讨论指导

-
-

> 亲情链接：-
> [个人网站](https://www.lyonghu.com)

[查看原网页: www.jianshu.com](https://www.jianshu.com/p/99d7b6ad9332)