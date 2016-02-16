title: Mac下php版本切换
date: 2016-02-16 10:44:46
categories: PHP
tags: PHP
---

<center>本文介绍在Mac开发环境下怎样进行php版本的切换</center>

<!--more-->

## 前言

默认php的安装方式是homebrew,如果不是,那就别看了。。。

## 安装php多版本

Mac下默认安装了php但是版本不是很高,用php -v查看php版本

```
localhost:~ Ken$ php -v
PHP 5.6.17 (cli) (built: Jan  8 2016 10:27:48) 
Copyright (c) 1997-2015 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2015 Zend Technologies
    with Xdebug v2.3.3, Copyright (c) 2002-2015, by Derick Rethans
localhost:~ Ken$ 
```

是php 5.6,我们希望安装php7又不把php5.6卸载,咋办呢？
首先,我们先用 brew install php70 安装php7

```
localhost:~ Ken$ brew install php70
==> Installing php70 from homebrew/php
Error: Cannot install homebrew/php/php70 because conflicting formulae are installed.

  php56: because different php versions install the same binaries.

Please `brew unlink php56` before continuing.

Unlinking removes a formula's symlinks from /usr/local. You can
link the formula again after the install finishes. You can --force this
install, but the build may fail or cause obscure side-effects in the
resulting software.
```
显示冲突,我们需要用 brew unlink取消homebrew与php56的关联,再安装php7

```
localhost:~ Ken$ brew install php70
==> Installing php70 from homebrew/php
==> Downloading https://homebrew.bintray.com/bottles-php/php70-7.
###################################################################### 100.0%
==> Pouring php70-7.0.2.el_capitan.bottle.10.tar.gz
==> Caveats
To enable PHP in Apache add the following to httpd.conf and restart Apache:
    LoadModule php7_module    /usr/local/opt/php70/libexec/apache2/libphp7.so
    
    <FilesMatch .php$>
        SetHandler application/x-httpd-php
    </FilesMatch>

Finally, check DirectoryIndex includes index.php
    DirectoryIndex index.php index.html

The php.ini file can be found in:
    /usr/local/etc/php/7.0/php.ini

✩✩✩✩ Extensions ✩✩✩✩

If you are having issues with custom extension compiling, ensure that
you are using the brew version, by placing /usr/local/bin before /usr/sbin in your PATH:

      PATH="/usr/local/bin:$PATH"

PHP70 Extensions will always be compiled against this PHP. Please install them
using --without-homebrew-php to enable compiling against system PHP.

✩✩✩✩ PHP CLI ✩✩✩✩

If you wish to swap the PHP you use on the command line, you should add the following to ~/.bashrc,
~/.zshrc, ~/.profile or your shell's equivalent configuration file:

      export PATH="$(brew --prefix homebrew/php/php70)/bin:$PATH"

✩✩✩✩ FPM ✩✩✩✩

To launch php-fpm on startup:
    mkdir -p ~/Library/LaunchAgents
    cp /usr/local/opt/php70/homebrew.mxcl.php70.plist ~/Library/LaunchAgents/
    launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.php70.plist

The control script is located at /usr/local/opt/php70/sbin/php70-fpm

OS X 10.8 and newer come with php-fpm pre-installed, to ensure you are using the brew version you need to make sure /usr/local/sbin is before /usr/sbin in your PATH:

  PATH="/usr/local/sbin:$PATH"

You may also need to edit the plist to use the correct "UserName".

Please note that the plist was called 'homebrew-php.josegonzalez.php70.plist' in old versions
of this formula.

To have launchd start homebrew/php/php70 at login:
  ln -sfv /usr/local/opt/php70/*.plist ~/Library/LaunchAgents
Then to load homebrew/php/php70 now:
  launchctl load ~/Library/LaunchAgents/homebrew.mxcl.php70.plist
==> Summary
🍺  /usr/local/Cellar/php70/7.0.2: 331 files, 49.3M
```

显示安装成功！

## 安装php－version

此时我们再安装php－version ==> php版本切换工具
安装命令：

```
brew install php-version
source $(brew --prefix php-version)/php-version.sh && php-version 5
```

执行php－version查看已存在的php版本,前面带＊的是当前环境正在使用的php版本,使用php－versiom＋版本号的方式切换php版本～

```
localhost:~ Ken$ php-version
  5.5.30
* 5.6.17
  7.0.2
localhost:~ Ken$ php-version 7.0.2
localhost:~ Ken$ php -v
PHP 7.0.2 (cli) (built: Jan  7 2016 10:40:26) ( NTS )
Copyright (c) 1997-2015 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2015 Zend Technologies
localhost:~ Ken$ 
```