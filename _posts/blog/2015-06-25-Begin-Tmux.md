---
layout: post
title: 开始使用Tmux
categories: blog
excerpt:
tags: []
---

tmux是指通过一个终端登录远程主机并运行后，在其中可以开启多个控制台的终端复用软件。

## 开始之前

一直没用类似于screen和tmux这种终端管理工具，最近决定从iterm2转到tmux，理由主要在于tmux强大的窗体存储和恢复功能。

tmux的主要用途在于远程服务器（也可以是本机）工作环境的保存和恢复，即不同机器之间编辑环境的共享和恢复。

在用tmux之前，首先要付出一定学习成本，主要就是学习基本概念。当然用一段时间再回来学习基本概念也是可行的，甚至更好。用好tmux要理解的三个概念：

* Session ：一个终端连接的任务。可以包含一组窗体。
* Window：单个可见的窗口，类似于vim和iterm2中的tab。可以包含多个窗格。
* Pane：窗格，即被划分成小个的窗口。类似于vim中的split。

## 基本操作

tmux的所有操作由`Prefix-Command`和快捷键组成，即所有快捷键前面必须加上前置操作，tmux中默认的前置操作是`Ctrl+b`。


### tmux命令

操作         | 快捷键
------------ | ------------
打开tmux     | $tmux
恢复tmux     | $tmux attach


### Session相关

操作         | 快捷键
------------ | -----------
切换session  | prefix + s
离开session  | prefix + d
命名session  | prefix + $

### 窗口相关

操作         | 快捷键
------------ | -----------
新建窗口     | prefix + c
关闭窗口     | prefix + &
切换窗口     | prefix + 窗口号
重命名窗口   | prefix + ,

### Panel相关

操作               | 快捷键
-----------------  | -----------
水平拆分窗格       | prefix + %
垂直拆分窗格       | prefix + "
暂时将窗格放大/返回| prefix + z
关闭面板           | prefix + x
显示面板编号       | prefix + q
选择面板           | prefix + o/方向键


## 其他操作

操作               | 快捷键
-----------------  | -----------
滚屏               | prefix + [


## tmux命令行

操作             | 作用        | 建议alias
--------         | -------     | ----------
tmux ls          | 查看所有连接| tls
tmux a -t [name] | 连接        | tat

## 参考链接

* cenalulu<https://cenalulu.github.io/linux/tmux/>
* 酷夏至末的博客<http://blog.csdn.net/stelalala/article/details/9025691>
