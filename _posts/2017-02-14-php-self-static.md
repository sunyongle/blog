---
layout: post
title:  "PHP中new static()与new self()的区别!"
date:   2017-2-14 11:11:01 +0800
categories: PHP
tag: 入门
---

* content
{:toc}
self:始终指向`self代码所在的类`的本身，无论这个类被继承了多少次，self都指向初使用self的类

static:指向`使用static的类`，只有通过继承后，才能体现出static的意义，否则static和self一样

```PHP
class A {
    public static function get_self() {
        return new self();
    }

    public static function get_static() {
        return new static();
    }
}

class B extends A {}

echo get_class(B::get_self());  // A
echo get_class(B::get_static()); // B
echo get_class(A::get_self()); // A
echo get_class(A::get_static()); // A
```

[stackoverflow:New self vs. new static](http://stackoverflow.com/questions/5197300/new-self-vs-new-static) 




