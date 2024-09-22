<center> arch </center>





[toc]





## arch 

> arch







### 1.  没有声音之解决办法

> [blog](https://www.ucloud.cn/yun/15124.html)

```shell
 amixer sset Master unmute
 
 #安装 ALSA 工具和一些固件
 sudo pacman -S alsa-utils alsa-firmware sof-firmware alsa-ucm-conf
```







### 2. 设置屏幕亮度

```shell
# 安装工具
sudo pacman -S light

# 设置 50%
light -S 50 

```







### 3. 密钥错误

> 重新添加密钥

```shell
sudo killall gpg-agent  # 一定要先杀死这个进程
sudo rm -rf /etc/pacman.d/gnupg/
sudo pacman-key --init
sudo pacman-key --populate archlinux && sudo pacman-key --populate archlinuxcn
sudo pacman -Sy archlinux-keyring archlinuxcn-keyring
sudo pacman-key --refresh-keys  # 更新密钥
sudo pacman -Syu  # upgrades work as expected, and no more gpg errors on startup
```



