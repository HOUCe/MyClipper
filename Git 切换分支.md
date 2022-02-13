# [Git 切换分支](https://chinese.freecodecamp.org/news/git-switch-branch/)

在 Git 中，你经常需要切换分支。你可以使用 `git checkout` 命令来实现。

## **如何在 Git 中创建新的分支**

使用 `git checkout` 命令在 Git 中创建一个新分支，在标记 `-b` 后面加上分支的名字。

这将在当前分支的基础上创建一个新分支。新分支的历史记录将从你基于其创建新分支的分支的当前位置开始。

假设你当前在一个名为 `master` 的分支上：

```
(master)$ git checkout -b my-feature
Switched to a new branch 'my-feature'
(my-feature)$
```

这里你可以看到创建了一个新分支 `my-feature`，它是在 `master` 分支上创建的。

## **如何在 Git 中切换到一个现有分支**

要切换到现有分支，可以再次使用 `git checkout`（不带 `-b` 标记），并输入要切换到的分支的名称：

```
(my-feature)$ git checkout master
Switched to branch 'master'
(master)$
```

还有一个方便的快捷方式，可以给 `git checkout` 后面加上 `-`，而不是分支名称，来返回上一个分支：

```
(my-feature)$ git checkout -
Switched to branch 'master'
(master)$ git checkout -
Switched to branch 'my-feature'
(my-feature)$
```

## 如何退出特定的提交（commit）

要退出或切换到特定的 commit，还可以使用 `git checkout`，并输入 commit 的 [SHA](https://en.wikipedia.org/wiki/Secure_Hash_Algorithms)，而不是分支名称。

毕竟，分支实际上只是 Git 历史中特定 commit 的指针和跟踪器。

### **如何找到一个 commit 的 SHA**

查找 commit 的 SHA 的一种方法是查看 Git 日志。

你可以使用 `git log` 命令查看日志：

```
(my-feature)$ git log
commit 94ab1fe28727b7f8b683a0084e00a9ec808d6d39 (HEAD -> my-feature, master)
Author: John Mosesman <johnmosesman@gmail.com>
Date:   Mon Apr 12 10:31:11 2021 -0500

    This is the second commmit message.

commit 035a128d2e66eb9fe3032036b3415e60c728f692 (blah)
Author: John Mosesman <johnmosesman@gmail.com>
Date:   Mon Apr 12 10:31:05 2021 -0500

    This is the first commmit message.
```

在 `commit` 一词之后的每次 commit 的第一行中，都有一长串字符和数字：`94ab1fe28727...`，这就是 SHA。SHA 是为每次 commit 生成的唯一标识符。

要退出特定的提交，你只需要将提交的 SHA 作为参数传递给 `git checkout`：

```
(my-feature)$ git checkout 035a128d2e66eb9fe3032036b3415e60c728f692
Note: switching to '035a128d2e66eb9fe3032036b3415e60c728f692'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 035a128 a
((HEAD detached at 035a128))$
```

> **注意：**通常，你只需要使用 SHA 的前几个字符，因为字符串的前四个或五个字符在整个项目中很可能是唯一的。

## **什么是分离头指针状态（detached HEAD state）**

退出特定 commit 的结果进入“分离头指针状态”。

[根据文档：](http://git-scm.com/docs/git-checkout#_detached_head)

> \[分离头指针状态\] 仅仅意味着 `HEAD` 指向一个特定的 commit，而不是指向一个命名的分支。

基本上，`HEAD`（跟踪你在 Git 历史中的位置的 Git 内部指针之一）已经偏离了已知分支，因此从这一点开始的更改将在 Git 历史中形成新的路径。

Git 希望确保这就是你想要的，因此它为你提供了“自由空间”进行实验——如输出所描述：

```
You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.
```

从这个位置，你有两个选择：

-   试验一下，然后返回上一个分支，放弃更改
-   从这里开始工作，然后从这一点开始新的分支

你可以使用 `git switch -` 命令撤消所做的任何更改并返回上一个分支。

如果你想保留所做的更改，并从此处继续，则可以使用 `git switch -c <新分支名称>` 从此处创建一个新分支。

## **小结**

`git checkout` 命令有多种用途。

你可以使用它来创建新分支、退出分支、退出特定的 commit 等。

如果你喜欢这篇教程，我还在 [Twitter](https://twitter.com/johnmosesman) 上讨论类似的主题，并在[我的网站](https://johnmosesman.com/)上写这些主题，欢迎查看。

原文：[Git Switch Branch – How to Change the Branch in Git](https://www.freecodecamp.org/news/git-switch-branch/)，作者：[John Mosesman](https://www.freecodecamp.org/news/author/johnmosesman/)
