---
layout: post
title:  "获取ubuntu中命令的源代码"
date:   2014-07-08 18:00:00
categories: Code
---

新技能 get √

Linux下的命令是非常方便快捷的，有时候我们想看看Linux的一些命令是如何编程实现的，可以通过以下方法：

以top命令为例：

STEP1:

使用 type 命令查找 top 所在的目录

`type top`

得到 top 所在目录是 `/usr/bin/top`


STEP2:

使用 sudo dpkg -S /usr/bin/top 查找该命令包含在哪个源文件中

`sudo dpkg -S /usr/bin/top`

结果是 `procps: /usr/bin/top` 


STEP3: 

使用 apt-get source procps 命令下载 procps 的源码

`apt-get source procps`


STEP4:

在当前目录下查看 procps 的源码


DONE.
