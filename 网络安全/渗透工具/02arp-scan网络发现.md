<center>arp-scan网络发现</center>







[toc]









## arp-scan网络发现

> 它使用 ARP 协议来发现本地网络上的 IP 主机并对其进行指纹识别。[arp-scan](https://github.com/royhills/arp-scan)









### 1. 安装

```shell
sudo apt install arp-scan
```







### 2. 使用

1. **扫描局域网**:

   ```shell
   sudo arp-scan -l
   ```

2. **使用特定接口扫描**:

   ```shell
   arp-scan -I eth0 -l
   ```

3. **输出结果到文件**:

   ```shell
    arp-scan -l -o results.txt
   ```

4. **多次扫描**:

   ```shell
    arp-scan -I wlan0 -l -r 3
   ```

5. **关闭 DNS 查找**:

   ```shell
    arp-scan -l -N
   ```

6. **过滤特定 MAC 地址**:

   ```shell
    arp-scan -l -f 00:11:22:33:44:55
   ```