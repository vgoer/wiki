---
title: 01PostGreSQL
description: 
published: 1
date: 2023-05-29T04:09:17.685Z
tags: 
editor: markdown
dateCreated: 2023-05-11T10:46:23.959Z
---

<center>PostGreSQL</center>



[toc]



# PostGreSQL 

> 最先进现代的开源对象 ==关系系数据库== [官网](https://www.postgresql.org/)



## 1. 优点

1. 开源：更友好的开源协议(用户可以任何目的修改发布)。` mysql`开源但属于`Oracle`
2. 先进：高度兼容SQL标准，支持事务的ACID原则
3. 功能丰富： [华为百科](https://www.huaweicloud.com/zhishi/db17.html)

4. 可扩展性:  支持任意语言, FDW(外部表,对接其他数据库)，扩展性很强



## 2. 安装

> 去官网下载：  [官网](https://www.postgresql.org/)

```shell
// 设置超级用户 密码

// 监听端口 5432
```



### 3. 初始化表

> `pgAdmin 4`   官方的管理工具 
>
> 来老师这里查看初始化的sql[dongxuyang](https://github.com/dongxuyang1985/postgresql_dev_guide)

```sql
// 新建数据库， 修改配置连接 到 新建的数据库
1. 创建表 01_create_table.sql 

2. 插入数据 02_insert_data.sql 

3. 创建索引  03_create_index.sql  

4. 删除表   99_delete_table.sql 
```



### 4. 查询语句

> 和 其他sql一样

```sql
// 1 查询
SELECT * FROM jobs;    # 所有字段 

SELECT job_id as "工作号码",job_title as "工作标题" FROM jobs;  # 查询单个， as 别名 

SELECT DISTINCT min_salary FROM jobs;  -- DISTINCT 去重

2. 注释
-- 我是一个注释呀
/*  我是第二个注释 */

3. 获取一下信息
SELECT 1*100 as "结果";
SELECT version() as "数据库版本";
```

> `WHERE` 条件语句查询

```sql
比较符
-- =  !=   > <  >= <=  
SELECT * FROM departments WHERE department_id = 10; 

之间 BETWEEN ... AND ... -- 之间
SELECT * FROM departments WHERE  department_id  BETWEEN 20 AND 60;

之内 IN   相当于= 
SELECT * FROM departments WHERE  department_id  IN (30,60,70);  


```

