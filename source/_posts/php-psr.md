title: PHP基本代码规范
date: 2016-03-24 22:53:52
categories: ProjectManage
tags: [PHP,projectmanage]
---

<center>根据PSR指定的项目代码规范</center>

<!--more-->

PHP基本代码规范
=====================

本篇参考PSR规范制定了代码基本元素的相关标准</br>
以确保共享的PHP代码间具有较高程度的技术互通性。



1. 文件
-----------

### 1.1 PHP标签

- PHP代码**必须**使用 `<?php ?>` 长标签 或 `<?= ?>` 短输出标签；
- **一定不可**使用其它自定义标签。
- 纯PHP代码文件**必须**省略最后的 `?>` 结束标签。

### 1.2 PHP编码

- PHP代码文件**必须**以 `不带BOM的 UTF-8` 编码；

### 1.3 行

- 所有PHP文件**必须**使用`Unix LF (linefeed)`作为行的结束符。
- 所有PHP文件**必须**以一个空白行作为结束
- 非空行后**一定不能**有多余的空格符。
- 空行**可以**使得阅读代码更加方便以及有助于代码的分块。
- 每行**一定不能**存在多于一条语句

### 1.4 缩进

- 代码**必须**使用4个空格符的缩进，**一定不能**用 tab键。
 备注: 使用空格而不是tab键缩进的好处在于，避免在比较代码差异、打补丁、重阅代码以及注释时产生混淆,并且使用空格缩进，让对齐变得更方便

### 1.5 关键字 以及 True/False/Null

PHP所有 [关键字](http://php.net/manual/en/reserved.keywords.php)  **必须**全部小写。
常量 `true` 、`false` 和 `null` 也**必须**全部小写

2. namespace 以及 use 声明
-----------

- `namespace` 声明后 必须 插入一个空白行。
- 所有 `use` 必须 在 `namespace` 后声明。
- 每条 `use` 声明语句 必须 只有一个 `use` 关键词。
- `use` 声明语句块后 必须 要有一个空白行。

3. 类、属性和方法
-------------

### 3.1 命名
- 类的命名**必须**遵循 `StudlyCaps` 大写开头的驼峰命名规范。
- 方法名称**必须**符合 `camelCase` 式的小写开头驼峰命名规范。
- 类中的常量所有字母都**必须**大写，单词间用下划线分隔。
- 根据规范，每个类都独立为一个文件，且命名空间至少有一个层次：顶级的组织名称（vendor name）。

### 3.2 结构
- 类的开始花括号(`{`)**必须**写在函数声明后自成一行，结束花括号(`}`)也**必须**写在函数主体后自成一行。
- 方法的开始花括号(`{`)**必须**写在函数声明后自成一行，结束花括号(`}`)也**必须**写在函数主体后自成一行。
- 类的属性和方法**必须**添加访问修饰符（`private`、`protected` 以及 `public`）， `abstract` 以及 `final` **必须**声明在访问修饰符之前，而 `static` **必须**声明在访问修饰符之后。
- 控制结构的关键字后**必须**要有一个空格符，而调用方法或函数时则**一定不能**有。
- 控制结构的开始花括号(`{`)**必须**写在声明的同一行，而结束花括号(`}`)**必须**写在主体后自成一行。
- 控制结构的开始左括号后和结束右括号前，都**一定不能**有空格符。

### 3.3 扩展与继承

- 关键词 `extends` 和 `implements`**必须**写在类名称的同一行。

```
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // constants, properties, methods
}
```

- `implements` 的继承列表也**可以**分成多行，这样的话，每个继承接口名称都**必须**分开独立成行，包括第一个。

```
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // constants, properties, methods
}
```

### 3.4 属性

- 每个属性都**必须**添加访问修饰符。
- **一定不可**使用关键字 `var` 声明一个属性。
- 每条语句**一定不可**定义超过一个属性。
- **不要**使用下划线作为前缀，来区分属性是 protected 或 private。

以下是属性声明的一个范例：

```
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```

### 3.5 方法及函数调用

- 方法及函数调用时，方法名或函数名与参数左括号之间**一定不能**有空格，参数右括号前也 **一定不能**有空格。每个参数前**一定不能**有空格，但其后**必须**有一个空格。

```
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

- 参数**可以**分列成多行，此时包括第一个参数在内的每个参数都**必须**单独成行。

```
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

### 3.6 流程判读语句

请参考范例


4. 范例
-------------

```
<?php
namespace Vendor\Package;	//根据规范，每个类都独立为一个文件且命名空间至少有一个层次：顶级的组织名称（vendor name）。
//namespace声明后插入一个空白行
use FooInterface;	//所有use必须在namespace后声明
use BarClass as Bar;	//每条use声明语句必须只有一个use关键词
use OtherVendor\OtherPackage\BazClass;	//use声明语句块后必须要有一个空白行

class FooController extends BaseController implements FooInterface	//关键词extends和implements写在类名称的同一行,
    //类的命名遵循大写开头的驼峰命名规范
{	//类的开始花括号 { 写在函数声明后自成一行

    public $foo = 'foo';	//每个属性都必须添加访问修饰符
    private $bar = null;    //常量true false和null必须全部小写
                            //每条语句一定不可定义超过一个属性
    public function sampleFunction($a, $b = null)//方法名称符合小写开头驼峰命名规范
    {	//方法的开始花括号 { 写在函数声明后自成一行
        $a = 4;     //符号两边要有空格
        $foo->bar($arg1);   //方法及函数调用时，方法名或函数名与参数左括号之间一定不能有空格，参数右括号前也一定不能有空格。
        Foo::bar($arg2, $arg3);     //每个逗号后面必须要有一个空格，而逗号前面一定不能有空格

    }	//结束花括号 } 写在函数主体后自成一行

    final public static function bar()	//类的属性和方法必须添加访问修饰符（`private`、`protected` 以及 `public`)
        //abstract以及final声明在访问修饰符之前
        //static声明在访问修饰符之后。
    {
        //标准的if结构如下代码所示，留意 括号、空格以及花括号的位置，
        //注意 else 和 elseif 都与前面的结束花括号在同一行。
        if ($expr1) {
            // if body
        } elseif ($expr2) {   //应该使用关键词elseif代替所有else if以使得所有的控制关键字都像是单独的一个词。
            // elseif body
        } else {
            // else body;
        }

        //标准的switch结构如下代码所示，留意括号、空格以及花括号的位置。
        //case语句必须相对switch进行一次缩进，而break语句以及case内的其它语句都必须相对case进行一次缩进。
        //如果存在非空的case直穿语句,主体里必须有类似 `// no break` 的注释。

        switch ($expr) {
            case 0:
                echo 'First case, with a break';
                break;
            case 1:
                echo 'Second case, which falls through';
                // no break
            case 2:
            case 3:
            case 4:
                echo 'Third case, return instead of break';
                return;
            default:
                echo 'Default case';
                break;
        }


        //一个规范的 `while` 语句应该如下所示，注意其 括号、空格以及花括号的位置。

        while ($expr) {
            // structure body
        }

        //标准的 `do while` 语句如下所示，同样的，注意其 括号、空格以及花括号的位置。

        do {
            // structure body;
        } while ($expr);


        //标准的 `for` 语句如下所示，注意其 括号、空格以及花括号的位置。

        for ($i = 0; $i < 10; $i++) {
            // for body
        }

        //标准的 foreach 语句如下所示，注意其 括号、空格以及花括号的位置。

        foreach ($iterable as $key => $value) {
            // foreach body
        }

        //标准的 try catch 语句如下所示，注意其 括号、空格以及花括号的位置。

        try {
            // try body
        } catch (FirstExceptionType $e) {
            // catch body
        } catch (OtherExceptionType $e) {
            // catch body
        }
    }

}	//结束花括号 } 写在函数主体后自成一行
    //纯PHP代码文件必须省略最后的 `?>` 结束标签;所有PHP文件必须以一个空白行作为结束

```


