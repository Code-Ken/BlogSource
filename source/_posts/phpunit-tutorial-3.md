title: PHPUnit第四章——编写PHPUnit测试
date: 2016-02-24 17:04:13
categories: PHPUnit
tags: PHPUnit
---

<center>本文主要讲解一些特殊类型的测试方法</center>

<!--more-->

## 对异常进行测试

在./PHPUnitTutorial下新建一个ThrowException.php文件用于抛出异常
在./PHPUnitTutorial/Test下新建一个ThrowExceptionTest.php文件用于对异常进行测试

### 被测试代码

```
<?php
namespace phpUnitTutorial;

use Prophecy\Exception\InvalidArgumentException;

class ThrowException{
    public function foo($bar=""){
        if(empty($bar)){
            throw new \Exception('$bar is empty!!!');
        }
        echo $bar;
    }
    public function foo2(){
        throw new InvalidArgumentException('Hi there',200);
    }
}
```

### 测试代码

```
<?php
namespace phpUnitTutorial\Test;

use phpUnitTutorial\ThrowException;
use PHPUnit_Framework_TestCase;

class ThrowExceptionTest extends PHPUnit_Framework_TestCase{
    /**
     * @expectedException Exception
     */
    public function testFooException()
    {
        $class = new ThrowException();
        $foo = $class->foo();
    }

    /**
     * @expectedException InvalidArgumentException
     * @expectedExceptionMessage Hi there
     * @expectedExceptionCode 200
     */
    public function testFoo2Exception()
    {
        $class = new ThrowException();
        $foo = $class->foo2();
    }
}
```
### 测试结果

![](1.png)

### 测试讲解

对于异常的抛出的测试，我们在测试文档的方法名前用注释来标明对异常的测试

```
expectedException InvalidArgumentException =>表示期待抛出的异常是InvalidArgumentException
expectedExceptionMessage Hi there =>表示期待抛出的异常信息是"Hi there"
expectedExceptionCode 200 =>表示期待抛出的异常Code是"200"
```


## 对输出进行测试

在./PHPUnitTutorial下新建一个Output.php文件用于抛出异常
在./PHPUnitTutorial/Test下新建一个OutputTest.php文件用于对异常进行测试

### 被测试代码

```
<?php
namespace phpUnitTutorial;

class Output{
    static function OutputString(){
        echo "2333";
    }
}
```

### 测试代码

```
<?php
namespace phpUnitTutorial\Test;

use phpUnitTutorial\Output;
use PHPUnit_Framework_TestCase;

class OutputTest extends PHPUnit_Framework_TestCase{
    public function testOutputString(){
        Output::OutputString();
        $this->expectOutputString("23334");
    }
}
```

### 测试结果

![](2.png)


### 测试讲解

有时候，想要断言（比如说）某方法的运行过程中生成了预期的输出（例如，通过 echo 或 print）。PHPUnit_Framework_TestCase 类使用 PHP 的 输出缓冲 特性来为此提供必要的功能支持。
使用expectOutputString()方法来设定所预期的输出。如果没有产生预期的输出，测试将计为失败。


