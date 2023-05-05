---
title: zsh和oh-my-zsh
description: zsh和oh-my-zsh
published: 1
date: 2023-05-05T11:43:46.179Z
tags: zsh, oh-my-zsh
editor: markdown
dateCreated: 2023-04-30T03:33:13.080Z
---

<center>zsh</center>





[toc]





## ZSH

> zsh 是一个兼容 bash 的 shell [zsh](https://www.zsh.org/) [blog](https://zhuanlan.zhihu.com/p/441676276)

- Tab 补全功能强大。命令、命令参数、文件路径均可以补全。
- 插件丰富。快速输入以前使用过的命令、快速跳转文件夹、显示系统负载这些都可以通过插件实现。
- 主题丰富。
- 可定制性高。





### 1. 安装

```shell
# mac
brew install zsh

# ubuntu 
sudo apt install zsh

# arch 
sudo pacman -S zsh
```

> `cat /etc/shells` 查看系统可以用shell

```shell
# 系统默认shell
chsh -s /bin/zsh
echo $SHELL
```



### 2. oh-my-zsh

> [oh-my-zsh](https://ohmyz.sh/)修改 zsh 的主题和安装常用的插件。

```shell
# 安装
sudo pacman -S curl wget git
#curl 
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# 或者wget
sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

> 配置文件就在 `~/.zshrc`

> 修改主题 [theme](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)

```shell
vim .zshrc

ZSH_THEME="" # 主题名称
```

> 插件 [plugin](https://github.com/unixorn/awesome-zsh-plugins)

```shell
vim .zshrc

# 1. 提示插件 zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# 2. 语法o校验 zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting 

# 配置
plugins=(
     # other plugins...
     git
     zsh-autosuggestions
     zsh-syntax-highlighting
     z
)
```

> 别名: 为较长命令设置一个别名

```shell
vim .zshrc

alias cdblog="cd ~/projects/alicode/blog" 
```















