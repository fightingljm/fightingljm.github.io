---
title: 上传代码到 Github.com
---

前面学会了如何在本地用 git 创建项目版本，本节咱们看看咋把新版本上传到 github.com 之上。


### 准备工作：删除第一天创建的项目

如何删除一个 github.com 的仓库呢？

首先到仓库页面：https://github.com/funnydeer/funnydeer.github.io

点 Settings（设置）这一个标签。打开的页面底部有一个 “Delete this repository” 按钮，意思是”删除这个仓库“，点击按钮。打开的界面中，输入一下这个仓库的名字 funnydeer.github.io 就可以把这个仓库删除了。

删除仓库之后，我们要做的事情是：

>如何把本地的已有仓库，上传到 github.com

### 第一步：创建本地项目

项目名称是任意的，但是我们这里想做的事情是上传比较，所以，本地这个仓库名，也必须是：

```
mkdir funnydeer.github.io
```

本地项目名要和 github.com 我们一会儿要创建的仓库名保持一致。

然后，我们就可以把我们想要上传的内容拷贝到这个文件夹内，并制作本地版本。

拷贝进来的内容，要符合第一天我们介绍的 github pages 的格式规范（其实最重要的一点就是每个 .md 文件中都要有头部，参考第一天我们的文档中的介绍）。


### 第二步：创建 github.com 上的同名仓库

```
不要初始化仓库，直接创建
两种方式创建https和ssh：https每次上传要输入用户名和密码；ssh第一次上传粘贴公钥，之后直接上传。
公钥在～.ssh里
门牌号的存放位置
cat .git/config
```

### 添加 ssh key

为了达成开发机和 github.com 的互信。因为开发过程中，我们需要用本地机器向 github.com 的仓库中
写东西（ git push ），同时我们又不想每次都输入密码，所以我们就用 ssh key 的形式来达成互信，过程
如下：

- 在本地机器上生成一对 ssh key ，一个公钥，一个私钥
- 把公钥添加到 github.com

具体操作如下：

- 首先本地运行 `ssh-keygen` 命令
- 到 `~/.ssh/id_rsa.pub` 也就是公钥文件中，拷贝公钥字符串
- 把字符串粘贴到 github.com -> setting -> ssh keys -> add

这样添加 ssh key 的工作就完成了，以后我们执行 git push 这样的命令就不会看到如下错误了：

```
...permission denied...
...make sure ... correct access right ...
```

大功告成。


### git clone 命令


要想把 github 上的一个项目代码下载到本地有两种方式，一种就是普通下载（ download ）。但是，开发者
基本上会选择另外一种方式，就是 clone 。

```
git clone git@github.com:happypeter/digicity.git
```

clone 的特点就是不仅仅可以得到最新代码，而且可以得到整个改版历史。而普通下载只能得到最新版本。


### git 各个命令的作用


- `git push` 把本地仓库中有，而远端对应仓库中没有的**版本**推送到远端
- `git pull` 把远端仓库中有，而本地对应仓库中没有的**版本**拉到本地
- `git clone` 把远端仓库，克隆到本地


### 学习 Github/Git 的学习目标

- 知道 git 是**版本控制**工具
- 每个同学要有一个 github 仓库
- 已经添加 ssh key 互信，也就是可以从本地仓库推送内容（ git push ）到 github 仓库
- 可以在本地仓库中任意添加，删除，修改文件，并作成版本

这样，github/git 的初级使用我们就有能力完成了。但是，作为成熟开发者，github 上面会发 push request ，本地 git 会开启新分支，都是必备知识。暂时我们先不涉及。

### 承前启后

课程进行到现在，程序员三大基本工具，我们就介绍完毕。

- 编辑器 atom
- 命令行 Linux
- 版本控制 Git/Github


其实呢，这三个工具的学习都不能一蹴而就，都是在实际写代码过程中逐步完善的。但是这里 Peter 有三门课程可以推荐：

- [Atom 爱上 JS](http://haoqicat.com/atom-love-js)
- [驾驭命令行怪兽](http://haoqicat.com/ride-cli-monster)
- [Git 北京](http://haoqicat.com/gitbeijing)

以上三门课程，大家可以根据自己所处的阶段，有选择的学习里面的部分章节。


学习上面三大工具的目的，就是为了更加高效的**写代码** 。
