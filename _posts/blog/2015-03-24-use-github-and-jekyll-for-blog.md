---
layout: post
title: 利用github和Jeklly搭建博客
categories: blog
excerpt:
tags: []
---
阮一峰说人们喜欢博客有三个阶段:

* 第一个阶段是使用免费的平台
* 第二个阶段有了一些定制化的需求，自己购买云主机、域名并搭建类似wordpress之类的博客
* 第三个阶段往往嫌麻烦，希望自己定制上层但不想操心主机和服务

在使用LAMP架构和wordpress搭建了一年以后，深刻的感受到其不方便之处。时间一长，主机和域名是比不小的开销；服务不稳定，经常需要重启服务；带宽受限制，不同的网络访问速度不同等。

最近终于有时间把利用github和Jekkly搭建一个轻量级的博客，想写一些短小而精致的东西，首先介绍下使用github和Jeklly构建博客。

1、创建github主页代码仓库

github掀起了社会化coding浪潮，github的产生在我看来是coding界一个重要的里程碑，除了社会化coding平台，git本身也是分布式版本管理的最好工具（没有之一），如果你还不会git，可能这篇文章不适合你，不过你可以看看这里:<a href = "https://rogerdudler.github.io/git-guide/">git简明教程</a>，除了git本身，你还需要学习下github的工作流，这里就不予介绍了。

我们的工作是在github上创建一个账户，例如:<a href = "https://github.com/fisherMartyn">这里</a>，并创建一个新的repository（代码仓库），代码仓库的名字为:yourName.github.io，其中yourName要改成你的github用户名。

这样你就有了一个拥有上层权限而主机和服务完全由github来托管的博客平台了。

2、使用Jeklly创建博客模板

Jeklly是一个博客模板，你可以理解成wordpress那样，但只是静态的页面，并能够和github进行完美的配合。使用Jeklly需要一定的学习量，但并不难，看看<a href="http://jekyllcn.com/">Jeklly中文</a>。

学习了Jeklly的配置、使用后，我们可以创建自己的博客了，但样式设计和排版不是所有人都能handle的，因此可以看看一些现成的模板，怎么获得模板呢？用git！看看这些主题吧，总有一款适合你：<a href="http://jekyllthemes.org/">主题</a>

3、社会化评论接口

上面我们的工作搭建了一个静态的博客，但没有动态的内容，例如评论。 我们可以采用第三方评论系统结合Jeklly来实现评论的功能。

社会化评论平台包括典型的disqus、国内的有搜狐畅言、有言等等，我们这里采用的是disqus。

注册disqus账号，增加到网页，采用add Disqus To Site，填写一些基本资料，最后生成一串js和html的页面。最简单的方式是在Jeklly的_include文件中创建一个disqus.html文件，将代码粘贴进去，然后在_posts的文章中增加对disqust.html的引用即可。

总结:

茫茫人海，滚滚红尘，你曾为做过的愚蠢的事情而高兴，你也曾为妄图纠正自己的愚蠢矫枉过正，而最终你才会明白自己想要的是什么，Only Follow Your Heart。
