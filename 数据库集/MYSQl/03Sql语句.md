---
title: 03.Sql语句
description: Sql语句
published: 1
date: 2023-04-29T17:48:33.370Z
tags: mysql
editor: markdown
dateCreated: 2023-04-22T17:51:25.842Z
---

<center>sql语句</center>

[toc]



## sql语句

> SQL (结构化查询语言)是用于执行查询的语法。
>
> ```
> SQL 对大小写不敏感！
> ```



### insert into(插入)

> 插入数据

```sql
//单
insert into `table`(`字段1`，`字段2`)values('值','值');
	
//多
insert into `table`(`字段1`，`字段2`)values('值1','值1'),('值2','值2');
```

> 不用**\`\`**号也可以，但是在代码中可以反正`sql注入`



### select (查询)**

> 查询语句

```sql
// 查询全部数据
select * from `table`;

//查询所需字段
select name,age... from `table`;

// as 修改字段别名
select name as '姓名' from `table` ;

// and or
select * from (id>4 or id<10) and name="aa"
```



#### 1.order by

```sql
// id 升序
select * from table order by id asc;

//时间  降序
select * from table order by time(你想要排序的字段) desc;
```



### 2. where

> 指定条件 `or 或者  and 并且`

```sql
// 1.条件
select * from table where id>5;    //查询id大于5的

// 2. or and
select * from table where (id>5 or id<20) and (name='a' or name='c');
// 查询 id大于5或小于20  并且  姓名等于a或c

// 3. between  and
select * from table where id between 3 and 10;
// 查询 id 3到10的记录

// in   和  not in
select * from table id in (2,4,6);     //id是2，4，6的记录
select * from table id not in (4,5,6);  //id不是4,5,6的其他数据

// limit 
select * from table limit 4;    //只查询 4条记录
select * from table limit 4,6;  //5开始查询6条记录

// group by (把相同的数据分到一组)
// 用到count()函数，计算数量
select sex as '性别',count(sex) from table group by sex; //统计出相同值得数量

//模糊查询  
select * from table where `name(字段)` like '%马%'; %在前--后面必须是马结尾
// 查询name字段 包含马字的记录     精准查询 直接 '数据
```

#### 3.联合查询

>使用 union 和 union all 关键字，将**两个表的数据按照一定的查询条件查询出来后，将结果合并到一起显示**

```sql
而 union 是将 union all 后的结果进行一次distinct，去除重复记录后的结果。

union 去重
select name form table1 union select name from table2; // order by id asc;排序
// 查询两个表 name字段 去重复
select id from table1 union all select id from table2;
// 查询两个表 id字段 没有去重
```

​                                                                                                                                                                           ‘

### update(更新)

> 更新修改数据

```sql
// 修改一条记录 
update `table` set 字段1='值1',字段2='值2',...  where 条件 
// 修改字段的 -- 条件 id=任意 name=什么呀 等等
// 多个就
update `table` set 字段1='值1',字段2='值2',...  where id in (1,2,3) //id=1 2 3 
```



### delete(删除)

> 删除数据

```sql
// 删除一条记录
delete from `table` where 条件; // 任意条件

// 多条记录
delete from `table` where `id` in (1,2,3);  // 删除id等于1，2，3的记录
```

f's



### 连表查询*

> 注意：**连表查询表一定加别名**  靠**外键**
>
> 清楚你的**主表和从表 主键和外键** 就好理解了

```sql
// 左连表  LEFT JOIN (常用)
以左边的表为基准，先把左边的表查出来，再查右边的表，若右边的表没对应的数据显示的就为NULL。

select a.name as '表一name',b.name as '表二name' from table1 as a LEFT JOIN table2 as b ON a.id = j.外键id;

// 右连表 RIGHT JOIN(与左连表相似 ，不常用)
以右边的表为基准，先把右边的表查出来，再查左边的表，若左边的表没对应的数据显示的就为NULL

select a.name as '表一name',b.name as '表二name' from table1 as a RIGHT JOIN table2 as b ON a.外键id = j.id; 

// 内连表 INNER join (常用)
以两个表中有至少一个匹配，否则不显示
select a.name as '表一name',b.name as '表二name' from table1 as a INNER JOIN table2 as b ON a.id = j.id; 
```

> **三连表**  

```sql
select d.name as '员工',b.name as '部门',c.`name` as '职位' FROM pre_person as d INNER JOIN pre_department as b ON d.depid=b.id INNER JOIN pre_job as c on d.jobid = c.id;
// 清楚你的主表和从表 主键和外键 就好理解了

//查出主表全部数据加上其他   a.*,
select a.*,b.name as '部门',c.`name` as '职位' FROM pre_person as a INNER JOIN pre_department as b ON a.depid=b.id INNER JOIN pre_job as c on a.jobid = c.id ORDER BY a.id ASC;
```



> group   by

```sql
// group by (把相同的数据分到一组)
// 用到count()函数，计算数量       
select sex as '性别',count(sex) from table group by sex; //统计出相同值得数量
```



> having

```sql
// 在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与合计函数一起使用

select age,sum(age) from table group by age having sum(age);
// 把想同年龄的 分到一组  求和
```

