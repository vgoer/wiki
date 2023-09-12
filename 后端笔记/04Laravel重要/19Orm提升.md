<center>Orm提升</center>





[toc]





## Orm提升

> orm进阶。







### 1. 创建项目

> 新建开始

```php
# 1. 创建项目
composer create-project laravel/laravel lar_app
    
# 2. 新建数据库修改 .env文件
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=learn_key
DB_USERNAME=learn_key
DB_PASSWORD=learn_key

    
# 3. 创建迁移和模型，控制器文件
php artisan make:model Blog -mc
    
# 4.迁移添加数据库字段
public function up()
{
    Schema::create('blogs', function (Blueprint $table) {
        $table->id();
        $table->integer('author_id')->comment('用户id');
        $table->string('title')->comment('标题');
        $table->json('content')->comment('内容');
        $table->timestamps();
    });
}


# 4. 生成结构
php artisan migrate
    
# 5.填充数据
```

> 准备阶段完毕。



### 2. 路由模型绑定

> 资源控制器有好几个都是，需要先查出数据，在修改数据，有共性。

```php
# 控制器
// 路由
Route::get('blogs/create',[App\Http\Controllers\BlogController::class,'create']);
Route::get('blogs/{id}',[App\Http\Controllers\BlogController::class,'show']);
Route::get('blogs/{id}/edit',[App\Http\Controllers\BlogController::class,'edit']);
Route::get('blogs',[APP\Http\Controllers\BlogController::class,'store']);

// 控制器
// 展示
public function show($id)
{
    $blog = Blog::query()->findOrFail($id);
    return $blog;
}
```

> 优化

```php
// 路由修改
Route::get('blogs/{blog}',[App\Http\Controllers\BlogController::class,'show']);

// 我去，自己去找了我们数据库里面的数据，不存在的返回404
public function show(Blog $blog)
{
    // $blog = Blog::query()->findOrFail($blog);
    return $blog;
}


```

> 使用id访问不太安全，一般使用别名或者has

```php
// 模型中
// title修改数据
public function getRouteKeyName()
{
    return 'title';
}
```

> 就可以用title来访问数据了。



### 3. 模型黑魔法

> 添加模型属性

```php
# 添加路由规则
// 这样就可以通过route获取到url了  route('blogs.show',$blog)
Route::get('blogs/{blog}',[App\Http\Controllers\BlogController::class,'show'])->name('blogs.show');

// 黑魔法，把连接变为属性 在模型中写
// 属性写到 get  Attribute中，添加属性
public function getPathAttribute()
{
    return route('blogs.show',$this);
}

$blog->path; 获取到path属性。
```

















