---
title: 00.laravel
description: laravel
published: 1
date: 2023-04-22T18:36:27.446Z
tags: laravel
editor: markdown
dateCreated: 2023-04-22T18:36:24.368Z
---

<center>Laravel</center>



[toc]



## Laravel

> Laravel是一个强大的MVC PHP框架.  
>
> 渐进式(会跟你一起成长)的web开发框架。



### 1. 安装

> 使用composer管理依赖 [该教程](https://juejin.cn/post/7028732196072456205)

```shell
composer create-project laravel/laravel mylar

# 启动
php artisan serve 

# 配置将web服务器执行public index.php作为入口文件
# .nev 没有配置App_KEY 执行
php artisan key:generate
#维护和启动网站
php artisan down/up
```



### 2. 路由

```php
# routes/web.php   api.php是接口的路由
# web.php
Route::get('/hello/{name}',function($name){
    return "my name is:".$name;
});
# http://127.0.0.1:8000/hello/go
```

```php
#api
Route::get('/getid/{id?}',function($id){
    return 'this id is:'.$id;
});
# http://127.0.0.1:8000/api/getid/10
```



### 3. 控制器

> C

```shell
php artisan make:controller UserController
```

```php
# 添加控制器方法
class UserController extends Controller
{
    //
    public function index()
    {
        return "User info Page";
    }
}
# 添加路由 web.php
Route::get('/userifno','UserController@index');
Route::get('/userinfo',UserController::class,'index');
```





### 4. 自定义路由

```shell
# 添加路由文件 routes/admin.php
<?php

/**
 * 自定义admin路由
 * Author goer
 */

use Illuminate\Routing\Route;

Route::get('/baseadmin',function(){
    return "this is admin page";
});
```

```php
# app\Providers\RouteServiceProvider.php中注册路由
Route::middleware('web')
    ->namespace($this->namespace)
    ->group(base_path('routes/web.php'));

Route::prefix('admin') # 路由前缀
    ->middleware('admin') # 中间件组
    ->namespace($this->namespace)
    ->group(base_path('routes/admin.php'));
```



### 5. 数据表生成

> 自动生成数据表

```shell
#1.  配置你的数据库 .env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=

# 2. 命令行创建migrations
php artisan make:migration create_user_info

# 3. 编辑生成的文件 \database\migrations\2021_11_09_124752_create_blogs.php 语法跟sql很是接近
public function up()
{
    Schema::create('blog', function (Blueprint $table) {
        $table->id();
        $table->string('title')->default('')->comment('标题');
        $table->text('content')->comment('内容');
        $table->bigInteger('author_id')->index()->default(0)->comment('作者id');
        $table->bigInteger('category_id')->index()->default(0)->comment('类别idid');
        $table->string('remarks')->default('')->comment('备注');
        $table->tinyInteger('status')->default(0)->comment('审核状态 0未审核 1审核通过');
        $table->timestamps();
    });
}

# 4. 执行命令
php artisan migrate
```

```shell
# 此时出现一个错误 需要配置 文件 app\Providers\AppServiceProvider.php 具体原因目前我还没研究明白
    public function boot()
    {
        //use Illuminate\Support\Facades\Schema; 需要顶部引入
        Schema::defaultStringLength(191);
    }

```



### 6. 提取service层

> **我理解的Service是，对于同一操作。比如说是【发表博客】，不管前端有多少套，后端有多少套。这一个功能只能通过一个Service提供服务。保证了数据不管在哪一端操作都是相同的结果。**

```php
# 新建model
php artisan make:mode BlogModel

class Blog extends Model
{
    use HasFactory;
	// 使用的表明
    protected $table = 'blog';
}

```

```php
# 创建文件 app\Http\Service\BlogService.php
<?php
namespace App\Http\Service;
use App\Models\Blog;

class BlogService
{
    public function getList($if=[],$size=5)
    {
        // 每页显示五条数据
        $res = Blog::query()->where($if)->paginate($size);

        return $res;
    }

    //发表文章
    public function add($data){
        $res = Blog::query()->create($data);
        
        return $res;
    }

    //修改文章
    public function edit($id,$data){
        $blog = Blog::query()->find($id);
        if(is_null($blog)){return 0;}
        $res = Blog::query()->where('id',$id)->update($data);
        
        return $res;
    }
    
}
```

```php
# 路由
Route::get('/blog','BlogController@index');
```

```php
# 控制器
class BlogController extends Controller
{
    // server层方法
    public function index()
    {

        $blog = new BlogService();

        return $blog->getList(['status'=>1]);
    }
}

```





### 7 接受参数入库

```php
public function createTitle(Request $request)
{
    // 获取所有参数
    $input = $request->all();

    $blogServe = new BlogService();
    $res = $blogServe->add($input);

    if($res){
        echo "添加成功";
    }
}
```

> 提示没有在模型中指定能操作的数据`allow mass assignment on`

```php
// blog 模型
// 设置可以操作的字段
protected $fillable = [
    'title',
    'content',
    'author_id',
    'category_id',
    'remarks',
];
```

> 我去，添加成功了





### 8. jwt安装

```php	
# 安装
composer require tymon/jwt-auth
# 生成配置文件
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider" 
# 此时会生成一个jwt的配置文件 config\jwt.php

#生成key
php artisan jwt:secret 
```







### 9.其他

> laravel  开发的时候需要跨域

```shell
# 在入口文件 index.php
// 跨域请求
header("Access-Control-Allow-Origin:*");
header('Access-Control-Allow-Methods:POST,GET,OPTIONS,DELETE');
```

> 添加助手函数

```shell
# 新建 app/Helper/function.php  助手函数

# composer.json  添加    "files": ["app/Helper/function.php"]
"autoload": {
    "psr-4": {
        "App\\": "app/",
        "Database\\Factories\\": "database/factories/",
        "Database\\Seeders\\": "database/seeders/"
    },
    "files": ["app/Helper/function.php"]
},

# 更新
composer dumpautoload
```

