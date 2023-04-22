---
title: 07.Model模型
description: Model模型
published: 1
date: 2023-04-22T18:21:47.112Z
tags: thinkphp
editor: markdown
dateCreated: 2023-04-22T18:21:44.351Z
---

<center>模型</center>

[toc]

## model

> 在app目录下 的common 公共目录下   新建模型和控制器

```php
// (必须登录才能进入页面) 的后台  
新建base控制器继承tp控制器，admin模块下的控制器继承base控制器

//模型 公共目录下的admin模型
<?php 

namespace app\common\model\Admin;

use think\Model;

class Admin extends Model
{   
    //保护属性的  表名
    protected $name = 'admin';
}
```

> 模型初始化

```php
class Admin extends base
{
    public function __construct()
    {
        parent::__construct();
        $this->AdminModel = model('common/admin/Admin');
    }
}
```









