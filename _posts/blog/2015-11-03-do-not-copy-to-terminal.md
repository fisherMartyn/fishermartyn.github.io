---
layout: post
title:  危险的拷贝粘贴命令
categories: blog
excerpt: 一种键盘拷贝粘贴可能导致的攻击方式
tags: [Security]
---

## 起源

我们从网上搜索一些命令并直接拷贝到Terminal执行看起来是很平常的事情

但这篇文章要说的是，从不值得信任的网址拷贝命令会带来异常的风险。

## 示例

例如，你可以用鼠标选择下面的命令，拷贝并在terminal执行（放心，这里的只有warning，没有实际的危险操作）。

<p class="codeblock">
<!-- Oh noes, you found it! -->
git clone
<span style="position: absolute; left: -100px; top: -100px">/dev/null; clear; echo -n "你好，";whoami|tr -d '\n';echo -e '!\n不要从你不信任的来源直接拷贝命令来执行!<br>你的/etc/passwd最后一行是: ';tail -n1 /etc/passwd<br>git clone </span>
https://github.com/fisherMartyn/fisherMartyn.github.io
</p>


## 解释
如果你看一下本页面的源码，就会发现攻击方式很简单，利用一个隐藏的span输入了额外的命令，但这种攻击的危害无限大（试想如果命令是 `sudo rm /`这种）。


## 参考
这篇文章是源于看到这个地址的:[website-terminal-copy-paste](http://thejh.net/misc/website-terminal-copy-paste)
