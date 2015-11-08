---
layout: post
title : 天朝必备技能
categories: blog
excerpt:
tags: [think]
---

我梦寐以求，是真爱和自由。

## 天朝必备技能

看图说话：

![Geometric pattern with fading gradient](/images/fq.png)

##  主要步骤

1、申请国外的vps（例如linode或者amazon ec2），阿里云之流是不行的。

2、选择linux或者unix操作系统。

3、安装PPTP服务器。

4、路由规则管理。

##  vps

用了amazon ec2的服务器，物理位置在Japan。

## 操作系统

用了centos，基本软件安装用yum或者rpm就够了。

## PPTP服务器安装和配置

### 软件安装

基本的软件安装主要是ppp、iptables和pptpd，ppp和iptables一般是系统自带的，需要自己安装pptpd，如果下载的pptpd版本太高，可以从[这里](http://rpmfind.net/linux/rpm2html/search.php?query=pptpd)下载一个版本较低的pptpd文件进行安装。

### 修改pptpd options配置文件

在rehl相关系统上是`/etc/ppp/options.pptpd`，在ubuntu等发布系统上是`/etc/ppp/pptpd-options`

{% highlight php %}

name pptpd #pptpd服务器名称
refuse-pap    #拒绝pap身份认证模式
refuse-chap   #拒绝chap身份认证模式
refuse-mschap #拒绝mschap身份认证模式
require-mschap-v2  #允许mschap-v2身份认证模式
require-mppe-128  #允许mppe 128位加密身份认证模式
ms-dns 8.8.8.8 #使用Google DNS
ms-dns 8.8.4.4 #使用Google DNS
proxyarp  #arp代理
debug #调试模式,可选
dump #服务启动时打印出所有配置信息，可选
lock #锁定TTY设备
nobsdcomp  #禁用BSD压缩模式
novj  #禁用vj压缩模式
novjccomp  #禁用vj压缩模式
nologfd #禁用stderr的log
idle 2592000 #设置idel时间？

{% endhighlight %}

### 增加用户名和密码

修改/etc/ppp/chap-secrets文件，增加用户名、密码和ip地址的限制。
{% highlight php %}
# Secrets for authentication using CHAP
# client        server  secret                  IP addresses
myusername pptpd mypasswd *
{% endhighlight %}
把`myusername`和`mypasswd`改成自己的用户名和密码，`*`表示不对ip地址进行限制

### 修改pptpd配置文件

文件位置是`/etc/pptpd.conf`，增加local ip和remote ip的地址段。

{% highlight php %}

option /etc/ppp/options.pptpd #指定pptpd options文件位置
logwtmp #使用wtmp(5)来记录客户端连接log
localip 192.168.0.1  #localip地址
remoteip 192.168.0.234-238,192.168.0.245 #remoteip地址

{% endhighlight %}


## 增加路由表规则

###   修改sysctl
编辑`/etc/sysctl.conf`文件，修改内容主要包括：
{% highlight php %}

net.ipv4.ip_forward = 1 #打开
# net.ipv4.tcp_syncookies = 1 #注释掉

{% endhighlight %}

并确保执行了`sysctl -p`重启该服务

利用iptables进行管理

修改iptables的nat转发:
{% highlight php %}

#下面两条命令都可以，如果上面的报错或者异常，可以尝试下面的
sudo iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth0 -j MASQUERADE
sudo iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth0 -j SNAT --to-source xxx.xxx.xxx.xxx

#注意保存和重启
sudo service iptables save
sudo service iptables restart
{% endhighlight %}

其中`-s`参数后面跟的是上面的`/etc/pptpd.conf`中的地址字段，`--to-source`后面跟的是自己vps的公网ip地址，`-o`参数后面跟的是公网的网卡名。

## iphone的连接

利用iphone自带的vpn，利用pptp协议连接即可。

 参考

1、[centos6.4 安装配置 pptp vpn](http://5323197.blog.51cto.com/5313197/1285738)

2、[在Ubuntu上安装pptp vpn服务器](http://blog.fens.me/ubuntu-vpn-pptp/)

