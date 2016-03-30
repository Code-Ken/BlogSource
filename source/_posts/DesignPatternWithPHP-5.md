title: PHP设计模式——观察者模式
date: 2016-02-29 18:02:24
categories: 设计模式
tags: [PHP,设计模式]
---

<center>PHP设计模式，观察者模式介绍</center>

<!--more-->



## 观察者模式

观察者模式对应了一种一对多的依赖关系，让多个观察者对象同时监听某一主题对象。这个主题发生变化时，会通知所有观察者对象，使他们能够自己自动更新。

## 优点

1.观察者和主题之间的耦合度较小；
2.支持广播通信；

## 缺点

由于观察者并不知道其它观察者的存在，它可能对改变目标的最终代价一无所知。这可能会引起意外的更新。

## PHP代码

```
<?php

namespace ObservePattern;
use SplObserver;
use SplSubject;

class subject implements SplSubject
{
    private $observers;

    public function __construct()
    {
        $this->observers = array();
    }

    public function attach(SplObserver $observer)
    {
        return array_push($this->observers, $observer);
    }

    public function detach(SplObserver $observer)
    {
        $idx = array_search($observer, $this->observers, true);
        if ($idx) {
            unset($this->observers[$idx]);
        }
    }

    public function notify()
    {
        foreach ($this->observers as $observer) {
            $observer->update($this);
        }
        return true;
    }
}

class observe implements SplObserver
{

    public function update(SplSubject $subject)
    {
        echo "通知观察者</br>";
    }
}

class observe1 implements SplObserver
{

    public function update(SplSubject $subject)
    {
        echo "通知另一观察者</br>";
    }
}

$subject = new subject();
$observer = new observe();
$observer1 = new observe1();

header("Content-type: text/html; charset=utf-8");
$subject->attach($observer);
$subject->attach($observer1);

$subject->notify();

$subject->detach($observer1);
$subject->notify();
```