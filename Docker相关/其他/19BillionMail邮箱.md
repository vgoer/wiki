<center>BillionMail邮箱</center>



[toc]









## BillionMail邮箱

> BillionMail 为您提供开源邮件服务器、新闻通讯和电子邮件营销服务——完全自托管，开发者友好，且免月费。
>
> [github](https://github.com/aaPanel/BillionMail)







### 1. 安装

```shell
mkdir billion 
sudo chown -R 1001:1001 ./billion
sudo chmod -R 777 ./billion

# 安装
git clone https://github.com/aaPanel/BillionMail && cd BillionMail && cp env_init .env && docker compose up -d || docker-compose up -d

```

