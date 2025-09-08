<center>Koodo阅读器</center>





[toc]







## Koodo阅读器

> 一款现代电子书管理器和阅读器，具有适用于 Windows、macOS、Linux、Android、iOS 和 Web 的同步和备份功能
>
> [github](https://github.com/koodo-reader/koodo-reader)







### 1. 安装

```shell
mkdir koodo 
sudo chown -R 1001:1001 ./koodo
sudo chmod -R 777 ./koodo
```

```yaml
services:
  koodo-reader:
    image: liwangsheng/koodo-reader
    container_name: koodo-reader
    restart: always
    ports:
      - 8081:80
    environment:
      - SERVER_USERNAME=admin
      - SERVER_PASSWORD=securePass123
    volumes:
      - ./data:/usr/src/app/static/ebooks
```

> 访问： ip:8081