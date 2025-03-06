<center>SaiAdmin中后台</center>







[toc]









## SaiAdmin中后台

> saiadmin是一款基于vue3 + webman 的极速开发框架
>
> [saiadmin](https://www.saithink.top/)



### 1. 后端安装

```shell
# 安装webman
composer create-project workerman/webman saiAdmin

# 安装saiAdmin
composer require saithink/saiadmin
```

> 本系统使用[ ThinkPHP ](https://www.thinkphp.cn/)的ORM，数据库文件位于plugin/saiadmin/db的目录下
>
> 如果是全新安装的：只需要将saiadmin.sql文件导入数据库。
>
> 导入完成后需要在`config/thinkorm.php文件`中配置数据库信息

```php
return [
    'default' => 'mysql',
    'connections' => [
        'mysql' => [
            // 数据库类型
            'type' => 'mysql',
            // 服务器地址
            'hostname' => '127.0.0.1',
            // 数据库名
            'database' => 'saiadmin',
            // 数据库用户名
            'username' => 'root',
            // 数据库密码
            'password' => '123456',
            // 数据库连接端口
            'hostport' => '3306',
            // 数据库连接参数
            'params' => [ \PDO::ATTR_TIMEOUT => 3 ],
            // 数据库编码默认采用utf8
            'charset' => 'utf8',
            // 数据库表前缀
            'prefix' => 'eb_',
            // 断线重连
            'break_reconnect' => true,
            // 是否开启SQL监听
            'trigger_sql' => false,
        ],
    ],
];
```

> 启动项目

```shell
# win
php windows.php

# linux 
php start.php start

# 正式环境
php start.php start -d
```







### 2. 前端安装

```shell
# clone 代码
git clone https://gitee.com/saigroup/saiadmin-vue.git

# 安装依赖
pnpm install

# 启动
npm run dev

# 打包
npm run build
```

