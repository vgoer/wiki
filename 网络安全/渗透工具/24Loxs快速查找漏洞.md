<center>Loxs快速查找漏洞</center>





[toc]









## Loxs快速查找漏洞

> 查找 SQLi、CRLF、XSS、LFi、OpenRedirect 的最佳工具 [github](https://github.com/coffinxp/loxs)











### 1. 安装

```shell
git clone https://github.com/coffinxp/loxs.git

# 安装
# venv
python3 -m venv loxs
source loxs/bin/activate
pip3 install -r requirements.txt

# 运行
python3 loxs.py
```

> 其他工具

```shell
# go 环境
sudo apt install -y golang

# bash zsh中
export GOROOT=/usr/lib/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
source .bashrc

go install github.com/projectdiscovery/katana/cmd/katana@latest

sudo apt install pipx -y
pipx install uro

go install -v github.com/tomnomnom/anew@latest

go install github.com/KathanP19/Gxss@latest

go install github.com/Emoe/kxss@latest
```

