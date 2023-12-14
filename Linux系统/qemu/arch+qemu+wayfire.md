<center>arch + qemu</center>





[toc]





## arch + qemu

> quem上安装arch  [bilibili](https://www.bilibili.com/video/BV1st4y187yx/?vd_source=86b829d6caeffc65037786a84ec2cb17)



### 1. 安装

> arch: [arch](https://archlinux.org/)

```shell
# 1. 创建镜像
qemu-img qemu-img create -f qcow2 arch.img 20g

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











