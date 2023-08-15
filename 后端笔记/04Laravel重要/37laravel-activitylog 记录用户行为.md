<center>laravel-activitylog 记录用户行为</center>



[toc]







### Laravel-activitylog 记录用户行为扩展包

> 在实际项目中，我们可能需要记录用户某些的行为，如登录、退出、发布文章等，使用 [spatie/laravel-activitylog](https://github.com/spatie/laravel-activitylog) 扩展包可以非常方便快捷的完成此逻辑。[blog](https://learnku.com/laravel/t/2813/extended-recommendation-laravel-activitylog-record-user-behavior-expansion-pack)







### 1. 安装

1. 使用 composer 安装：

```php
composer require spatie/laravel-activitylog
```

2. 添加注册者

>  修改 `config/app` 文件，在 `providers` 数组内追加如下内容：

```php
'providers' => [
    ...
    Spatie\Activitylog\ActivitylogServiceProvider::class,
],
```

3. 生成数据表和配置文件

```php
php artisan vendor:publish --provider="Spatie\Activitylog\ActivitylogServiceProvider" --tag="migrations"
php artisan migrate 

php artisan vendor:publish --provider="Spatie\Activitylog\ActivitylogServiceProvider" --tag="config"
```





### 2. 基础用法

1. 记录当前登录用户行为

```php
activity()->log('登录成功');
activity()->log('发帖成功');
```

2. 某个用的行为

```php
$user = User::find(31);
activity()->causedBy($user)->log('登录成功');
activity()->causedBy($user)->log('发帖成功');
```

