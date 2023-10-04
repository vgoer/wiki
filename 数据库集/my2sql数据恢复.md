---
title: my2sql数据恢复
description: 
published: 1
date: 2023-09-08T03:52:31.210Z
tags: 
editor: markdown
dateCreated: 2023-09-08T03:52:29.188Z
---

<center>My2Sql</center>





[toc]







## My2Sql

> go版MySQL binlog解析工具，通过解析MySQL binlog ，可以生成原始SQL、回滚SQL、去除主键的[INSERT](https://so.csdn.net/so/search?q=INSERT&spm=1001.2101.3001.7020) SQL等，也可以生成DML统计信息。 [github](https://github.com/liuhr/my2sql) [gitee](https://gitee.com/mirrors/my2sql)





### 1. 安装

> 源码编译：官方没有打包

```shell
git clone https://github.com/liuhr/my2sql.git
cd my2sql/
go build .
```





### 2. 数据库案例

> **误删整张表数据，需要紧急回滚**

> mysql启用`bin_log`

```shell
# 开启 确保你已经启用了二进制日志功能。在MySQL配置文件（一般是my.cnf或my.ini）中，确保下面的配置项被正确设置：
log_bin = ON

# 重启 产生日志的操作，例如插入、更新或删除数据，创建或修改表结构等。

# 使用以下命令检查是否成功生成了binlog日志：
SHOW BINARY LOGS;
```

> 数据库操作

```shell
# 新建数据库
create database testdb；

# 数据表结构
CREATE TABLE `student` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `number` int(11) NOT NULL,
  `name` varchar(255) DEFAULT NULL,
  `add_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP COMMENT '添加的时间',
  `content` json DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `idx_name` (`number`,`name`)
) ENGINE=InnoDB AUTO_INCREMENT=1234 DEFAULT CHARSET=utf8


# 添加数据
INSERT INTO `testdb`.`student` (`id`,`number`,`name`,`add_time`,`content`) VALUES (1233,26,'ranran','2020-07-15 19:06:03',null);
INSERT INTO `testdb`.`student` (`id`,`number`,`name`,`add_time`,`content`) VALUES (1232,134,'asdf','2020-07-12 11:08:41',null);
INSERT INTO `testdb`.`student` (`id`,`number`,`name`,`add_time`,`content`) VALUES (1231,21,'chenxi','2020-07-12 10:12:45',null);
INSERT INTO `testdb`.`student` (`id`,`number`,`name`,`add_time`,`content`) VALUES (1229,20,'chenxi','2020-07-11 16:20:50',null);
INSERT INTO `testdb`.`student` (`id`,`number`,`name`,`add_time`,`content`) VALUES (1227,18,'hanran','2020-07-06 21:55:48','{\"age\":13,\"author\":\"liuhan\"}');
INSERT INTO `testdb`.`student` (`id`,`number`,`name`,`add_time`,`content`) VALUES (1226,17,'hanran','2020-07-06 21:55:48','{\"age\":13,\"author\":\"liuhan\"}');
INSERT INTO `testdb`.`student` (`id`,`number`,`name`,`add_time`,`content`) VALUES (1224,16,'hanran','2020-07-06 21:55:48','{\"author\":\"liuhan\"}');


#为了方便演示，重新生成一个binlog
flush logs;

# 删除数据
delete from student;

# 查看数据
select * from student;
```



> 查看生成的binlog

```shell
show master status;
+-----------+----------+--------------+------------------+-------------------+
| File      | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+-----------+----------+--------------+------------------+-------------------+
| ON.000003 |      837 |              |                  |                   |
+-----------+----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)
```



> **根据操作时间解析binlog，生成回滚日志**

```php
# linux
./my2sql  -user root -password root123  -port 3306 \
-host 127.0.0.1 -databases testdb  -tables student \
-work-type rollback   -start-file ON.000002 \
-start-datetime "2023-09-07 00:00:00" --stop-datetime "2023-09-08 00:00:00" \
-output-dir tmpdir/
    
# win
my2sql.exe -user root -password root123 -port 3306 ^
-host 127.0.0.1 -databases testdb -tables student ^
-work-type rollback -start-file ON.000002 ^
-start-datetime "2023-09-07 00:00:00" --stop-datetime "2023-09-08 00:00:00" ^
-output-dir .
```



> 我去，牛皮，大声，作者牛皮。 







