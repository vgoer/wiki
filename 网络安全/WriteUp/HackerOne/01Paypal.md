<center>Paypal</center>



[toc]









## Paypal

> **PayPal全球支付平台**  [paypal](https://www.paypal.com/)
>
> [youtube](https://www.youtube.com/watch?v=Dtx4kNXj0OQ)









### 1. 信息收集

> *Censys*帮助组织、个人和研究人员*查找*和监控互联网上的每台服务器。 [sensys](https://search.censys.io/)

* 子域名查找

> [chaos](https://chaos.projectdiscovery.io/)

* 工具查找

```shell
# subfinder
sudo apt install subfinder
# 帮助
subfinder -h
# 域名
subfinder -d paypal.com -all > paypal_sub.txt
```

```shell
#assetfinder
sudo apt install assetfinder

# 帮助
assetfinder -h
# 使用
assetfinder -subs-only paypal.com > paypal_sub2.txt
```

```shell
# ffuf
sudo apt install ffuf
# 帮助
ffuf -h

# 使用
ffuf -u https://FUZZ.paypal.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt 
```

> 操作

```shell
# 去重
sort -u paypal_sub.txt paypal_sub2.txt > paypal_domain.txt

# 查看存活的子域名

```



