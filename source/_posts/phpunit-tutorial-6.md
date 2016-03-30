title: PHPUnit第七章——数据库测试
date: 2016-02-24 17:15:43
categories: PHPUnit
tags: PHPUnit
---

<center>介绍怎么对数据库进行测试</center>

<!--more-->

## 数据库测试的四个阶段

>Gerard Meszaros 在他的书《xUnit 测试模式》中列出了单元测试的四个阶段：
建立基境(fixture)
执行被测系统
验证结果
拆除基境(fixture)

## PHPUnit 数据库测试用例的配置

一般而言，使用 PHPUnit 时，测试用例都是按如下方式扩展自 PHPUnit_Framework_TestCase 类,但如果测试代码用到了数据库扩展模块，那么建立的过程就会更复杂一些，需要扩展另一个抽象 TestCase 类，它要求实现两个抽象方法，getConnection() 和 getDataSet()

```
<?php
class MyGuestbookTest extends PHPUnit_Extensions_Database_TestCase
{
    /**
     * @return PHPUnit_Extensions_Database_DB_IDatabaseConnection
     */
    public function getConnection()
    {
        $pdo = new PDO('sqlite::memory:');
        return $this->createDefaultDBConnection($pdo, ':memory:');
    }

    /**
     * @return PHPUnit_Extensions_Database_DataSet_IDataSet
     */
    public function getDataSet()
    {
        return $this->createFlatXMLDataSet(dirname(__FILE__).'/_files/guestbook-seed.xml');
    }
}
?>
```

## 使用自己的抽象数据库TestCase类

从前面的实现范例中容易发现 getConnection() 方法是相当稳定的，可以在不同的数据库测试用例中重用。另外，为了保持测试的性能良好和数据库的开销较低，可以对代码进行一点重构，来为应用程序形成一个通用的抽象测试用例，同时依然可以为每个具体测试用例指定不同的数据基境


我们在 phpUnitTutorial 文件夹下建立Fixtures文件夹，抽象自己的TestCase类，并在Fixtures文件夹下建立fixtures子文件夹用于存放xml数据

建立自己的抽象类FixturesTestCase

```
<?php
namespace phpUnitTutorial\Fixtures;

use PHPUnit_Extensions_Database_TestCase;
use PDOException;
use PDO;
use PHPUnit_Extensions_Database_DataSet_CompositeDataSet;

class FixturesTestCase extends PHPUnit_Extensions_Database_TestCase{
    public $fixtures = array(
        //table表名
        'posts',
        'postmeta',
        'options'
    );

    //连接变量
    private $conn = null;

    //设立基境
    public function setUp()
    {

        //获取mysql连接
        $conn = $this->getConnection();
        //建立PDO连接
        $pdo = $conn->getConnection();
        // set up tables
        $fixtureDataSet = $this->getDataSet($this->fixtures);
        foreach ($fixtureDataSet->getTableNames() as $table) {
            // drop table
            $pdo->exec("DROP TABLE IF EXISTS `$table`;");
            // recreate table
            //Returns a table meta data object for the given table
            $meta = $fixtureDataSet->getTableMetaData($table);
            $create = "CREATE TABLE IF NOT EXISTS `$table` ";
            $cols = array();
            foreach ($meta->getColumns() as $col) {
                $cols[] = "`$col` VARCHAR(200)";
            }
            $create .= '('.implode(',', $cols).');';
            $pdo->exec($create);
        }
        parent::setUp();
    }

    public function tearDown()
    {
        $allTables =
            $this->getDataSet($this->fixtures)->getTableNames();
        foreach ($allTables as $table) {
            // drop table
            $conn = $this->getConnection();
            $pdo = $conn->getConnection();
            $pdo->exec("DROP TABLE IF EXISTS `$table`;");
        }
        parent::tearDown();
    }

    public function getConnection()
    {
        // TODO: Implement getConnection() method.
        if($this->conn == null){
            try {
                $pdo = new PDO('mysql:host=localhost;dbname=waimai', 'root', '');
                $this->conn = $this->createDefaultDBConnection($pdo, 'test');
            } catch (PDOException $e) {
                echo $e->getMessage();
            }
        }
        return $this->conn;
    }

    public function getDataSet($fixtures = array())
    {
        // TODO: Implement getDataSet() method.
        if (empty($fixtures)) {
            $fixtures = $this->fixtures;
        }

        $compositeDs = new
        PHPUnit_Extensions_Database_DataSet_CompositeDataSet(array());
        $fixturePath = dirname(__FILE__) . DIRECTORY_SEPARATOR . 'fixtures';


        foreach ($fixtures as $fixture) {
            $path =  $fixturePath . DIRECTORY_SEPARATOR . "$fixture.xml";
            $ds = $this->createMySQLXMLDataSet($path);
            $compositeDs->addDataSet($ds);
        }
        return $compositeDs;
    }

    public function loadDataSet($dataSet) {
        // set the new dataset
        $this->getDatabaseTester()->setDataSet($dataSet);
        // call setUp whateverhich adds the rows
        $this->getDatabaseTester()->onSetUp();
    }
}
```

在fixtures文件夹下建立xml数据

options.xml

```
<?xml version="1.0"?>
<mysqldump xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<database name="test">
  <table_data name="prefix_options">
		<row>
			<field name="option_id">1</field>
			<field name="option_name">siteurl</field>
			<field name="option_value">http://wordpress.local</field>
		</row>
		<row>
			<field name="option_id">2</field>
			<field name="option_name">home</field>
			<field name="option_value">http://wordpress.local</field>
		</row>
	</table_data>
</database>
</mysqldump>

```

postmeta.xml

```
<?xml version="1.0"?>
<mysqldump xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<database name="test">
  <table_data name="postmeta">
		<row>
			<field name="meta_id">1</field>
			<field name="post_id">1</field>
			<field name="meta_key">Something</field>
			<field name="meta_value">For Nothing</field>
		</row>
		<row>
			<field name="meta_id">2</field>
			<field name="post_id">2</field>
			<field name="meta_key">MetaKey</field>
			<field name="meta_value">MetaValue</field>
		</row>
		<row>
			<field name="meta_id">3</field>
			<field name="post_id">1000</field>
			<field name="meta_key">More</field>
			<field name="meta_value">Value</field>
		</row>
		<row>
			<field name="meta_id">4</field>
			<field name="post_id">1001</field>
			<field name="meta_key">Even More</field>
			<field name="meta_value">Value</field>
		</row>
	</table_data>
</database>
</mysqldump>

```

post.xml

```
<?xml version="1.0"?>
<mysqldump xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<database name="test">
  <table_data name="posts">
		<row>
			<field name="ID">1</field>
			<field name="post_content">Welcome to WordPress. This is your first post. Edit or delete it, then start blogging!</field>
			<field name="post_title">Hello world!</field>
			<field name="post_name">hello-world</field>
			<field name="guid">http://wordpress.local/?p=1</field>
		</row>
		<row>
			<field name="ID">2</field>
			<field name="post_content">This is an example of a WordPress page, you could edit this to put information about yourself or your site so readers know where you are coming from. You can create as many pages like this one or sub-pages as you like and manage all of your content inside of WordPress.</field>
			<field name="post_title">About</field>
			<field name="post_name">about</field>
			<field name="guid">http://wordpress.local/?page_id=2</field>
		</row>
	</table_data>
</database>
</mysqldump>

```

## 建立测试

在Tests文件夹下建立测试文件
DatabaseTest.php

```
<?php
namespace phpUnitTutorial\Test;

use phpUnitTutorial\Fixtures;
use PDO;

class DatabaseTest extends Fixtures\FixturesTestCase
{

    public $fixtures = array(
        'posts',
        'postmeta',
        'options'
    );

    function testReadDatabase() {
        $conn = $this->getConnection()->getConnection();

        // fixtures auto loaded, let's read some data
        $query = $conn->query('SELECT * FROM posts');
        $results = $query->fetchAll(PDO::FETCH_COLUMN);
        $this->assertEquals(2, count($results));

        // now delete them
        $conn->query('TRUNCATE posts');

        $query = $conn->query('SELECT * FROM posts');
        $results = $query->fetchAll(PDO::FETCH_COLUMN);
        $this->assertEquals(0, count($results));

        // now reload them
        $ds = $this->getDataSet(array('posts'));
        $this->loadDataSet($ds);

        $query = $conn->query('SELECT * FROM posts');
        $results = $query->fetchAll(PDO::FETCH_COLUMN);
        $this->assertEquals(2, count($results));
    }

}
```


