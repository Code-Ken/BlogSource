title: PHP扩展Mysqli常用操作
date: 2016-01-26 13:49:31
categories: PHP
tags: [PHP,Mysql]
---

<center>本文介绍php mysqli扩展库的常用操作</center>

<!--more-->


# mysqli常用操作

## 连接关闭数据库

```
mysqli_connect(host,username,password,dbname,port,socket);

$mysqli = new mysqli("localhost", "user", "password", "database");
$mysqli->close();
```


## 设置字符串编码

为了避免显示乱码问题需要指定字符串编码

```
$mysqli->set_charset('utf8');
```
## 查看字符串编码

返回当前数据库连接的默认字符编码


```
string mysqli::character_set_name ( void )
```

## 执行sql语句

```
$mysqli->query($sql); //失败时返回FALSE
```

# mysqli错误信息

## 显示上一次错误描述

```
string $mysql->error
```

## 显示错误列表


```
array $mysqli->error_list;
//返回样例
Array
(
    [0] => Array
        (
            [errno] => 1193
            [sqlstate] => HY000
            [error] => Unknown system variable 'a'
        )

)

```

## 返回连接错误编号和内容


```
mysqli_connect_errno //Returns an error code value. Zero if no error occurred    
mysqli_connect_error //Return an error message,NULL if no error

//example

<?php
$mysqli = new mysqli("localhost", "my_user", "my_password", "world");

/* check connection */
if ($mysqli->connect_errno) {
    printf("Connect failed: %s\n", $mysqli->connect_error);
    exit();
} 

```

# mysqli事务


```
$mysqli->begin_transaction();//开启事务
$mysqli->rollback();//事务回滚
$mysqli->commit();//提交事务

<?php
$mysqli = new mysqli("127.0.0.1", "my_user", "my_password", "sakila");

if ($mysqli->connect_errno) {
    printf("Connect failed: %s\n", $mysqli->connect_error);
    exit();
}

$mysqli->begin_transaction(MYSQLI_TRANS_START_READ_ONLY);

$mysqli->query("SELECT first_name, last_name FROM actor");
$mysqli->commit();

$mysqli->close();

```

# mysqli_stmt类

代表mysqli预处理类


```
$stmt->prepare($sql)//预处理语句 成功返回TRUE，失败返回FALSE
$stmt->bind_param()//绑定参数 成功返回TRUE，失败返回FALSE
>参数类型:i => 整数,d => 浮点数,s => 字符串

$stmt->execute()//执行预处理语句 成功返回TRUE，失败返回FALSE
$stmt->close()//关闭预处理语句 成功返回TRUE，失败返回FALSE
$stmt->bind_result()//绑定结果集  成功返回TRUE，失败返回FALSE
$stmt->fetch()//依次取结果集 成功返回TRUE，失败返回FALSE
$stmt->get_result()//获取预处理语句的结果集
$stmt->store_result()//保存结果集
$stmt->free_result()//释放结果集
$stmt->num_rows()//返回储存结果集的数量
```

```
<?php
$mysqli = new mysqli("localhost", "my_user", "my_password", "world");

if (mysqli_connect_errno()) {
    printf("Connect failed: %s\n", mysqli_connect_error());
    exit();
}

/* prepare statement */
if ($stmt = $mysqli->prepare("SELECT Code, Name FROM Country ORDER BY Name LIMIT 5")) {
    $stmt->execute();

    /* bind variables to prepared statement */
    $stmt->bind_result($col1, $col2);

    /* fetch values */
    while ($stmt->fetch()) {
        printf("%s %s\n", $col1, $col2);
    }

    /* close statement */
    $stmt->close();
}
/* close connection */
$mysqli->close();

?>
```

输出结果

```
AFG Afghanistan
ALB Albania
DZA Algeria
ASM American Samoa
AND Andorra
```
```
<?php
/* Open a connection */
$mysqli = new mysqli("localhost", "my_user", "my_password", "world");

/* check connection */
if (mysqli_connect_errno()) {
    printf("Connect failed: %s\n", mysqli_connect_error());
    exit();
}

$query = "SELECT Name, CountryCode FROM City ORDER BY Name LIMIT 20";
if ($stmt = $mysqli->prepare($query)) {

    /* execute query */
    $stmt->execute();

    /* store result */
    $stmt->store_result();

    printf("Number of rows: %d.\n", $stmt->num_rows);

    /* close statement */
    $stmt->close();
}

/* close connection */
$mysqli->close();
?>
```

输出结果

```
Number of rows: 20.
```

# mysqli_result类

代表从一个数据库查询中获取的结果集。




```
//函数从结果集中取得所有行作为关联数组，或数字数组，或二者兼有
mixed mysqli_result::fetch_all ([ int $resulttype = MYSQLI_NUM ] )

//函数从结果集中取得一行作为关联数组，或数字数组，或二者兼有。
mixed mysqli_result::fetch_array ([ int $resulttype = MYSQLI_BOTH ] )

$resulttype 可选。规定应该产生哪种类型的数组。
可以是以下值中的一个：
                MYSQLI_ASSOC
                MYSQLI_NUM
                MYSQLI_BOTH
```














