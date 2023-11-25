<center>cheat-帮助</center>





[toc]





## cheat.sh

> linux命令帮助。[github](https://github.com/chubin/cheat.sh)  [cheat](https://cheat.sh/)



```shell
# 使用： 

curl cheat.sh   # 查看帮助
```



> 查看其他命令

```shell
# 其他
curl cheat.sh/ls  
curl cheat.sh/ps
```







###  2. cheat 工具

> cheat [cheat](https://github.com/cheat/cheat)

```shell
cd /tmp \
  && wget https://github.com/cheat/cheat/releases/download/4.4.0/cheat-linux-amd64.gz \
  && gunzip cheat-linux-amd64.gz \
  && chmod +x cheat-linux-amd64 \
  && sudo mv cheat-linux-amd64 /usr/local/bin/cheat
  
# cheat 使用
cheat --help

cheat tar  # 查看命令使用

cheat -e tar # 编辑tar的使用文档
```

```shell
#  cd ~/.config/cheat

# nvim conf.yml

# The editor to use with 'cheat -e <sheet>'. Defaults to $EDITOR or $VISUAL.
editor: nvim

# Should 'cheat' always colorize output?
colorize: false
# Which 'chroma' colorscheme should be applied to the output?
# Options are available here:
#   https://github.com/alecthomas/chroma/tree/master/styles
style: monokai

# Which 'chroma' "formatter" should be applied?
# One of: "terminal", "terminal256", "terminal16m"
formatter: terminal256

# Through which pager should output be piped?
# 'less -FRX' is recommended on Unix systems
# 'more' is recommended on Windows
pager: less
```

