<center>Waydroid手机模拟器</center>



[toc]









## Waydroid手机模拟器

> Waydroid手机模拟器。[github](https://github.com/waydroid/waydroid)









### 1. 安装

```shell
# 安装
yay -S waydroid

# 初始化
sudo waydroid init

# 启动服务
sudo systemctl start waydroid-container

# 启动
waydroid session start
waydroid show-full-ui
```

> qemu开

```shell
# 创建虚拟机脚本
#!/bin/bash
qemu-system-x86_64 \
    -enable-kvm \
    -m 2048 \
    -smp 2 \
    -cpu host \
    -device virtio-vga \
    -display gtk,gl=on \
    -drive file=android.img,format=qcow2 \
    -net nic,model=virtio \
    -net user
```

> 安装apk

```shell
# Waydroid
waydroid app install app.apk
```

> 下载apk: [apk](https://apps.evozi.com/apk-downloader)

```shell
# 设置合适的宽高
waydroid prop set persist.waydroid.width 506
waydroid prop set persist.waydroid.height 1133
```





