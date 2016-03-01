title: PHP设计模式——桥接模式
date: 2016-03-01 11:39:31
categories: 设计模式
tags: [PHP,设计模式]
---


<center>PHP设计模式,桥接模式介绍</center>

<!--more-->


## 桥接模式

在软件系统中，某些类型由于自身的逻辑，它具有两个或多个维度的变化，那么如何应对这种“多维度的变化”？
这时我们需要使用桥接模式。
桥接模式（Bridge），将抽象部分与它的实现部分分离，使它们都可以独立地变化。

桥梁模式所涉及的角色有：
抽象化(Abstraction)角色：抽象化给出的定义，并保存一个对实现化对象的引用。

修正抽象化(Refined Abstraction)角色：扩展抽象化角色，改变和修正父类对抽象化的定义。

实现化(Implementor)角色：这个角色给出实现化角色的接口，但不给出具体的实现。必须指出的是，这个接口不一定和抽象化角色的接口定义相同，实际上，这两个接口可以非常不一样。实现化角色应当只给出底层操作，而抽象化角色应当只给出基于底层操作的更高一层的操作。

具体实现化(Concrete Implementor)角色：这个角色给出实现化角色接口的具体实现。


桥梁模式的用意是"将抽象化(Abstraction)与实现化(Implementation)脱耦，使得二者可以独立地变化"。这句话有三个关键词，也就是抽象化、实现化和脱耦。
- 抽象化
存在于多个实体中的共同的概念性联系，就是抽象化。作为一个过程，抽象化就是忽略一些信息，从而把不同的实体当做同样的实体对待。

- 实现化
抽象化给出的具体实现，就是实现化。

- 脱耦
所谓耦合，就是两个实体的行为的某种强关联。而将它们的强关联去掉，就是耦合的解脱，或称脱耦。在这里，脱耦是指将抽象化和实现化之间的耦合解脱开，或者说是将它们之间的强关联改换成弱关联。
将两个角色之间的继承关系改为聚合关系，就是将它们之间的强关联改换成为弱关联。因此，桥梁模式中的所谓脱耦，就是指在一个软件系统的抽象化和实现化之间使用`组合/聚合关系而不是继承关系`，从而使两者可以相对独立地变化。这就是桥梁模式的用意。

## 优点

1.将实现予以解耦，让它和界面之间不再永久绑定。
2.抽象和实现可以独立扩展，不会影响到对方。
3.对于具体实现的修改，不会影响到客户端。
 
## 缺点

1.增加了设计复杂度。
2.抽象类的修改影响到子类。

## PHP代码
```
<?php
namespace Bridge;

abstract class info
{
    protected $send = null;

    public function __construct($send)
    {
        $this->send = $send;
    }

    abstract public function msg($content);

    public function send($to, $content)
    {
        $content = $this->msg($content);
        $this->send->send($to, $content);
    }
}

class email
{
    public function send($to, $content)
    {
        echo 'email给' . $to . '的内容是' . $content;
    }
}

class sns
{
    public function send($to, $content)
    {
        echo 'sns给' . $to . '的内容是' . $content;
    }
}



class commonInfo extends info
{
    public function msg($content)
    {
        return '普通' . $content;
    }
}

class warnInfo extends info
{
    public function msg($content)
    {
        return '紧急' . $content;
    }
}

//====客户端====
header("Content-type:text/html;charset = utf-8");

$send = new warnInfo(new email());
$send->send("小明","内容");
```