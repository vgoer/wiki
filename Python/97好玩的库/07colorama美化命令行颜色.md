<center>colorama美化命令行颜色</center>





[toc]









## colorama美化命令行

> colorama: [github](https://github.com/tartley/colorama)Python 中的简单跨平台彩色终端文本. 







### 1. 安装

> install

```shell
pip install colorama
```







### 2. 使用

> 如Fore用于前景色，Back用于背景色，Style用于文本样式。

```python
from colorama import Fore, Back, Style


# 前景色
print(Fore.RED + 'This text is red.')
print(Fore.GREEN + 'This text is green.')
print(Fore.BLUE + 'This text is blue.')


# 背景色
print(Back.YELLOW + 'Yellow background.')
print(Back.MAGENTA + 'Magenta background.')

# 样式
print(Style.BRIGHT + 'Bright text.')
print(Style.DIM + 'Dimmed text.')
print(Style.RESET_ALL)  # 重置所有样式到默认状态
```

> **自动初始化**： 为了确保colorama在Windows上的正常工作，通常在程序开始时初始化

```python
from colorama import init
init(autoreset=True)  # 自动重置每个打印语句后的颜色
```

> 组合使用

```python
print(Fore.RED + Back.GREEN + Style.BRIGHT + 'Red foreground on green background, bright style.')
```

