---
title: 01nixos安装
description: 
published: 1
date: 2023-07-29T14:47:28.809Z
tags: 
editor: markdown
dateCreated: 2023-06-17T01:21:19.878Z
---

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

我选择的是用 `grub` 启动，所以把 `grub` 相关的选项都启用了（删掉行首的 `#`）。值得注意的是，需要修改以下这些行

```shell
boot.loader.efi.efiSysMountPoint = "/boot" #这里本来是 /boot/efi
boot.loader.grub.device = "/dev/vda" # 这里本来是 sda,但是我们是虚拟机，是 vda
```

其他的时区、键盘、网络这些我都先没有改。我下一个改的是

```shell
enviroment.systemPackages = with pkgs; [
    vim
    wget
];
```

不然默认没有 `vim` 用，而且要记得取消掉最后那个`];` 的注释，我之前就是因为这个，安装的时候，配置文件检查始终不通过。

我还启用了 OpenSSH daemon

```shell
services.openssh.enable =true
```

改好了之后退出（`nano` 下面提示的 `^` 表示 `Ctrl`）

最后我们就可以运行 `nixos-install` 来安装啦。如果没成功，你可以使用如下命令来看哪里有问题

```shell
nixos-install --show-trace
```

并返回上一步去修改 `/mnt/etc/nixos/configuration.nix`，再重新执行 `nixos-install`。
成功了的话，在最后一步， `nixos-install` 会提示你设置 root 密码。

```shell
setting root password...
New password: ***
Retype new password: ***
```

最后执行 `reboot` 重启就装好了。

装好 Nix 之后我们仍然可以继续修改 `/etc/nixos/configuration.nix` ，要切换到修改后的配置需要手动运行

```shell
nixos-rebuild switch
```





### 2. 安装ssh

> 安装配置ssh

```shell
# 添加用户和密码
useradd goer
passwd goer

# 添加用户到sudoers文件
vim /etc/sudoers
root ALL = (ALL) ALL这一行，在下一行加入username ALL = (ALL) ALL。

# 安装软件
vim /etc/nixos/configuration.nix
# 系统软件
enviroment.systemPackages = with pkgs; [
    vim
    wget
    neofetch
    btop
    htop
];

# 开启ssh 注释打开
services.openssh.enable =true

# 安装
sudo nixos-rebuild switch
```

> 查看sshd服务和连接

```shell
# 查看
systemctl status sshd

# 查看ip
ip a

# 用主机连接服务器
ssh goer@ip  # 输入密码
```

> 添加ssh的key
>
> 下面教我们找到结果的方法，而不是直接给你结果 [nixos](https://search.nixos.org/)

```shell
# 添加key
  users.users.goer = {
    extraGroups = [ "networkmanager" "wheel" "docker" ];
    # 该用户的软件 
    packages = with pkgs; [
      bat
    ];
  # Add ssh public key
    openssh.authorizedKeys.keys = [
      "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDJG+12SoXKQcrdoEL3JR3mo35/EUyEF9+SzQ7XO8lUnHKCklG0pngS/I/tcsURIqz1wc17sLUqYoaILa4CKbhkFAA5/RhcmsAir/zKs8ndKS7cXSCJru9LoN0aZYIiubbnGYGGLa5l/RdDeBI5Hb1HhC2u8nNkVyyCKssVYwL2smGj8LkXTmXfKTcUS01ea1DPZP5f8g2vzr7gbxmMNG6OTx1QeM4b9BqZ5G4aUBSe0jClAcsFnhe+cwwEhCapy7tps8fstUmMyqhb/09Wlo3fAEqHCOGbYhKFipcXRg7oRHWl5VaZiitQVYVNhAgjFNdssonou877BDX0v/Ei3VLCSwLgGea+sHpla+0W62xGDqxP6mXfIK9FOQ6svpKHCSVhGiOkuoyTV+ZxnTBeFWMRZLGohFmnuloK4WdGXqxuynraywR1sjRCVMplfgRr4KMgWDIKYtfSLBuStsCrf3d/44mre1utI/tgqod3LoCMMqRyrCRKGPw0J2oiDeh4bE8= hi_goer@163.com
"
    ];
  };
```

这里这个 `192.168.122.50` 就是我们需要的 ip. 之后就可以在外部通过如下的命令连接虚拟机 (`~/.ssh/YOU_PRIVATE_KEY` 替换成你的私钥文件路径)

```shell
ssh root@192.168.122.50 -i ~/.ssh/YOU_PRIVATE_KEY

# 哇哦，连接成功了 牛皮

# 禁止使用密码登录
services.openssh.settings.PasswordAuthentication = false

# 给ssh 分类扩起来
services.openssh = {
    enable = true;
    # Disable SSH password log in
    passwordAuthentication = false;
};
```

> 修改nixos安装源 [nixos一些列问题](https://github.com/nixos-cn/NixOS-FAQ/blob/main/answers/how-to-mannually-download-file-while-nixos-rebuild.md)

可以通过额外的 CLI 参数来指定 Binary Cache 的地址

```shell
sudo nixos-rebuild switch --option substituters "https://mirror.sjtu.edu.cn/nix-channels/store"
```

也可以把以下配置加到 `configuration.nix` 使得**后续**每次都优先走国内镜像, **若需要本次立即生效还是看上面**

```shell
  # 添加 lib模块 在顶部
  { lib, ... }
  
  #注意, 旧版本的 nixos 里, 该选项叫 nix.binaryCaches
  nix.settings.substituters = lib.mkBefore [
    #"https://mirror.sjtu.edu.cn/nix-channels/store"
    "https://mirrors.tuna.tsinghua.edu.cn/nix-channels/store"
    #"https://mirrors.bfsu.edu.cn/nix-channels/store"
  ];
  
 # 更新
 sudo nix-channel --update
```

(选其中一个即可, 用多个镜像并不一定就能让速度变快...)





### 3. docker安装

> 安装docker 软件必备 [nixos系列](https://zhuanlan.zhihu.com/p/612323162)

```shell
# Install Docker
virtualisation.docker.enable = true;


# 添加用户到docker组
  users.users.goer = {
    extraGroups = [ "networkmanager" "wheel" "docker" ];
    # 该用户的软件
    packages = with pkgs; [
      bat
    ];
 };
 
 # 然后退出系统进入 
 docker ps 
 docker composer 
```





### 4. 更新包

```shell
# 查看list
sudo nix-channel --list
# 更新
sudo nix-channel --update
sudo nixos-rebuild switch
```







### 5. 拓展

> 腾讯云使用密钥登录

```shell
# 1. 生成ssh密钥对
sudo -i
ssh-keygen -t rsa

# 2. 回车
# 3. 复制本地公钥， 添加服务器的公钥里面
nano /root/.ssh/authorized_keys 
# 4. 重启ssh服务
ssh root@192.168.122.50 -i ~/.ssh/YOU_PRIVATE_KEY
```













