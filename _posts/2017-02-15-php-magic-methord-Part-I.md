---
layout: post
title:  "PHP魔术方法__set __get __isset __unset用法分析"
date:   2017-2-15 16:06:01 +0800
categories: PHP
tag: 入门
---

* content
{:toc}
通常来说，把类的属性定义为private总是符合现实逻辑的，但是，对属性的读取和赋值操作是非常频繁的。因此在PHP5中，预定义了两个函数__get()和__set()来获取和赋值其属性,以及检查属性的__isset()和删除属性的方法__unset()。


`__get()`方法用来获取私有成员属性值，它需要传入一个参数，传入你要获取的成员属性的名称，返回获取属性的值。


`__set()`方法给不可访问属性赋值，它有2个参数！第一个参数为你要设置的属性名，第二个参数为你要给属性设置的值，其没有返回值。
如果没有使用`__set()`方法，是不允许给不可访问属性直接赋值的。比如：$this->name = 'KOBE'，这样会报错。但是如果在类中加上了__set($property_name, $value)，即可达到赋值的目的。如果成员属性不封装成私有的，对象本身就不会去自动调用这个方法。


```PHP
 class Person{
    private $name;  
    private $sex;  
    private $age;  
    
    function __get($property_name){  
        echo "在直接获取私有属性值的时候，自动调用了这个__get()方法<br>";  
        if (isset($this->$property_name)) {  
            return ($this->$property_name);  
        } else {  
            return null;  
        }  
    }  
      
    function __set($property_name, $value) {  
        echo "在直接设置私有属性值的时候，自动调用了这个__set()方法为私有属性赋值<br>";  
        $this->$property_name = $value;  
    }  
} 

$man = new Person();
$man->name = 'Lin';
echo $man->name;
```

运行结果:

```
在直接设置私有属性值的时候，自动调用了这个__set()方法为私有属性赋值
在直接获取私有属性值的时候，自动调用了这个__get()方法
Lin
```

以上代码如果不加上__get()和__set()方法，程序就会出错，因为不能在类的外部操作私有成员，而上面的代码是通过自动调用__get()和__set()方法来帮助我们直接存取封装的私有成员的。


`__isset()`,我们先来看看[isset()](http://php.net/manual/zh/function.isset.php) — 检测变量是否设置，并且不是NULL。如果传入的变量存在则传回true，否则传回false。


那么是否可以在一个对象外部使用“isset()”去检测对象里面的成员是否被设置呢？有两种情况，如果对象里面成员是公有的，我们就可以使用这个函数来测定成员属性，如果是私有的成员属性，这个函数就不起作用了，原因就是因为私有的被封装了，在外部不可见。


问题来了，如何在对象的外部使用"isset()"函数来测定私有成员属性是否被设定了呢？


很简单，通过在类里面加上一个`__isset()`即可，当在类外部使用"isset()"函数来测定对象里面的私有成员是否被设定时，就会自动调用类里面的"__isset()"方法了帮我们完成这样的操作。

```PHP
function __isset($param){
	echo "当在类外部使用isset()函数测定私有成员$param时，自动调用<br>";  
    return isset($this->$param); 
}
```


`__unset()`,我们也先看看[unset()](http://php.net/manual/zh/function.unset.php) - 销毁指定的变量。

那么如果在一个对象外部去删除对象内部的成员属性用"unset()"函数可不可以呢，也是分两种情况，如果一个对象里面的成员属性是公有的，就可以使用这个函数在对象外面删除对象的公有属性，如果对象的成员属性是私有的，使用这个函数就没有权限去删除，但同样如果你在一个对象里面加上"__unset()"这个方法，就可以在对象的外部去删除对象的私有成员属性了。


在对象里面加上了"__unset()"这个方法之后，在对象外部使用"unset()"函数删除对象内部的私有成员属性时，自动调用"__unset()"函数来帮
我们删除对象内部的私有成员属性，这个方法也可以在类的内部定义成私有的。在对象里面加上下面的代码就可以了：

```PHP
function __unset($param){
	echo "当在类外部使用unset()函数来删除私有成员时自动调用的<br>";  
    unset($this->$param);
}
```
 

最后补充一个完整的实例：

```PHP
<?php  
class Person {  
    private $name;  
    private $sex;  
    private $age;  
    public function __get($property_name) {  
        if(isset($this->$property_name))  
        {  
            return ($this->$property_name);  
        } else {  
            return (NULL);  
        }  
    }  
    public function __set($property_name, $value) {  
        $this->$property_name = $value;  
    }  
    public  function __isset($param) {  
        echo "isset()函数测定私有成员时，自动调用<br>";  
        return isset($this->$param);  
    }  
    public  function __unset($param) {  
        echo "当在类外部使用unset()函数来删除私有成员时自动调用的<br>";  
        unset($this->$param);  
    }  
}  
$p = new Person();  
$p->name = "LinDD";  
echo var_dump(isset($p->name))."<br>";  
echo $p->name."<br>";  
unset($p->name);  
echo $p->name;  
```

输出结果为:

```
isset()函数测定私有成员时，自动调用
LinDD
当在类外部使用unset()函数来删除私有成员时自动调用的
isset()函数测定私有成员时，自动调用
```



如有不对，请指正！



