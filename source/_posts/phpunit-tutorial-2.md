title: PHPUnit第三章——Assertion断言
date: 2016-02-24 17:00:57
categories: PHPUnit
tags: PHPUnit
---

<center>PHPUnit教程第三章，phpunit断言介绍</center>
<!--more-->

>断言(Assertions)是PHPUnit提供的一系列对程序执行结果测试的方法。通俗的讲，就是断言执行程序结果为我们期待的值，如果不是则测试失败


## 布尔类型

|断言|解释| 
|:----:|:----| 
|assertTrue|断言为真|
|assertFalse|断言为假|

## NULL类型

|断言|解释|
|:----:|:----| 
|assertNull|断言为NULL|
|assertNotNull|断言非NULL|

## 数字类型

|断言|解释|
|:----:|:----| 
|assertEquals|断言等于|
|assertNotEquals|断言不等于|
|assertGreaterThan|断言大于|
|assertGreaterThanOrEqual|断言大于等于|
|assertLessThan|断言小于|
|assertLessThanOrEqual|断言小于等于|

## 字符类型

|断言|解释|
|:----:|:----| 
|assertEquals|断言等于|
|assertNotEquals|断言不等于|
|assertContains|断言包含|
|assertNotContains|断言不包含|
|assertContainsOnly|断言只包含|
|assertNotContainsOnly|断言不只包含|

## 数组类型

|断言|解释|
|:----:|:----| 
|assertEquals|断言等于|
|assertNotEquals|断言不等于|
|assertArrayHasKey|断言有键|
|assertArrayNotHasKey|断言没有键|
|assertContains|断言包含|
|assertNotContains|断言不包含|
|assertContainsOnly|断言只包含|
|assertNotContainsOnly|断言不只包含|

## 对象类型

|断言|解释|
|:----:|:----| 
|assertAttributeContains|断言属性包含|
|assertAttributeContainsOnly|断言属性只包含|
|assertAttributeEquals|断言属性等于|
|assertAttributeGreaterThan|断言属性大于|
|assertAttributeGreaterThanOrEqual|断言属性大于等于|
|assertAttributeLessThan|断言属性小于|
|assertAttributeLessThanOrEqual|断言属性小于等于|
|assertAttributeNotContains|断言不包含|
|assertAttributeNotContainsOnly|断言属性不只包含|
|assertAttributeNotEquals|断言属性不等于|
|assertAttributeNotSame|断言属性不相同|
|assertAttributeSame|断言属性相同|
|assertSame|断言类型和值都相同|
|assertNotSame|断言类型或值不相同|
|assertObjectHasAttribute|断言对象有某属性|
|assertObjectNotHasAttribute|断言对象没有某属性|

## class类型

|断言|解释|
|:----:|:----| 
|assertClassHasAttribute|断言类有某属性|
|assertClassHasStaticAttribute|断言类有某静态属性|
|assertClassNotHasAttribute|断言类没有某属性|
|assertClassNotHasStaticAttribute|断言类没有某静态属性|

## 文件相关

|断言|解释|
|:----:|:----| 
|assertFileEquals|断言文件内容等于|
|assertFileExists|断言文件存在|
|assertFileNotEquals|断言文件内容不等于|
|assertFileNotExists|断言文件不存在|

## XML相关

|断言|解释|
|:----:|:----| 
|assertXmlFileEqualsXmlFile|断言XML文件内容相等|
|assertXmlFileNotEqualsXmlFile|断言XML文件内容不相等|
|assertXmlStringEqualsXmlFile|断言XML字符串等于XML文件内容|
|assertXmlStringEqualsXmlString|断言XML字符串相等|
|assertXmlStringNotEqualsXmlFile|断言XML字符串不等于XML文件内容|
|assertXmlStringNotEqualsXmlString|断言XML字符串不相等|


