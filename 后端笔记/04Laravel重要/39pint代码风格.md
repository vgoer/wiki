---
title: 39pint代码风格
description: 
published: 1
date: 2023-08-21T11:46:35.031Z
tags: 
editor: markdown
dateCreated: 2023-08-21T11:46:32.527Z
---

<center>Pint代码风格</center>





[toc]







## Pint代码风格

> [Laravel Pint](https://github.com/laravel/pint) 是一款面向极简主义者的 PHP 代码风格固定工具。Pint 是建立在 PHP-CS-Fixer 基础上，使保持代码风格的整洁和一致变得简单。[blog](https://learnku.com/docs/laravel/10.x/pint/14912)







### 1. 安装

```php
composer require laravel/pint --dev
```





### 2. 使用

> 可以通过调用你项目中的 `vendor/bin` 目录下的 `pint` 二进制文件来指示 Pint 修复代码风格问题：

```php
./vendor/bin/pint
    
# 在特定得文件
./vendor/bin/pint app/Models
    
# Pint 将显示它所更新的所有文件的详细列表。
./vendor/bin/pint -v
 
# 如果你只想 Pint 检查代码中风格是否有错误，而不实际更改文件
./vendor/bin/pint --test
```

> 我去，牛皮。代码格式都给你搞好了









