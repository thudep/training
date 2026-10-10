## Linux 简介

Linux 内核最初只是由芬兰人林纳斯·托瓦兹（Linus Torvalds）在赫尔辛基大学上学时出于个人爱好而编写的。

Linux 是一套免费使用和自由传播的类 Unix 操作系统，是一个基于 POSIX 和 UNIX 的多用户、多任务、支持多线程和多 CPU 的操作系统。

### 一些概念

+ Unix：⼀种操作系统。

+ POSIX：Portable Operating System Interface，操作系统的国际标准。只要我在⼀个满⾜ POSIX 的操作系统上 写⼀个程序，那么任何满⾜ POSIX 的操作系统都能跑这个程序。GNU/Linux，macOS，或者其他的类 Unix 系统都满⾜ POSIX 标准，很可惜，Windows 不是⼀个满⾜ POSIX 的操作系统，因此，对于 Windows ⽤户我们需要 WSL 来填充我们对于 POSIX 的需求。

+ distro. (Linux 发⾏版)：我们⽇常中说的 Linux ⼀般是某种发⾏版，而发行版是什么呢？发行版为一般用户预先集成好 Linux 操作系统及各种应⽤软件。常⻅的有 Ubuntu, Debian, CentOS, Arch ...

+ WSL: Windows Subsystem for Linux，在 windows 上提供 POSIX 环境。

+ GUI vs. CLI

  使用计算机一定要鼠标吗？鼠标出现以前人们是怎么与操作系统交互的？

  + GUI：Graphical User Interface，图形用户界面。我们常用的以鼠标 + 显示器与操作系统进行交互的方式。
  + CLI：Command Line Interface，命令行界面。我们通过键盘向 **Shell** 输入一串命令。**Shell** 也是一个程序，会解析我们的字符串命令并与操作系统交互，告诉计算机做相应的事情。

### WSL 的安装及基本命令

#### 列出已安装的 Linux 发行版

```powershell
wsl --list
wsl -l
```

#### 查看可通过在线商店下载的可用 Linux 发行版

```powershell
wsl --list --online
wsl -l -o
```

#### 设置默认 Linux 发行版

```powershell
wsl --set-default <Distro>
wsl -s <Distro>
```
