---
layout: post
title: 服务管理工具Deamontools
categories: blog
excerpt: 一个service管理工具
tags: [运维]
---

# 简介

<a href="http://cr.yp.to/daemontools.html">Daemontools</a>是一个进程管理工具，其提供了一系列的工具来管理用户进程，当进程由于某些原因挂掉以后，deamontools会自动重启进程。

# 安装

Mac 下可以通过homebrew安装 : `brew install daemontools`，另外也可以通过<a href="http://cr.yp.to/daemontools/install.html">源码安装</a>。

# 启动

源码安装方式会创建`/command`目录，可以通过`/command/svscanboot &`来启动。

Homebrew的安装一般安装在`/usr/local/bin/svscanboot`目录，通过`/usr/local/bin/svscanboot &`来启动。

启动后检查进程运行情况`ps -ef |grep svs`，可以看到两个进程svscanboot和svscan。

另外，可以将其设置为<a href="http://cr.yp.to/daemontools/start.html">开机自启动</a>。

# 使用

1. 创建进程目录，例如`mkdir /opt/svc/nginx`。

2. 在该目录中创建run脚本，修改权限为755，脚本中启动进程，例如：

{% highlight bash %}

#!/bin/sh
exec /home/vagrant/nginx/sbin/nginx   #假设启动nginx进程

{% endhighlight %}

3. 创建符号链接: `ln -s /opt/svc/nginx /service/`，创建完成后，该进程会自动启动

# 验证

可以用`ps`查到nginx进程，并手动`kill`掉，再看会发现已经被daemontool启动起来了。

# 常用命令

1. 启动进程：`svc -u /service/nginx`
2. 关闭进程：`svc -d /service/nginx`
3. 查看进程：`svstat /service/nginx`
4. 重启进程：`svc -t /service/nginx`

可以使用通配符`*`来批量启动、重启。


