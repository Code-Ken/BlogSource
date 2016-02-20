title: PHPUnit第二章——第一个PHPUnit程序
date: 2016-02-20 21:37:45
categories: PHPUnit
tags: PHPUnit
---

<center>PHPUnit教程第二章，编写phpunit入门程序</center>
<!--more-->

​
## 一些测试惯例


- 针对类Class的测试写在类ClassTest中。
- ClassTest(通常)继承自 PHPUnit_Framework_TestCase。 
- 测试都是命名为test*的公用方法。也可以在方法的文档注释块(docblock)中使用 @test 标注将其标记为测试方法。
- 在测试方法内,类似于assertEquals()这样的断言方法用来对实际值与预期值的匹配做出断言。
- 文件结构和文件名:
  要成一一对应的关系，例如：
  
  ```
  #要测试的文件
  ./phpUnitTutorial/Foo.php
  ./phpUnitTutorial/Bar.php
  ./phpUnitTutorial/Controller/Baz.php

  #测试文件
  ./phpUnitTutorial/Test/FooTest.php
  ./phpUnitTutorial/Test/BarTest.php
  ./phpUnitTutorial/Test/Controller/BazTest.php
  ```

## 第一个测试文件

### 要测试文件

在 ./phpUnitTutorial下创建 URL.php,我们创建一个公共方法sluggify用于将一串字符串转换成URL-safe的字符串:
"This string will be sluggified" ===> "this-string-will-be-sluggified".


```
<?php

namespace phpUnitTutorial;

class URL
{
    public function sluggify(string,string,separator = '-', $maxLength = 96)
    {
        title=iconv(′UTF−8′,′ASCII//TRANSLIT′,title=iconv(′UTF−8′,′ASCII//TRANSLIT′,string);
        title=pregreplace("title=pregreplace("title);
        title=strtolower(trim(substr(title=strtolower(trim(substr(title, 0, $maxLength), '-'));
        title=pregreplace("/[\/|+−]+/",title=pregreplace("/[\/|+−]+/",separator, $title);

        return $title;
    }
}
```

### 测试文件


在./phpUnitTutorial/Test文件夹下创建URLTest.php


```
<?php

namespace phpUnitTutorial\Test;

use phpUnitTutorial\URL;
use PHPUnit_Framework_TestCase;

class URLTest extends PHPUnit_Framework_TestCase
{
    public function testSluggifyReturnsSluggifiedString()
    {
        $originalString = 'This string will be sluggified';
        $expectedResult = 'this-string-will-be-sluggified';

        $url = new URL();

        result=result=url->sluggify($originalString);

        this−>assertEquals(this−>assertEquals(expectedResult, $result);
    }
}
```

### 运行测试

在终端下运行测试

![](1.png)

显示OK，表明测试成功！

### 测试讲解

查看URLTest.php文件，发现编写测试文件十分的简便。
仅仅几行，就可验证URL类下的sluggify方法是否正确，而验证是否正确的关键函数是assertEquals()断言函数来验证(后面会详细讲解断言函数)
assertEquals()这个函数的作用就是验证传入的expectedResult,expectedResult,result两个值是否相等
对于一个方法，我们可能需要多组数据
但是一个test方法只能有一个assert,因此我们可以写多个test方法来测试不同的数据

```
    public function testSluggifyReturnsExpectedForStringsContainingNumbers()
    {
        $originalString = 'This1 string2 will3 be 44 sluggified10';
        $expectedResult = 'this1-string2-will3-be-44-sluggified10';

        $url = new URL();

        result=result=url->sluggify($originalString);

        this−>assertEquals(this−>assertEquals(expectedResult, $result);
    }

    public function testSluggifyReturnsExpectedForStringsContainingSpecialCharacters()
    {
        originalString = 'This! @string#originalString = 'This! @string# %$will ()be "sluggified';
        $expectedResult = 'this-string-will-be-sluggified';

        $url = new URL();

        result=result=url->sluggify($originalString);

        this−>assertEquals(this−>assertEquals(expectedResult, $result);
    }

    public function testSluggifyReturnsExpectedForStringsContainingNonEnglishCharacters()
    {
        $originalString = "Tänk efter nu – förr'n vi föser dig bort";
        $expectedResult = 'tank-efter-nu-forrn-vi-foser-dig-bort';

        $url = new URL();

        result=result=url->sluggify($originalString);

        this−>assertEquals(this−>assertEquals(expectedResult, $result);
    }

    public function testSluggifyReturnsExpectedForEmptyStrings()
    {
        $originalString = '';
        $expectedResult = '';

        $url = new URL();

        result=result=url->sluggify($originalString);

        this−>assertEquals(this−>assertEquals(expectedResult, $result);
    }
```

### 测试优化

但这样是在是太麻烦了，PHPUnit为我们提供了@dataProvider方法来为一个方法提供多组数据


```
<?php

namespace phpUnitTutorial\Test;

use phpUnitTutorial\URL;
use PHPUnit_Framework_TestCase;

class URLTest extends PHPUnit_Framework_TestCase
{
    /**
     * @param string $originalString String to be sluggified
     * @param string $expectedResult What we expect our slug result to be
     *
     * @dataProvider providerTestSluggifyReturnsSluggifiedString
     */
    public function testSluggifyReturnsSluggifiedString($originalString, $expectedResult)
    {
        $url = new URL();

        $result = $url->sluggify($originalString);

        $this->assertEquals($expectedResult, $result);
    }

    public function providerTestSluggifyReturnsSluggifiedString()
    {
        return array(
            array('This string will be sluggified', 'this-string-will-be-sluggified'),
            array('THIS STRING WILL BE SLUGGIFIED', 'this-string-will-be-sluggified'),
            array('This1 string2 will3 be 44 sluggified10', 'this1-string2-will3-be-44-sluggified10'),
            array('This! @string#$ %$will ()be "sluggified', 'this-string-will-be-sluggified'),
            array("Tänk efter nu – förr'n vi föser dig bort", 'tank-efter-nu-forrn-vi-foser-dig-bort'),
            array('', ''),
        );
    }
}
```

运行phpunit

![](2.png)









