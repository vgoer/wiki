---
title: 04dcat-admin
description: 
published: 1
date: 2023-08-30T10:37:14.756Z
tags: 
editor: markdown
dateCreated: 2023-08-30T10:37:13.098Z
---

<center>Dcat Admin</center>





[toc]





## Dcat Admin

> Dcat admin 让后台开发更简单
>
> [github](https://github.com/jqhph/dcat-admin)  [官网](http://www.dcatadmin.com/)   [doc](https://learnku.com/docs/dcat-admin/2.x)









### 1. 安装

> 首先需要安装 `laravel` 框架

```php
# 安装
composer create-project --prefer-dist laravel/laravel 项目名称
```

> 修改 `.env` 设置正确的数据库

```php
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=dcat-admin
DB_USERNAME=root
DB_PASSWORD=
```

> 安装 `dcat-admin`

```php
cd {项目名称}

composer require dcat/laravel-admin:"2.*" -vvv
```

> 如果上一步出现报错，则更改 `composer.json` 的参数 `minimum-stability` 的值为 `dev`。

> 然后运行下面的命令来发布资源：

```php
php artisan admin:publish
```

> 修改：add