<center>swagger文档</center>

[toc]







## Swagger 文档

> [l5-swagger](https://github.com/DarkaOnLine/L5-Swagger) 是一个 Laravel 扩展，它可以帮助您在 Laravel 项目中生成 Swagger API 规范文档。 [doc](https://learnku.com/articles/60737)





### 1. 安装

```php
composer require "darkaonline/l5-swagger"
```





### 2. 注册者

> 打开`config/app.php`配置文件，并添加以下服务提供者：

```php
Jlapp\Swaggervel\SwaggervelServiceProvider::class,
```

```php
'Swaggervel' => Jlapp\Swaggervel\SwaggervelFacade::class,
```





### 3. 生成配置文件

```php
php artisan vendor:publish --provider "Jlapp\Swaggervel\SwaggervelServiceProvider"
```





### 4. 添加方法

```php
/**
     * @OA\OpenApi(
     *     @OA\Info(
     *         title="API 文档",
     *         version="1.0.0",
     *     ),
     *     @OA\PathItem(
     *         path="/api/users",
     *         @OA\Get(
     *             summary="获取用户列表",
     *             @OA\Response(response="200", description="成功"),
     *             @OA\Response(response="401", description="未授权"),
     *         ),
     *         @OA\Post(
     *             summary="创建用户",
     *             @OA\Response(response="201", description="成功"),
     *             @OA\Response(response="400", description="请求无效"),
     *         ),
     *     ),
     * )
     */
public function index()
{
    $learn = Learn::query()->where('status','=',1)->get();

    echo json_encode($learn);
}
```



### 5. 生成文件

```php
php artisan l5-swagger:generate
```

> 在浏览器中访问`/api/documentation`URL，您将看到自动生成的Swagger UI界面，显示项目中定义的API路由和相关信息。

