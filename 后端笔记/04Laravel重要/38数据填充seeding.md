<center>数据填充Seeding</center>





[toc]





### 数据填充

> Laravel 使用 seed 类提供一种简便的方法填充你用于测试的数据。所有的 seed 类文件都被存储在 `database/seeds` 目录下。 [blog](https://www.jianshu.com/p/e05121a90f59)





### 1. 使用

```php
# 放在 database/seeds
# 修改代码

/**
     * Run the database seeds.
     *
     * @return void
     */
public function run()
{
    DB::table('users')->insert([
        'name' => $this->generateRandomString(10),
        'email' => $this->generateRandomString(10).'@gmail.com',
        'password' => bcrypt('secret'),
    ]);
}


public function generateRandomString($length = 16) {
    $characters = array_merge(range('a', 'z'), range('A', 'Z'), range('0', '9'));
    $max = count($characters) - 1;
    $string = '';
    for ($i = 0; $i < $length; $i++) {
        $string .= $characters[mt_rand(0, $max)];
    }
    return $string;
}
```

> 执行命令

```php
# 1. 生成迁移
php artisan migrate

# 2. 插入数据
php artisan db:seed --class=UsersTableSeeder
```





> 调用其他的Seeders

```php
/**
 * Run the database seeds.
 *
 * @return void
 */
 public function run()
 {
   $this->call(UsersTableSeeder::class);
   $this->call(PostsTableSeeder::class);
   $this->call(CommentsTableSeeder::class);
 }
```

