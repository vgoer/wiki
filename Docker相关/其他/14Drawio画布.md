<center>Drawio画布</center>





[toc]







## Drawio画布

> draw.io 是一个用于一般图表绘制的 JavaScript 客户端编辑器。
>
> [github](https://github.com/jgraph/drawio)





### 1. 安装

```shell
mkdir drawio
sudo chown -R 1001:1001 ./drawio
sudo chmod -R 777 ./drawio
```

```yaml
services:
  drawio:
    image: jgraph/drawio:latest
    container_name: drawio
    restart: always
    ports:
      - 8080:8080
    volumes:
      - ./data:/data
    environment:
      - TZ=Asia/Shanghai
```

> 访问：ip:80端口。

