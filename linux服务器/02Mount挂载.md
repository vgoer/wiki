<center>Mount挂载</center>







[toc]









## Mount挂载

> 挂载硬盘。





### 1.安装

```shell
# 格式化 ext4/btrfs
sudo mkfs.ext4 /dev/nvme0n1p6
sudo mkfs.btrfs /dev/nvme0n1p6


# 创建文件和挂载
sudo mkdir -p /mnt/nvme0n1p7/data
sudo mount /dev/nvme0n1p6 /mnt/nvme0n1p7/data

# 确保挂载成功
df -h

# 持久化挂载
sudo nano /etc/fstab

/dev/nvme0n1p7 /mnt/nvme0n1p7 ext4 defaults 0 2
/dev/nvme0n1p6 /mnt/nvme0n1p7/data ext4 defaults 0 2

/dev/nvme0n1p7 /mnt/nvme0n1p7 btrfs defaults 0 2
/dev/nvme0n1p6 /mnt/nvme0n1p7/data btrfs defaults 0 2

# 运行
sudo mount -a

```

