<center>Dpanel面板</center>



[toc]









## Dpanel面板

> 轻量化的 Docker 可视化管理面板，提供完善的容器管理功能。
>
> [github](https://github.com/donknap/dpanel)







### 1. 安装

```shell
mkdir -p ./dpanel
sudo chown -R 1001:1001 ./dpanel
sudo chmod -R 777 ./dpanel
```

> `docker-compose.yaml`

```yaml
services:
  dpanel:
    image: dpanel/dpanel:latest
    container_name: dpanel # 更改此名称后，请同步修改下方 APP_NAME 环境变量
    restart: always
    ports:
      - 80:80
      - 443:443
      - 8807:8080 # 替换 8807 可更改面板访问端口
    environment:
      APP_NAME: dpanel # 请保持此名称与 container_name 一致
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./dpanel:/dpanel # 将 /home/dpanel 更改为你想要挂载的宿主机目录
    networks:
      - dpanel-local

networks:
  dpanel-local:
    driver: bridge

```

> 访问： `ip:8807`