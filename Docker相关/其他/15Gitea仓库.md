<center>Gitea仓库</center>



[toc]







## Gitea仓库

> 一杯茶就能搞定 Git！轻松自托管的一体化软件开发服务，包含 Git 托管、代码审查、团队协作、软件包注册和 CI/CD
>
> [github](https://github.com/go-gitea/gitea)









### 1. 安装

```shell
mkdir gitea
sudo chown -R 1001:1001 ./gitea
sudo chmod -R 777 ./gitea
```

```yaml
services:
  gitea:
    image: gitea/gitea:latest
    container_name: gitea
    environment:
      - USER_UID=1000
      - USER_GID=1000
      - TZ=Asia/Shanghai
    restart: always
    ports:
      - "3000:3000"   # Web UI
      - "222:22"      # SSH
    volumes:
      - ./data:/data
      - ./config:/etc/gitea
```

> 启动： http://127.0.0.1:3000/
>
> 蛙去。