<center>Amass资产发现</center>





[toc]













## Amass资产发现

> 深入的攻击面映射和资产发现
>
> [github](https://github.com/owasp-amass/amass)







### 1. 安装

> 看仓库，有很多种安装方式。

>  预构建包

1. 只需解压[软件包即可](https://github.com/owasp-amass/amass/releases/latest)
2. 将预编译的二进制文件放入你的路径中
3. 开始使用 OWASP Amass！

````shell
# 下载
wget https://github.com/owasp-amass/amass/releases/download/v4.2.0/amass_Linux_arm64.zip

#解压
uzip amass_Linux_arm64.zip 

# 移动到path目录
sudo cp amass /usr/local/bin

# 使用
amass enum -d paypal.com -o recon/amass.txt
````





