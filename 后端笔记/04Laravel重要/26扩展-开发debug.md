<center>开发调试包</center>



[toc]







## Debug开发调试

> [laravel-debugbar](https://github.com/barryvdh/laravel-debugbar) 用于直观的显示调试及错误信息，提高开发效率。 [doc](http://phpdebugbar.com/docs/readme.html)





### 1. 安装

```php
composer require barryvdh/laravel-debugbar
```







### 2. 配置

> 安装完成后，修改 `config/app.php` 在 `providers` 数组内追加 Debugbar 的 Provider

```php
'providers' => [
    ...
    Barryvdh\Debugbar\ServiceProvider::class,
],
```

```php
'aliases' => [
    ...
    'Debugbar' => Barryvdh\Debugbar\Facade::class,
]
```



### 3. 生成配置文件

```php
php artisan vendor:publish --provider="Barryvdh\Debugbar\ServiceProvider"
```





### 4. 配置

> 打开 `config/debugbar.php`，将 `enabled` 的值设置为：

```php
'enabled' => env('APP_DEBUG', false),
```

> 修改完以后，Debugbar 分析器的启动状态将由 `.env` 文件中 `APP_DEBUG` 值决定。
>
> 打开页面，我去，报错了可以看到很多信息。牛皮





