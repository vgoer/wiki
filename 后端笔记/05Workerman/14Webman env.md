<center>Webman env</center>





[toc]









## Webman env

> `vlucas/phpdotenv`是一个环境变量加载组件，用来区分不同环境(如开发环境、测试环境等)的配置。









### 1. 安装

```shell
composer require vlucas/phpdotenv
```







### 2. 配置文件

> 新建： `.evn`

```php
APP_DEBUG = true

[APP]
DEFAULT_TIMEZONE = Asia/Shanghai

[LANG]
default_lang = zh-cn


[DATABASE]
TYPE = mysql
HOSTNAME = 127.0.0.1
DATABASE = xdlyxs
USERNAME = xdlyxs
PASSWORD = JX4WYGJRidtySRr4
HOSTPORT = 3306

```





### 2. 使用

> `config/database.php`

```php
return [
    // 默认数据库
    'default' => 'mysql',

    // 各种数据库配置
    'connections' => [
        'mysql' => [
            'driver'      => 'mysql',
            'host'        => getenv('DB_HOST'),
            'port'        => getenv('DB_PORT'),
            'database'    => getenv('DB_NAME'),
            'username'    => getenv('DB_USER'),
            'password'    => getenv('DB_PASSWORD'),
            'unix_socket' => '',
            'charset'     => 'utf8',
            'collation'   => 'utf8_unicode_ci',
            'prefix'      => '',
            'strict'      => true,
            'engine'      => null,
        ],
    ],
];
```

