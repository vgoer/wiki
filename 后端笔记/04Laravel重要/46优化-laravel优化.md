---
title: 46优化-laravel优化
description: 
published: 1
date: 2023-09-08T03:52:25.836Z
tags: 
editor: markdown
dateCreated: 2023-09-08T03:52:22.441Z
---

<center>laravel优化</center>



[toc]





### Laravel优化

> Laravel应用性能优化项







### 0. 关闭debug

> 打开`.env` 文件，把 `debug` 设置为 `false`

```php
APP_ENV=local
APP_DEBUG=false
```



### 1. 路由缓存

> 每次服务器执行请求时，都会注册所有的路由，这会花费一些时间。但是，你可以选择缓存路由列表来跳过这个步骤。

```php
php artisan route:cache
```

> 但是，如果你添加或修改了任意一个路由信息，请不要忘记清除之前的缓存以及重新执行缓存命令。

```shell
php artisan route:clear

# 然后，再次执行
php artisan route:cache
```

> 注意，这只对控制器类路由有效。







### 2. 缓存配置

> 就如路由一样，你同样可以在应用中缓存配置文件。

```php
php artisan config:cache
```

> 编辑后

```php
php artisan config:clear

# 然后，再来一次...
php artisan config:cache
```







### 3. 优化Composer自动加载

> 通常，Composer 生成自动加载文件非常快。但是，在生产环境中，如果设置了 PSR-4 和 PSR-0 自动加载规则，这可能会变慢。
>
> 您可以通过将下面命令添加到部署脚本来优化自动加载器文件创建过程。

```php
composer dump-autoload  -o
```





### 4. 部署项目

> laravel 很方便，但脚手架工程就 45M 左右，使用 `composer install --no-dev` 也要 24M 左右

```php
# 只安装项目的生产环境依赖包
composer install --no-dev
```

> [blog](https://learnku.com/laravel/t/18863)  待续...









