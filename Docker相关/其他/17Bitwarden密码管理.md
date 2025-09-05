<center>Bitwarden密码管理</center>





[toc]







## Bitwarden密码管理

> Bitwarden 是一款开源的密码管理器，支持多平台（Web、桌面、移动端、浏览器插件等），可以安全地存储和管理你的账号密码、信用卡、笔记等敏感信息。
>
> [github](https://github.com/bitwarden/server)







### 1. 安装

```shell
mkdir bitwarden 
sudo chown -R 1001:1001 ./bitwarden
sudo chmod -R 777 ./bitwarden
```

```yaml
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: always
    ports:
      - 8080:80
    volumes:
      - ./data:/data
    environment:
      - TZ=Asia/Shanghai
```

> 自己去下载客户端：[blog](https://github.com/bitwarden/clients)
>
> 访问：`ip:8080`