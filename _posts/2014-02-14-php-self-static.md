---
layout: post
title:  "PHP中new static()与new self()的区别!"
date:   2017-2-10 13:31:01 +0800
categories: PHP
tag: 面试
---

* content
{:toc}
self:始终指向`self代码所在的类`的本身，无论这个类被继承了多少次，self都指向初使用self的类

<br/><br/>

static:指向`使用static的类`，只有通过继承后，才能体现出static的意义，否则static和self一样

<br/>

```PHP
class A {
	public  function  getSelf(){
        return new self();
    }
    public  function  getStatic(){
        return new static();
    }
    
}
class B extends A{

}
var_dump((new B())->getSelf());//A
var_dump((new B())->getStatic());//B
```




