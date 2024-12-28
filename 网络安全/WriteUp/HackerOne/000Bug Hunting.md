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







