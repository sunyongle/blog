---
layout: post
title:  "PHP 抽象方法和抽象类(abstract)"
date:   2017-2-24 15:20:01 +0800
categories: PHP
tag: 入门
---

* content
{:toc}
在OOP语言中，一个类可以有一个或多个子类。每个类至少都会有一个公有方法作为外部代码访问其的接口，而抽象方法就是为了方便继承引入的。

那么什么是抽象方法？在类中定义没有方法体的方法就是抽象方法。`没有方法体指的是在声明方法时没有大括号{}，及没有其中的内容，而是在方法名后直接加上;结束。另外，声明抽象方法时还需要加 abstract 关键字来修饰`。

如：

```PHP

abstract function test();
abstract function read();

```

那么什么是[抽象类](https://secure.php.net/manual/zh/language.oop5.abstract.php)呢？`在类中，只要有一个方法是抽象方法，那么这个类就要定义成抽象类。`抽象类也要使用 “abstract” 关键字来修饰，但是抽象类中可以有不是抽象的方法和成员属性。

如：

```PHP

abstract class Person{
	public $name;

	function __construct($name){
		$this->name = $name;
	}

	//强制要求子类定义这些方法
	abstract function say();
	abstract function run();


	//普通方法
	function eat(){
		echo 'eat something';
	}
}

class Test extends Person{
	function say(){
		echo $this->name.'<br />';
	}

	function run(){
		echo 'I can run'.'<br />';
	}
}


//抽象类不能产生实例对象，这样是错的，实例化对象由子类实现
$person = new Person('lin');


//子类可以产生实例对象，因为子类实现了父类的所有抽象方法
$test = new Test('lin');
$test->say();				//lin
$test->run();				//I can run
$test->eat();				//eat something

```

注意，抽象类不能产生实例对象，所以不能直接使用。我们把抽象类作为子类重载的模板使用。定义抽象类相当于定义了一套规范，要求子类去遵循这些规范，子类继承抽象类后`必须把父类中所有的抽象方法全部实现，否则子类中仍然存在抽象方法，仍然是抽象类，不能实例化！`
