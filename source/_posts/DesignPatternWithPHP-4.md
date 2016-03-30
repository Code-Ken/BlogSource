title: PHP设计模式——单例模式
date: 2016-02-29 17:59:41
categories: 设计模式
tags: [PHP,设计模式]
---

<center>PHP设计模式，单例模式介绍</center>

<!--more-->


## 单例模式


保证一个类有且仅有一个实例，并且提供一个访问它的全局访问点。

## 优点

1、提供了对唯一实例的受控访问。
2、由于在系统内存中只存在一个对象，因此可以节约系统资源，对于一些需要频繁创建和销毁的对象单例模式无疑可以提高系统的性能。
3、允许可变数目的实例。

## 缺点

1、由于单利模式中没有抽象层，因此单例类的扩展有很大的困难。
2、单例类的职责过重，在一定程度上违背了“单一职责原则”。
3、滥用单例将带来一些负面问题，如为了节省资源将数据库连接池对象设计为的单例类，可能会导致共享连接池对象的程序过多而出现连接池溢出；如果实例化的对象长时间不被利用，系统会认为是垃圾而被回收，这将导致对象状态的丢失。

## PHP代码

```
<?php
namespace SingletonPattern;

class single
{
    protected static $ins = null;

    public static function getins()
    {
        if (self::$ins == null) {
            self::$ins = new self();
        }

        return self::$ins;
    }

    //方法前加final,则方法不能被覆盖,不能被继承
    final protected function __construct()
    {
    }

    //方法前加final,则方法不能被覆盖,不能被克隆
    final protected function __clone()
    {
    }
}

$s1 = single::getins();
$s2 = single::getins();

header("Content-type: text/html; charset=utf-8");

if ($s1 === $s2) {
    echo "一个对象";
} else {
    echo "不是一个对象";
}
```
