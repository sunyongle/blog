---
layout: post
title:  "PHP 对象接口"
date:   2017-03-09 16:20:01 +0800
categories: PHP
tag: 入门
---

* content
{:toc}

### 对象接口

继承特性简化了对象、类的创建，增强了代码的可重性。但PHP只支持单继承，如果想实现多重继承就只能使用类的`接口(interface)`。
那么什么是[接口](http://www.php.net/manual/zh/language.oop5.interfaces.php) ，如果一个抽象类里面所有的方法都是[抽象方法](https://lintaoonline.github.io/2017/02/24/php-abstract/),并且没有声明变量（可声明常量），而且其中的所有的成员权限都是`public`，那么这种特殊的抽象类就是接口，一般约定接口总以字母 I 或者 i 开头。


### 定义

接口使用关键字`interface`来定义


### 实现

要实现一个接口，使用`implements`操作符。类中必须实现接口中定义的所有方法，否则会报一个致命错误。类可以实现多个接口，用逗号来分隔多个接口的名称。
同时，也可以在继承一个类时实现多个接口。
如：

```php

class 子类 extends 父类 implemtns 接口1, 接口2, ...
{
    ......
}

```


tips:

```php

1 实现多个接口时，接口中的方法不能有重名。
2 接口也可以继承，通过使用 extends 操作符。
3 接口也可以继承，通过使用 extends 操作符。

```


### 常量

接口中也可以定义常量。接口常量和类常量的使用完全相同，但是不能被子类或子接口所覆盖。


实例：

```php

interface IUser{
	const NAME = '我是接口';				//定义常量
	public function getDiscount();			//抽象方法
	public function getUserType();			//抽象方法
}


class VipUser implements IUser{
	//通过implements实现,多个接口class VipUser implements IUser,IUser2{}
	
	private $discount = 0.8;
	public function getDiscount(){			//子类必须全部实现接口中的方法
		return $this->discount;
	}

	public function getUserType(){
		return 'VIP用户';
	}
}


class Good{
	private $price = 100;

	function buy($user){
		$discount = $user->getDiscount();
		$usertype = $user->getUserType();
        	echo $usertype."商品价格：".$this->price*$discount;
	}
}

$display = new Good();
$display ->buy(new VipUser());				//可以是普通用户
echo "<br />";
echo IUser::NAME;

程序输出：
VIP用户商品价格：80
我是接口

```


### 接口与抽象类的区别

接口是特殊的抽象类，可以看做是一个模型的规范。其大致的区别如下：

* 一个子类如果implements一个接口，就必须实现接口中所有的方法(不论是否需要)。如果继承抽象类，只需实现需要的方法即可。

* 如果一个接口中的方法改变了，那么所有实现此接口的子类需要同步更新方法名。而抽象类中的方法名改变了，其子类对应的方
法名不受影响，只是变成了一个新方法而已(相对老的方法实现)。

* 抽象类只能继承，当一个子类需要实现的功能来自继承于多个父类时，就必须使用接口。



