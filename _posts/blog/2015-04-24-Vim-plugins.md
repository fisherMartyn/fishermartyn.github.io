---
layout :  post
title : Vim一些插件
categories: blog
excerpt: 
tags: []
---

仔细想想自己用Vim作为编辑器也是好几年的事情了，当时也只是为了用而用，觉得连wind$s上的notepad++都比vim好用，为什么大家却称vim为编辑器之神，直到现在都不敢说懂得了vim的精髓，不过慢慢玩了一些插件，感觉到自己对于vim的使用脱离了最基础的水平，因此本文可以称为vim插件的进阶吧。

### 插件管理工具Vundle

通常的管理工具都是为了减少人们的管理成本，这也意味着如果不使用管理工具同样可以进行插件安装和管理，工具只是让人们更方便而已。

不过用工具还是会简单一些，例如这个工具<a href= "https://github.com/gmarik/Vundle.vim">Vundle</a>，只需要简单的添加一个插件名，并执行一条命令，即可实现插件的自动安装，不过其只负责插件的安装，配置仍然需要手动操作。

有时，工具会让插件的使用的成本降低，但同时降低的还有你对vim插件配置过程的理解。

### 高亮颜色molokai

高亮颜色是提升编程效率的重要工具，有各种颜色主题，我们要做的就是选择一款，使用一段时间，形成自己的习惯。

<a href = "https://github.com/tomasr/molokai">molokai</a>是一款个人认为比较简单而且普遍评价比较高的插件了。

### 文件导航插件nerdtree

vim同样可以做到很多IDE的文件导航功能，这个插件 <a href="github.com/scrooloose/nerdtree">nerdtree</a>，只需简单的配置并进行简单的学习，就可以实现一个比较好用的文件导航功能。

### 类和方法导航插件 tagbar

在写代码时，合理的使用方法导航和类导航能在一定程度上提升开发效率，虽然个人认为需要这种导航的文件通常都需要进行拆分了。

不过<a href = "https://github.com/scrooloose/nerdtree">tagbar</a>的插件还可以通过配置来支持例如markdown和css等语言。

### 自动补全

<a href = "https://github.com/Valloric/YouCompleteMe">YCM(YouCompleteMe)</a>目前普遍认为是比较好用的自动补齐插件，不过配置和安装的过程比较复杂，而且容易出问题，不过还是十分简易。工欲善其事，必先利其器，恩。

### vim中执行命令

这个插件<a href= "https://github.com/christoomey/vim-run-interactive">vim-run-interactive</a>不退出vim而进行命令的执行，如果通过配置成快捷键，则更简单一些。


### 感谢和其它

感谢看到了<a href = "http://yuez.me/jiang-ni-de-vim-da-zao-cheng-qing-qiao-qiang-da-de-ide/">这篇文章</a>，让我重新燃起了折腾下vim的兴趣。

本文的目的只是通过进行插件的介绍，因为无论多么详细的记录都会根据个人的系统和情况存在响应的问题，因此还列出了各个插件的github仓库，插件的作者应该是最权威的说明者吧。

最后上一张图吧：

![Geometric pattern with fading gradient](/img/myvim.png)
