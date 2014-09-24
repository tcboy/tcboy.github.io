---
layout: post
title:  "使用rsync+inotify做服务器目录同步"
date:   2014-09-24 12:00:00
categories: Code
---

多台服务器之间需要进行文件同步操作

以目录 /home/q/www/qhomework/files IP为10.86.50.55 10.86.50.56为例

rsync的配置:
`rsyncd.conf`
{% highlight %}
uid=tomcat
gid=tomcat
use chroot=no
max connections=10
pid file=/var/run/rsyncde.pid
lock file=/var/run/rsync.lock
log file=/var/log/rsyncd.log
Timeout = 300
Log format=%t %a %m %f %b
[backup]
path=/home/q/www/qhomework/files
ignore errors
read only=no
list=no
auth users=rsync
secrets file=/etc/rsyncd.secrets
hosts allow=10.86.50.55
hosts deny=0.0.0.0/0
{% endhighlight %}

`rsyncd.secrets`
{% highlight %}
rsync:vdd0ew5lkahQldm4
{% endhighlight %}

`rsyncdConnect.secrets`
{% highlight %}
vdd0ew5lkahQldm4
{% endhighlight %}

将这3个文件复制到/etc/目录下，并配置权限
{% highlight bash %}
#!/bin/sh

export USER="root"
chown $USER:$USER /etc/rsyncd.secrets
chown $USER:$USER /etc/rsyncd.conf
chown $USER:$USER /etc/rsyncdConnect.secrets
chmod 0600 /etc/rsyncd.secrets
chmod 0600 /etc/rsyncd.conf
chmod 0600 /etc/rsyncdConnect.secrets
{% endhighlight %}

使用命令`rsync --daemon`启动守护进程

将两台服务器都配置好之后

将本地目录同步到远程服务器的脚本如下:
{% highlight bash %}
#!/bin/sh
rsync -vzrtopg --delete --progress --password-file=/etc/rsyncdConnect.secrets /home/q/www/qhomework/files/attachment rsync@10.86.50.56::backup
{% endhighlight %}

但我们不知道什么时候该进行同步

这就需要我们对同步文件夹进行监控

Linux为我们提供了一个工具[inotify][inotify]

安装后即可直接使用

运行脚本为:

{% highlight bash %}
#!/bin/bash

files=/home/q/www/qhomework/files
inotifywait -mrq -e close_write,delete,create,attrib $files | while read file 
do
        rsync -vzrtopg --delete --progress --password-file=/etc/rsyncdConnect.secrets $files/attachment rsync@10.86.50.56::backup
done
{% endhighlight %}


[inotify]:http://www.oooly.com/src/inotify-tools-3.14.tar.gz


