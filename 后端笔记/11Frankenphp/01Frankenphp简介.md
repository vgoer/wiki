<center>Frankenphp简介</center>

[toc]









## Frankenphp简介

> FrankenPHP 是建立在 [Caddy](https://caddyserver.com/) Web 服务器之上的现代 PHP 应用程序服务器。[github](https://frankenphp.dev/cn/docs/)
>
> FrankenPHP 凭借其令人惊叹的功能为您的 PHP 应用程序提供了超能力：[早期提示](https://frankenphp.dev/cn/docs/early-hints/)、[worker 模式](https://frankenphp.dev/cn/docs/worker/)、[实时功能](https://frankenphp.dev/cn/docs/mercure/)、自动 HTTPS、HTTP/2 和 HTTP/3 支持……
>
> FrankenPHP 可与任何 PHP 应用程序一起使用，并且由于提供了与 worker 模式的集成，使您的 Symfony 和 Laravel 项目比以往任何时候都更快。
>
> FrankenPHP 也可以用作独立的 Go 库，将 PHP 嵌入到任何使用 net/http 的应用程序中。







### 1. 开始

> docker

```php
mkdir project
cd project 
    
docker run -v $PWD:/app/public \
    -p 80:80 -p 443:443 -p 443:443/udp \
    dunglas/frankenphp
```

> 访问 `https://localhost`, 并享受吧!

