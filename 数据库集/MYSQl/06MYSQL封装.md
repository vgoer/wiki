---
title: 06.MYSQL封装
description: MYSQL封装
published: 1
date: 2023-04-29T17:48:37.629Z
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















