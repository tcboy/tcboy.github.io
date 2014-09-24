---
layout: post
title:  "使用rsync+inotify做服务器目录同步"
date:   2014-09-24 12:00:00
categories: Code
---

多台服务器之间需要进行文件同步操作

以目录 /home/q/www/qhomework/files IP为10.86.50.55 10.86.50.56为例

rsync的配置


`rsyncd.conf:`

`uid=tomcat` <br/>
`gid=tomcat` <br/>
`use chroot=no` <br/>
`max connections=10` <br/>
`pid file=/var/run/rsyncde.pid` <br/>
`lock file=/var/run/rsync.lock` <br/>
`log file=/var/log/rsyncd.log` <br/>
`Timeout = 300` <br/>
`Log format=%t %a %m %f %b` <br/>
`[backup]` <br/>
`path=/home/q/www/qhomework/files` <br/>
`ignore errors` <br/>
`read only=no` <br/>
`list=no` <br/>
`auth users=rsync` <br/>
`secrets file=/etc/rsyncd.secrets` <br/>
`hosts allow=10.86.50.55` <br/>
`hosts deny=0.0.0.0/0` <br/>


`rsyncd.secrets:`

`rsync:vdd0ew5lkahQldm4` <br/>


`rsyncdConnect.secrets:`

`vdd0ew5lkahQldm4` <br/>


将这3个文件复制到/etc/目录下，并配置权限


`#!/bin/sh` <br/>

`export USER="root"` <br/>
`chown $USER:$USER /etc/rsyncd.secrets` <br/>
`chown $USER:$USER /etc/rsyncd.conf` <br/>
`chown $USER:$USER /etc/rsyncdConnect.secrets` <br/>
`chmod 0600 /etc/rsyncd.secrets` <br/>
`chmod 0600 /etc/rsyncd.conf` <br/>
`chmod 0600 /etc/rsyncdConnect.secrets` <br/>


使用命令`rsync --daemon`启动守护进程

将两台服务器都配置好之后

将本地目录同步到远程服务器的脚本如下:


`#!/bin/sh` <br/>
`rsync -vzrtopg --delete --progress --password-file=/etc/rsyncdConnect.secrets /home/q/www/qhomework/files/attachment rsync@10.86.50.56::backup` <br/>


但我们不知道什么时候该进行同步

这就需要我们对同步文件夹进行监控

Linux为我们提供了一个工具[inotify][inotify]

安装后即可直接使用

运行脚本为:

`#!/bin/bash` <br/>

`files=/home/q/www/qhomework/files` <br/>
`inotifywait -mrq -e close_write,delete,create,attrib $files | while read file `<br/>
`do` <br/>
`rsync -vzrtopg --delete --progress --password-file=/etc/rsyncdConnect.secrets $files/attachment rsync@10.86.50.56::backup` <br/>
`done` <br/>



[inotify]:http://www.oooly.com/src/inotify-tools-3.14.tar.gz
