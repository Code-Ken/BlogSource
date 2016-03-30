title: PHP设计模式——适配器模式
date: 2016-02-29 18:09:45
categories: 设计模式
tags: [PHP,设计模式]
---

<center>PHP设计模式,适配器模式介绍</center>

<!--more-->


## 适配器模式

将一个类的接口，转换成客户端希望的另外一个接口，适配器模式使原本由于接口不兼容而不能一起工作的类可以一起工作了。

## 优点
1 通过适配器，客户端可以调用同一接口，因而对客户端来说是透明的。这样做更简单、更直接、更紧凑。
2 复用了现存的类，解决了现存类和复用环境要求不一致的问题。
3 将目标类和适配者类解耦，通过引入一个适配器类重用现有的适配者类，而无需修改原有代码。
4 一个对象适配器可以把多个不同的适配者类适配到同一个目标，也就是说，同一个适配器可以把适配者类和它的子类都适配到目标接口。

## 缺点

过多的使用适配器，会让系统非常零乱，不易整体进行把握。比如，明明看到调用的是A接口，其实内部被适配成了B接口的实现，一个系统如果太多出现这种情况，无异于一场灾难。因此如果不是很有必要，可以不使用适配器，而是直接对系统进行重构。

## PHP代码

```
<?php
namespace Adapter;

class Adaptee
{
    static function sendParams()
    {
        return array(
            "tb" => "淘宝",
            "tm" => "天猫",
            "jd" => "京东",
        );
    }
}

class Adapter extends Adaptee
{
    static function sendParams()
    {
        $params = parent::sendParams();
        $params = json_encode($params);
        return $params;
    }
}


header("Content-type:text/html;charset = utf-8");

//=====客户端1======

$client1 = Adaptee::sendParams();
echo "tb => " . $client1['tb'] . "<br />";
echo "tm => " . $client1['tm'] . "<br />";
echo "jd => " . $client1['jd'] . "<br />";

//=====客户端1======

$client2 = Adapter::sendParams();
$client2 = json_decode($client2);
echo "tb => " . $client2->tb . "<br />";
echo "tm => " . $client2->tm . "<br />";
echo "jd => " . $client2->jd . "<br />";
```

