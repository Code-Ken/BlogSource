title: PHPUnit第六章——基境(fixture)
date: 2016-02-24 17:12:25
categories: PHPUnit
tags: PHPUnit
---

<center>基境简介</center>

<!--more-->

## 什么是基境

在编写测试时，最费时的部分之一是编写代码来将整个场景设置成某个已知的状态，并在测试结束后将其复原到初始状态。这个已知的状态称为测试的 基境(fixture)。
相当于php里面的构造函数__construct()和析构函数__destructor()

## setUp() 

用于建立栈的基境

## tearDown()

用于取消栈的基境

## 实例

参考第七章——数据库测试


