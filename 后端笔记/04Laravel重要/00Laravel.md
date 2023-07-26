---
title: 00.laravel
description: laravel
published: 1
date: 2023-05-29T04:15:37.952Z
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

> php好用的函数代码

```php
<?php

/**
 * @author goer 
 * @version 1.0.0 2023/07/21
 * 
 */

/**
 * 成功打印的函数
 *
 * @param array $data 数据
 * @param integer $code 状态码
 * @param string $msg   返回提示
 * @param integer $count 获取次数
 * @return json 返回json
 */
function success($data = [], $code = 200, $msg ='', $count = 0)
{
    return json_encode([
        'code'   =>   $code,
        'msg'    =>   $msg == '' ? '操作成功' : $msg,
        'count'  =>   $count == 0 ? count($data) : $count,
        'data'   =>   $data
    ]);
}

/**
 * 返回错误信息
 *
 * @param integer $code 错误码
 * @return void|json $res_data 返回json
 */
function error($code = null)
{
    $error_code = [
        // 系统
        50000   =>  '系统繁忙',

        // 登录返回
        50100   =>   '账号不存在',

        // 等等
    ];

    if(empty($code)) return false;

    $res_json = [
        'code'  => $code,
        'msg'   => $error_code[$code]
    ];

    return json_encode($res_json);
}



// 数字转财务货币
function rmb($money = 0, $int_unit = '元', $is_round = true, $is_extra_zero = false)
{
    // 将数字切分成两段
    $parts = explode( '.', $money, 2);
    $int = isset($parts[0]) ? strval($parts[0]) : '0';
    $dec = isset($parts[1]) ? strval($parts[1]) : '';
    // 如果小数点后多于2位，不四舍五入就直接截，否则就处理
    $dec_len = strlen ( $dec );
    if (isset ( $parts [1] ) && $dec_len > 2) {
        $dec = $is_round ? substr(strrchr(strval(round(floatval("0." . $dec), 2)), '.'), 1) : substr ($parts [1], 0, 2);
    }
    // 当number为0.001时，小数点后的金额为0元
    if (empty($int) && empty($dec)) {
        return '零';
    }
    // 定义
    $chs = ['0', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖'];
    $uni = ['', '拾', '佰', '仟'];
    $dec_uni = ['角', '分'];
    $exp = ['', '万'];
    $res = '';
    // 整数部分从右向左找
    for($i = strlen($int) - 1, $k = 0; $i >= 0; $k ++) {
        $str = '';
        // 按照中文读写习惯，每4个字为一段进行转化，i一直在减
        for($j = 0; $j < 4 && $i >= 0; $j ++, $i --) {
            $u = $int[$i] > 0 ? $uni[$j] : ''; // 非0的数字后面添加单位
            $str = $chs[$int[$i]] . $u . $str;
        }
        $str = rtrim($str, '0'); // 去掉末尾的0
        $str = preg_replace("/0+/", "零", $str); // 替换多个连续的0
        if (! isset ($exp[$k])) {
            $exp[$k] = $exp[$k - 2] . '亿'; // 构建单位
        }
        $u2 = $str != '' ? $exp[$k] : '';
        $res = $str . $u2 . $res;
    }
    // 如果小数部分处理完之后是00，需要处理下
    $dec = rtrim($dec, '0');
    // 小数部分从左向右找
    if (! empty($dec)) {
        $res .= $int_unit;
        // 是否要在整数部分以0结尾的数字后附加0，有的系统有这要求
        if ($is_extra_zero) {
            if (substr($int, - 1) === '0') {
                $res .= '零';
            }
        }
        for($i = 0, $cnt = strlen($dec); $i < $cnt; $i ++) {                 
            $u = $dec[$i] > 0 ? $dec_uni[$i] : ''; // 非0的数字后面添加单位
            $res .= $chs[$dec[$i]] . $u;
            if ($cnt == 1) $res .= '整';
        }
        $res = rtrim($res, '0'); // 去掉末尾的0
        $res = preg_replace("/0+/", "零", $res); // 替换多个连续的0
    } else {
        $res .= $int_unit . '整';
    }
    return $res;
}
```





### 10. 验证码

> larvael 验证码

```shell
composer require mews/captcha
```

```php
// php 代码
return ['src' => captcha_src('custom') ];
```

