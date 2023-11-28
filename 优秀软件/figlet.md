<center>figlet</center>





[toc]







## figlet

> figlet: [github](https://github.com/cmatsuoka/figlet)打印出多样的字符 [doc](http://www.figlet.org/)







## 1. 安装

```shell
yay -S figlet
```







## 2. 使用

```shell
# 简单使用
figlet goer

# 查看可使用的字体
showfigfonts

# 使用字体
figlet -f smslant goer

# 炫酷的时钟
watch -n1 "date '+%D%n%T'|figlet -k"

# 更好的
while true; do echo "$(date '+%D %T' | toilet -f term -F border --gay)"; sleep 1; done

# yes
while true; do echo "$(date '+%D %T' | toilet -f term -F border --gay)"; sleep 1; done | lolcat -a  -p -F 10
```













