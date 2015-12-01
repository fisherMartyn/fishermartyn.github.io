---
layout :  post
title : Vim一些插件
categories: blog
excerpt: 
tags: [vim]
---

本文主要讲述vim的一些常用的插件

### 插件管理工具Vundle

<a href= "https://github.com/gmarik/Vundle.vim">Vundle</a>可以使得人们优雅的对各个vim插件进行管理

对于大部分插件的安装，你需要做的就是在vimrc中增加一行，然后执行一个`:BundleInstall`即可

### 高亮颜色molokai

高亮颜色是提升编程效率的重要工具，有各种颜色主题，我们要做的就是选择一款，使用一段时间，形成自己的习惯。

<a href = "https://github.com/tomasr/molokai">molokai</a>是一款个人认为比较简单而且普遍评价比较高的插件了。

### 文件导航插件nerdtree

vim同样可以做到很多IDE的文件导航功能，这个插件 <a href="github.com/scrooloose/nerdtree">nerdtree</a>，只需简单的配置并进行简单的学习，就可以实现一个比较好用的文件导航功能

### 类和方法导航插件 tagbar

合理的使用方法导航和类导航能在一定程度上提升开发效率，<a href = "https://github.com/scrooloose/nerdtree">tagbar</a>的插件还可以通过配置来支持例如markdown和css等语言

### 自动补全

<a href = "https://github.com/Valloric/YouCompleteMe">YCM(YouCompleteMe)</a>是目前在用的补全工具，对于c系列的语言其支持语法分析，而对于php和perl等，只是做了利用vim自带的omni补全，不过也基本够用

### 感谢和其它

感谢<a href = "http://yuez.me/jiang-ni-de-vim-da-zao-cheng-qing-qiao-qiang-da-de-ide/">这篇文章</a>，另外也参考了<a href="http://www.zhihu.com/question/22904741">知乎上关于vim的讨论</a>

最后上一张图吧：

![Geometric pattern with fading gradient](/images/myvim.png)
