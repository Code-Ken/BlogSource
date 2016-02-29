title: PHP设计模式——责任链模式
date: 2016-02-29 18:05:06
categories: 设计模式
tags: [PHP,设计模式]
---

<center>PHP设计模式,责任链模式介绍</center>

<!--more-->

​
## 责任链模式

使多个对象都有机会处理请求，从而避免请求的发送者和接受者耦合的关系。将这个对象形成一条链，并沿着这条链传递该请求，直到有对象处理它为止。

## 优点
实现了请求者与处理者代码分离：发出这个请求的客户端并不知道链上的哪一个对象最终处理这个请求，这使得系统可以在不影响客户端的情况下动态地重新组织和分配责任。提高系统的灵活性和可扩展行。

## 缺点
每次都是从链头开始：这也正是链表的缺点。

## PHP代码

```
<?php
namespace ChainOfResponsibility;

class Hander
{
    /**
     * 持有后继的责任对象
     */
    public $successor;

    /**
     * 示意处理请求的方法，虽然这个示意方法是没有传入参数的
     * 但实际是可以传入参数的，根据具体需要来选择是否传递参数
     */
    public function handleRequest()
    {

    }

    /**
     * 赋值方法，设置后继的责任对象
     */
    public function setSuccessor(Hander $hander)
    {
        $this->successor = $hander;
    }
}

class ConcreteHandler extends Hander
{
    public function handleRequest()
    {
        if ($this->successor == null) {
            echo "处理请求</br>";
        } else {
            echo "放过请求</br>";
            $this->successor->handleRequest();
        }
    }
}

class ProjectManager extends Hander
{
    public function payMoney($dollar)
    {
        if ($dollar < 100) {
            echo "项目经理出钱</br>";
        } else {
            echo "项目经理不出钱</br>";
            $this->successor->payMoney($dollar);
        }
    }

}

class DeptManager extends Hander
{
    public function payMoney($dollar)
    {
        if ($dollar < 200) {
            echo "部门经理出钱</br>";
        } else {
            echo "部门经理不出钱</br>";
            $this->successor->payMoney($dollar);
        }
    }
}

class GeneralManager extends Hander
{
    public function payMoney($dollar)
    {
        if ($dollar < 300) {
            echo "总经理出钱</br>";
        } else {
            echo "总经理不出钱</br>";
            $this->successor->payMoney($dollar);
        }
    }
}


header("Content-type: text/html; charset=utf-8");

$client1 = new ConcreteHandler();
$client2 = new ConcreteHandler();
$client1->setSuccessor($client2);
$client1->handleRequest();


$h1 = new ProjectManager();
$h2 = new DeptManager();
$h3 = new GeneralManager();

$h1->setSuccessor($h2);
$h2->setSuccessor($h3);

$h1->payMoney(250);

```