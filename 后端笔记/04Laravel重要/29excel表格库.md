---
title: 29excel表格库
description: 
published: 1
date: 2023-08-17T11:17:52.958Z
tags: 
editor: markdown
dateCreated: 2023-08-17T11:17:51.408Z
---

<center>Execl库</center>





[toc]







### Execl库

> 用于导入和导出 Excel 文件的工具。[doc](https://docs.laravel-excel.com/3.1/getting-started/)







### 1. 安装和注册

```php
composer require maatwebsite/excel
    
# 配置服务提供者 
# 编辑 config/app.php 配置文件，将以下行添加到 providers 数组中：
Maatwebsite\Excel\ExcelServiceProvider::class,

# 配置门面别名
'Excel' => Maatwebsite\Excel\Facades\Excel::class,

# 发布配置文件
php artisan vendor:publish --provider="Maatwebsite\Excel\ExcelServiceProvider" --tag="config"
# 这将在 config 目录下创建一个名为 excel.php 的配置文件。
```





### 2. 使用

> 创建到处类，并指定模型

```php
php artisan make:export UsersExport --model=User
```

> 这将在 `app/Exports` 目录下创建一个名为 `UsersExport.php` 的导出类。您可以根据需要编辑此类。

```php
# 导出数据到execl文件中
use App\Exports\UsersExport;
use Maatwebsite\Excel\Facades\Excel;

public function export()
{
    return Excel::download(new UsersExport, 'users.xlsx');
}
```

> 导入 Excel 文件数据：

```php
use Maatwebsite\Excel\Facades\Excel;

public function import(Request $request)
{
    $file = $request->file('import_file');

    Excel::import(new UsersImport, $file);

    // 处理导入的数据
}
```

