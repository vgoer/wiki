---
title: 03build-admin重要
description: 
published: 1
date: 2023-08-05T00:51:52.631Z
tags: 
editor: markdown
dateCreated: 2023-08-05T00:51:51.168Z
---

<center>Build-Admin</center>





[toc]





## Build-admin

> 可视化生成CRUD代码、内置WEB终端 的后台管理系统
>
> vue3+tp6  [github](https://github.com/build-admin/buildadmin)  [doc](https://doc.buildadmin.com/)





### 1. 安装

> 首先安装好环境

| 序号 | 描述                                                         |
| :--: | ------------------------------------------------------------ |
|  1   | 克隆[BuildAdmin (opens new window)](https://gitee.com/wonderful-code/buildadmin.git)代码到本地或下载[完整包](https://gitee.com/wonderful-code/buildadmin/releases/download/v2.0.0/badmin-v2.0.0-full.zip) |
|  2   | PHP >= 8.0.2 (开发环境为PHP8.0.2版本)                        |
|  3   | Mysql >= 5.7 (需支持innodb引擎、开发环境为8.0版本)           |
|  4   | NodeJs >= 14.13.1                                            |
|  5   | Npm >= 6.14.0                                                |
|  6   | Composer（[完整包](https://gitee.com/wonderful-code/buildadmin/releases/download/v2.0.0/badmin-v2.0.0-full.zip)不必要，Git克隆包**必需**安装） |

```shell
# 1.下载
git clone https://github.com/build-admin/buildadmin.git
git clone https://gitee.com/wonderful-code/buildadmin.git


# 22.  git克隆的代码需要执行这条命令，完整包不需要，若找不到命令，可以尝试:composer.phar install
composer install

# 3. Linux下推荐使用:sudo php think run
# Linux下若加sudo后仍然异常，请确保 buildadmin 目录的所有者和执行此命令的用户一致，推荐root
php think run
# 指定IP和端口
php think run -H tp.com -p 80
```





### 2. 开发和线上

> 1. 开发

```shell
1. 本地pc安装BuildAdmin 作为开发环境
2. 命令启动服务
	php think run 
3. 填写数据库文件 在安装 BuildAdmin 时您已经填写了系统的数据库资料，需要开启对应的数据库服务，数据库资料被保存在config/database.php文件。

4. 在/web目录内，执行npm run dev命令，在浏览器打开localhost:1818 ，域名一定是localhost（后端已配置它允许跨域）。

5. 开发时，建议开启TP框架的调试模式：找到网站根目录的.env-example重命名为.env。参：开启调试模式。
```

> 2.线上

```shell
1. 删除 /install 目录
2. 线上环境可以选择不上传/web目录，前端每次重新发布后，只将/public/assets 目录和/public/index.html 文件，同步到服务器上即可。
3. 使用Nginx、Apache等服务器软件运行站点，而不再是php think run，请把BuildAdmin当做常规站点，站点的根目录配置为buildadmin目录，站点运行目录为buildadmin/public，如无运行目录配置项，请直接将根目录配置为buildadmin/public。
4. 可以选择配置：隐藏index.html
```

> ### 常见问题
>
> #### [#](https://doc.buildadmin.com/guide/other/developerMustSee.html#为什么开发环境一定是使用php-think-run启动的服务-而不是nginx或其他)为什么开发环境一定是使用`php think run`启动的服务，而不是`Nginx或其他`
>
> - 该服务通过执行一条命令启动，在执行这条命令时，我们能够读取到当前的`环境变量`，以此来实现`WEB终端`的命令执行功能，这条命令启动了站点的`服务端(API服务)`。
>
> #### [#](https://doc.buildadmin.com/guide/other/developerMustSee.html#web目录下执行npm-run-dev的意义)`web`目录下执行`npm run dev`的意义？
>
> - `vue`项目不同于传统`js、jQuery`项目，开发者所有的改动是需要`编译`的（工程化）；而该命令启动了`Vite`的热更新服务，热更新服务可以实现：`开发环境下无需编译快速查看修改效果`，执行该命令后打开的`localhost:1818`，就是具备热更新、热重载等功能的开发专用站点。
>
> #### [#](https://doc.buildadmin.com/guide/other/developerMustSee.html#每次改动都需要重新发布)每次改动都需要`重新发布`？
>
> - 错误。如以上的第`2`点所描述，只需要启动热更新服务进行开发工作即可，只在开发工作完成将要上线时，才进行重新发布。





### 3. 进阶功能

> 进阶功能： [plus](https://doc.buildadmin.com/senior/)
>
> 







