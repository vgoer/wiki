<center>雷池SafeLine</center>







[toc]









## 雷池SafeLine

> SafeLine，中文名 "雷池"，是一款简单好用, 效果突出的 **`Web 应用防火墙(WAF)`**，可以保护 Web 服务不受黑客攻击。
>
> [safe](https://docs.waf-ce.chaitin.cn/zh/home)







### 1. docker安装

> docker安装

```shell
mkdir -p /data/safeline
cd /data/safeline

wget "https://waf-ce.chaitin.cn/release/latest/compose.yaml"

# .env 环境变量
```

```shell
SAFELINE_DIR=/data/safeline
IMAGE_TAG=latest
MGT_PORT=9443
POSTGRES_PASSWORD=goerAbc123 #-------（自定义密码使用数字+英文大小写组合，勿使用特殊字符）
SUBNET_PREFIX=172.22.222
IMAGE_PREFIX=swr.cn-east-3.myhuaweicloud.com/chaitin-safeline
ARCH_SUFFIX=
RELEASE=
REGION=
```

> 如果是 ARM 服务器需要把 `ARCH_SUFFIX`改成 `-arm`
>
> ```none
> ARCH_SUFFIX=-arm
> ```
>
> 
>
> 如果是安装 LTS 版本需要把 `RELEASE` 改成 `-lts`
>
> ```none
> RELEASE=-lts
> ```

> 重置密码：

```shell
docker exec safeline-mgt resetadmin
```









### 2. 使用

> 雷池基于 `Nginx` 进行开发, 作为 `反向代理` 接入网络，。 [blog](https://docs.waf-ce.chaitin.cn/zh/%E4%B8%8A%E6%89%8B%E6%8C%87%E5%8D%97/%E6%B7%BB%E5%8A%A0%E5%BA%94%E7%94%A8)