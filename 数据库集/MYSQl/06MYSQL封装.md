---
title: 06.MYSQL封装
description: MYSQL封装
published: 1
date: 2023-06-09T10:16:46.081Z
tags: mysql, php
editor: markdown
dateCreated: 2023-04-22T17:54:48.680Z
---

<center>封装</center>

[toc]

## 封装mysql函数

> 增删改查

> 连接页面

```php
header('Content-Type:text/html;charset=utf-8');
/* 
    连接数据库
        host        主机地址
        username    用户名
        password    密码
        dbname      默认使用数据库
        port        连接到 MySQL 服务器的端口
        socket      socket 或要使用的已命名 pipe
    */
$link=mysqli_connect('localhost','root','root');
if(!$link){//如果连接数据库失败
    echo "错误码：". mysqli_connect_errno();
    echo "错误：". mysqli_connect_error();
    exit;
}
//选择数据库
mysqli_select_db($conn, "company");
//设置默认客户端字符集
mysqli_set_charset($conn,"utf8");

// 引入下面的函数库
include_once('fun.php');



这里写我们的mysql 函数


//关闭
mysqli_close($link);
```



> 查询 

```php
// 查询一条记录
function getone($sql){
    /**
    */
    global $conn; //将$conn变成全局变量
    $res=mysqli_query($conn,$sql);
    $data=array();
    if($res && mysqli_num_rows($res)>0){
        $data=mysqli_fetch_assoc($res);
    }
    return $data;
}

// 查询索引记录  二维数组
function getall($sql){
    /**
         * 查询函数 
         * $sql1:查询的数据库 语句
         */
    global $link;
    $res = mysqli_query($link,$sql);
    $data = [];

    if($res && mysqli_num_rows($link)){
        while($row = mysqli_fetch_assoc($res)){
            $data[] = $link;
        }
    }
    mysqli_free_result($link);
    //方法2
    // $data = mysqli_fetch_all($res,MYSQLI_ASSOC);
    return $data;
}
```



> 添加

```php
// 添加 返回修改的记录id
function add_data($tables,$data){
    /**
         * 添加
         * 表
         * 数据$
         */
    global $link;
    // $arr = array_keys($data); //获取键当一个索引数组
    $field = "(`".implode('`,`',array_keys($data))."`)";
    $column = "('".implode("','",$data)."')";
    $sql = "INSERT INTO $tables $field values $column";
    echo $sql;
    $res = mysqli_query($link,$sql);
    if($res){
        $insert_id = mysqli_insert_id($link); //添加的记录的id
    }else{
        echo mysqli_error($link);
        exit;
    }
    return $insert_id;
}
```



> 修改

```php
// 修改     返回修改记录的数量
function up_data($tables,$data,$cond){
    /**
         * 修改函数
         * 表
         * 数据
         * $cond 条件
         */
    global $link;
    $field = '';
    foreach($data as $k=>$v){
        $field .= "`$k`='$v',"; //拼接为一个大字符串 注意 ，
    }
    var_dump($field);
    $field = rtrim($field,','); //去掉最右边的，
    //var_dump($field);
    $sql = "UPDATE $tables set $field where $cond";

    $res = mysqli_query($link,$sql);

    if($res){
        $aff_id = mysqli_affected_rows($li$k);//修改记录的数量
    }else{
        echo mysqli_error($link);
        exit;
    }
    return $aff_id;

}
```



> 删除

```php
// 返回 修改的数量
function delete($tables,$cond){
    /**
         * 删除
         * 表
         * 条件
         */
    global $link;
    $sql = "DELETE FROM $tables where $cond";
    $res = mysqli_query($link,$sql);
    if($res){
        $aff_id = mysqli_affected_rows($link);
    }else{
        echo mysqli_error($link);
        exit;
    }
    return $aff_id; //修改的数量
}
```





> 代码全

```php
<?php

namespace DB;

class Database
{
    private string $host;
    private string $username;
    private string $password;
    private string $database;
    private $conn;
    private string $sql;
    private string $dbpre_fix;

    public function __construct(string $host, string $username, string $password, string $database, string $dbpre_fix)
    {
        $this->host = $host;
        $this->username = $username;
        $this->password = $password;
        $this->database = $database;
        $this->dbpre_fix = $dbpre_fix;

        $this->connect();
    }


    private function connect()
    {
        $this->conn = mysqli_connect($this->host, $this->username, $this->password, $this->database);

        if(!$this->conn){
            echo "网站升级中...";
            exit;
        }

        mysqli_set_charset($this->conn, "utf8");
    }

    public function select(string $fields="*")
    {
        $this->sql = "SELECT $fields";
        return $this;
    }


    public function from(string $table)
    {
        // SLECT * FROM pre_person
        $this->sql .= " FROM {$this->dbpre_fix}{$table}";

        return $this;
    }

    public function join(string $join_table, string $join_on, string $join_type="LEFT")
    {
        $this->sql .= " $join_type JOIN {$this->dbpre_fix}$join_table ON $join_on";

        return $this;
    }


    public function where(string $condition)
    {
        $this->sql .= " WHERE $condition";

        return $this;
    }

    public function group(string $group)
    {
        $this->sql .= " GROUP BY $group";

        return $this;
    }

    public function order(string $order)
    {
        $this->sql .= " ORDER BY $order";

        return $this;
    }


    public function limit(int $limit)
    {
        $this->sql .= " LIMIT $limit";
    }


    public function count()
    {
        $res = mysqli_query($this->conn, $this->sql);

        if($res && mysqli_num_rows($res) > 0){
            return mysqli_num_rows($res);
        }
        return 0;
    }

    public function get_one()
    {
        $res = mysqli_query($this->conn, $this->sql);

        $data  = [];
        if($res && mysqli_num_rows($res) > 0){
            $data = mysqli_fetch_assoc($res);
        }

        return $data;
    }

    public function get_all()
    {
        $res = mysqli_query($this->conn, $this->sql);

        $date = [];
        if($res && mysqli_num_rows($res) > 0){
            $data = mysqli_fetch_all($res, MYSQLI_ASSOC);
        }

        return $data;
    }

    public function insert(string $table, array $data)
    {
        // INSERT INTO pre_person (name, age) VALUES ('张三', 18)
        $this->sql = "INSERT INTO {$this->dbpre_fix}$table";
        $this->sql .= " (" . implode(",", array_keys($data)) . ")";
        $this->sql .= " VALUES ('" . implode("','", array_values($data)) . "')";

        return $this;
    }

    public function save(string $table, array $data)
    {
        $this->sql = "UPDATE {$this->dbpre_fix}$table SET ";
        $this->sql .= implode(",", array_map(function($v, $k){
            return "$k='$v'";
        }, $data, array_keys($data)));

        return $this;
    }


    public function delete(string $table, string $condition)
    {
        $this->sql = "DELETE FROM {$this->dbpre_fix}$table WHERE $condition";

        return $this;
    }

    public function query()
    {
        $res = mysqli_query($this->conn, $this->sql);

        if($res){
            return $res;
        }
        echo mysqli_error($this->conn);
        exit;
    }

    public function get_sql()
    {
        return $this->sql;
    }

    public function __destruct()
    {
        mysqli_close($this->conn);
    }
}

```

> 列子

```php
<?php

// 实例化数据库类
$db = new Database();
$db->connect();

// 查询示例
$person = $db->getOne("SELECT * FROM v1_person WHERE id = ?", [1]);
$allPersons = $db->getAll("SELECT * FROM v1_person WHERE depid = ?", [2]);

// 插入示例
$data = [
    'name' => '张三',
    'mobile' => '13800138000',
    'sex' => '1',
    'depid' => 1,
    'create_time' => date('Y-m-d H:i:s')
];
$newId = $db->insert('v1_person', $data);

// 更新示例
$updateData = [
    'name' => '李四',
    'mobile' => '13900139000'
];
$affected = $db->update('v1_person', $updateData, 'id = 1');

// 删除示例
$deleted = $db->delete('v1_person', 'id = 1');
```











