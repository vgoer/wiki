---
title: 04.Sql扩展
description: Sql扩展
published: 1
date: 2023-04-22T17:52:20.863Z
tags: mysql
editor: markdown
dateCreated: 2023-04-22T17:52:17.817Z
---

<center>sql拓展</center>

[toc]



运算符

**算术运算符**

| 符号 | 说明              |
| :--- | :---------------- |
| +    | 加法运算          |
| -    | 减法运算          |
| *    | 乘法运算          |
| /    | 除法运算,返回商   |
| %    | 求余运算,返回余数 |

**比较运算符**

| 符号  | 说明     |
| :---- | :------- |
| >     | 大于     |
| <     | 小于     |
| >=    | 大于等于 |
| <=    | 小于等于 |
| !=,<> | 不等于   |
| =     | 等于     |



**逻辑运算符**

| 符号         | 说明   |
| :----------- | :----- |
| NOT或者 !    | 逻辑非 |
| AND或者 &&   | 逻辑与 |
| OR 或者 \|\| | 逻辑或 |

> 合计函数

| 函数          | 描述                             |
| :------------ | :------------------------------- |
| AVG(column)   | 返回某列的平均值                 |
| COUNT(column) | 返回某列的行数（不包括 NULL 值） |
| COUNT(*)      | 返回被选行数                     |
| MAX(column)   | 返回某列的最高值                 |
| MIN(column)   | 返回某列的最低值                 |
| SUM(column)   | 返回某列的总和                   |

```sql
/* 版本 */
select VERSION();

/* 用户 */
select USER();
select SYSTEM_USER();
select SESSION_USER();
select CURRENT_USER();

/* 当前数据库 */
select DATABASE();
```



> 命令行

```shell
1. 连接
mysql (-h localhost) -u root -p +密码

2. 查看所有数据库
show databases;

3. 创建数据库，设置编码
create database 数据库名 charset utf-8;

4. 删除数据库
drop database;

5. 使用数据库
use 数据库名

6. 查看数据库所用的表
show tables;

7.查看存储引擎
show engines; 

8. 创建表
create table 表名

9.删除表
drop table 表名

10.修改表
alter table 表名1 rename to 表名2

11. 查看指定数据库的表
show tables from 数据库名;
```

> 导入和导出数据库

```shell
// 导出
mysqldump -u 用户名 -p 数据库名 > 存放位置/导出的文件名
eg: mysqldump -u root -p 数据库 > c:\u.sql
//导出数据表
mysqldump -u 用户名 -p 数据库名 表名> 存放位置/导出的文件名

// 导入
//1. 首页要创建一个空数据库
mysql -u 用户名 -p 数据库名 < 存放位置(导出sql语句的文件)
mysql -u root -p 你创建的数据库 < c:\u.sql

//2. 进入mysql
mysql -uroot -p 
show databases;
create database demo; use demo;
source c:\u.sql;
```

> 清空表

```sql
// 空表的数据，并且让自增的id从1开始自增
truncate 表名   
注：有外键的从表，必须先清除设置主表
//delete 删除数据 id永远不会变
delete from table where id=4; // id=4永远也没用这条数据
```



