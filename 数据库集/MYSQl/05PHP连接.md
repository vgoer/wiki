---
title: 05.PHP连接
description: PHP连接
published: 1
date: 2023-05-05T11:54:52.817Z
tags: mysql, php
editor: markdown
dateCreated: 2023-04-22T17:53:36.053Z
---

<center>link</center>

[toc]

## 连接

> php 连接mysql 了

> **mysqli_connect()** 连接函数   --   重要

```php

/* 
    连接数据库
        host        主机地址
        username    用户名
        password    密码
        dbname      默认使用数据库(可不写)
        port        连接到 MySQL 服务器的端口 默认3306(可不写)
        socket      socket 或要使用的已命名 pipe
*/
$link = mysqli_connect('localhost','root','root','数据库'，'端口')
if(!$link){
    echo 'no';
}else{
    echo 'yes';
}
```

> mysqli_connect_error()  错误描述 

```php
// mysqli_connect_error()返回上一次连接错误的错误描述 
// 指针读取的方式
if(!$link){//如果连接数据库失败
    echo "错误码：". mysqli_connect_errno(); //错误码
    echo "错误：". mysqli_connect_error();   // 错误信息
}
```



> mysqli_close()  关闭数据库

```php
mysqli_close($link); //关闭资源
```



> mysqli_query()  **查询语句**

```php
$sql = 'select *  from table';
$res = mysqli_query($link,$sql);  //资源  否则返回false
```



> mysqli_select_db() **选择数据库**

```php
mysqli_select_db($link,'数据库名');
```



> mysqli_num_rows()  获取返回结果行数

```php
$num = mysqli_num_rows($link,$res);  //查询语句的资源
echo $num; // 得到的时数据的有多少条
```



> mysqli_set_charset() 设置**客户端字符集**

```php
mysqli_set_charset($link,'utf-8');
```



> mysqli_fetch_assoc() **获取数据作为关联数组**

```php
$res = mysqli_query($link,$sql);
$data = array();
if($res && mysqli_num_rows($res)>0){
    while ($arr = mysqli_fetch_assoc($res)) {
        // 指针下移，一次读取一条数据
        $data[] = $arr;
    }
}
//释放内存
mysqli_free_result($res);
```

>  mysqi_fetch_all() **获取数据作为关联数组**

```php
$data = mysqli_fetch_all($res,MYSQLI_ASSOC); //数组和上面一样
```



```php
// mysqli_get_server_info 获取MySQL服务器版本号
 echo mysqli_get_server_info($link);

// mysqli_insert_id 返回最后一个查询中自动生成的 ID
echo mysqli_insert_id($link);
```



