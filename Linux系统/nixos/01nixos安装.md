<center>nixos安装</center>





[toc]







## nixos

> nixos，更加现代化的linux发行版 
>
> [nixos](https://nixos.org/)  [nixos cn](https://nixos-cn.org/)  [github](https://github.com/NixOS-CN)
>
> linux:一切皆文件
>
> nixos：一切皆配置文件
>
> 1. 原子性升级和回滚： 无论是个人使用或者服务器使用都是非常好的
>
> 2. 声明式系统配置
>
> 3. 一致的开发环境
>
>    > 同一个配置文件，可以复刻一模一样的系统







### 1. 安装

> from [blog ](https://blog.askk.cc/2022/09/18/nixos-installation/index.html)[blogg](https://blog.jogle.top/2022/06/19/nixos-get-started/) [bili](https://tech.aufomm.com/)



#### a.前期准备

> [download](https://nixos.org/download.html#nixos-iso)

```shell
下载一个最小化系统
新建虚拟机
进入 livecd 切换到sudo root 
sudo -i
查看连接网络，ip a
```

#### b.磁盘分区

然后我们就可以开始给磁盘分区了，官方教程用的 `gparted` 我更喜欢用 `fdisk`。先用 `fdisk -l` 看下我们的虚拟磁盘叫什么名字，不出意外是叫 `vda` (我打命令的时候老是打成 `sda`)。

现在我们使用 `fdisk` 对磁盘进行分区。

```shell
fdisk /dev/sda
```

接下来在 新建一个`boot` 分区，输入 `n` (表示 new), 起始点默认，大小选择 `+512M` 然后回车。接着创建主分区，输入 `n` 然后一路回车（我们把剩下所有空间都分给它）。

接着调整分区类型，输入 `t` (表示 type)，先选 `1`(这是我们的 `boot` 分区)， 然后类型输入 `EF` (表示 EFI)， 接着继续输入 `t`, 选择 `2`, 类型选择 `linux`。
更改完类型之后输入 `w` (表示 write) 然后输入 `q` 退出。

我们还要格式化磁盘，首先格式化 `boot` 分区为 `FAT32` 格式。

```
SHELL
mkfs.fat -F 32 -n boot /dev/vda1
```

格式化主分区为你喜欢的格式(这里我选的 `btrfs`, 你也可以使用 `ext4`, 反正随便尝试嘛，我就想试试这个，我自己笔记本电脑上还是用的 `ext4`).

```
SHELL
mkfs.btrfs -L nixos /dev/vda2
```

现在我们已经调整好磁盘了，开始准备 `chroot`了。



#### c.开始安装

先将 主分区 `vda2` 挂载到 `/mnt` 下面

```shell
mount /dev/vda2 /mnt
```

然后在 `/mnt` 下面创建 `boot` 目录并挂载 `vda1` 到 该目录

```shell
mkdir /mnt/boot
mount /dev/vda1 /mnt/boot
```

接着就可以生成配置文件了

```shell
nixos-generate-config --root /mnt
```

我们先修改配置文件（在这一步我踩了好多坑，改了好多次配置文件才通过），你不喜欢 `nano` 的话可以用 `vim`, 我自己也使用 `vim` (不会 `nano`)

```shell
nano /mnt/etc/nixos/configuration.nix
```









