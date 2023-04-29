---
title: vim
description: vim编辑器
published: 1
date: 2023-04-29T17:41:23.476Z
tags: app, vim
editor: markdown
dateCreated: 2023-04-19T15:28:06.052Z
---

<center>VIM</center>


## vim

> 好用的代码编辑器 [vim](https://vimawesome.com/)

### 写入模式

```shell
i  光标之前插入
shift + i 本行最开始插入

a  光标之后插入
shift + a 本行最后面插入

o 下一行插入
shift + o 上一行插入
```

### 命令模式

```shell
# 光标移动
h 上
j 右
k 下
l 左
```

```shell
操作  + 动作

d (剪切)  动作 d4j  右剪切4个字符 dd 剪切到前行

p 粘贴

y (复制) 动作 y10l 左边复制10个字符 yy 复制当前行

c (改变) 动作 c6j 删除右边6个字符 进入插入模式

f (找) fv --> 找v字母 
df: --> 复制找到冒号的内容

w --> 下一个单词
b --> 上一个单词
i --> 自己
eg:  ciw --> 删除这个单词进入编辑
eg: " who am i "
di" 剪切双引号里面的内容 yi" 复制里面的内容

```
> 可视块
```shell
# 块操作
v   进入块操作

头尾加字符
v 选中要的行
:<，'>normal I/A想加的字符  I头/A尾

ctrl+v 列编辑模式
ctrl+i 编辑好
esc 退出 所以列都改变了

```



### 输入模式
> `:`进入输入模式

```shell
set nu 显示行号

split 上下分屏

vsplit 左右分屏

color 切换配色

```
```shell
# 打开文件
e + 文件路径 

# 打开文件
cat /home/a.txt

鼠标移动到上面 gf(g to file)

# 回到光标上次修改的地方
ctrl+i  
ctrl+o

# 修改了需要管理员权限的文件
1.先保存到其他地方
: w /home/Desktop

2. vim执行终端命令
：w !sudo tee %(当前文件)


# 生成美化字符的小工具 figlet 
map tx :r !figlet 字符


# 打印文件 html pdf
:%TOhtml  当前文件打印html文件

```





### vim 配置文件

> 家目录 `.vim/vimrc`

```shell

# noremap 修改原来的快捷键
noremap h i # i 替代原来的h键

# map 自定义快捷键
map S :wq<CR>  # 大S 保存退出
# <CR> --> 确认 <nop> --> 没有键

# 自定义 不用退出生效当前配置文件
map R :source $MYVIMRC<CR>  # R 生效当前配置文件

# 代码高亮
syntax on 

# 显示行号
set number 
set relativenumber 

# 显示线条
set cursorline 

# 换行
set wrap

# 显示按键
set showcmd

# 命令模式下 tab补齐菜单
set wildmenu 

# 高亮搜索
set hlsearch
set incsearch # 变输入变高亮

# 忽略大小写
set ignorecase 
set smartcase 

# 自定义分屏快捷键
# split  # vsplit    splitright 光标在右
map si :set splitright<CR>:vsplit<CR>
map sn :set nosplitright<CR>:vsplit<CR>
map su :set nosplitbelow<CR>:split<CR>
map se :set splitbelow<CR>:split<CR>
# ctrl+w ->  hjkl 光标移动


# 修改vim分屏大小
# res 上下 vertical resize 
map <up> :res +5<CR>
map <down> :res -5<CR>
map <left> :vertical resize -5<CR>
map <right> :vertical resize +5<CR>

# 打开新的vim标签
# 命令 tabe 上下切换
map tu :tabe<CR>
map tn :-tabnext<CR>  
map ti :+tabnext<CR>

# 始终上下有几行
set scrolloff=5 



```



###  安装插件

1. vim-plug  插件管理器

> 地址[plug](https://github.com/junegunn/vim-plug)

```shell
# 安装插件
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

call plug#begin()

# 插件
Plug 'junegunn/vim-easy-align'

# Initialize plugin system
call plug#end()

:PlugInstall 安装
```

2. vim-airline 状态栏
> 地址[airline](https://github.com/vim-airline/vim-airline)
```shell

Plug 'vim-airline/vim-airline'

```

3. vim-snazzy 主题
> 地址[snazzy](https://github.com/vim-airline/vim-airline)
>
> 好看的vim主题 [vim-theme](https://www.jianshu.com/p/e121324429b5)
```shell

Plug 'connorholyday/vim-snazzy'

# vim配置文件
color snazzy 

# 透明背景
let g:SnazzyTransparent = 1

```

4. nerdtree 目录树

> 地址[nerdtree](https://github.com/preservim/nerdtree)
>

```shell

Plug 'preservim/nerdtree'

```

4. YouCompleteMe 代码补全插件

> 地址[YouCompleteMe](https://github.com/ycm-core/YouCompleteMe)
>

```shell

Plug 'ycm-core/YouCompleteMe'

进入这个文件目录
安装 cmake 
sudo python3 install.py

```

4. vim-floaterm  vim上 浮动终端

> 地址[floaterm](https://github.com/voldikss/vim-floaterm)
>
> 简单使用 [blog](https://blog.csdn.net/weixin_39795268/article/details/111344410)

```shell

Plug 'voldikss/vim-floaterm'

# vim配置  :FloatermNew 打开  :FloatermToggle y隐藏
map 使用快捷键


```