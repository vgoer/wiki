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











