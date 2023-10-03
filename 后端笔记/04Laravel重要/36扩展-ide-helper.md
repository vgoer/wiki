---
title: 36扩展-ide-helper
description: 
published: 1
date: 2023-08-17T11:18:14.173Z
tags: 
editor: markdown
dateCreated: 2023-08-17T11:18:12.495Z
---

<center> Laravel-ide-helper</center>



[toc]





##  Laravel-ide-helper

>  扩展包能让你的 IDE (PHPStorm, Sublime) 实现自动完成、代码智能提示和代码跟踪等功能，大大提高你的开发效率。 [ide](https://learnku.com/laravel/t/2532/extended-recommendation-laravel-ide-helper-efficient-ide-smart-tips-plugin)





### 1. 安装

```php
# 
composer require barryvdh/laravel-ide-helper
    
# 安装完成后，在 config/app.php 添加以下内容到 providers 数组。
Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,


# 生成配置文件
php artisan vendor:publish --provider="Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider" --tag=config

#  
php artisan ide-helper:generate
```



