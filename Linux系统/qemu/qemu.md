<center> QEMU</center>





[toc]





## QEMU

> qemu是一个虚拟机，或者说是==模拟器==，类似VMware。嵌入式开发中使用广泛 
>
> [官网](https://www.qemu.org/)  







### 1. 安装

> [blog](https://cyan-io.github.io/tags/qemu/)

```shell
# arch
yay -S qemu-full
```







### 2. 快速开始

> [blog](https://cyan-io.github.io/posts/2023-07-30-qemu-quickstart/)

根据冯诺依曼结构，一个计算机可分为：

- 运算器
- 控制器
- 存储器
- 输入设备
- 输出设备



1. 创建存储器

> 创建一个16G的虚拟磁盘文件

```shell
qemu-img create -f qcow2 drive 16G
```

2. 虚拟硬件环境

- 运算器、控制器 -> CPU、各种加速器*
- 输入、输出设备 -> 网卡、CXL设备等

```shell
qemu-system-x86_64 -machine q35 \
	-smp 4,sockets=1,cores=4,threads=1 \
	-m 4096
```

> 现在启动了一个机型为q35，处理器1插槽4核4线程，内存4096MB，使用默认网络配置的虚拟机，由于没有启动盘，并不会启动某个系统

3. 利用虚拟硬件环境启动系统

> 我们已经有了一块硬盘`drive`作为启动盘，指定给虚拟环境

```shell
qemu-system-x86_64 -machine q35 \
	-smp 4,sockets=1,cores=4,threads=1 \
	-m 4096 \
	-drive file=drive
```

> 会发现，指定后并没有什么区别，是因为这个“硬盘”我们没有给它“安装”系统

4. 安装系统

参考为物理机安装系统：

- 物理机硬盘 -> 虚拟磁盘文件`drive`
- 安装媒介（写入了镜像的U盘） -> 系统镜像文件

```shell
qemu-system-x86_64 -machine q35 \
	-smp 4,sockets=1,cores=4,threads=1 \
	-m 4096 \
	-drive file=drive \
	-drive file=<系统镜像，如ubuntu-22.04.2-live-server-amd64.iso>,media=cdrom
```

```shell
qemu-system-x86_64 -machine q35 \
        -smp 4,sockets=1,cores=4,threads=1 \
        -m 4096 \
        -drive file=drive \
        -drive file=../ubuntu-22.04-desktop-amd64.iso,media=cdrom
```



> 按照ISO文件的引导进行安装即可，完成后可移除ISO文件重启

```shell
qemu-system-x86_64 -machine q35 \
        -smp 4,sockets=1,cores=4,threads=1 \
        -m 4096 \
        -drive file=ubuntu.img
```





### 3. 快照和恢复

> 快照和恢复

```shell
# 创建快照

qemu-img.exe snapshot -c snapshot_name arch.img

# 恢复快照
qemu-img.exe snapshot -a snapshot_name arch.img

# 删除快照
qemu-img.exe snapshot -d snapshot-name arch.img


# 查看保存的快照
qemu-img snapshot -l arch.img
```





### 4. 网络

> 设置虚拟机和主机的网络

* 桥接

```shell
# 在主机上创建一个桥接接口，例如 br0：
sudo ip link add name br0 type bridge
sudo ip link set dev br0 up

# 假设物理网卡为 eth0，将其添加到桥接接口中：
sudo ip link set dev eth0 master br0

# 启动配置桥接网络
qemu-system-x86_64 -enable-kvm -machine type=pc,accel=kvm \
-device virtio-vga-gl -display sdl,gl=on -smp 12 -m 4096 \
-netdev bridge,id=net0,br=br0 -device virtio-net-pci,netdev=net0 \
arch.img
```



