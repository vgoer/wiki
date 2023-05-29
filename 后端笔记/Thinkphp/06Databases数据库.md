---
title: 06.Databases数据库
description: Databases数据库
published: 1
date: 2023-05-29T04:16:19.280Z
tags: thinkphp
editor: markdown
dateCreated: 2023-04-22T18:20:50.314Z
---

<center>数据库</center>

[toc]

## database

> 应用定义数据库配置文件（`appliation/database.php`），里面设置了应用的全局数据库配置信息。

```php
return [
    // 数据库类型
    'type'        => 'mysql',
    // 服务器地址
    'hostname'    => '127.0.0.1',
    // 数据库名
    'database'    => 'test',
    // 数据库用户名
    'username'    => 'root',
    // 数据库密码
    'password'    => '',
    // 数据库连接端口
    'hostport'    => '',
    // 数据库连接参数
    'params'      => [],
    // 数据库编码默认采用utf8
    'charset'     => 'utf8',
    // 数据库表前缀
    'prefix'      => '',
    // 数据库调试模式
    'debug'       => true,
    'params' => [
        // 使用长连接
        \PDO::ATTR_PERSISTENT => true,
    ], 
];
```

#### 原生查询

> 数据库连接信息后，我们就可以直接进行==原生的SQL查询==操作了，==DB类包括`query`和`execute`两个方法==，分别用于查询操作和写操作

**创建（create）**

```php
// 插入记录
$result = Db::execute('insert into think_data (id, name ,status) values (5, "thinkphp",1)');
dump($result);
```

**更新（update）**

```php
// 更新记录
$result = Db::execute('update think_data set name = "framework" where id = 5 ');
dump($result);
```

**读取（read）**

```php
// 查询数据
$result = Db::query('select * from think_data where id = 5');
dump($result);
```

**删除（delete）**

```php
// 删除数据
$result = Db::execute('delete from think_data where id = 5 ');
dump($result);
```

> 读操作都使用`query`方法，写操作使用`execute`方法



#### 查询构造器

> 数据库查询构造器，可以更方便执行数据库操作  -> 查询构造器基于PDO实现

```php
// 修改表前缀 就可以这样查询了
// 插入记录
Db::name('data')
    ->insert(['id' => 18, 'name' => 'thinkphp']);

// 更新记录
Db::name('data')
    ->where('id', 18)
    ->update(['name' => "framework"]);

// 查询数据
$list = Db::name('data')
    ->where('id', 18)
    ->select();
dump($list);

// 删除数据
Db::name('data')
    ->where('id', 18)
    ->delete();
```

> 系统提供的==助手函数`db`==

```php
$db = db('data');  //数据库
// 插入记录
$db->insert(['id' => 20, 'name' => 'thinkphp']);

// 更新记录
$db->where('id', 20)->update(['name' => "framework"]);

// 查询数据
$list = $db->where('id', 20)->select();
dump($list);

// 删除数据
$db->where('id', 20)->delete();
```

#### 链式操作

使用链式操作可以完成复杂的数据库查询操作，例如：

```php
// 查询十个满足条件的数据 并按照id倒序排列
$list = Db::name('data')
    ->where('status', 1)   //条件
    ->field('id,name')     //查询字段
    ->order('id', 'desc')  //排序
    ->limit(10)           //数据数量
    ->select();
dump($list);
```

链式操作不分先后，只要在查询方法（这里是`select`方法）之前调用就行，所以，下面的查询是等效的：

```php
// 查询十个满足条件的数据 并按照id倒序排列
$list = Db::name('data')
    ->field('id,name')
    ->order('id', 'desc')
    ->where('status', 1)
    ->limit(10)
    ->select();
dump($list);
```

支持链式操作的查询方法包括：

| 方法名  | 描述         |
| :------ | :----------- |
| select  | 查询数据集   |
| find    | 查询单个记录 |
| insert  | 插入记录     |
| update  | 更新记录     |
| delete  | 删除记录     |
| value   | 查询值       |
| column  | 查询列       |
| chunk   | 分块查询     |
| count等 | 聚合查询     |



#### 事务支持

对于事务的支持，最简单的方法就是使用`transaction`方法，只需要把需要执行的事务操作封装到闭包里面即可自动完成事务，例如：

```php
Db::transaction(function () {
    Db::table('think_user')
        ->delete(1);
    Db::table('think_data')
        ->insert(['id' => 28, 'name' => 'thinkphp', 'status' => 1]);
});
```

一旦`think_data`表写入失败的话，系统会自动回滚，写入成功的话系统会自动提交当前事务。

也可以手动控制事务的提交，上面的实现代码可以改成：

```php
// 启动事务
Db::startTrans();
try {
    Db::table('think_user')
        ->delete(1);
    Db::table('think_data')
        ->insert(['id' => 28, 'name' => 'thinkphp', 'status' => 1]);
    // 提交事务
    Db::commit();
} catch (\Exception $e) {
    // 回滚事务
    Db::rollback();
}
```