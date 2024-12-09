<center>IBM</center>





[toc]







## IBM

> 国际商业机器公司。 [ibm](https://www.ibm.com/us-en)







### 1. 信息收集

* 子域名收集

> [youtube](https://www.youtube.com/watch?v=NfUJ5Kd9slc)

```shell
# 安装
git clone https://github.com/gotr00t0day/spyhunt.git

python3 -m venv py 
source py/bin/activate
# 依赖
pip3 install -r requirements.txt 

# 使用
python3 spyhunt.py -s ibm.com --save ibmsb

# 存活的子域名
cat ibmsb | httpx > ibmsubdomains.txt
```

> 漏洞猎人的侦察 [github](https://github.com/gotr00t0day/spyhunt)

```shell
# 对各个域名进行扫描
dirsearch -u https://admin.a959-86e4e29e.us-south.apiconnect.cloud.ibm.com -w /usr/share/seclists/Discovery/Web-Content/big.txt
```

