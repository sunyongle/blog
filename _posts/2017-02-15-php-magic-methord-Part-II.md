---
layout: post
title:  "PHP魔术方法__call()用法分析"
date:   2017-2-16 10:25:01 +0800
categories: PHP
tag: 经典面试
---

* content
{:toc}
__call() 方法用于监视错误的方法调用。
-------

为了避免在调用不存在的方法时产生错误，可以使用`__call()`方法来避免，该方法在调用不存在的方法时自动调用，程序会继续执行下去。

语法:

```PHP
function __call($function_name,$arguments){
	//...
}
```

此方法有2个参数，第一个参数 $function_name 会自动接收不存在的方法名，第二个参数$arg则是以数组的方式接收不存在方法的多个参数。

在类中加入：

```PHP
function _call($function_name,$arguments){
	echo "你所调用的函数：$function_name(参数：<br />";
    var_dump($args);
    echo ")不存在！";
}
```

单调用一个不存在的方法时，如test();

```PHP
$p = new Person();
$p->test(2,'Lin');
```

则会输出以下结果:

```PHP
你所调用的函数：test(参数：
array(2) {
    [0]=>int(2)
    [1]=>string(4) "Lin"
}
)不存在！
```