<center>Nuclei 漏洞扫描</center>





[toc]









## Nuclei 漏洞扫描

> Nuclei 是一款快速、可定制的漏洞扫描程序，由全球安全社区提供支持
>
> [github](https://github.com/projectdiscovery/nuclei)





### 1. 安装

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

> [nuclei](https://github.com/projectdiscovery/nuclei?tab=readme-ov-file)  [templates](https://github.com/projectdiscovery/nuclei-templates)

```shell
# 安装
go install github.com/projectdiscovery/katana/cmd/katana@latest
# 复制到环境变量中 
sudo cp ~/go/bin/katana /usr/local/bin

katana -u https://adjvendor.paypal.com -o urls.txt
```





### 2. pdtm

> ProjectDiscovery 的开源工具管理器  [github](https://github.com/projectdiscovery/pdtm)

```shell
go install -v github.com/projectdiscovery/pdtm/cmd/pdtm@latest

# 使用
qdtm


# 安装全部项目
pdtm -install-all
```







### 3. cvemap

> 使用 CVEMAP 轻松浏览常见漏洞和暴露 (CVE) 丛林，CVEMAP 是一种命令行界面 (CLI) 工具，旨在为各种漏洞数据库提供结构化且易于导航的界面。 [github](https://github.com/projectdiscovery/cvemap)

```shell
go install github.com/projectdiscovery/cvemap/cmd/cvemap@latest

# 使用
cvemap -h



```

