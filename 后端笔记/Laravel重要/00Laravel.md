---
title: 00.laravel
description: laravel
published: 1
date: 2023-07-22T00:59:32.815Z
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

```php
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

```php
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

```php
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
```

```php
<?php

if (!function_exists('success')) {
    /**
     * 成功返回json内容
     * @param array|string $data
     * @param int $code
     * @param string $msg
     * @return Response
     */
    function success(array|string $data = [], int $code = 200, string $msg = '操作成功'): Response
    {
        // 类型分流处理
        if (is_string($data)) {
            $parsed = parseJsonOrReturnString($data);
            // 当输入是字符串且解析出数组时升级为数据
            if (is_array($parsed)) {
                $data = $parsed;
            }
            // 当输入是字符串且解析出字符串时作为消息
            if (is_string($parsed)) {
                $msg = $parsed;
            }
        }
        return json(['code' => $code, 'message' => $msg, 'data' => $data]);
    }
}

if (!function_exists('fail')) {
    /**
     * 失败返回json内容
     * @param string|array $msg
     * @param int $code
     * @return Response
     */
    function fail(string|array $msg = 'fail', int $code = CommonEnum::RETURN_CODE_FAIL): Response
    {
        return json(['code' => $code, 'message' => $msg]);
    }
}


function parseJsonOrReturnString(string $str): mixed
{
    try {
        return json_decode($str, true, 512, JSON_THROW_ON_ERROR);
    } catch (JsonException $e) {
        return $str;
    }
}


if (!function_exists('convertAmountToCn')) {
    /**
     * 将数值金额转换为中文大写金额
     * @param float $amount 金额(支持到分)
     * @param bool $isRound 是否对小数进行四舍五入
     * @return string 中文大写金额
     */
    function convertAmountToCn($amount, $isRound = true) {
        // 判断输出的金额是否为数字或数字字符串
        if (!is_numeric($amount)) {
            return "要转换的金额只能为数字!";
        }

        // 金额为 0,则直接输出"零元整"
        if ($amount == 0) {
            return "人民币零元整";
        }

        // 金额不能为负数
        if ($amount < 0) {
            return "要转换的金额不能为负数!";
        }

        // 金额不能超过万亿,即 12 位
        if (strlen($amount) > 12) {
            return "要转换的金额不能为万亿及更高金额!";
        }

        // 预定义中文转换的数组
        $digital = array('零', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖');
        // 预定义单位转换的数组
        $position = array('仟', '佰', '拾', '亿', '仟', '佰', '拾', '万', '仟', '佰', '拾', '元');

        // 将金额的数值字符串拆分成数组
        $amountArr = explode('.', $amount);

        // 将整数位的数值字符串拆分成数组
        $integerArr = str_split($amountArr[0], 1);

        // 将整数部分替换成大写汉字
        $result = '';
        $integerArrLength = count($integerArr);     // 整数位数组的长度
        $positionLength = count($position);         // 单位数组的长度
        for ($i = 0; $i < $integerArrLength; $i++) {
            // 如果数值不为 0,则正常转换
            if ($integerArr[$i] != 0) {
                $result = $result . $digital[$integerArr[$i]] . $position[$positionLength - $integerArrLength + $i];
            } else {
                // 如果数值为 0, 且单位是亿,万,元这三个的时候,则直接显示单位
                if (($positionLength - $integerArrLength + $i + 1) % 4 == 0) {
                    $result = $result . $position[$positionLength - $integerArrLength + $i];
                }
            }
        }

        // 如果小数位也要转换
        if (count($amountArr) > 1) {
            // 将小数位的数值字符串拆分成数组
            $decimalArr = str_split($amountArr[1], 1);
            // 将角替换成大写汉字. 如果为 0,则不替换
            if ($decimalArr[0] != 0) {
                $result = $result . $digital[$decimalArr[0]] . '角';
            }
            // 将分替换成大写汉字. 如果为 0,则不替换
            if (count($decimalArr) > 1 && $decimalArr[1] != 0) {
                $result = $result . $digital[$decimalArr[1]] . '分';
            }
        } else {
            $result = $result . '整';
        }

        return $result;
    }
}


```

