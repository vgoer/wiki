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







### 3. 多环境

>  从启动参数,或从.env文件指定配置文件
>
> [blog](https://www.workerman.net/a/1737)

```shell
php start.php start -e  APP_ENV=development
```

```php
<?php
namespace app\common;
class Env
{
    public static function init(): void
    {
        global $argv;
        $env = false;
        $base_path = base_path() . '/.env.';
        // 从环境变量获取 APP_ENV
        foreach ($argv as $key => $value) {
            if ($value == '-e' && isset($argv[$key + 1]) && str_contains($argv[$key + 1], '=')) {
                list($name, $value) = explode('=', $argv[$key + 1]);
                $envFilePath = $base_path . $value;
                if ($name == 'APP_ENV' && file_exists($envFilePath)) {
                    $env = self::parseEnvFile($envFilePath);
                    if ($env) {
                        self::initEnv($env, $value);
                        return;
                    }
                }
            }
        }
        // 如果没有环境变量，则从 .env.development 或 .env.production 中获取
        $file = 'development';
        if (!file_exists($base_path . $file)) $file = 'production';
        $env = self::parseEnvFile($base_path . $file);
        if (!$env) {
            error_log("Error: .env.development or .env.production file not found.");
            exit('Error LINE:' . __LINE__);
        }
        self::initEnv($env, $file);
    }

    private static function parseEnvFile($file): false|array
    {
        if (!file_exists($file)) return false;
        return parse_ini_file($file, true) ?? false;
    }

    /**
     * @param array $env
     * @param string $envFilePath
     * @return void
     */
    protected static function initEnv(array $env, string $envFilePath): void
    {
        self::printEnv($envFilePath);
        //初始化ENV 配置
        foreach ($env as $key => $val) {
            //过滤首字母#或者'//'开头的注释
            if (str_starts_with($key, '#') || str_starts_with($key, '//')) continue;
            if (is_array($val)) {
                foreach ($val as $k => $v) { //如果是二维数组 item = PHP_KEY_KEY
                    $item = $key . '_' . $k;
                    putenv("$item=$v");
                }
            } else {
                putenv("$key=$val");
            }
        }

    }

    /**
     * 多进程下只输出一次 env 名字
     * @param string $envFilePath .env 名字
     * @return void
     */
    private static function printEnv(string $envFilePath): void
    {
        $lockFilePath = runtime_path() . '/env_printed.lock';
        $timeout = 3;

        $lockFile = fopen($lockFilePath, 'a+');
        if ($lockFile === false) {
            error_log("Could not open lock file: " . $lockFilePath);
            return;
        }
        if (!flock($lockFile, LOCK_EX)) {
            error_log("Could not acquire lock on file: " . $lockFilePath);
            fclose($lockFile);
            return;
        }
        try {
            if (file_exists($lockFilePath)) {
                $modifiedTime = filemtime($lockFilePath);
                if (time() - $modifiedTime > $timeout) {
                    // 锁失效，输出语句并更新时间
                    echo "Load env file: " . $envFilePath . PHP_EOL;
                    touch($lockFilePath); // 更新修改时间为当前时间
                }
                // 如果锁未失效，不输出
            } else {
                // 文件不存在，输出语句并创建文件
                echo "Load env file: " . $envFilePath . PHP_EOL;
                touch($lockFilePath); // 创建文件并设置修改时间为当前时间
            }
        } finally {
            flock($lockFile, LOCK_UN);
            fclose($lockFile);
        }
    }
}
```

> 掰开 config/server.php   在return 之前加入一行

```php
\app\common\Env::init();
return [....];
```

> 使用 `getenv` 获取环境变量
> 或者封装一个助手函数

```php
function envs($key, $default = null): string|array|null
{
    $value = getenv($key, true);
    if ($value === false) return $default;
    return $value;
}

```

> 或者`start.php`

```php
#!/usr/bin/env php
<?php
chdir(__DIR__);
require_once __DIR__ . '/vendor/autoload.php';

# 环境变量添加
\app\Library\Env\Env::init();

support\App::run();
```

> 哇咯。牛。
