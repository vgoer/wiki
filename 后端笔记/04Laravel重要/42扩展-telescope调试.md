---
title: 42扩展-telescope调试
description: 
published: 1
date: 2023-08-30T10:37:22.869Z
tags: 
editor: markdown
dateCreated: 2023-08-30T10:37:22.869Z
---

<center> Telescope 调试 </center>



[toc]

 



## Laravel Telescope 

> [Laravel Telescope](https://github.com/laravel/telescope) 是 Laravel 本地开发环境的绝佳伴侣。Telescope 可以洞察你的应用程序的请求、异常、日志条目、数据库查询、排队的作业、邮件、消息通知、缓存操作、定时计划任务、变量打印等。 [doc](https://learnku.com/docs/laravel/10.x/telescope/14917)







## 1. 安装

```php
composer require laravel/telescope
```

> 发布和生成数据库

```php
php artisan telescope:install

php artisan migrate
```

> 访问： `http://127.0.0.1:8000/telescope` 加上后缀： `telescope`
>
> 我去，牛皮。爱了。dug哪里逃







### 2. 仪表板授权

> 访问 `/telescope` 即可显示仪表盘。默认情况下，你只能在 `local` 环境中访问此仪表板。

```php
use App\Models\User;

/**
 * 注册 Telescope gate。
 *
 * 该 gate 确定谁可以在非本地环境中访问 Telescope
 */
protected function gate(): void
{
    Gate::define('viewTelescope', function (User $user) {
        return in_array($user->email, [
            'taylor@laravel.com',
        ]);
    });
}
```





### 3. 数据缓存

> 有了数据修改， `telescope_entries` 表可以非常快速地累积记录。 为了缓解这个问题，你应该使用 [调度](https://learnku.com/docs/laravel/10.x/scheduling) 每天运行 `telescope:prune` 命令：

```php
$schedule->command('telescope:prune')->daily();

# 以下命令将删除 48 小时前创建的所有记录：
$schedule->command('telescope:prune --hours=48')->daily();
```

> 显示： [sum](https://zhuanlan.zhihu.com/p/48896240)











