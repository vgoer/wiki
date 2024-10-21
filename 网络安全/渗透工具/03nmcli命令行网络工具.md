<center>nmcli命令行网络工具</center>









[toc]







## nmcli

> nmcli命令行网络工具。 [nmcli](https://networkmanager.dev/docs/api/latest/nmcli.html)





### 1. 安装

```shell
# Debian/Ubuntu/Mint 
sudo apt install network-manager

# arch 
sudo pacman -S networkmanager
```



**1. 查看网络设备状态：**

```bash
nmcli device status
```

这将显示所有网络设备的状态，包括连接状态、连接名称、设备类型等。

**2. 连接到 WiFi 网络：**

```bash


nmcli device wifi connect <SSID> password <password>
```

将 `<SSID>` 替换为 WiFi 网络的名称 (SSID)，`<password>` 替换为 WiFi 密码。

**3. 断开 WiFi 连接：**

```bash


nmcli device disconnect <device_name>
```

将 `<device_name>` 替换为要断开的网络设备名称，例如 `wlan0` 或 `eth0`。

**4. 列出可用的 WiFi 网络：**

```bash


nmcli device wifi list
```

这将列出所有可用的 WiFi 网络，包括 SSID、信号强度、安全类型等。

**5. 创建新的 WiFi 连接：**

```bash


nmcli connection add type wifi con-name <connection_name> ifname <interface_name> ssid <SSID> password <password>
```

这将创建一个新的 WiFi 连接配置。将 `<connection_name>` 替换为连接的名称，`<interface_name>` 替换为网络接口名称 (例如 `wlan0`)，`<SSID>` 替换为 WiFi 网络的名称，`<password>` 替换为 WiFi 密码。

**6. 激活已保存的 WiFi 连接：**

```bash


nmcli connection up id <connection_name>
```

将 `<connection_name>` 替换为要激活的连接的名称。

**7. 删除 WiFi 连接：**

```bash


nmcli connection delete id <connection_name>
```

将 `<connection_name>` 替换为要删除的连接的名称。

**8. 显示所有连接：**

```bash


nmcli connection show
```

这将显示所有已保存的网络连接。

**9. 修改连接：**

```bash


nmcli connection modify <connection_name> <property> <value>
```

例如，修改 `MyWiFi` 连接的 IP 地址：

```bash


nmcli connection modify MyWiFi ipv4.addresses 192.168.1.100/24
```