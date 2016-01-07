title: PhpDocument的使用
date: 2016-01-06 20:17:52
categories: PHP
tags: phpdoc
---

<center>本文主要介绍怎样使用PhpDocumentor</center>
<!--more-->

# phpDocumentor是什么

phpDocumentor（以下用phpdoc代替），是一个能为php文件自动生成API文档的自动化工具。

# phpdoc的目标

>“Provide PHP Developers with all the tools and resources necessary to write effective and comprehensive documentation with as little effort possible.”



让php开发者用最少的精力开发出有效全面的文档

# 第一组phpdoc文档

## 对php源文件

在php源文件中我们要写入一系列的代码块(DocBlock),来让phpdoc识别怎么样，并生成文档

## 哪些元素可以被写入文档

- Function                            函数

- Constant                           常量

- Class                                类

- Interface                           借口

- Trait                                 特征

- Class constant                类常量

- Property                          属性

- Method                            方法



## 代码块怎么用

### 代码块长啥样

以`/**`开始，以`*/`结束，在开始和结束之间每一行都以`*`开头

```

<?php

 /** 

 * This is a DocBlock.

 */ 

function associatedFunction() { }

```

### 代码块内容

代码快被分为三个部分：Summary，Description，Tags，每一部分都是可选的，但是要写Description必须要有Summary。

- Summary

有时被称为一个简短的描述,简要介绍相关元素的功能。

Summary遇到以下任意一种方式，就被视为结束：

1.遇到`‘.’`+一个回车符

2.遇到连续的两个回车符

- Description

有时也被称为长描述,可以提供更多的信息。即Summary的详细说明。


Description遇到以下任意一种方式，就被视为结束：

1.遇到第一个`tag`标签

2.遇到块标签截止符`‘*/’`

- Tags

即标签，提供简短统一的标签符来诠释某些特征与方法（后面会有详细介绍）

每一个标签都是以`@`符开始，且一行只能有一个标签

- 例子



```

<?php

 /**  

* A summary informing the user what the associated element does.    

*  

* A *description*, that can span multiple lines, to go _in-depth_ into the details of this element  

* and to provide some background information or textual references.  

* 

* @param string $myArgument With a *description* of this argument, these may also 

*    span multiple lines.  

*  

* @return void 

*/  

function myFunction($myArgument) 

 {

 }

```



### 根据代码快内容生成phpdoc文档

在终端下执行phpdoc命令

```

ken@ken-K42JY:~$ cd '/var/www/info' 

ken@ken-K42JY:/var/www/info$ ls -l

total 8

-rwxrwxrwx 1 root root 438 Dec 14 17:01 info.php

-rwxrwxrwx 1 ken  ken  771 Dec 14 17:01 info.php~

ken@ken-K42JY:/var/www/info$ phpdoc -f info.php -t ./demo/    //将info.php文件生成phpdoc文档

Collecting files .. OK

Initializing parser .. OK

Parsing files

Parsing /var/www/info/info.php

  No summary was found for this file  

Storing cache in "/var/www/info/demo" .. OK

Load cache                                                         ..    0.001s

Preparing template "clean"                                         ..    0.022s

Preparing 17 transformations                                       ..    0.000s

Build "elements" index                                             ..    0.000s

Replace textual FQCNs with object aliases                          ..    0.000s

Resolve @link and @see tags in descriptions                        ..    0.000s

Enriches inline example tags with their sources                    ..    0.000s

Build "packages" index                                             ..    0.001s

Build "namespaces" index and add namespaces to "elements"          ..    0.000s

Collect all markers embedded in tags                               ..    0.000s

Transform analyzed project into artifacts                          .. 

Applying 17 transformations

  Initialize writer "phpDocumentor\Plugin\Core\Transformer\Writer\FileIo"

  Initialize writer "phpDocumentor\Plugin\Twig\Writer\Twig"

  Initialize writer "phpDocumentor\Plugin\Graphs\Writer\Graph"

  Execute transformation using writer "FileIo"

  Execute transformation using writer "FileIo"

  Execute transformation using writer "FileIo"

  Execute transformation using writer "FileIo"

  Execute transformation using writer "FileIo"

  Execute transformation using writer "twig"

  Execute transformation using writer "twig"

  Execute transformation using writer "twig"

  Execute transformation using writer "twig"

  Execute transformation using writer "twig"

  Execute transformation using writer "twig"

  Execute transformation using writer "twig"

  Execute transformation using writer "twig"

  Execute transformation using writer "twig"

  Execute transformation using writer "twig"

  Execute transformation using writer "twig"

  Execute transformation using writer "Graph"

Unable to find the `dot` command of the GraphViz package. Is GraphViz correctly installed and present in your path?   0.075s

Analyze results and write report to log                            ..    0.000s

ken@ken-K42JY:/var/www/info$ 

```



phpdoc –h
会得到一个详细的参数表，其中几个重要的参数如下：

```

-f 要进行分析的文件名，多个文件用逗号隔开
-d 要分析的目录，多个目录用逗号分割
-t 生成的文档的存放路径
-o 输出的文档格式，结构为输出格式：转换器名：模板目录。


--template=responsive-twig   将模板换成responsive-twig，自带的模板：用`phpdoc template:list`指令查看

```

生成的文档如下图所示：


![](/phpdoc-1.png)



文档目录如下图所示：

![](/phpdoc-2.png)

其中index.html是入口文件



### No summary was found for this file  

我们注意到上一段代码中的第11行报了一个错误`No summary was found for this file  `

这是因为

>Does the top of your file have two such docblocks? The first docblock is expected to be the one for the file itself, and a second docblock should pair up with the first documentable code element that appears after it.
However, if you only have one docblock at the top of the file, it will get paired up with the first code element found, thus the "file itself" will seem to be missing its docblock. That is what that error is supposed to indicate.



故我们这样写我们的代码块：

```

<?php

 /**

 * 文档自身的Summary.

 *

 * 文档自身的Description

 */



 /**

 * A summary informing the user what the associated element does.第一个函数的summary

 *

 * A *description*, that can span multiple lines, to go _in-depth_ into the details of this element

 * and to provide some background information or textual references.

 *

 * @param string $myArgument With a *description* of this argument, these may also

 *    span multiple lines.

 *

 * @return void

 */

  function myFunction($myArgument)

  {

  }

```

# tags标签详解

## 标记用途描述




| 标签  | 用途 | 描述
| :-----|----|:----
|@abstract|     |  用于描述抽象类的变量和方法
|@access|public, private or protected| 文档的访问、使用权限. @access private 表明这个文档是被保护的
|@author   | author name <author@email> | 描述当前元素的作者
|@copyright| 名称 时间 |文档版权信息
|@deprecated|version|文档中被废除的方法
|@deprec||同@deprecated
|@example| /path/to/example |文档的外部保存的示例文件的位置
|@exception||文档中方法抛出的异常，也可参照 @throws
|@ignorel | | 忽略文档中指定的关键字
|@internal || 开发团队内部信息
|@link| URL |类似于license 但还可以通过link找到文档中的更多个详细的信息
|@name |变量别名 |为某个变量指定别名
|@magic | |phpdoc.de compatibility
|@package| 封装包的名称|一组相关类、函数封装的包名称
|@var|type|文档中的变量及其类型
|@version||文档、类、函数的版本信息
|@global  | 类型：$globalvarname  | 文档中的全局变量及有关的方法和函数
|@static| |记录静态类、方法
|@static| var |在类、函数中使用的静态变量
|@subpackage| |子版本
|@throws||某一方法抛出的异常
|@todo||表示文件未完成或者要完善的地方  
|@return | 如返回bool|函数返回结果描述，一般不用在void（空返回结果的）的函数中
|@see|如 Class Login（）| 文件关联的任何元素（全局变量，包括，页面，类，函数，定义，方法，变量)
|@since|version|记录什么时候对文档的哪些部分进行了更改
|@param||变量含义注释

























## 实例

```

<?php 

 /**

  * start page for webaccess

  *

  * PHP version 5

  *

  * @category  PHP

  * @package   PSI_Web

  * @author    Michael Cramer <BigMichi1@users.sourceforge.net>

  * @copyright 2009 phpSysInfo

  * @license   http://opensource.org/licenses/gpl-2.0.php GNU General Public License

  * @version   SVN: $Id: class.Webpage.inc.php 412 2010-12-29 09:45:53Z Jacky672 $

  * @link      http://phpsysinfo.sourceforge.net

  */

  /**

  * generate the dynamic webpage

  *

  * @category  PHP

  * @package   PSI_Web

  * @author    Michael Cramer <BigMichi1@users.sourceforge.net>

  * @copyright 2009 phpSysInfo

  * @license   http://opensource.org/licenses/gpl-2.0.php GNU General Public License

  * @version   Release: 3.0

  * @link      http://phpsysinfo.sourceforge.net

  */

 class Webpage extends Output implements PSI_Interface_Output

 {

     /**

      * configured language

      *

      * @var String

      */

     private $_language;

     

     /**

      * configured template

      *

      * @var String

      */

     private $_template;

     

     /**

      * all available templates

      *

      * @var Array

      */

     private $_templates = array();

     

     /**

      * all available languages

      *

      * @var Array

      */

     private $_languages = array();

     

     /**

      * check for all extensions that are needed, initialize needed vars and read config.php

      */

     public function __construct()

     {

         parent::__construct();

         $this->_getTemplateList();

         $this->_getLanguageList();

     }

     

     /**

      * checking config.php setting for template, if not supportet set phpsysinfo.css as default

      * checking config.php setting for language, if not supported set en as default

      *

      * @return void

      */

     private function _checkTemplateLanguage()

     {

         $this->_template = trim(PSI_DEFAULT_TEMPLATE);

         if (!file_exists(APP_ROOT.'/templates/'.$this->_template.".css")) {

             $this->_template = 'phpsysinfo';

         }

         

         $this->_language = trim(PSI_DEFAULT_LANG);

         if (!file_exists(APP_ROOT.'/language/'.$this->_language.".xml")) {

             $this->_language = 'en';

         }

     }

     

     /**

      * get all available tamplates and store them in internal array

      *

      * @return void

      */

     private function _getTemplateList()

     {

         $dirlist = CommonFunctions::gdc(APP_ROOT.'/templates/');

         sort($dirlist);

         foreach ($dirlist as $file) {

             $tpl_ext = substr($file, strlen($file) - 4);

             $tpl_name = substr($file, 0, strlen($file) - 4);

             if ($tpl_ext === ".css") {

                 array_push($this->_templates, $tpl_name);

             }

         }

     }

     

     /**

      * get all available translations and store them in internal array

      *

      * @return void

      */

     private function _getLanguageList()

     {

         $dirlist = CommonFunctions::gdc(APP_ROOT.'/language/');

         sort($dirlist);

         foreach ($dirlist as $file) {

             $lang_ext = substr($file, strlen($file) - 4);

             $lang_name = substr($file, 0, strlen($file) - 4);

             if ($lang_ext == ".xml") {

                 array_push($this->_languages, $lang_name);

             }

         }

     }

     

     /**

      * render the page

      *

      * @return void

      */

     public function run()

     {

         $this->_checkTemplateLanguage();

         

         $tpl = new Template("/templates/html/index_dynamic.html");

         

         $tpl->set("template", $this->_template);

         $tpl->set("templates", $this->_templates);

         $tpl->set("language", $this->_language);

         $tpl->set("languages", $this->_languages);

         

         echo $tpl->fetch();

     }

 }

 ?>

```



# 参考文档

1.[小五的博客](http://www.cnblogs.com/picaso/archive/2012/10/04/2711435.html)

2.[phpdoc官方文档](http://www.phpdoc.org)

