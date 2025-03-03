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

> 开启事务，加锁

```php
    public function create_case_no()
    {
        // 设置时区
        date_default_timezone_set('Asia/Shanghai');

        // 获取日期
        $today = date('Y-m-d 00:00:00', time());
        $tomorrow = date('Y-m-d 00:00:00', strtotime('+1 day'));

        // 生成案件号
        \DB::beginTransaction();
        $count = CaseModel::where('created_at', '>', $today)->where('created_at', '<', $tomorrow)->count();
        if ($count == 0) {
            $no = sprintf('%04d', 1);
        } else {
            $last = CaseModel::lockForUpdate()->where('created_at', '>', $today)->where('created_at', '<', $tomorrow)->orderBy('id', 'desc')->first();
            $last_no = substr($last->case_no, -4);
            $no = sprintf('%04d', ((int) $last_no + 1));
        }
        $case_no = 'DZBH03' . date('Ymd', time()) . $no;
        try {
            $case = new CaseModel;
            $case->case_no = $case_no;
            $case->save();
            \DB::commit();
            return $case_no;
        } catch (\Exception $e) {
            \DB::rollBack();
            return False;
        }
    }
```





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





### 4. 分页

> Laravel的`illuminate/database`提供了方便的分页功能。

```php
# 安装
composer require illuminate/pagination
    
# 使用
    $per_page = 10;
    $users = Db::table('user')->paginate($per_page);
    return view('index/index', ['users' => $users]);
```

```php
# 查询用户
    public function userList(Request $request)
    {
        $input = $request->all();
        $page_size = empty($input['page_size']) ? 10 : $input['page_size'];
        $page_number = empty($input['page_number']) ? 1 : $input['page_number'];
    
        // 根据需求修改查询条件
        $conditions = [];
        if (isset($input['enterprise_name'])) {
            $conditions[] = ['enterprise_name', 'like', '%' . $input['enterprise_name'] . '%'];
        }
        
        if (isset($input['status'])) {
            $conditions[] = ['status', $input['status']];
        }
        // 查询数据
        $query = FrontUser::query();
        if (!empty($conditions)) {
            $query->where($conditions);
        }
        
        // 获取总条数
        $countQuery = clone $query;
        $total_count = $countQuery->count();
    
        // 获取分页数据
        $data = $query->forPage($page_number, $page_size)
            ->get(['id', 'step', 'mobile', 'enterprise_name', 'cert_no', 'area', 'status', 'address', 'license', 'permit', 'created_at']);
        
        $url_prefix = "https://baotongdzbh.oss-cn-guangzhou.aliyuncs.com/";
        
        foreach ($data as &$item) {
            $item['license'] = $item['license'] ? $url_prefix . $item['license'] : null;
            $item['permit']  = $item['permit'] ? $url_prefix . $item['permit'] : null;
        }
        
        // 返回结果
        return success($data, 200, '操作成功', $total_count);
    }
```





### 5. 数据库迁移

> Phinx 可以让开发者简洁的修改和维护数据库。 它避免了人为的手写 SQL 语句，它使用强大的 PHP API 去管理数据库迁移。
>
> [github](https://github.com/cakephp/phinx) [doc](https://tsy12321.gitbooks.io/phinx-doc)

```shell
# 安装
composer require robmorgan/phinx

# 初始化
vendor/bin/phinx init

# 新建目录文件
mkdir -p db/migrations
mkdir -p db/seeds
```

迁移脚本

```php
# 生成迁移
php vendor/bin/phinx create UserNewMigration
```

> Phinx 0.2.0 介绍了一个新功能-逆迁移（回滚）。现在这个功能成为了脚本的默认方法。在这个方法中，你只需要定义 `up` 的逻辑，Phinx 可以在回滚的时候自动识别出如何down。比如：

```php
<?php

declare(strict_types=1);

use Phinx\Migration\AbstractMigration;

final class UserNewMigration extends AbstractMigration
{
    /**
     * Change Method.
     *
     * Write your reversible migrations using this method.
     *
     * More information on writing migrations is available here:
     * https://book.cakephp.org/phinx/0/en/migrations.html#the-change-method
     *
     * Remember to call "create()" or "update()" and NOT "save()" when working
     * with the Table class.
     */
    public function change(): void
    {
        // 创建表
        $table = $this->table("users");
        $table->addColumn("user_id", "integer")
            ->addColumn("created","datetime")
            ->create();
    }

    /**
     * Migrate Up.
     */
    public function up()
    {

    }

    /**
     * Migrate Down.
     */
    public function down()
    {

    }
}

```

> 当执行这个迁移脚本，Phinx 将创建 `users` 表，并且在回滚的时候自动删除该表。注意，当 `change` 方法存在的时候，`up` 和 `down` 方法会被自动忽略。如果你想用这些方法建议你创建另外一个迁移脚本。

```shell
# 执行迁移
php vendor/bin/phinx migrate
```

> 对比

```php
# laravel
    public function up()
    {
        Schema::create('v7_admins', function (Blueprint $table) {
            $table->id()->comment('id');
            $table->string('account', 50)->nullable()->comment('用户名');
            $table->string('truename', 50)->nullable()->comment('真实姓名');
            $table->string('avatar', 255)->nullable()->comment('头像');
            $table->string('mobile',50)->nullable()->comment('电话');
            $table->string('password', 500)->nullable()->comment('密码');
            $table->string('salt', 255)->nullable()->comment('密码盐');
            $table->string('token', 500)->nullable()->comment('token认证');
            $table->string('roles', 50)->nullable()->comment('admin-超级管理员 servicer-客服 insurance_admin-管理员 insurance_servicer-审单人员');
            $table->integer('status')->default(0)->comment('0:禁用, 1正常');
            $table->timestamps();

            $table->comment('管理员表');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('v7_admins');
    }


# phinx
	public function change(): void
    {

        $table = $this->table('v1_admins', [
            'id' => true, 
            'signed' => false,
            'comment' => '管理员表'
        ]);
        
        $table->addColumn('account', 'string', [
                'limit' => 50,
                'null' => true,
                'comment' => '用户名'
            ])
            // 添加索引
            ->addColumn('email', 'string', [
                'limit' => 100,
                'null' => true,
                'comment' => '邮箱地址'
            ])
            ->addIndex(['email'], ['unique' => true], [
                'comment' => '邮件'
            ])
            ->addColumn('truename', 'string', [
                'limit' => 50,
                'null' => true,
                'comment' => '真实姓名'
            ])
            ->addColumn('avatar', 'string', [
                'limit' => 255,
                'null' => true,
                'comment' => '头像'
            ])
            ->addColumn('mobile', 'string', [
                'limit' => 50,
                'null' => true,
                'comment' => '电话'
            ])
            ->addColumn('password', 'string', [
                'limit' => 500,
                'null' => true,
                'comment' => '密码'
            ])
            ->addColumn('salt', 'string', [
                'limit' => 255,
                'null' => true,
                'comment' => '密码盐'
            ])
            ->addColumn('token', 'string', [
                'limit' => 500,
                'null' => true,
                'comment' => 'token认证'
            ])
            ->addColumn('roles', 'string', [
                'limit' => 50,
                'null' => true,
                'comment' => 'admin-超级管理员 servicer-客服 insurance_admin-管理员 insurance_servicer-审单人员'
            ])
            ->addColumn('status', 'integer', [
                'default' => 0,
                'comment' => '0:禁用, 1正常'
            ])
            // 时间戳  Laravel: $table->timestamps()
            ->addColumn('created_at', 'timestamp', [
                'null' => true
            ])
            ->addColumn('updated_at', 'timestamp', [
                'null' => true
            ])
            ->create();
    }
```

> 数据填充
>
> Phinx 0.5.0 支持数据库中使用seeding插入测试数据

```php
# 新建填充
php vendor/bin/phinx seed:create UserSeeder

    
public function run(): void
{
    $data = array(
        array(
            'body'    => 'foo',
            'created' => date('Y-m-d H:i:s'),
        ),
        array(
            'body'    => 'bar',
            'created' => date('Y-m-d H:i:s'),
        )
    );

    $posts = $this->table("v1_users");
    $posts->insert($data)
        ->save();
}


# 执行
php vendor/bin/phinx seed:run
```

> 可以使用 [Faker library](https://github.com/fzaninotto/Faker) 方法来注入测试数据。首先使用 Composer 安装 Faker：

```shell
composer require fzaninotto/faker
```

```php
    public function run(): void
    {
        $faker = Faker\Factory::create();
        $data = [];
        for ($i = 0; $i < 100; $i++) {
            $data[] = [
                'username'      => $faker->userName,
                'password'      => sha1($faker->password),
                'password_salt' => sha1('foo'),
                'email'         => $faker->email,
                'first_name'    => $faker->firstName,
                'last_name'     => $faker->lastName,
                'created'       => date('Y-m-d H:i:s'),
            ];
        }

        $this->insert('users', $data);
    }
```

