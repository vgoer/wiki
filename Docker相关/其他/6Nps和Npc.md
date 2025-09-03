<center>Nps和Npc</center>



[toc]









## Nps和Npc

> 一款轻量级、高性能、功能强大的内网穿透代理服务器
>
> [github](https://github.com/ehang-io/nps/tree/master)  [doc](https://ehang-io.github.io/nps/#/)









### 1. 安装

```shell
mkdir -p ./nps
sudo chown -R 1001:1001 ./nps
sudo chmod -R 777 ./nps

# 服务端
docker pull ffdfgdfg/nps
# 客户端
docker pull ffdfgdfg/npc
```

> nps

```shell
docker run -itd \
  --name nps-server \
  -p 8080:8080 \
  -p 8024:8024 \
  -p 80:80 \
  -p 443:443 \
  -v ./nps/conf:/conf \
  -v ./nps/logs:/logs \
  ffdfgdfg/nps
  
```

> npc

```shell
docker run -d \
  --name nps-client \
  -v /your/npc/conf:/conf \
  ffdfgdfg/npc -server=服务端IP:8024 -vkey=你的密钥
```

