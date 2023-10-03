---
title: 07.Composer包管理
description: Composer包管理
published: 1
date: 2023-06-09T10:16:37.137Z
tags: php, composer
editor: markdown
dateCreated: 2023-04-22T18:12:16.970Z
---

<center>Composer</center>



[toc]



## composer

> Composer是 PHP 用来管理依赖（dependency）关系的工具，也就是一个安装**包管理工具**。

- [Packagist 英文官网](https://packagist.org/)
- [composer文档](https://docs.phpcomposer.com/)
- [composer官网](https://www.phpcomposer.com/)
- [Packagist中国全量镜像](https://pkg.phpcomposer.com/)
- [阿里镜像源](https://mirrors.aliyun.com/composer/)

* [php中文站](https://www.p2hp.com/)



**下载安装**

下载：[composer](https://getcomposer.org/download/)

选择 php版本，修改环境变量

```
我的电脑->属性->高级系统设置->环境变量，在用户变量中添加环境变量

php -v    //php 版本
composer  //提示
```



**修改镜像源**

```
github和packagist在国内有时会访问不了，或者速度很慢，这就会导致使用Composer时的各种问题
还好我们国内有Packagist中国全量镜像和阿里镜像可以解决这个问题
```



> **全局配置(推荐的方式)**

```php
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/  //阿里
```

> 取消全局

```
composer config --unset repos.packagist
```



### 调试

- composer 命令增加 -vvv 可输出详细的信息，命令如下：

  >  composer -vvv require alibabacloud/sdk

遇到问题？

1. 建议先将Composer版本升级到最新：

``` 
composer self-update  / composer selfupdate
```

2. 如果想进行版本的回滚，可以使用以下命令：

```
composer selfupdate --rollback
```

2. 执行诊断命令：

```
composer diagnose
```

3. 清除缓存：

```
composer clear
```

4. 若项目之前已通过其他源安装，则需要更新 composer.lock 文件，执行命令：

```
composer update --lock
```



### Composer的基本使用

**composer.json**

```php
安装完Composer后，想要在项目里开始使用它，你唯一需要做的就是创建一个composer.json文件。
这个文件描述了你这个项目所依赖的包以及一些其他的元信息。
```

**require**

```php
通过require这个配置项，我们可以指定项目的依赖
require的值是一个对象，对象里的每一个键对应一个依赖
假设我们项目需要用到monolog/monolog这个日志库，那么我们可以这样配置composer.json文件：

{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

**安装依赖**

```shell
composer会根据上面配置的版本约定下载最新版本的`monolog/monolog`到默认目录`vendor`

composer install
```

**更新依赖**

```shell
更新所有的依赖
composer update

更新某个依赖
composer update monolog/monolog
```

**卸载依赖**

```
composer remove monolog/monolog
```

**composer.lock文件**

```php
运行完上面的install命令后，你会发现除了vendor目录，还会多了一个composer.lock文件
这个文件保存了项目已经安装的每个包的具体版本
在运行install命令的时候，如果存在这个文件，则Composer会根据这个文件下载对应版本的包
这样的好处是可以保证各个环境的依赖的版本一致，否则如果没有这个文件
每个环境在运行install时可能下载到的版本就不一致了
所以建议把composer.lock文件也放到版本控制里
```

**Packagist搜索库**

```
要下载什么包，可以去 https://packagist.org/ 找一下包名及其版本信息

安装命令也直接搜索出来
```

**不需要composer.json文件安装依赖**

```php
不需要配置composer.json文件
直接使用 composer require 命令下载类包(自动更新composer.json文件)
下面以下载 phpexcel 为例：

composer require phpexcel/phpexcel
```

**composer常用命令**

```php
composer list    获取帮助信息
composer search    在当前项目中搜索依赖包
composer show    列举所有可用的资源包
```











