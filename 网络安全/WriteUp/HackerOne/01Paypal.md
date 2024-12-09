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
# 添加https://
awk '{print "https://" $0}' paypal_domain.txt > paypal_s_domain.txt
# httpx https://github.com/projectdiscovery/httpx/releases/tag/v1.6.9
cat paypal_domain.txt| httpx -status-code -tech-detect -title > live_paypal.txt
cat paypal_domain.txt | sudo  httpx > live_paypal.txt
```

```shell
# 安装 Go
sudo apt install golang-go
# subzy
go install -v github.com/PentestPad/subzy@latest
# 复制到环境变量中 
sudo cp ~/go/bin/subzy /usr/local/bin
# 使用
subzy run --targets paypal_domain.txt
```

```shell
# 安装
sudo apt install nuclei
# git
git clone https://github.com/projectdiscovery/nuclei-templates.git
# 或者
# 自动更新
nuclei -update
# 模板会自动下载到 .local目录下
nuclei -ut

# 使用 -t 模板
nuclei -l paypal_domain.txt -t takeovers/ 
```
