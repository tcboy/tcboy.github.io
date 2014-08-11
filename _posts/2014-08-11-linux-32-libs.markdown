---
layout: post
title:  "ubuntu14.04无法安装ia32-libs的解决方法"
date:   2014-08-11 12:00:00
categories: Code
---

从ubuntu13.10版本之后取消了ia32-libs

但有时我们还需要用到32位的库

比如在安装Android SDK的时候

如果遇到需要安装ia32-libs的情况 请使用以下命令替代

`sudo apt-get install -y libc6-i386 lib32stdc++6 lib32gcc1 lib32ncurses5 lib32z1`
