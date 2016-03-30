title: PHP注销SESSION的方法
date: 2016-03-23 22:25:17
categories: PHP
tags: [PHP,SESSION]
---

<center>本文介绍php对session的销毁操作</center>

<!--more-->

​
## 删除session方法

- unset()

```
    unset ($_SESSION['xxx']) 删除单个session
    unset($_SESSION)销毁，而且还没有可行的办法将其恢复。用户也不再可以注册$_session变量
    So千万不要用！
```
- array()

```
    $_SESSION = array()  删除多个session
```

- session_destroy()

```
    session_destroy()是注销所有的session变量,并且结束session会话
    但在内存中的$_SESSION变量内容依然保留！！！
```
- session_unset()

```
    session_unset()并不注销session变量,但把所有的session变量的值清空
```


## 一般登出操作消除SESSION的方法


```
    session_unset();
    session_destory();
```