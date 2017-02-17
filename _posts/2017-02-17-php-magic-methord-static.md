---
layout: post
title:  "PHP 类的静态成员属性与静态方法 static 关键字"
date:   2017-2-17 14:56:01 +0800
categories: PHP
tag: 经典面试
---

* content
{:toc}
## 静态static


把类成员或方法申明为static，就可以不实例化类而直接访问，不能通过一个对象实例来访问静态成员（静态方法除外），静态成员属于类，不属于任何对象实例，但类的对象实例都能共享。

例子:
```PHP
class Person{
	public static $country  = '中国';
	
	static function say(){
	//内部访问静态成员属性
	echo "我是".self::$country."人<br />";
	}
}


class Worker extends Person{
	function study(){
	echo "我也来自".parent::$country."<br />";
	}
}


echo Person::$country."<br />";				//中国
Person::say();						//我是中国人

$p = new Person();
//$p->country; 错误写法！！！
$p->say();						//我是中国人

echo Worker::$country."<br />";				//中国
$p2 = new Worker();
$p2->study();						//我也来自中国
```


运行以上代码，输出：

```PHP
中国
我是中国人
我是中国人
中国
我也来自中国
```

小结:
在类的内部访问静态成员或属性时，使用`self::`(注意不是$self)，如：

```PHP
self::$country
self::say()
```

在子类内部访问父类属性方法时，使用`parent::`(注意不是$parent::)，如：

```PHP
parent::$country
parent::say()
```

外部访问静态成员或方法时，使用 类名/子类名::，如：

```PHP
Person::$country
Person::say()
Worker::study()
```

需要注意的是，静态方法可以通过普通对象的形式访问！