---
title: 04.sql安全
description: sql关系型数据库安全
published: 1
date: 2023-06-09T10:16:53.721Z
tags: web safe, mysql
editor: markdown
dateCreated: 2023-04-19T17:21:26.041Z
---

<center>sql注入和简单绕过</center>

[toc]



## sql注入

> `sql`:结构化查询语言 (Structured Query language)

> `sql注入`:==用户提交的数据可以被数据库解析执行==（前端写的sql语句被后端执行）

1. 连接mysql

> mysql -u root -p    密码



2. 查看版本

> select version();



3. 基本操作

```sql
show databases;  	//查看数据库
create database test;   //新建test数据库  
drop database test  //小心删除

use test;  //使用数据库
show tables; //查看表

//创建表
create table admin(id int not null auto_increment, username varchar(100) not null, password varchar(100) not null, primary key(id));
// not null 不为空 
// auto_increment  自增
// primary key(id) 主键 id

// 插入数据
insert into admin(username,password) values("admin","password");
// insert into 插到哪里  admin表的字段
// values() 值

delete from admin where id= ;  //删除字段id=

show full columns from admin;   // 查看字段 意义
```

> `google :  inurl:php?id=`**搜索语法**



```sql 
select * from admin where id=1; //查询admin表id为1的数据

```

> 联合查询

```sql
// 两张表再设计时就有关联
// 联合查询   union
select * from admin where id=1 union select * from user;
// 字段要相同 不然报错
// 构建字段相同
select * from admin where id=1 union select 1,2,3 from user; 
// admin 有三个字段，user也要有是三个字段 
select * from admin where id=1 union select name,2,3 form user;
select * from admin where id=1 union select name,2,version() form user;
// 版本信息

//查询是否存在字段->username
select * from admin where id = 1 and exists(select username from student);
```

> 绕过

```shell
# 如果 过滤了union呢

# 这样也可以执行 UnioN  -> 大写绕过
select * from admin UnioN select 1,2,3 from blogs 
```





> php 连接数据库

```php
// 获取到表单接受到得数据
$name = $_POST['name'];
$pass = $_POST['pass'];

// 连接函数
$link = mysqli_connect(
    'localhost',    //连接mysql地址
    'root',         // 连接用户名
    'root',         //连接用户名密码
    'mysql'         // 连接数据库名
);

// 查看是否错误
if(!$link){
    // 返回连接数据库错误信息
    printf("Con't  to Mysql :%s",mysqli_connect_error());
    exit;
}else{
    echo '数据库连接成功'.'</br>';
}

// sql语句
$sql = "select * from user where name ='".$name."'";
// 查询
if($result = mysqli_query($link,$sql)){
    // p($result); //对象
    // exit;
    // 查询结果 返回的值
    while($row = mysqli_fetch_assoc($result)){
        //p($row);
        echo $row['name']."&nbsp;"."<br>";
    }
    mysqli_free_result($result); // 释放内存
}

mysqli_close($link); // 关闭连接
```

==函数==

```php
// mysqli_query() 函数执行某个针对数据库的查询。
//参数
//   返回一个 mysqli_result 对象。
```

| 参数         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| *connection* | 必需。规定使用的 MySQL 连接。                                |
| *query*      | 必需，规定查询字符串。                                       |
| *resultmode* | 可选。一个常量。可以是下列值中的任意一个：MYSQLI_USE_RESULT（如果需要检索大量数据，请使用这个）MYSQLI_STORE_RESULT（默认） |

> mysqli_fetch_assoc()    就是查询结果返回来变成关联数组

```php
// 结果集中取得一行作为关联数组：
// 参数 必须mysqli_query()、mysqli_store_result() 或 mysqli_use_result() 返回的结果集标识符。

mysqli_free_result($result); 释放内存
```





#### 自学

```shell
# 1.了解其他表的作用

# 2. 了解mysql函数
```





















