<center>Nginx</center>



[toc]





## Nginx

> `nginx`:Nginx 是一个开源的网页服务器. [nginx](https://nginx.org/en/) [docker+nginx](https://juejin.cn/post/6885618130471780366)





### 1. 反向代理和负载均衡

> [blog](https://blog.csdn.net/zpf1813763637/article/details/109455451) [blog](https://juejin.cn/post/6885618130471780366)

> `docker-compose.yml`准备4个Nginx容器，1个用来做负载均衡服务器，3个是App Server 

```shell
# 1. 新建文件
rmdir nginx-load app1 app2 app3

# 2. 复制 nginx-load/nginx.conf

# 3. 复制 ./app1和2和3/html/index.html

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
> 负载均衡服务器配置 `nginx.conf` 如下：

```shell
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  10240;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    upstream backend-server {
        server app-server1:80;
        server app-server2:80;
        server app-server3:80;
    }
    server {
	server_name localhost;
	listen 80 ;
	access_log /var/log/nginx/access.log;
	location / {
		proxy_pass http://backend-server;
	}
    }
}

```


```yaml
services:
  nginx-load-balancing:
    image: nginx:alpine
    container_name: nginx-load-balancing
    ports:
      - 9090:80
    volumes:
      - ./nginx-load/nginx.conf:/etc/nginx/nginx.conf
      - ./nginx-load/nginx_logs:/var/log/nginx
    depends_on:
      - app-server1
      - app-server2
      - app-server3

  app-server1:
    image: nginx:alpine
    container_name: app-server1
    volumes:
      - ./app1/nginx_logs:/var/log/nginx
      - ./app1/html:/usr/share/nginx/html

  app-server2:
    image: nginx:alpine
    container_name: app-server2
    volumes:
      - ./app2/nginx_logs:/var/log/nginx
      - ./app2/html:/usr/share/nginx/html

  app-server3:
    image: nginx:alpine
    container_name: app-server3
    volumes:
      - ./app3/nginx_logs:/var/log/nginx
      - ./app3/html:/usr/share/nginx/html
    
networks:
  default:
    driver: bridge


```



> 由于4个容器都在同一docker网络中，因此在配置 `upstream` 模块时，可以直接使用容器名称
>
> 使用 `docker-compose up -d` 启动应用



### 测试

> 分别进入容器 `app-server1` ， `app-server2` ，`app-server3` ，在 `/usr/share/nginx/html/index.html` 中标注当前的容器名称，使用浏览器访问 `http://localhost:9090` 

```shell
docker exec -it app-server1 sh

vi /usr/share/nginx/html/index.html
```



### 2. 负载均衡upstream分配方式

> `Nginx` 的 `upstream` 模块主要支持以下五种分配方式：



轮询
 这是默认的分配方式，根据请求的时间顺序均匀的分配到每个服务器

```vbscript
upstream backend-server {
    server app-server1:80;
    server app-server2:80;
    server app-server3:80;
}
```

weight
 这是轮询的加强版，没有设置取默认值1，负载均衡服务器会以 1:2:3 的比例将请求转发到3台服务器，主要用于服务器配置差异的场景

```ini
upstream backend-server {
    server app-server1:80;
    server app-server2:80 weight=2;
    server app-server3:80 weight=3;
}
```

ip_hash
 根据每个请求IP地址的Hash结果进行分配，使得每个访客会固定访问同一台服务器

```ini
upstream backend-server {
    ip_hash;
    server app-server1:80;
    server app-server2:80;
    server app-server3:80;
}
```

fair
 根据后端服务器响应时间来分配，响应时间越短越优先

```ini
upstream backend-server {
    server app-server1:80;
    server app-server2:80;
    server app-server3:80;
    fair;
}
```

url_hash
 根据访问url的hash结果来分配请求，使得每个url会固定访问同一台服务器，使用了hash算法则server中不能再使用weight等参数

```ini
upstream backend-server {
    server app-server1:80;
    server app-server2:80;
    server app-server3:80;
    hash $request_uri;    hash_method crc32;
}
```