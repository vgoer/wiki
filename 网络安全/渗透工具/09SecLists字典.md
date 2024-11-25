<center>SecLists字典</center>





[toc]









## SecLists字典

> SecLists 是一个非常全面的字典集合。
>
> [github](https://github.com/danielmiessler/SecLists)









### 1. 安装

```shell
# 安装 SecLists
sudo apt install seclists
```







### 2. 使用

```shell
# 安装后字典位置
cd /usr/share/seclists

1. Discovery（发现）目录
    # 常用文件
    /Discovery/Web-Content/
    ├── common.txt           # 最常见的目录和文件名
    ├── directory-list-2.3-medium.txt    # 中等大小的目录列表
    ├── directory-list-2.3-small.txt     # 小型目录列表
    ├── directory-list-2.3-big.txt       # 大型目录列表
    ├── raft-large-files.txt    # 大型文件名列表
    ├── raft-large-directories.txt    # 大型目录名列表
    └── api-endpoints.txt      # API端点列表
    
	# DNS相关字典
    /Discovery/DNS/
    ├── subdomains-top1million-5000.txt    # 常见子域名
    ├── subdomains-top1million-20000.txt   # 扩展子域名
    └── dns-Jhaddix.txt                    # 综合DNS字典
    
2. Passwords（密码）目录
    /Passwords/
    ├── Common-Credentials/
    │   ├── 10k-most-common.txt      # 最常见的10000个密码
    │   └── top-passwords-shortlist.txt  # 短密码列表
    ├── Default-Credentials/         # 默认凭证
    ├── Leaked-Databases/           # 泄露数据库中的密码
    └── Software/                   # 特定软件的默认密码
    
3. Usernames（用户名）目录
    /Usernames/
    ├── Names/
    │   ├── names.txt              # 常见人名
    │   └── malenames.txt          # 男性名字
    ├── CommonAdminBase64.txt      # Base64编码的管理员用户名
    └── top-usernames-shortlist.txt # 常见用户名短列表
    
4. Fuzzing（模糊测试）目录
    /Fuzzing/
    ├── XSS/                      # XSS漏洞测试
    ├── SQLi/                     # SQL注入测试
    ├── LFI/                      # 本地文件包含测试
    └── User-Agents/             # User-Agent字符串
    
5. Pattern-Matching（模式匹配）
    /Pattern-Matching/
    ├── credit-cards.txt          # 信用卡号码模式
    ├── social-security-numbers.txt # 社会安全号码
    └── phone-numbers.txt         # 电话号码模式
```

