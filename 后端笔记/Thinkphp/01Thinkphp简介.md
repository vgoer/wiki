---
title: 01.Thinkphp简介
description: Thinkphp简介
published: 1
date: 2023-06-09T10:16:12.397Z
tags: thinkphp
editor: markdown
dateCreated: 2023-04-22T18:15:32.782Z
---

<center>tp</center>

[toc]



## thinkphp

> thinkphp 是免费开源的、轻量级的、快速开发且敏捷的php框架



1. 安装

```
// thinkphp5* 环境要求
PHP >= 5.4.0
PDO PHP Extension
MBstring PHP Extension
CURL PHP Extension
```



* 官网

获取`ThinkPHP`的方式很多，

官方网站（[http://thinkphp.cn](http://thinkphp.cn/)）提供了[稳定版本](http://thinkphp.cn/down/framework.html)或者带扩展完整版本的下载。



* Composer

win: [Composer安装](http://www.kancloud.cn/thinkphp/composer/35669) 

linux\mac

```
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```

镜像源

```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
									//版本    //项目名称
composer create-project topthink/think=5.0.* tp5  --prefer-dist  //下载
```



* git

[gitee](https://gitee.com/liu21st)



把源码放入到web容器内,根目录放到`tp/public`

```
http://localhost/tp5/public/
```

>如果是mac或者linux环境，请确保`runtime`目录有可写权限 

> 升级

```
composer updata tothink/framework
```



## 模块设计

>  目录结构如下：

```
project  应用部署目录
├─application           应用目录（可设置）
│  ├─common             公共模块目录（可更改）
│  ├─index              模块目录(可更改)
│  │  ├─config.php      模块配置文件
│  │  ├─common.php      模块函数文件
│  │  ├─controller      控制器目录
│  │  ├─model           模型目录
│  │  ├─view            视图目录
│  │  └─ ...            更多类库目录
│  ├─command.php        命令行工具配置文件
│  ├─common.php         应用公共（函数）文件
│  ├─config.php         应用（公共）配置文件
│  ├─database.php       数据库配置文件
│  ├─tags.php           应用行为扩展定义文件
│  └─route.php          路由配置文件
├─extend                扩展类库目录（可定义）
├─public                WEB 部署目录（对外访问目录）
│  ├─static             静态资源存放目录(css,js,image)
│  ├─index.php          应用入口文件
│  ├─router.php         快速测试文件
│  └─.htaccess          用于 apache 的重写
├─runtime               应用的运行时目录（可写，可设置）
├─vendor                第三方类库目录（Composer）
├─thinkphp              框架系统目录
│  ├─lang               语言包目录
│  ├─library            框架核心类库目录
│  │  ├─think           Think 类库包目录
│  │  └─traits          系统 Traits 目录
│  ├─tpl                系统模板目录
│  ├─.htaccess          用于 apache 的重写
│  ├─.travis.yml        CI 定义文件
│  ├─base.php           基础定义文件
│  ├─composer.json      composer 定义文件
│  ├─console.php        控制台入口文件
│  ├─convention.php     惯例配置文件
│  ├─helper.php         助手函数文件（可选）
│  ├─LICENSE.txt        授权说明文件
│  ├─phpunit.xml        单元测试配置文件
│  ├─README.md          README 文件
│  └─start.php          框架引导文件
├─build.php             自动生成定义文件（参考）
├─composer.json         composer 定义文件
├─LICENSE.txt           授权说明文件
├─README.md             README 文件
├─think                 命令行入口文件
```



> 入口文件

```
ThinkPHP5.0版本的默认自带的入口文件位于public/index.php
实际部署的时候public目录为你的应用对外访问目录

浏览器访问路径为：http://localhost/tp5/public/
```



> 静态资源访问

```
网站的资源文件一般放入public目录的子目录下面
public
├─index.php     应用入口文件
├─static        静态资源目录   
│  ├─css          样式目录
│  ├─js         脚本目录
│  └─img          图像目录

//注：千万不要在public目录之外的任何位置放置资源文件，包括application目录。
```

> ==隐藏入口文件==

```
// 没有隐藏
http:www.tp.com/index.php/admin/index
// 省略index.php 入口文件

应用入口文件同级目录添加.htaccess文件
```

```
//伪静态文件配置
//这样就可以省略入口文件index.php
//在入口文件同级目录下的.htaccess

<IfModule mod_rewrite.c>
    Options +FollowSymlinks -Multiviews
    RewriteEngine on

    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.*)$ index.php?/$1 [QSA,PT,L]
</IfModule>
```





> ==静态常量==：
>
> 模板中可以使用`__STATIC__` 来输出项目目录但是在路径中缺少public目录

```php
// 配置config文件
// 视图输出字符串内容替换
'view_replace_str'       => [
	'adminJs' => '/static/admin/js',
	'adminImage' => '/static/admin/image',

];

// 加载资源
<script src="__adminJS__/style.js"></script>
<img src="__adminIMG__/user_logo.jpg" />
```



> ==调试模式==：
>
> 默认关闭调试模式，需要自己开启，(开发下打开调试模式，上线关闭)

```
// 关闭调试模式
'app_debug' =>  false,

//为了安全考虑，避免泄露你的服务器WEB目录信息等资料，一定记得正式部署的时候关闭调试模式
```



> url 访问： URL是不区分大小写的

```
//1. 
http://localhost/index.php/Index/Blog/read
// 和下面的访问是等效的
http://localhost/index.php/index/blog/read

//2.驼峰命名的控制器  BlogTest.php
http://localhost/index.php/Index/blog_test/read
```

> 如果需要==url严格区分大小写==

```
// 关闭URL中控制器和操作名的自动转换
'url_convert'    =>  false,
```



### 模板布局

**7.模板布局**

在application\config.php里找到template，加上下面的代码

```php
// 模板布局
'layout_on'     =>  true,
'layout_name'   =>  'layout',
```

完整

```php
'template'               => [
        // 模板引擎类型 支持 php think 支持扩展
        'type'         => 'Think',
        // 默认模板渲染规则 1 解析为小写+下划线 2 全部转换小写
        'auto_rule'    => 1,
        // 模板路径
        'view_path'    => '',
        // 模板后缀
        'view_suffix'  => 'html',
        // 模板文件名分隔符
        'view_depr'    => DS,
        // 模板引擎普通标签开始标记
        'tpl_begin'    => '{',
        // 模板引擎普通标签结束标记
        'tpl_end'      => '}',
        // 标签库标签开始标记
        'taglib_begin' => '{',
        // 标签库标签结束标记
        'taglib_end'   => '}',
        // 模板布局
        'layout_on'     =>  true,
        'layout_name'   =>  'layout',
    ],
```

> application/Admin/view目录下`创建layout.html`文件，再创建Common作为公共部分，`index.html进行头尾分离`，目录如下

**layout.html**

```php
{include file="Common/meta" /}
{include file="Common/header" /}
{include file="Common/menu" /}
{__CONTENT__}
{include file="Common/js" /}
```



### 创建模块和模型

```php
//创建Admin模块
php think build --module Admin

//创建Home模块
php think build --module Home
    
//创建公共模型
php think make:model common/User/User
```





