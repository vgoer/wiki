<center>arch + qemu</center>





[toc]





## arch + qemu

> quem上安装arch  [bilibili](https://www.bilibili.com/video/BV1st4y187yx/?vd_source=86b829d6caeffc65037786a84ec2cb17)



### 1. 安装

> arch: [arch](https://archlinux.org/)

```shell
# 1. 创建镜像
qemu-img create -f qcow2 arch.img 20g

# 2. 准备好镜像

# 3. 启动 开启opengl
qemu-system-x86_64 --enable-kvm -machine type=pc,accel=kvm -vga virtio -display gtk,gl=on -smp 12 -m 4096 arch.img -boot d -cdrom ../archlinux-2023.09.01-x86_64.iso
```

> archinstall: 安装脚本

```shell
archinstall

# 关机
shutdown -h now
```

> 从镜像启动

```shell
qemu-system-x86_64 --enable-kvm -machine type=pc,accel=kvm -device virtio-vga-gl -display sdl,gl=on -smp 12 -m 4096 arch.img
```





### 3. 软件安装

````shell
yay -S fish  # 终端


sudo pacman -S terminus-font # 字体
setfont ter-v32n  # 设置字体


# 添加 cn源
sudo vim /etc/pacman.conf
```shell
Color  #开启颜色

[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch

```

# 命令安装 archlinuxcn-keyring 包导入 GPG key。
sduo pacman -Sy archlinuxcn-keyring


#  yay 助手
sudo pacman -S yay

# 下载 wayfire
yay -S wayfire-git

# 添加环境变量
sudo vim /etc/environment
WLR_NO_HARDWARE_CURSORS=1


# 开启 seatd 服务
systemctl start seatd
systemctl enable seatd
systemctl status seatd





````











