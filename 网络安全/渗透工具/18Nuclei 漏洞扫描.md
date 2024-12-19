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