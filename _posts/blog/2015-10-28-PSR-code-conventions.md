---
layout: post
title: PHP的PSR编码规范
categories: blog
excerpt: 一种PHP编码规范
tags: [PHP]
---

## PSR
PSR，也就是PHP Standard Recommendation，是PHP-FIG委员会指定的编码实践，主要用于解决php编码过程中最常遇到的相关问题，其中PSR-1和PSR-2标准是和编码规范相关的。

在这里将PSR规范中和编码规范相关的内容总结提炼一下。

## PSR-1
PSR-1描述了最基本的编码规范，也比较简单，描述如下：

* PHP tags
  * PHP代码只能使用`<?php ?>`和 `<?= ?>`的tag包括

* Encoding
  * PHP文件必须用UTF-8编码

* 类
  * 一个PHP文件里不要同时做定义符号（`class`、`trait`、`function`，`constant`等）和实际动作（产生输出或者操作数据）
* 类名
  * 类名采用首字母大写的驼峰式，例如`CamelCase`、`TitleCase`
* 常量名
  * 常量必须全部用大写字母，中间用下划线区分单词，例如`GREAT_SCOTT`。
* 方法名
  * 方法名采用首字母小写的驼峰式，例如`camelCase`、`phoIsAwesome`

## PSR-2
PSR-2是更严格的编码指导：

* 执行PSR-1
  * PSR-2需要先执行PSR-1
* 缩进
  * 关于缩进的争论由来已久，但PHP的缩进使用4个空格
* 文件和行
  * PHP文件必须使用Unix linefeed（LF）结尾格式，即**必须使用空行**作为结尾
  * 文件不能包含?>作为结尾的标志
  * 每行**不应该**超过80个字符，**禁止**超过120个字符
  * 行尾不能包含多余的空格
* 关键字
  * 所有的php关键字使用全小写，例如`true`,`false`
* 命名空间
  * 命名空间的声明后面要有空行，如果要引入或者重命名命名间（`use`），在`use`后要有空行，例如：

{% highlight PHP %}
<?php
  namespace My\Component;

  use Symfony\Components\HttpFoundation\Request;
  use Symfony\Components\HttpFoundation\Response;

  class App 
  {
    // Class definition body
  }
{% endhighlight %}

* 类
  * 类声明的左侧和右侧大括号必须新起一行，和类名对齐
  * 如果类使用了`extends`或者`implements`关键字，需要和类名同一行

{% highlight PHP %}
<?php
  namespace My\App;

  class Adminstrator extends User
  {
    //Class defniation body
  }

{% endhighlight %}

* 方法
  * 方法括号的使用和类的一致，不再加以详细说明

* 可见性
  * 对于类的属性和方法，必须指定可见性，只使用`public`，`private`和`protected`中的一个
  * `abstract`和`final`关键字放在可见性之前，`static`关键字放在可见性之后

* 控制结构
  * 控制关键字，例如`if`，`elseif`，`try`等等，后面要跟一个空格
  * 如果控制关键字需要加括号，左括号需要和关键字在一行中
{% highlight PHP %}
<?php
$gorilla = new \Animals\Gorilla;
$ibis = new \Animals\StrawNeckedIbis;

if ($gorilla->isAwake() === true) {
  do {
    $gorilla->beatChest();
  } while ($ibis->isAsleep() === true);

  $ibis->flyAway();
}


{% endhighlight %}

## 其它

PSR-3定义了一个Logger相关的interface。

PSR-4定义了php的autoloader的策略。

关于编码规范的工具，例如[PHP Code Sniffer](https://pear.php.net/manual/en/package.php.php-codesniffer.intro.php) 和 [PHP-CS-Fixer](http://cs.sensiolabs.org/)
