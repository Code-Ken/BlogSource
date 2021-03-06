title: PHP设计模式——简介.md
date: 2016-02-29 17:48:27
categories: 设计模式
tags: [PHP,设计模式]
---

<center>PHP设计模式简介</center>

<!--more-->

​
## 定义
>在软件开发过程中，经常出现的典型场景的典型解决方案称为设计模式
使用设计模式的根本原因是为了代码复用，增加可维护性

## 六大原则

1.开闭原则(Open Close Principle）
此原则是由Bertrand Meyer提出的。原文是：
>“Software entities should be open for extension,but closed for modification”。

就是说模块应对扩展开放，而对修改关闭。

2.里氏代换原则（Liskov Substitution Principle）
里氏代换原则是面向对象设计的基本原则之一。 里氏代换原则中说:
>任何基类可以出现的地方，子类一定可以出现。

LSP是继承复用的基石，只有当派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤就是抽象化，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。

3.依赖倒转原则（Dependence Inversion Principle）
这个原则是开闭原则的基础，具体内容：
>针对对接口编程，依赖于抽象而不依赖于具体。

4、接口隔离原则（Interface Segregation Principle）
这个原则的意思是：
>使用多个隔离的接口，比使用单个接口要好。

它还有另外一个意思是：降低类之间的耦合度。
由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合。

5、迪米特法则，又称最少知道原则（Demeter Principle）
最少知道原则是指：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。

6、合成复用原则（Composite Reuse Principle）
合成复用原则是指：尽量使用合成/聚合的方式，而不是使用继承


## 作用

- 降低耦合度
- 容易修改
- 增加复用



