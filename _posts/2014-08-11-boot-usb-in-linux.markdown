---
layout: post
title:  "如何在LINUX下制作启动U盘"
date:   2014-08-11 18:00:00
categories: Code
---

在LIUNX下又很多工具可以用来制作启动盘

其实完全可以用LINUX下一个自带的小工具来完成

只需要三条命令

1.使用df(或fdisk)查看U盘的设备名


2.假设U盘对应的设备名是sdb2, 使用以下命令卸载U盘:

`sudo umount /dev/sdb2`


3.将镜像写入U盘

`sudo dd if=/home/user/system.iso of=/dev/sdb2`


一定要注意of后面的设备名称, 并保证U盘里面的文件已经备份.
