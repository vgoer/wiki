---
title: 03.php环境
description: php环境
published: 1
date: 2023-06-09T10:11:17.002Z
tags: docker env, php
editor: markdown
dateCreated: 2023-04-22T05:07:37.444Z
---

<center>PHP</center>





[toc]





## php

> `docker php`环境
>
> 来自这个博主的配置：[gihub ](https://github.com/scobin/docker-php)[blog](https://cyublog.com/articles/php-zh/zh-docker-php8/)



```shell
git clone https://github.com/scobin/docker-php.git
```

>  `nginx.conf`中的root设定里，**example-app**是等一下要建立的Laravel应用程式的专案名称，你可以自行修正，但修正后等一下建立Laravel应用时请一定要用一样的名称。

```shell
# 进入容器
docker exec -it php-laravel bash

# change path according to your nginx.conf
cd .. # 到www目录下

# create project
composer create-project laravel/laravel example-app
```

> 如果权限错误

```shell
UnexpectedValueExceptionThe stream or file “/var/www/example-app/storage/logs/laravel.log” could not be open in append mode: Failed to open stream: Permission denied
```

```shell 
#进入php容器
docker-compose exec php bash
# 修改权限
chown -R www-data:www-data /var/www/example-app/storage
chown -R www-data:www-data /var/www/example-app/bootstrap/cache
chmod -R 775 /var/www/example-app/storage
chmod -R 775 /var/www/example-app/bootstrap/cache
```



> 我自己改造的file [dockerfile](https://github.com/vgoer/dockerLaravel)











### 2. webman

> 构建容器







```shell
# 构建容器
docker build -t xdly/webman:8.1 .

# 如果有php和composer则直接启动项目 注意映射路径 /home/goer/docker_data/webman:/app
docker run -itd -p 8787:8787 -v /home/goer/docker_data/webman:/app --name webman xdly/webman:8.1

# 没有php环境需要先进入容器，安装webman
docker exec -it webman bash
composer create-project workerman/webman xdly_api
#退出容器
exit
# 重新找到安装好的webman路径，启动项目
docker run -itd -p 8787:8787 -v /home/goer/docker_data/webman/xdly_api/:/app/xdly_api --name webman xdly/webman:8.1
# 修改项目属组 就有权限可以修改了
sudo chown -R $USER:$USER /home/goer/docker_data/webman/xdly_api/
```





> 公司的构建

```shell
# 导入容器 容器名和标签 xdly/webman:8.1
docker load -i xdly_webman_docker.tar

# 有webman项目 确定路径/home/goer/docker_data/webman
docker run -itd -p 8787:8787 -v /home/goer/docker_data/webman:/app --name webman xdly/webman:8.1

# 如果要安装依赖的话 进入容器
docker exec -it webman bash
composer requeire xxxx
```

