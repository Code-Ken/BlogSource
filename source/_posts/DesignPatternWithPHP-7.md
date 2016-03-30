title: PHP设计模式——装饰模式
date: 2016-02-29 18:07:54
categories: 设计模式
tags: [PHP,设计模式]
---

<center>PHP设计模式,装饰模式介绍</center>

<!--more-->


## 装饰模式

动态的给一个对象添加一些额外的职责，就增加功能来说，装饰模式比生成子类更为灵活。

## 优点

1. Decorator模式与继承关系的目的都是要扩展对象的功能，但是Decorator可以提供比继承更多的灵活性。
2. 通过使用不同的具体装饰类以及这些装饰类的排列组合，设计师可以创造出很多不同行为的组合。

## 缺点

1. 这种比继承更加灵活机动的特性，也同时意味着更加多的复杂性。
2. 装饰模式会导致设计中出现许多小类，如果过度使用，会使程序变得很复杂。
3. 装饰模式是针对抽象组件（Component）类型编程。但是，如果你要针对具体组件编程时，就应该重新思考你的应用架构，以及装饰者是否合适。当然也可以改变Component接口，增加新的公开的行为，实现“半透明”的装饰者模式。在实际项目中要做出最佳选择。

## PHP代码

```
<?php
namespace DecoratorPattern;

class Person
{
    protected $component;

    public function __construct(Person $component = null)
    {
        $this->component = $component;
    }

    public function show()
    {
        if ($this->component != null) {
            $this->component->show();
        }else{
            echo "人";
        }
    }
}

class Tshirt extends Person
{
    public function show()
    {
        echo "穿Tshirt的";
        parent::show();
    }
}

class Glass extends Person
{
    public function show()
    {
        echo "戴眼镜的";
        parent::show();
    }
}
class Trouser extends Person
{
    public function show()
    {
        echo "穿裤子的";
        parent::show();
    }
}



header("Content-type: text/html; charset=utf-8");
$p = new Person();
$t = new Tshirt($p);
$g = new Glass($t);
$t = new Trouser($g);

$t->show();
```