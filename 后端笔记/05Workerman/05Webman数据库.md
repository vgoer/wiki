<center>Webman数据库配置</center>







[toc]









### 数据库配置

> [webman/database](https://github.com/webman-php/database)是基于[illuminate/database](https://github.com/illuminate/database)开发的，并加入了连接池功能，支持协程和非协程环境，用法与laravel相同。





### 1. 基础使用

#### a. 安装

```shelll
composer require -W webman/database illuminate/pagination illuminate/events symfony/var-dumper
```

> 安装后需要restart重启(reload无效)

> **提示**
> 如果不需要分页、数据库事件、记录SQL，则只需要执行
> `composer require -W webman/database`

#### b. 配置

> `config/database.php`

```php
<?php
    
return [
    // 默认数据库
    'default' => 'mysql',

    // 各种数据库配置
    'connections' => [
        'mysql' => [
            'driver'      => 'mysql',
            'host'        => '127.0.0.1',
            'port'        => 3306,
            'database'    => 'test',
            'username'    => 'root',
            'password'    => '',
            'unix_socket' => '',
            'charset'     => 'utf8',
            'collation'   => 'utf8_unicode_ci',
            'prefix'      => '',
            'strict'      => true,
            'engine'      => null,
            'options' => [
                PDO::ATTR_EMULATE_PREPARES => false, // 当使用swoole或swow作为驱动时是必须的
            ],
            'pool' => [ // 连接池配置
                'max_connections' => 5, // 最大连接数
                'min_connections' => 1, // 最小连接数
                'wait_timeout' => 3,    // 从连接池获取连接等待的最大时间，超时后会抛出异常。仅在协程环境有效
                'idle_timeout' => 60,   // 连接池中连接最大空闲时间，超时后会关闭回收，直到连接数为min_connections
                'heartbeat_interval' => 50, // 连接池心跳检测时间，单位秒，建议小于60秒
            ],
        ],
    ],
];
```

> 使用

```php
<?php
namespace app\controller;

use support\Request;
use support\Db;

class UserController
{
    public function db(Request $request)
    {
        $default_uid = 29;
        $uid = $request->get('uid', $default_uid);
        $name = Db::table('users')->where('uid', $uid)->value('username');
        return response("hello . $name");
    }
}
```





### 2. 配置数据库

> 多个数据库

```php
return [
 // 默认数据库
 'default' => 'mysql',
 // 各种数据库配置
 'connections' => [

     'mysql' => [
         'driver'      => 'mysql',
         'host'        => '127.0.0.1',
         'port'        => 3306,
         'database'    => 'webman',
         'username'    => 'webman',
         'password'    => '',
         'unix_socket' => '',
         'charset'     => 'utf8',
         'collation'   => 'utf8_unicode_ci',
         'prefix'      => '',
         'strict'      => true,
         'engine'      => null,
         'pool' => [ // 连接池配置，仅支持swoole/swow驱动
            'max_connections' => 5, // 最大连接数
            'min_connections' => 1, // 最小连接数
            'wait_timeout' => 3,    // 从连接池获取连接等待的最大时间，超时后会抛出异常
            'idle_timeout' => 60,   // 连接池中连接最大空闲时间，超时后会关闭回收，直到连接数为min_connections
            'heartbeat_interval' => 50, // 连接池心跳检测时间，单位秒，建议小于60秒
        ],
     ],

     'sqlite' => [
         'driver'   => 'sqlite',
         'database' => '',
         'prefix'   => '',
         'pool' => [ // 连接池配置，仅支持swoole/swow驱动
            'max_connections' => 5, // 最大连接数
            'min_connections' => 1, // 最小连接数
            'wait_timeout' => 3,    // 从连接池获取连接等待的最大时间，超时后会抛出异常
            'idle_timeout' => 60,   // 连接池中连接最大空闲时间，超时后会关闭回收，直到连接数为min_connections
            'heartbeat_interval' => 50, // 连接池心跳检测时间，单位秒，建议小于60秒
        ],
     ],

     'pgsql' => [
         'driver'   => 'pgsql',
         'host'     => '127.0.0.1',
         'port'     => 5432,
         'database' => 'webman',
         'username' => 'webman',
         'password' => '',
         'charset'  => 'utf8',
         'prefix'   => '',
         'schema'   => 'public',
         'sslmode'  => 'prefer',
         'pool' => [ // 连接池配置，仅支持swoole/swow驱动
            'max_connections' => 5, // 最大连接数
            'min_connections' => 1, // 最小连接数
            'wait_timeout' => 3,    // 从连接池获取连接等待的最大时间，超时后会抛出异常
            'idle_timeout' => 60,   // 连接池中连接最大空闲时间，超时后会关闭回收，直到连接数为min_connections
            'heartbeat_interval' => 50, // 连接池心跳检测时间，单位秒，建议小于60秒
        ],
     ],

     'sqlsrv' => [
         'driver'   => 'sqlsrv',
         'host'     => 'localhost',
         'port'     => 1433,
         'database' => 'webman',
         'username' => 'webman',
         'password' => '',
         'charset'  => 'utf8',
         'prefix'   => '',
         'pool' => [ // 连接池配置，仅支持swoole/swow驱动
            'max_connections' => 5, // 最大连接数
            'min_connections' => 1, // 最小连接数
            'wait_timeout' => 3,    // 从连接池获取连接等待的最大时间，超时后会抛出异常
            'idle_timeout' => 60,   // 连接池中连接最大空闲时间，超时后会关闭回收，直到连接数为min_connections
            'heartbeat_interval' => 50, // 连接池心跳检测时间，单位秒，建议小于60秒
        ],
     ],
 ],
];
```

> 多个数据库

> 通过`Db::connection('配置名')`来选择使用哪个数据库，其中`配置名`为配置文件`config/database.php`中的对应配置的`key`。
>
> 例如如下数据库配置：

```php
 return [
     // 默认数据库
     'default' => 'mysql',
     // 各种数据库配置
     'connections' => [

         'mysql' => [
             'driver'      => 'mysql',
             'host'        =>   '127.0.0.1',
             'port'        => 3306,
             'database'    => 'webman',
             'username'    => 'webman',
             'password'    => '',
             'unix_socket' =>  '',
             'charset'     => 'utf8',
             'collation'   => 'utf8_unicode_ci',
             'prefix'      => '',
             'strict'      => true,
             'engine'      => null,
         ],

         'mysql2' => [
              'driver'      => 'mysql',
              'host'        => '127.0.0.1',
              'port'        => 3306,
              'database'    => 'webman2',
              'username'    => 'webman2',
              'password'    => '',
              'unix_socket' => '',
              'charset'     => 'utf8',
              'collation'   => 'utf8_unicode_ci',
              'prefix'      => '',
              'strict'      => true,
              'engine'      => null,
         ],
         'pgsql' => [
              'driver'   => 'pgsql',
              'host'     => '127.0.0.1',
              'port'     =>  5432,
              'database' => 'webman',
              'username' =>  'webman',
              'password' => '',
              'charset'  => 'utf8',
              'prefix'   => '',
              'schema'   => 'public',
              'sslmode'  => 'prefer',
          ],
 ];
```

```php
# 使用
// 使用默认数据库，等价于Db::connection('mysql')->table('users')->where('name', 'John')->first();
$users = Db::table('users')->where('name', 'John')->first();; 
// 使用mysql2
$users = Db::connection('mysql2')->table('users')->where('name', 'John')->first();
// 使用pgsql
$users = Db::connection('pgsql')->table('users')->where('name', 'John')->first();
```



### 3. 查询构造器

> 数据库查询

```php
# 查询所有行
$all = Db::table("admin")->get();

#  查询指定列 
$all = Db::table("admin")->select("id","username")->get();

# 查询指定列
$all = Db::table("admin")->where("id", $id)->select("id","username")->get();

# 获取第一行数据
$all = Db::table("admin")->where("id", $id)->first();
return response("name: " . $all->username);

# 获取一列
$username = Db::table("admin")->pluck("username");

# 获取单个值
$email = Db::table('users')->where('name', 'John')->value('email');

# 去重
$email = Db::table('user')->select('nickname')->distinct()->get();

# 分块结果
```

> 如果你需要处理成千上万条数据库记录，一次性读取这些数据会很耗时，并且容易导致内存超限，这时你可以考虑使用 chunkById 方法。该方法一次获取结果集的一小块，并将其传递给 闭包 函数进行处理。例如，我们可以将全部 users 表数据切割成一次处理 100 条记录的一小块：

```php
Db::table('users')->orderBy('id')->chunkById(100, function ($users) {
    foreach ($users as $user) {
        //
    }
});

# 聚合
$username = Db::table("admin")->count();

# 判断是否存在
return Db::table('orders')->where('finalized', 1)->exists();
return Db::table('orders')->where('finalized', 1)->doesntExist();
```

> 原生sql
>
> 有时候你可能需要在查询中使用原生表达式。你可以使用 `selectRaw()` 创建一个原生表达式：

```php
$orders = Db::table('orders')
                ->selectRaw('price * ? as price_with_tax', [1.0825])
                ->get();

# join语句
// join
$users = Db::table('users')
            ->join('contacts', 'users.id', '=', 'contacts.user_id')
            ->join('orders', 'users.id', '=', 'orders.user_id')
            ->select('users.*', 'contacts.phone', 'orders.price')
            ->get();

// leftJoin            
$users = Db::table('users')
            ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
            ->get();

// rightJoin
$users = Db::table('users')
            ->rightJoin('posts', 'users.id', '=', 'posts.user_id')
            ->get();

// crossJoin    
$users = Db::table('sizes')
            ->crossJoin('colors')
            ->get();

# union语句
$first = Db::table('users')
            ->whereNull('first_name');

$users = Db::table('users')
            ->whereNull('last_name')
            ->union($first)
            ->get();


# where语句
$users = Db::table('users')->where('votes', '=', 100)->get();

// 当运算符为 等号 时可省略，所以此句表达式与上一个作用相同
$users = Db::table('users')->where('votes', 100)->get();

$users = Db::table('users')
                ->where('votes', '>=', 100)
                ->get();

$users = Db::table('users')
                ->where('votes', '<>', 100)
                ->get();

$users = Db::table('users')
                ->where('name', 'like', 'T%')
                ->get();

# 多个条件
$users = Db::table('users')->where([
    ['status', '=', '1'],
    ['subscribed', '<>', '1'],
])->get();

# 多个条件
$users = Db::table('users')
                    ->where('votes', '>', 100)
                    ->orWhere('name', 'John')
                    ->get();
// SQL: select * from users where votes > 100 or (name = 'Abigail' and votes > 50)
$users = Db::table('users')
            ->where('votes', '>', 100)
            ->orWhere(function($query) {
                $query->where('name', 'Abigail')
                      ->where('votes', '>', 50);
            })
            ->get(); 

# orderBy
$users = Db::table('users')
                ->orderBy('name', 'desc')
                ->get();

# groupBy / having
$users = Db::table('users')
                ->groupBy('account_id')
                ->having('account_id', '>', 100)
                ->get();
// 你可以向 groupBy 方法传递多个参数
$users = Db::table('users')
                ->groupBy('first_name', 'status')
                ->having('account_id', '>', 100)
                ->get();

# offset / limit
$users = Db::table('users')
                ->offset(10)
                ->limit(5)
                ->get();
```

> 增加，删除，修改

```php
# 插入
Db::table('users')->insert(
    ['email' => 'john@example.com', 'votes' => 0]
);

Db::table('users')->insert([
    ['email' => 'taylor@example.com', 'votes' => 0],
    ['email' => 'dayle@example.com', 'votes' => 0]
]);


# 更新
$affected = Db::table('users')
              ->where('id', 1)
              ->update(['votes' => 1]);

# 有时您可能希望更新数据库中的现有记录，或者如果不存在匹配记录则创建它：
Db::table('users')
    ->updateOrInsert(
        ['email' => 'john@example.com', 'name' => 'John'],
        ['votes' => '2']
    );


# 删除
Db::table('users')->delete();

Db::table('users')->where('votes', '>', 100)->delete();

# 清空表
Db::table('users')->truncate();
```

> ==调试==你可以使用 dd 或者 dump 方法输出查询结果或者 SQL 语句。 

```php
# 安装
composer require symfony/var-dumper

Db::table('users')->where('votes', '>', 100)->dd();
Db::table('users')->where('votes', '>', 100)->dump();
```

> 打印在控制台。





### 3. 模型

> webman模型 基于 [Eloquent ORM](https://laravel.com/docs/7.x/eloquent) 。每个数据库表都有一个对应的「模型」用来与该表交互。你可以通过模型查询数据表中的数据，以及在数据表中插入新记录。

```php
<?php
namespace app\model;

use support\Model;

class User extends Model
{
    /**
     * 与模型关联的表名
     *
     * @var string
     */
    protected $table = 'user';

    /**
     * 重定义主键，默认是id
     *
     * @var string
     */
    protected $primaryKey = 'uid';

    /**
     * 指示是否自动维护时间戳
     *
     * @var bool
     */
    public $timestamps = false;
    
        /**
     * 模型的连接名称
     *
     * @var string
     */
    protected $connection = 'connection-name';
    
        /**
     * 模型的默认属性值。
     *
     * @var array
     */
    protected $attributes = [
        'delayed' => false,
    ];
    
    
}
```

> 模型关联和laravel一样的。
>
> [learnku](https://learnku.com/docs/laravel/11.x/eloquent-relationshipsmd/16703)



