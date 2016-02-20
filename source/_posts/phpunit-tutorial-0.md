title: PHPUnit第一章——介绍
date: 2016-02-20 21:33:40
categories: PHPUnit
tags: PHPUnit
---

<center>本文主要对PHPunit做一个简短的介绍,教程的源码都在我的github里</center>
<!--more-->


## 简介

PHPUnit是对PHP语言进行测试的框架
高质量的单元测试是保证项目质量的基础，能够有效的减少BUG，改善程序。

## 安装

建议通过composer安装

```
{    
    "require-dev": {
        "phpunit/phpunit": "5.0.*"  
        "phpunit/dbunit": ">=1.2"
    }
}
```

>移植到 PHP/PHPUnit 上的 DbUnit 用于提供对数据库交互测试的支持。
PHPUnit 的 PHAR 分发中已经包含了此组件包。若要通过Composer 安装此组件包,添加如下 "require-dev" 依赖项: "phpunit/dbunit": ">=1.2"

### 安装需求

代码覆盖率分析报告功能需要 Xdebug（2.2.1以上）与tokenizer扩展。生成 XML 格式的报告需要有xmlwriter扩展。

## 运行PHPUnit

![](1.png)

证明安装成功

## 设置phpunit.xml

运行PHPUnit可以通过默认的命令行进行,但这样非常的繁琐，有一个更好的方式:
    在项目的根目录编写一个*phpunit.xml*配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<phpunit colors="true">
    <testsuites>
        <testsuite name="Application Test Suite">
            <directory>./phpUnitTutorial/Test/</directory>
            <file>/path/to/MyTest.php</file>
            <exclude>/path/to/exclude</exclude>
        </testsuite>
    </testsuites>

    <php>
        <includePath>.</includePath>
        <ini name="foo" value="bar"/>
        <const name="foo" value="bar"/>
        <var name="foo" value="bar"/>
        <env name="foo" value="bar"/>
        <post name="foo" value="bar"/>
        <get name="foo" value="bar"/>
        <cookie name="foo" value="bar"/>
        <server name="foo" value="bar"/>
        <files name="foo" value="bar"/>
        <request name="foo" value="bar"/>
    </php>
    

</phpunit>

```

```
解释:
colors="true" => 确保你的测试结果是彩色的,便于阅读
<testsuites> => 带有一个或多个 <testsuite> 子元素的 <testsuites> 元素用于将测试套件及测试用例

<php> => 以上 XML 配置对应于如下 PHP 代码:
    ini_set('foo', 'bar');
    define('foo', 'bar');
    $GLOBALS['foo'] = 'bar';
    $_ENV['foo'] = 'bar';
    $_POST['foo'] = 'bar';
    $_GET['foo'] = 'bar';
    $_COOKIE['foo'] = 'bar';
    $_SERVER['foo'] = 'bar';
    $_FILES['foo'] = 'bar';
    $_REQUEST['foo'] = 'bar';
```



















