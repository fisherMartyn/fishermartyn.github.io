---
layout: post
title : 天朝必备技能
categories: blog
excerpt: 大(Great)火(Fire)墙(Wall)，你懂得
tags: [vpn]
---

我梦寐以求，是真爱和自由。

## 天朝必备技能

看图说话：

![Geometric pattern with fading gradient](/images/fq.png)

##  主要步骤

1、申请国外的vps（例如linode或者amazon ec2），地点最好选美国。

2、选择linux或者unix操作系统。

3、安装PPTP服务器。

4、路由规则管理。

##  vps

用了amazon ec2的服务器，物理位置在加利福尼亚州北部。

## 操作系统

用了Amazon的Aws系统，也是REHL系列，基本软件安装用yum就够了。

## PPTP服务器安装和配置

### 软件安装

基本的软件安装主要是pptpd，我是从[这里](http://poptop.sourceforge.net/yum/stable/rhel6/x86_64/pptpd-1.4.0-1.el6.x86_64.rpm)下载的pptpd文件。

使用`yum localinstall xxx`(刚才下载的文件名)进行安装。

### 修改pptpd options配置文件

在rehl相关系统上是`/etc/ppp/options.pptpd`

{% highlight php %}

ms-dns 8.8.8.8 #使用Google DNS
ms-dns 8.8.4.4 #使用Google DNS

{% endhighlight %}

### 增加用户名和密码

修改`/etc/ppp/chap-secrets`文件，增加用户名、密码和ip地址的限制。
{% highlight php %}
# Secrets for authentication using CHAP
# client        server  secret                  IP addresses
myusername pptpd mypasswd *
{% endhighlight %}
把`myusername`和`mypasswd`改成自己的用户名和密码，`*`表示不对ip地址进行限制

### 修改pptpd配置文件

文件位置是`/etc/pptpd.conf`，增加local ip和remote ip的地址段。

{% highlight php %}

localip 192.168.9.1  #localip地址
remoteip 192.168.9.11-30 #remoteip地址

{% endhighlight %}


## 增加路由表规则

###   修改sysctl

编辑`/etc/sysctl.conf`文件，修改内容主要包括：
{% highlight php %}

net.ipv4.ip_forward = 1 #打开

{% endhighlight %}

并确保执行了`sysctl -p`重启该服务

### 利用iptables进行管理

修改iptables的nat转发:

{% highlight php %}

sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

{% endhighlight %}

## 重启服务

{% highlight php %}
service pptpd start
chkconfig pptpd on
{% endhighlight %}

## iphone的连接

利用iphone自带的vpn，利用pptp协议连接即可。

## 其它说明

1、按照上面的流程，Amazon Japan ECS貌似不好使了，换成了美国加利福尼亚北部以后，可以正常连接。

2、Amazon ECS默认是只开放了22端口，需要自己在安全组里打开1723端口。

3、日志的位置是`/var/log/message`，出现问题可以在这里查看。


 参考

1、[Amazon Ec2上搭建pptp服务器](http://www.yzhang.net/blog/2013-03-07-pptp-vpn-ec2.html)

2、[centos6.4 安装配置 pptp vpn](http://5323197.blog.51cto.com/5313197/1285738)

3、[在Ubuntu上安装pptp vpn服务器](http://blog.fens.me/ubuntu-vpn-pptp/)

