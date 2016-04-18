---
layout: post
title: PHP设计模式
categories: blog
excerpt: 介绍如何在PHP中使用设计模式
tags: [design pattern]
---

本文是翻译自<a href="https://www.script-tutorials.com/design-patterns-in-php/">Design Patterns in PHP</a>。

先看一张四人帮(GOF)的设计模式图：

![Geometric pattern with fading gradient](/images/gof_design_patterns.png)

PHP中的设计模式，具体来说，我们讨论的将是PHP在web开发中的设计模式。有经验的开发者可能对此有些熟悉，但这篇文章对新手程序员也会很有帮助。什么是设计模式？设计模式并不像分析模式，它们也不是像链表这种结构的普通的描述，更不是特殊的应用或者框架的设计。设计模式是"设计的相互通讯的对象和类的描述，用于解决特定环境下的通用设计问题"(descriptions of communicating objects and classes that are customized to solve a general design problem in a particular context)。换句话说,设计模式提供了一个通用的、可复用的解决方案，用于解决我们每天遇到的编程问题。设计模式不是可以直接用到你系统的既有的类库，因为它们不是具体的解决方案。它们是一种模式，或者模板，可以实现来解决不同的特定问题。设计模式能够加速开发过程，因为模板是固定的，用户只需要实现即可。设计模式不仅使软件开发更快捷，更是用简单的方式概括了重要的思想。另外，乱用设计模式可能会导致令人不爽的解决方案。除了理论以外，我们还将介绍设计模式基本的抽象和简单的例子。

> 每个模式都介绍了一个出现了一次又一次的问题...
> 它们描述了解决问题的核心，你可以使用一套解决方案一万次而不需要重复实现甚至两次 
> - Christopher Alexander

目前有23种不同的设计模式，根据用途可以分为三类：

* 创建型模式：用于创建对象，将其从实现系统中解耦
* 结构型模式：用于组织大型对象，组合不同的对象
* 行为型模式：用于管理算法、关系、对象间的职责

下面是全部设计模式的列表：

|  分类 | 设计模式 | 差异的方面 |
| :------| :-------| :-----|
| 创建模式   | 抽象工厂| 产品对象族类|
| 创建模式   | 生成器模式| 一个复杂对象是如何组装的|
| 创建模式   | 工厂方法| 实例化的子类对象|
| 创建模式   | 原型模式| 实例化的对象的类|
| 创建模式   | 单例模式| 一个类的唯一对象|
|----
| 结构模式 | 适配器模式| 对象的接口|
| 结构模式 | 桥接模式 | 对象的实现|
| 结构模式 | 组成模式 | 对象的结构和组成 |
| 结构模式 | 装饰模式 | 无子类的对象的职责|
| 结构模式 | 外观模式 | 子系统的接口 |
| 结构模式 | 享元模式 | 对象的存储消耗 |
| 结构模式 | 代理模式 | 对象如何被访问、位置|
| ----
| 行为模式 | 责任链模式 | 满足一个需求的对象 |
| 行为模式 | 命令模式 | 一个需求何时实现 |
| 行为模式 | 解释器模式| 语言的语法和解释|
| 行为模式 | 迭代器模式| 元素族如何被访问和遍历|
| 行为模式 | 中介者模式| 对象之间如何交互|
| 行为模式 | 备忘录模式| 私有信息如何在对象外部存储|
| 行为模式 | 观察者模式| 依赖于其他对象的一系列对象如何保持更新|
| 行为模式 | 状态模式| 对象的状态|
| 行为模式 | 策略模式 |算法|
| 行为模式 | 模板方法 | 算法的步骤|
| 行为模式 | 访问者模式| 应用到类上的操作|
{: .table}

### 单例模式

单例模式是最常用的设计模式。开发应用时，访问一个类的唯一实例是很常见的场景。单例模式可以达到这个效果：

{% highlight php %}
<?php
final class Product
{
    private static $instance;
    public $mixed;
    
    public static function getInstance() {
        if (!(self::$instance instanceof self)) {
            self::$instance = new self();
        }
        return self::$instance;
    }
    
    private function __clone() {
    }
    private function __construct() {
    }
}

$firstProduct = Product::getInstance();
$secondProduct = Product::getInstance();
$firstProduct->mix = 'test';
$secondProduct->mix = 'example';

var_dump($firstProduct->mix);
var_dump($secondProduct->mix);


{% endhighlight %}

### 多单例

可能在项目中需要一系列的单例：

{% highlight php %}
<?php

abstract class FactoryAbstract {

    protected static $instances = array();

    public static function getInstance() {
        $className = static::getClassName();
        if (!array_key_exists($className, self::$instances) ||
!(self::$instances[$className] instanceof $className)) {
            self::$instances[$className] = new $className();
        }
        return self::$instances[$className];
    }

    public static function removeInstance() {
        $className = static::getClassName();
        if (array_key_exists($className, self::$instances)) {
            unset(self::$instances[$className]);
        }
    }

    final protected static function getClassName() {
        return get_called_class();
    }

    protected function __construct() { }

    final protected function __clone() { }
}

abstract class Factory extends FactoryAbstract {

    final public static function getInstance() {
        return parent::getInstance();
    }

    final public static function removeInstance() {
        parent::removeInstance();
    }
}

class FirstProduct extends Factory {
    public $a = [];
}
class SecondProduct extends FirstProduct {
}

FirstProduct::getInstance()->a[] = 1;
SecondProduct::getInstance()->a[] = 2;
FirstProduct::getInstance()->a[] = 3;
SecondProduct::getInstance()->a[] = 4;

print_r(FirstProduct::getInstance()->a);
// array(1, 3)
print_r(SecondProduct::getInstance()->a);
// array(2, 4)

{% endhighlight %}

### 策略模式

策略模式是基于算法。对于解决相同问题的不同算法，客户端类来调用不同的算法无需了解其实际实现。

{% highlight php %}
<?php
interface OutputInterface 
{
    public function load($arrayOfData);
}

class SerializedArrayOutput implements OutputInterface
{
    public function load($arrayOfData) {
        return serialize($arrayOfData);
    }
}

class JsonStringOutput implements OutputInterface
{
    public function load($arrayOfData) {
        return json_encode($arrayOfData);
    }
}

class ArrayOutput implements OutputInterface
{
    public function load($arrayOfData) {
        return $arrayOfData;
    }
}
$data = array(
    '1' => "first",
    '2' => "second",
);
var_dump((new SerializedArrayOutput())->load($data));
var_dump((new JsonStringOutput())->load($data));
var_dump((new ArrayOutput())->load($data));
{% endhighlight %}

### 装饰模式

装饰模式使得可以在运行时对对象添加新的行为：

{% highlight php %}
<?php 
abstract class MessageBoardHandler
{
    public function __construct(){}
    abstract public function filter($msg);
}

class MessageBoard extends MessageBoardHandler
{
    public function filter($msg)
    {
        return "common processing ".$msg;
    }
}
$msg = new MessageBoard();
echo $msg->filter("common message"),PHP_EOL;

//decorator
class MessageBoardDecorator extends MessageBoardHandler
{
    private $_handler = null;

    public function __construct($handler)
    {
        parent::__construct();
        $this->_handler = $handler;
    }

    public function filter($msg)
    {
        return $this->_handler->filter($msg);
    }
}

class HtmlFilter extends MessageBoardDecorator
{
    public function __construct($handler)
    {
        parent::__construct($handler);
    }

    public function filter($msg)
    {
        return "html processing and ".parent::filter($msg);;
    }
}
$htmldecorator = new HtmlFilter(new MessageBoard());
echo $htmldecorator->filter("some message"),PHP_EOL;
{% endhighlight %}

### 注册器

该模式有点特殊，因为它不是创建型模式。注册器，是hash，通过静态方法访问变量。

{% highlight php %}
<?php
class Package
{
    protected static $data = array();
    
    public static function set($key, $value) {
        self::$data[$key] = $value;
    } 
    
    public static function get($key) {
        return isset(self::$data[$key])? self::$data[$key] : null;
    }
    
    final public static function removeObject($key) {
        if (array_key_exists($key, self::$data)) {
            unset(self::$data[$key]);
        }
    }
}

Package::set('name', 'Package name');
var_dump(Package::get('name'));
{% endhighlight %}

### 工厂模式

这也是常用的模式，顾名思义，工厂方法就是产生对象的工厂。换句话说，我们只需要知道工厂，通过工厂来生产产品，而无需知道产品是如何被制造的，也不能通过别的渠道获取商品。

{% highlight php %}
<?php
interface Factory
{
    public function getProduct();
}

interface Product
{
    public function getName();
}

class FirstFactory implements Factory
{
    public function getProduct() {
        return new FirstProduct();
    }
}

class SecondFactory implements Factory
{
    public function getProduct() {
        return new SecondProduct();
    }
}

class FirstProduct implements Product
{
    public function getName() {
        return "The first Product";
    }
}

class SecondProduct implements Product
{
    public function getName() {
        return "The Second Product";
    }
}

$factory1 = new FirstFactory();
$product1 = $factory1->getProduct();
$factory2 = new SecondFactory();
$product2 = $factory2->getProduct();
var_dump($product1->getName());
var_dump($product2->getName());
{% endhighlight %}


### 抽象工厂

有时候我们有很多相似的工厂，并希望把选择的逻辑封装起来，这时候可以用到抽象工厂

{% highlight php %}
<?php
class Config
{
    public static $factory = 1;
}

interface Product {
    public function getName();
}

abstract class AbstractFactory
{
    public static function getFactory() {
        switch(Config::$factory) {
            case 1:
                return new FirstFactory();
                break;
            case 2:
                return new SecondFactory();
        }
        throw new Exception('bad config');
    }
    abstract public function getProduct();
}

class FirstFactory extends AbstractFactory {

    public function getProduct() {
        return new FirstProduct();
    }
}

class FirstProduct implements Product {

    public function getName() {
        return 'The product from the first factory';
    }
}

class SecondFactory extends AbstractFactory {
    public function getProduct() {
        return new SecondProduct();
    }
}

class SecondProduct implements Product {
    public function getName() {
        return 'The product from second factory';
    }
}

$firstProduct = AbstractFactory::getFactory()->getProduct();
Config::$factory = 2;
$secondProduct = AbstractFactory::getFactory()->getProduct();

var_dump($firstProduct->getName());
var_dump($secondProduct->getName());

{% endhighlight %}

### 观察者

可被观察的对象在变化时，会发送消息给注册的观察者。

{% highlight php %}
<?php
interface Observer
{
    function onChanged($sender, $args);
}

interface Observable
{
    function addObserver($observer);
}

class CustomerList implements Observable
{
    private $_observers = array();
    
    public function addCustomer($name) {
        foreach ($this->_observers as $obs) {
            $obs->onChanged($this, $name);
        }
    }
    
    public function addObserver($observer) {
        $this->_observers[] = $observer;
    }
}

class CustomerListLogger implements Observer
{
    public function onChanged($sender, $args) {
        echo( "'$args' Customer has been added to the list \n" );
    }
}

$ul = new CustomerList();
$ul->addObserver( new CustomerListLogger() );
$ul->addCustomer( "Jack" );
{% endhighlight %}

### 适配器模式

适配器可以将复用类，来提供新的接口：

{% highlight php %}
<?php
class SimpleBook {
    private $author;
    private $title;
    function __construct($author_in, $title_in) {
        $this->author = $author_in;
        $this->title  = $title_in;
    }
    function getAuthor() {
        return $this->author;
    }
    function getTitle() {
        return $this->title;
    }
}
class BookAdapter {
    private $book;
    function __construct(SimpleBook $book_in) {
        $this->book = $book_in;
    }
    function getAuthorAndTitle() {
        return $this->book->getTitle().' by '.$this->book->getAuthor();
    }
}

$book = new SimpleBook("Gamma, Helm, Johnson, and Vlissides", "Design
Patterns");
$bookAdapter = new BookAdapter($book);
echo 'Author and Title: '.$bookAdapter->getAuthorAndTitle();

{% endhighlight %}

### 延迟初始化

假设有一个工厂，不知道何时才需要产品，可以在使用的时候再初始化。

{% highlight php %}
<?php
interface Product {
    public function getName();
}

class Factory {

    protected $firstProduct;
    protected $secondProduct;

    public function getFirstProduct() {
        if (!$this->firstProduct) {
            $this->firstProduct = new FirstProduct();
        }
        return $this->firstProduct;
    }

    public function getSecondProduct() {
        if (!$this->secondProduct) {
            $this->secondProduct = new SecondProduct();
        }
        return $this->secondProduct;
    }
}

class FirstProduct implements Product {
    public function getName() {
        return 'The first product';
    }
}

class SecondProduct implements Product {
    public function getName() {
        return 'Second product';
    }
}


$factory = new Factory();

var_dump($factory->getFirstProduct()->getName());
var_dump($factory->getSecondProduct()->getName());
var_dump($factory->getFirstProduct()->getName());

{% endhighlight %}

### 响应链模式

该模式有另外一个名字，命令链模式。消息通过一系列的handler，由handler控制是否执行该查询。当有一个handler可以处理消息时处理过程停止。

{% highlight php %}
<?php

interface Command {
    function onCommand($name, $args);
}

class CommandChain {
    private $_commands = array();

    public function addCommand($cmd) {
        $this->_commands[]= $cmd;
    }

    public function runCommand($name, $args) {
        foreach($this->_commands as $cmd) {
            if ($cmd->onCommand($name, $args))
                return;
        }
    }
}

class CustCommand implements Command {
    public function onCommand($name, $args) {
        if ($name != 'addCustomer')
            return false;
        echo("This is CustomerCommand handling 'addCustomer'\n");
        return true;
    }
}

class MailCommand implements Command {
    public function onCommand($name, $args) {
        if ($name != 'mail')
            return false;
        echo("This is MailCommand handling 'mail'\n");
        return true;
    }
}

$cc = new CommandChain();
$cc->addCommand( new CustCommand());
$cc->addCommand( new MailCommand());
$cc->runCommand('addCustomer', null);
$cc->runCommand('mail', null);

{% endhighlight %}

### 对象池

利用hash来存储对象，使用的时候从对象池获取对象。

{% highlight php %}
<?php

class Product 
{
    protected $id;
    
    public function __construct($id) {
        $this->id = $id;
    }
    
    public function getId() {
        return $this->id;
    }
}

class Factory
{
    protected static $products = array();
    
    public static function pushProduct(Product $product) {
        self::$products[$product->getId()] = $product;
    }
    public static function getProduct($id) {
        return isset(self::$products[$id]) ? self::$products[$id] : null;
    }
    public static function removeProduct($id) {
        if (array_key_exists($id, self::$products)) {
            unset(self::$products[$id]);
        }
    }
}

Factory::pushProduct(new Product('first'));
Factory::pushProduct(new Product('second'));

var_dump(Factory::getProduct('first')->getId());
var_dump(Factory::getProduct('second')->getId());

{% endhighlight %}

### 原型模式

有时，对象会被初始化多次，这时保存初始化过程，特别是初始化需要耗费大量资源和时间时。原型模式是一种预先初始化，使用时clone的模式：

{% highlight php %}
<?php

interface Product 
{
}

class Factory
{
    private $product;
    
    public function __construct(Product $product) {
        $this->product = $product;
    }
    
    public function getProduct() {
        return clone $this->product;
    }
}

class SomeProduct implements Product
{
    public $name;
}

$prototypeFactory = new Factory(new SomeProduct());

$firstProduct = $prototypeFactory->getProduct();
$firstProduct->name = 'The first product';

$secondProduct = $prototypeFactory->getProduct();
$secondProduct->name = 'Second product';

var_dump($firstProduct->name);
var_dump($secondProduct->name);

{% endhighlight %}

### 生成器模式

组装复杂对象时，往往需要用到生成器模式：

{% highlight php %}
<?php
class Product 
{
    private $name;
    
    public function setName($name) {
        $this->name = $name;
    }
    
    public function getName() {
        return $this->name;
    }
}

abstract class Builder {
    protected $product;
    final public function getProduct() {
        return $this->product;
    }

    public function buildProduct() {
        $this->product = new Product();
    }
}

class FirstBuilder extends Builder {
    public function buildProduct() {
        parent::buildProduct();
        $this->product->setName('The product of the first builder');
    }
}

class SecondBuilder extends Builder {
    public function buildProduct() {
        parent::buildProduct();
        $this->product->setName('The product of second builder');
    }
}

class Factory {

    private $builder;
    public function __construct(Builder $builder) {
        $this->builder = $builder;
        $this->builder->buildProduct();
    }
    public function getProduct() {
        return $this->builder->getProduct();
    }
}

$firstDirector = new Factory(new FirstBuilder());
$secondDirector = new Factory(new SecondBuilder());

var_dump($firstDirector->getProduct()->getName());
var_dump($secondDirector->getProduct()->getName());

{% endhighlight %}
