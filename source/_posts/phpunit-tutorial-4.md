title: PHPUnit第五章——测试private/protected的方法
date: 2016-02-24 17:09:37
categories: PHPUnit
tags: PHPUnit
---

<center>PHPUnit测试private/protected的方法</center>

<!--more-->

一般情况下PHPUnit只能测试公共方法，对于private/protected方法的测试,一般采取间接测试的方式，即测试公共方法，如果公共方法测试成功，则private/protected方法也间接的测试成功了，但如果有一个重要的方法被隐藏在私有或者保护的访问权限中，我们不能知道故障从何而起，因此我们希望直接测试某些private/protected方法

## 被测文件

首先我们依旧在./phpUnitTutorial/下新建一个php文件，文件名为PrivateOrProtected,代码如下

```
<?php

namespace phpUnitTutorial;


class PrivateOrProtected
{
    protected $property = 'Protected Property';


    private function method()
    {
        return 'Protected Method';
    }
}
```

非常的简单，里面有一个protected属性和一个private方法。

## 直接测试private/protected

在./phpUnitTutorial/Test下新建php测试文件，文件名称为PrivateOrProtectedTest

```
<?php

namespace phpUnitTutorial\Test;

use PHPUnit_Framework_TestCase;
use phpUnitTutorial\PrivateOrProtected;
use ReflectionProperty;
use ReflectionMethod;


class PrivateOrProtectedTest extends PHPUnit_Framework_TestCase
{
    /**
     * Read protected property
     */
    public function testProtectedProperty()
    {
        $foo = new PrivateOrProtected();

        $bar = function () {
            return $this->property;
        };

        $baz = $bar->bindTo($foo, $foo);

        $this->assertEquals($baz(), 'Protected Property');
    }

    /**
     * Run private method
     */
    public function testProtectedMethod()
    {
        $foo = new PrivateOrProtected();

        $bar = function () {
            return $this->method();
        };

        $baz = $bar->bindTo($foo, $foo);

        $this->assertEquals($baz(), 'Protected Method');
    }

    public function testProtectProperty2()
    {
        $fixture = new PrivateOrProtected();
        //Note that the property will only become accessible using the ReflectionProperty class.
        // The property is still private or protected in the class instances.
        //if class does not exist in current file,you need to add namespace
        $reflector = new ReflectionProperty('phpUnitTutorial\PrivateOrProtected','property');
        $reflector->setAccessible(true);

        $bar = $reflector->getValue($fixture);

        $this->assertEquals($bar,'Protected Property');
    }

    public function testProtectedMethod2()
    {
        $fixture = new PrivateOrProtected();
        $reflector = new ReflectionMethod('phpUnitTutorial\PrivateOrProtected','method');

        $bar = $reflector->getClosure($fixture);
        $bar = call_user_func_array($bar,array());

        $this->assertEquals($bar,'Protected Method');
    }
}
```

以上代码通过两种方式：

- 匿名函数的方式
- 使用Reflection类的方式

实现了对private/protected的直接测试,注意使用Reflection类时，php版本需要在5.3.2以上。




