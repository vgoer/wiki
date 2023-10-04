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



