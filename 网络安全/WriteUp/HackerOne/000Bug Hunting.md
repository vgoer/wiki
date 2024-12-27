<center>Bug Hunting</center>





[toc]









## Bug Hunting

> 初学者渗透。









### 1. 子域名收集

```shell
mkdir test
cd test

# 子域名1
subfinder -d facebook.com -all -recursive > subdomain1.txt

# crt.sh
sudo apt  install jq -y
curl -s "https://crt.sh/?q=%.example.com&output=json" | jq -r '.[].name_value' | sed 's/\*\.//g' | sort -u > subdomain2.txt

# 合并
cat subdomain1.txt subdomain2.txt  | sort | uniq > subdomains.txt

```

> 子域名： [subfinder](https://github.com/projectdiscovery/subfinder) 
>
> 子域名： [crt.sh](https://github.com/crtsh)





