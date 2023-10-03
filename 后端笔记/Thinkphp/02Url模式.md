---
title: 02.Url模式
description: Url模式
published: 1
date: 2023-06-09T10:16:14.049Z
tags: thinkphp
editor: markdown
dateCreated: 2023-04-22T18:16:39.661Z
---

<center>URL模式</center>

[toc]

### url解析

> 域名指向:` public 目录`     下的index.php 入口文件

```
// url
http://servername/index.php/模块/控制器/操作(方法)/参数/值
http://tp.com/index.php/index/eta/name/小王
```



> 如果环境没有开启url伪静态，tp不支持url重写

```
//没有只能传
http://tp.com/index.php?index/eta/name/小东

//找到httpd.conf  加载  mod_rewrite.so 去掉注释
```

### 配置信息




> 模块下的类库文件命名空间同一 `namespace app\模块名`

> 空模块，访问空模块时指定到404模块呀

```
// 开启多模块
// 默认的空控制器名
'empty_controller'       => 'Error',
```

> 开启debug

```
// 应用调试模式
'app_debug'              => false,
```

> 单一模块

```
// 是否支持多模块
'app_multi_module'       => true,
```

> 配置模块

```
//项目配置文件
return [
    // 默认模块名
    'default_module'        => 'index',
    // 默认控制器名
    'default_controller'    => 'Index',
    // 默认操作名
    'default_action'        => 'index',
    //更多配置参数
    //...
];
```

> 数据库配置

```php
return [
    // 数据库类型
    'type'            => 'mysql',
    // 服务器地址
    'hostname'        => '127.0.0.1',
    // 数据库名
    'database'        => 'thinkphp',
    // 用户名
    'username'        => 'thinkphp',
    // 密码
    'password'        => '123456',
    // 端口
    'hostport'        => '',
    // 连接dsn
    'dsn'             => '',
    // 数据库连接参数
    'params'          => [],
    // 数据库编码默认采用utf8
    'charset'         => 'utf8',
    // 数据库表前缀
    'prefix'          => 'pre_',
    // 数据库调试模式
    'debug'           => true,
    // 数据库部署方式:0 集中式(单一服务器),1 分布式(主从服务器)
    'deploy'          => 0,
    // 数据库读写是否分离 主从式有效
    'rw_separate'     => false,
    // 读写分离后 主服务器数量
    'master_num'      => 1,
    // 指定从服务器序号
    'slave_no'        => '',
    // 自动读取主库数据
    'read_master'     => false,
    // 是否严格检查字段是否存在
    'fields_strict'   => true,
    // 数据集返回类型
    'resultset_type'  => 'array',
    // 自动写入时间戳字段
    'auto_timestamp'  => false,
    // 时间字段取出后的默认时间格式
    'datetime_format' => 'Y-m-d H:i:s',
    // 是否需要进行SQL性能分析
    'sql_explain'     => false,
];
```

> 读取配置文件

```
//先引入config 配置类
use think\Config;

//读取config 配置
Config::get('配置名称');

//或者你需要判断是否存在某个设置参数
Config::has('配置参数2');

//设置配置参数
Config::set('配置参数','配置值');
// 或者使用助手函数
config('配置参数','配置值');

//也可以批量设置，例如：
Config::set([
    '配置参数1'=>'配置值',
    '配置参数2'=>'配置值'
]);
// 或者使用助手函数
config([
    '配置参数1'=>'配置值',
    '配置参数2'=>'配置值'
]);
```
