---
title: 31PostgreSql
description: 
published: 1
date: 2023-08-17T11:17:59.249Z
tags: 
editor: markdown
dateCreated: 2023-08-17T11:17:57.518Z
---

<center>PostgreSql</center>





[toc]







## PostgreSql

> 更加友好的数据库 以前有学过基本语法 [blog](https://github.com/vgoer/wiki/blob/master/%E6%95%B0%E6%8D%AE%E5%BA%93%E9%9B%86/01PostGreSQL.md)



### 1. 配置

> 打开 `config/database.php` 文件，将 PostgreSQL 连接信息添加到 `connections` 数组中

```php
'pgsql' => [
    'driver' => 'pgsql',
    'host' => env('DB_HOST', '127.0.0.1'),
    'port' => env('DB_PORT', '5432'),
    'database' => env('DB_DATABASE', 'your_database_name'),
    'username' => env('DB_USERNAME', 'your_username'),
    'password' => env('DB_PASSWORD', 'your_password'),
    'charset' => 'utf8',
    'prefix' => '',
    'schema' => 'public',
    'sslmode' => 'prefer',
],
```

> 配置默认连接：`config/database.php` 文件，将 `'default'` 连接设置为 `'pgsql'`：

```php
'default' => env('DB_CONNECTION', 'pgsql'),
```





### 2. 使用

* 创建迁移

```php
php artisan make:model Title -mc  # 创建 模型 控制器 迁移文件

 # 修改迁移文件
class CreateYourTableNameTable extends Migration
{
    public function up()
    {
        Schema::create('your_table_name', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('your_table_name');
    }
}
```

> 生成迁移

```php
php artisan migrate
```





### 3. 使用

> PostgreSQL 数据库的基本使用



```php
# 查询
// 使用模型查询所有记录
$records = YourModel::all();

// 使用查询构建器执行自定义查询
$records = DB::table('your_table_name')->where('column', 'value')->get();

// 执行原始查询
$records = DB::select('SELECT * FROM your_table_name WHERE column = ?', ['value']);
```

```php
# 插入
// 使用模型插入记录
$record = new YourModel;
$record->column1 = 'value1';
$record->column2 = 'value2';
$record->save();

// 使用查询构建器插入记录
DB::table('your_table_name')->insert([
    'column1' => 'value1',
    'column2' => 'value2',
]);
```

```php
# 更新
// 使用模型更新记录
$record = YourModel::find($id);
$record->column1 = 'new_value1';
$record->save();

// 使用查询构建器更新记录
DB::table('your_table_name')->where('column', 'value')->update([
    'column1' => 'new_value1',
]);
```

```php
# 删除
// 使用模型删除记录
$record = YourModel::find($id);
$record->delete();

// 使用查询构建器删除记录
DB::table('your_table_name')->where('column', 'value')->delete();
```





