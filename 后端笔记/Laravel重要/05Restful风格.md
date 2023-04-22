---
title: 05.Restful风格
description: Restful风格
published: 1
date: 2023-04-22T18:41:38.037Z
tags: laravel
editor: markdown
dateCreated: 2023-04-22T18:41:38.037Z
---

<center>Restful</center>





[toc]





### Restful

> restful风格： 从资源的角度安排路由

```shell
实现:  资源 + 7种操作

资源: Blog(任意) 
操作： crud: create read update delete + list(聚合显示:多条查询) + create view(添加页面) + edit view (修改页面)

操作都有公用的url:
create,list:         blogs
delete,update,read:  blogs/:id
create view:         blogs/create
edit view:           blogs/:id/edit 

http methods:  GET POST  PUT  DELETE  // 获取资源， 提交资源，修改资源，删除资源

整合url
create:  POST   blogs
list:    GET    blogs
delete:   DELETE blogs/:id
update:   PUT    blogs/:id
read:     GET    blogs:id
create view: GET blogs/create
edit view:  GET  blogs/:id/edit
```



> 生成资源控制器

```shell
php artisan make:controller ProjectController -r 
```

```php
# 对应关系
index  =>  get   blogs 
create =>  get   blogs/create
store  =>  post  blogs
show   =>  get   blogs/:id
edit   =>   get  blogs/:id/edit
update =>   put   blogs/:id
destroy => delete blogs/:id
```

```shell
php artisan help make:controller 
php artisan make:controller ProjectController --api --force
```

> 遇到路由被包含了，需调整路由的规则

```shell
//  create 的方法会被 当做id传入到方法中，所以要调换位置
Route::get('/project/{id}');
Route::get('/project/create');
```





























