---
title: 进入 Linux 命令行的黑色世界
---


### 为什么要用 Linux ?

北京的（包扩硅谷）的职业开发者一般都用苹果本（ Macbook ）做开发（写代码）。我们是做 Web 开发的，Web 项目要运行在服务器（ server ）上，服务器的业界标准是使用  Linux 做操作系统呢。

幸运的是， MacOS 和 Linux 都是 Unix 的变种，使用起来大同小异。所以我们课程中使用 Linux 做开发也是非常棒一个选择。Peter 本人用 Linux 做开发，用了5年。

### Linux 系统有多种发行版

我们课上安装的是国内的一个发行版，叫做 **深度 Linux** ，这个系统跟国外的 Ubuntu Linux 非常类似。
所以如果有同学电脑装不了深度，可以尝试安装 Ubuntu 。

其他的知名 Linux 发行版还有很多，例如 Red Hat ，Suse，Fedora , CentOS 等等。


### 打开命令行界面

学习 linux 最重要的就是来使用它的命令行。

Ctrl-Alt-T 是**深度 Linux** 系统上打开命令行窗口的快捷键。这个命令行窗口程序，在 Mac 系统上叫 iTerm ，在深度系统上叫”深度终端“。

命令行窗口中可以运行的程序不唯一。默认启动的程序叫做 Bash ，这个是我们这个要学习的核心。在 Bash 下就可以来输入各种 Linux 命令了。例如，可以敲

```
$ ls
```

来列出当前位置都有哪些文件。

但是，命令行窗口中也能启动其他的程序，例如 Python/Javascript 的解析器。这些我们不管，所谓
学习 Linux 命令行，其实就是学习 Bash （ Mac 系统上用的命令行也是 Bash）。

### Bash 简介

Bash 是各种命令行中最流行的一种，其中后两个字符 sh 是
shell 这个单词的缩写，shell 的意思就是“命令行”，前面 Ba 是人名，我们不用管。

Bash 的常用命令：

- ls 列出当前位置所有文件
- rm 删除文件，或者文件夹
- cd 改变当前位置
- mv 移动文件
- 等等等等


### 命令行能干什么？

命令行和鼠标（图形化的界面）一样是人类操作电脑的一种方式。基本上鼠标能干得活，命令行都能干（个别的像 Photoshop 的大部分操作，还是鼠标好用一些）。

同时，只要命令行能干的事情，都会比鼠标更高效，因为命令行是可以批处理的。实际开发中，我们用命令行最经常的操作就是，创建文件，删除移动文件等。

举个例子，如果我要在桌面上创建一个文件夹，可以用鼠标右键来创建，同时如果用命令行，操作如下


```
$ cd Desktop
$ mkdir FolderName
```

- 上面 `cd Desktop` 改变当前位置到桌面。
- `mkdir` 是创建一个文件夹的命令

最终操作结果是等价的。但是如果要学习命令行操作，第一步就是要掌握文件系统结构。

### 插播一个 Linux 八卦

Linux 系统运行在所有的 Andriod 手机上，全球最强的十台 super computer 其中有九台（或者十台）运行 Linux 。80% 的服务器都用 Linux 。

所以说 Linux 很牛，但是它的价格是：0 元。它的价值是多少？大约200亿美元。到底是谁做成了200亿的东西，却 give it for free 。这个人就是 [Linus](https://en.wikipedia.org/wiki/Linus_Torvalds) 。Linux 是一个 Unix 的 clone 版，名字的意思就是 Linus 的 Unix 。


Linus 是芬兰人，他父亲是政治家（共产主义者）。Linus 在大二的时候创作 Linux 操作系统。69年出生，今天依然在写代码。Linus 的另外一个作品，就是 Git 。

Linus 的书《 Coding For Fun 》。

### 参考

- [驾驭命令行怪兽](http://haoqicat.com/ride-cli-monster)
