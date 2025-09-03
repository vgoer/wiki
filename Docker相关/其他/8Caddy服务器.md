<center>Caddy服务器</center>





[toc]









## Caddy服务器

> 快速且可扩展的多平台 HTTP/1-2-3 Web 服务器，具有自动 HTTPS
>
> Caddy Server 是一个开源的现代 Web 服务器，具有自动 HTTPS（自动申请和续签 SSL 证书）、易于配置、高性能等特点。它支持反向代理、负载均衡、静态文件服务等功能，适合用作网站、API 网关、微服务入口等场景。
>
> [github](https://github.com/caddyserver/caddy)







### 1. 安装

```shell
# 1. 文件
mkdir caddy

sudo chown -R 1001:1001 ./caddy
sudo chmod -R 777 ./caddy
```

> 创建 Caddyfile（配置文件）

```shell
:80

# 返回体
# respond "Hello, Caddy!"

# 设置目录
root * /srv
file_server
```

> 编写 docker-compose.yml

```yaml
services:
  caddy:
    image: caddy:latest
    container_name: caddy
    ports:
      - 80:80
      - 443:443
    volumes:
      - Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
      - caddy_config:/config

```

> 访问： 80端口。







### 2. php项目

> php项目

```yaml
services:
  caddy:
    image: caddy:latest
    container_name: caddy
    restart: unless-stopped
    ports:
      - 80:80
      - 443:443
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./site:/srv
      - ./caddy_data:/data
      - ./caddy_config:/config
    depends_on:
      - php

  php:
    image: php:8.2-fpm
    container_name: php-fpm
    restart: unless-stopped
    volumes:
      - ./site:/srv
```

> 创建 Caddyfile（配置文件）

```php
:80

root * /srv
# php
php_fastcgi php:9000
file_server
```







### 3. 负载均衡。

> 创建 Caddyfile（配置文件）

```shell
:80

root * /srv
file_server
```

> 均衡器的 Caddyfile（配置文件）

```shell
:80

reverse_proxy app1:80 app2:80 app3:80
```

> docker-compose.yaml

```yaml
services:
  caddy-load-balance:
    image: caddy:latest
    container_name: caddy-load-balance
    ports:
      - 9090:80
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./caddy-load:/srv
    depends_on:
      - app1
      - app2
      - app3

  app1:
    image: caddy:latest
    container_name: app1
    volumes:
      - ./app1:/srv
      - ./app1/Caddyfile:/etc/caddy/Caddyfile

  app2:
    image: caddy:latest
    container_name: app2
    volumes:
      - ./app2:/srv
      - ./app2/Caddyfile:/etc/caddy/Caddyfile

  app3:
    image: caddy:latest
    container_name: app3
    volumes:
      - ./app3:/srv
      - ./app3/Caddyfile:/etc/caddy/Caddyfile
```

> 启动就可以了。



> 分配方式：

```shell
# 1. 轮询（默认，不写也行）

:80
reverse_proxy app1:80 app2:80 app3:80
```

```shell
# 2. 随机分配

:80
reverse_proxy app1:80 app2:80 app3:80 {
    lb_policy random
}
```

```shell
# 3. 最少连接

:80
reverse_proxy app1:80 app2:80 app3:80 {
    lb_policy least_conn
}
```

```shell
# 4. 按客户端 IP 分配（ip_hash）

:80
reverse_proxy app1:80 app2:80 app3:80 {
    lb_policy ip_hash
}
```

```shell
# 5. 进阶：后端权重分配

:80
reverse_proxy {
    to app1:80 app2:80 app3:80
    lb_policy round_robin
    lb_weight app1:80 3
    lb_weight app2:80 1
    lb_weight app3:80 1
}
```

