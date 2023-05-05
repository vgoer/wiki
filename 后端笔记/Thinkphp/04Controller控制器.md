---
title: 04.Controller控制器
description: Controller控制器
published: 1
date: 2023-05-05T11:53:47.785Z
tags: thinkphp
editor: markdown
dateCreated: 2023-04-22T18:18:40.339Z
---

<center>控制器</center>

[toc]

## controller

> 可以无需继承任何的基础类，也可以继承官方封装的`\think\Controller`类或者其他的控制器类。



#### 控制器定义

控制器的实际位置：application\index\controller\Index.php

```php
namespace app\index\controller;

class Index 
{
    public function index()
    {
        return 'index';
    }
}
```

> 命名空间默认以`app`为根命名空间。 `app\模块\controller`



控制器的根命名空间可以设置，例如我们在应用配置文件中修改：

```php
// 修改应用类库命名空间
'app_namespace' => 'application',
```



> 继承了think\Controller类的话，可以直接调用think\View`及`think\Request类的方法

```php
namespace app\index\controller;

use think\Controller;

class Index extends Controller
{
    public function index()
    {
        // 获取包含域名的完整URL地址
        $this->assign('domain',$this->request->url(true));
        return $this->fetch('index');
    }
}
```



#### 控制器初始化

> 制器类继承了\think\Controller类的话，可以`定义控制器初始化方法_initialize`，在该控制器的方法调用之前首先执行。

```
public function _initialize()
{
	//控制器的方法调用之前首先
	echo '最开始执行我';
}
```



#### 页面跳转

> 经常会遇到一些带有提示信息的跳转页面，例如操作成功或者操作错误页面，并且自动跳转到另外一个目标页面。系统的`\think\Controller`类内置了两个跳转方法`success`和`error`，用于页面跳转提示。

```php
public function index()
{	
    if($res){
        $this->success('添加成功','跳转地址');
	}else{
        $this->error('添加失败');
    }
}
```

`success`和`error`方法都可以对应的模板

```php
//默认错误跳转对应的模板文件
'dispatch_error_tmpl' => APP_PATH . 'tpl/dispatch_jump.tpl',
//默认成功跳转对应的模板文件
'dispatch_success_tmpl' => APP_PATH . 'tpl/dispatch_jump.tpl',
```



#### 重定向

`\think\Controller`类的`redirect`方法可以实现页面的重定向功能。

```php
//重定向到News模块的Category操作
$this->redirect('News/category', ['cate_id' => 2]);
```

上面的用法是跳转到News模块的category操作，重定向后会改变当前的URL地址。

或者直接重定向到一个指定的外部URL地址，例如：

```php
//重定向到指定的URL地址 并且使用302
$this->redirect('http://thinkphp.cn/blog/2',302);
```

可以在重定向的时候通过session闪存数据传值，例如

```php
$this->redirect('News/category', ['cate_id' => 2], 302, ['data' => 'hello']);
```

使用redirect助手函数还可以实现更多的功能，例如可以记住当前的URL后跳转

```php
redirect('News/category')->remember();
```



### 空操作

空操作是指系统在找不到指定的操作方法的时候，会定位到空操作（`_empty`）方法来执行，利用这个机制，我们可以实现错误页面和一些`URL`的优化。

例如，下面我们用空操作功能来实现一个城市切换的功能。 我们只需要给City控制器类定义一个_empty （空操作）方法：

```php
<?php
namespace app\index\controller;

class City 
{
    public function _empty($name)
    {
        //把所有城市的操作解析到city方法
        return $this->showCity($name);
    }

    //注意 showCity方法 本身是 protected 方法
    protected function showCity($name)
    {
        //和$name这个城市相关的处理
         return '当前城市' . $name;
    }
}
```

接下来，我们就可以在浏览器里面输入

```php
http://serverName/index/city/beijing/
http://serverName/index/city/shanghai/
http://serverName/index/city/shenzhen/
```

由于City并没有定义`beijing`、`shanghai`或者`shenzhen`操作方法，因此系统会定位到空操作方法 `_empty`中去解析，`_empty`方法的参数就是当前URL里面的操作名，因此会看到依次输出的结果是：

```php
当前城市:beijing
当前城市:shanghai
当前城市:shenzhen
```











