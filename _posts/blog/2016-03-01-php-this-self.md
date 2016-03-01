---
layout: post
title: 深度理解PHP中的this和self
categories: blog
excerpt:  this是实例，self是静态？实际上不仅如此
tags: [Class, PHP]
---

最近在深入研究面向对象，但是PHP中的一个关于this和self用法的例子引发了我的好奇。

首先看一段启发代码：

{% highlight php %}
<?php
class A
{
    public static $id = 0;
    public function test()
    {
        self::load();  // 看这里
        $this->load(); // 再看这里
    }
    public function load()
    {
        echo "A load()",PHP_EOL;
    }
}

class B extends A
{
    public function test()
    {
        parent::test();
    }
    public function load()
    {
        echo "B load()",PHP_EOL;
    }
}
$c  = new B();
$c->test();
{% endhighlight %}

运行的结果是:
A load()
B load()

我看到这个结果的时候，有两个惊讶：

1. self居然还能调用非静态方法！
2. self调用的方法居然得到的是A的结果！

查了一些文档，包括<a href="http://stackoverflow.com/questions/151969/when-to-use-self-over-this"> stackoverflow</a>上的解释和<a href="http://board.phpbuilder.com/showthread.php?10354489-RESOLVED-php-class-quot-this-quot-or-quot-self-quot-keyword">一些博客</a>，理解到如下问题：

1. self常用于访问当前类的静态方法，但也可以访问实例方法。self只是当前类名的一个简写。
2. $this指的是当前的实例，调用的方法要基于具体的类，会`默认进行向下转换`。
3. 学到了关于<a href="http://php.net/manual/en/language.oop5.late-static-bindings.php">后期静态绑定</a>的一些东西，主要是self、static和parent的区别。

看下面例子：

{% highlight php %}
<?php
class A
{
    public function test()
    {
        self::load();
        static::load();
        $this->load();
    }
    public static function load()
    {
        echo "A load()",PHP_EOL;
    }
}

class B extends A
{
    public function test()
    {
        parent::test();
    }
    public static function load()
    {
        echo "B load()",PHP_EOL;
    }
}
$c  = new B();
$c->test();
{% endhighlight %}

结果是：
A load()
B load()
B load()

是的，同样令人震惊的有两点：

1. $this也可以调用静态方法
2. `static::load()`能够做到后期静态绑定，也就是会调用B的方法；而`self::load()`只是当前类。
