---
title: 35扩展-envoy远程任务
description: 
published: 1
date: 2023-08-17T11:18:11.402Z
tags: 
editor: markdown
dateCreated: 2023-08-17T11:18:09.908Z
---

<center>Envoy</center>





[toc]





### Laravel Envoy

> Laravel Envoy 优雅的 SSH 远程任务执行工具 [ssh ](https://learnku.com/laravel/t/24/laravel-envoy-elegant-ssh-remote-task-execution-tool)[envoy doc](https://laravel.com/docs/8.x/envoy)
>
> 在远程服务器上执行各种操作，如部署代码、运行远程命令等。





### 1. 安装

```php
# 安装
composer require laravel/envoy --dev
    
# 全局
composer global require "laravel/envoy"
    
# 更新
composer global update 
```





### 2. 创建

```php
envoy init vagrant@192.168.10.10
```

> 此文件夹下生成一个 `Envoy.blade.php` 的文件

```php
@servers(['web' => 'user@example.com'])

# 切换到项目目录、从 Git 拉取最新代码、安装生产环境所需的依赖、运行数据库迁移。
@task('deploy', ['on' => 'web'])
    cd /path/to/project
    git pull origin master
    composer install --no-dev
    php artisan migrate --force
@endtask
```

> 更多功能 看下文档





### 3. 执行

```php
# 其中 [task] 是你在 Envoy.blade.php 文件中定义的任务的名称。
envoy run [task]

envoy run deploy
```

> 这将连接到远程服务器，并在该服务器上执行 `deploy` 任务。































