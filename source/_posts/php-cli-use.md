title: 在命令行下执行PHP
date: 2016-01-07 13:07:57
categories: PHP
tags: [PHP,命令行,CLI]
---

<center>本文主要讲解在命令行下执行PHP</center>
<!--more-->
## CLI
从版本 4.3.0 开始，PHP 提供了一种新类型的 CLI SAPI（Server Application Programming Interface，服务端应用编程端口）支持，名为 CLI，意为 Command Line Interface，即命令行接口。顾名思义，该 CLI SAPI 模块主要用作 PHP 的开发外壳应用。
## CLI运行PHP代码
CLI SAPI 模块有以下三种不同的方法来获取要运行的 PHP 代码：
1.让 PHP 运行指定文件。

```
php my_script.php 
php -f my_script.php
```
以上两种方法（使用或不使用 -f 参数）都能够运行给定的 my_script.php 文件。可以选择任何文件来运行，指定的 PHP 脚本并非必须要以 .php 为扩展名，它们可以有任意的文件名和扩展名。
2.在命令行直接运行 PHP 代码。

```
php -r 'print_r(get_defined_constants());'
```
在使用这种方法时，请注意外壳变量的替代及引号的使用。
>Note:
请仔细阅读以上范例，在运行代码时没有开始和结束的标记符！加上 -r 参数后，这些标记符是不需要的，加上它们会导致语法错误。

3.`(这种没用过。。。)`通过标准输入（stdin）提供需要运行的 PHP 代码。
以上用法提供了非常强大的功能，使得可以如下范例所示，动态地生成 PHP 代码并通过命令行运行这些代码：
```
$ some_application | some_filter | php | sort -u >final_output.txt
```

以上三种运行代码的方法不能同时使用。

## CLI参数详解

![](/cli-1.png)

## CLI传参
```
当使用这个命令执行：php script.php arg1 arg2 arg3

以上例程的输出类似于：
array(4) {
  [0]=>  string(10) "script.php"
  [1]=>  string(4) "arg1"
  [2]=>  string(4) "arg2"
  [3]=>  string(4) "arg3" 
}

```

## CLI交互式shell模式运行php
用过 Python 的朋友对Python的交互式shell比较熟悉，在命令行下，如果我们直接输入python命令，则会进入python的交互式shell程序，接下来就可以交互式的执行一些计算任务。
![](/cli-2.png)
在PHP命令行中，同样提供了类似的功能，使用`-a`参数即可进入交互shell模式。
![](/cli-3.png)

## 运行内建的Web服务器
从PHP 5.4.0开始，PHP的命令行模式提供了一个内建的web服务器。使用`-S`开始运行web服务。
在该目录中，执行以下命令可以启动内建web服务器，并且默认以当前目录为工作目录

![](/cli-4.png)
![](/cli-5.png)


以上我们在启动内建服务器的时候，只指定了`-S`参数让PHP以web服务器的方式运行，这时，PHP会使用当前目录作为工作目录，因此回到当前目录下寻找请求的文件，我们还可以使用`-t`参数指定其它的目录作为工作目录（文档根目录）。

## 查找PHP的配置文件
在有的时候，由于服务器上软件安装比较混乱，我们可能安装了多个版本的PHP环境，这时候，如何定位我们的PHP程序使用的是那个配置文件就比较重要了。在PHP命令行参数中，提供了`--ini`参数，使用该参数，可以列出当前PHP的配置文件信息

```
gaosongdeMacBook-Pro:sync Ken$ php --ini
Configuration File (php.ini) Path: /usr/local/etc/php/5.5
Loaded Configuration File:         /usr/local/etc/php/5.5/php.ini
Scan for additional .ini files in: /usr/local/etc/php/5.5/conf.d
Additional .ini files parsed:      /usr/local/etc/php/5.5/conf.d/ext-igbinary.ini,
/usr/local/etc/php/5.5/conf.d/ext-libevent.ini,
/usr/local/etc/php/5.5/conf.d/ext-memcache.ini,
/usr/local/etc/php/5.5/conf.d/ext-memcached.ini,
/usr/local/etc/php/5.5/conf.d/ext-mongo.ini,
/usr/local/etc/php/5.5/conf.d/ext-mongodb.ini,
/usr/local/etc/php/5.5/conf.d/ext-xdebug.ini

gaosongdeMacBook-Pro:sync Ken$ 
```

## CLI语法检查
有时候，我们只需要检查php脚本是否存在语法错误，而不需要执行它，比如在一些编辑器或者IDE中检查PHP文件是否存在语法错误。
使用`-l`（--syntax-check）可以只对PHP文件进行语法检查。
```
gaosongdeMacBook-Pro:sync Ken$ php -l index.php
No syntax errors detected in index.php

$ php -l index.php 
PHP Parse error:  syntax error, unexpected 'echo' (T_ECHO) in index.php on line 3
Parse error: syntax error, unexpected 'echo' (T_ECHO) in index.php on line 3
Errors parsing index.php
```

## 参考文献
1.[PHP命令行下的世界](http://segmentfault.com/a/1190000002949021)
2.[PHP手册](http://php.net/manual/zh/features.commandline.php)