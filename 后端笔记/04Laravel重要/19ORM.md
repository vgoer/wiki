---
title: 19ORM
description: 
published: 1
date: 2023-08-05T00:52:17.987Z
tags: 
editor: markdown
dateCreated: 2023-08-05T00:52:16.631Z
---

<center>ORM对象关系映射</center>



[toc]





## ORM 对象关系映射

> 通过 Eloquent 提供了简洁且强大的数据库关联功能。







### 1. 一对一

> 1. 一对一关联（One-to-One Relationship）： 假设我们有两个模型：User 和 Phone。一个用户只拥有一个电话号码。

> 定义模型和结构

```php
# user表
php artisan make:model User -mc
    
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string("name",100)->comment("姓名");
            $table->string("passwd",255)->comment("密码");
            $table->text("info")->comment("介绍");
            $table->timestamps();
        });
    }

# phone
php artisan make:model Phone -mc

    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('phones', function (Blueprint $table) {
            $table->id();
            # 关联字段类型和主键id类型保持一致
            $table->unsignedBigInteger("user_id")->comment("user id");
            $table->string('num')->comment("电话");
            $table->timestamps();
        });
    }

# 生成表
php artisan migrate
```

> 关联字段类型和主键id类型保持一致  一般是`unsignedBigInteger`



> 建立模型工程填充数据

```shell
php artisan make:factory UserFactory --model=User


# 写入数据
    protected $model = User::class;
    /**
     * Define the model's default state.
     *
     * @return array
     */
    public function definition()
    {
        return [
            'name' => "goer",
            'passwd' => bcrypt('password'),
            'info' => "goer123...",
        ];
    }
    
# 执行 tinker
php artisan tinker

User::factory()->create();  
```



```php
# User 模型中定义关联方法：

public function phone()
{
    return $this->hasOne(Phone::class);
}
```

```php
# Phone 模型中定义反向关联方法：
public function user()
{	
    # 属于
    return $this->belongsTo(User::class);
}
```

> 使用

```php
// 获取用户的电话号码
php artisan tinker	
    
# 获取一个用户 和查电话。
user = User::first();
$user->phone;

# 方向
$phone = Phone::first();
$phone->user;
```







### 2. 一对多

> 1. 一对多关联（One-to-Many Relationship）： 假设我们有两个模型：Post 和 User。一个用户有多篇文章。

```php
# 添加模型
php artisan make:model Post -mc
    
   	    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string("title",255)->comment("标题");
            $table->text("content")->comment("内容");
            $table->timestamps();
        });
    }

# 如果需要添加新的字段需要在建一个迁移，与第一个合并。
php artisan make:migration add_user_id_to_blogs_table
    
    public function up()
    {
        Schema::table('blogs', function (Blueprint $table) {
            //
            $table->unsignedBigInteger("user_id")->nullable()->comment("关联id");
        });
    }

    public function down()
    {
        Schema::table('blogs', function (Blueprint $table) {
            //删除id
            $table->dropColumn("user_id");
        });
    }

# 生成表
php artisan migrate
```

> ==import==迁移需要添加字段，可以使用这个方法

```php
# User 模型中定义关联方法：
public function bolgs()
{
    return $this->hasMany(Blog::class);
}


# 使用  我去，牛皮。
$user = User::first();
# 还可以对集合对象操作 count()  
# 加查询条件呀  ->where("") 常用的。 $user->bolgs->where("title","4");
$user->bolgs
```

```php
# Blog 模型中定义反向关联方法：
    public function user()
    {
        return $this->belongsTo(Blog::class);
    }

# 使用
$blog = Blog::first();
$blog->user
```

> ==记得==关联字段加索引：提升查询效率

```php
$table->unsignedBigInteger("user_id")->index()->comment("user id");
```



### 3. 多对多

> 1. 多对多关联（Many-to-Many Relationship）： 假设我们有两个模型：User 和 Groups表。一个用户可以属于多个角色，一个角色也可以被多个用户拥有。
>
>    这时候就需要中间表 group_user表。存放
>
>    [bilibili](https://www.bilibili.com/video/BV1st4y167je/)

```php
# 创建群和 中间表
php artisan make:model Group -mc
    
    Schema::create('groups', function (Blueprint $table) {
        $table->id();
        $table->string("name");
        $table->timestamps();
    });

 php artisan make:migration create_group_user_table
     
      Schema::create('group_user', function (Blueprint $table) {
            $table->id();
            # 添加关联
            $table->unsignedBigInteger("user_id")->index();
            $table->unsignedBigInteger("group_id")->index();
            $table->timestamps();

            # 唯一索引
            $table->unique(["user_id", "group_id"]);
        });

# 执行迁移
php artisan migrate
```

```php
#  Group 模型中定义关联方法：
public function users()
{
    // 关联多个用户                模型             关联中间表名
    return $this->belongsToMany(User::class, 'group_user');
}

# 使用
$user = User::find(1); 
# 获取组  还可以对集合对象操作
$user->groups 
```

```php
# User 模型中定义反向关联方法：
public function groups()
{
    return $this->belongsToMany(Group::class, 'group_user');
}

# 使用
$groupA = Group::find(1); 
$groupA->users 
```

> 牛皮。哈哈



### 4. 多态关联

> 1. 多态关联（Polymorphic Relationship）： 假设我们有两个模型：Post 和 Video。评论可以属于这两个模型中的任何一个。

```php
// Post 模型
public function comments()
{
    return $this->morphMany(Comment::class, 'commentable');
}

// Video 模型
public function comments()
{
    return $this->morphMany(Comment::class, 'commentable');
}
```

```php
# Comment 模型中定义反向关联方法：
public function commentable()
{
    return $this->morphTo();
}

```

> 使用

```php
1. 创建

创建 posts 表用于存储文章信息，包括 id、title 等字段。
创建 videos 表用于存储视频信息，包括 id、title 等字段。
创建 comments 表用于存储评论信息，包括 id、content 等字段，还需要创建 commentable_id 和 commentable_type 用于多态关联。
    
2. 模型
use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    public function comments()
    {
        return $this->morphMany(Comment::class, 'commentable');
    }
}

class Video extends Model
{
    public function comments()
    {
        return $this->morphMany(Comment::class, 'commentable');
    }
}

class Comment extends Model
{
    public function commentable()
    {
        return $this->morphTo();
    }
}

3. 使用
// 创建一篇文章及其评论
$post = Post::create(['title' => 'Hello, World!']);
$comment1 = $post->comments()->create(['content' => 'Great post!']);
$comment2 = $post->comments()->create(['content' => 'Nice article!']);

// 创建一个视频及其评论
$video = Video::create(['title' => 'Introduction to Laravel']);
$comment3 = $video->comments()->create(['content' => 'Informative video']);
$comment4 = $video->comments()->create(['content' => 'Well-explained tutorial']);

// 查询一篇文章的所有评论
$post = Post::find(1);
$comments = $post->comments;

// 查询一个视频的所有评论
$video = Video::find(1);
$comments = $video->comments;
```











### 4. 捕获sql语句

> `输出sql`语句
>
> `AppServiceProvider.php`

```php
public function boot()
{
    //
    Schema::defaultStringLength(200);

    \DB::listen(function ($query){
        // 日志里面
        // \Log::info(\Str::replaceArray("?", $query->bindings, $query->sql));

        echo (\Str::replaceArray("?", $query->bindings, $query->sql));
    });
}
```

> 测试： 

```shell
$groupA = Group::find(1);                                                                                            
[!] Aliasing 'Group' to 'App\Models\Group' for this Tinker session.
sql: select * from `groups` where `groups`.`id` = 1 limit 1⏎
```

```log
[2024-01-25 05:57:03] local.INFO: select * from `groups` where `groups`.`id` = 1 limit 1  
[2024-01-25 05:57:08] local.INFO: select `users`.*, `group_user`.`group_id` as `pivot_group_id`, `group_user`.`user_id` as `pivot_user_id` from `users` inner join `group_user` on `users`.`id` = `group_user`.`user_id` where `group_user`.`group_id` = 1  
```













