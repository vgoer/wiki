<center>config配置</center>



[toc]









## config配置

> 读取配置文件。python官方 [config](https://docs.python.org/zh-cn/3.7/library/configparser.html)





> `.env`

```env
[db]
ip = 1.1.1.1
port = 123

[web]
uid = webadmin
pwd = 123123
ip = 111.111.111.1

```





> 代码： 写入

```python
import configparser as ini


# 配置文件路径
filename = "./config.env"

# 配置文件读取
inifile = ini.ConfigParser()
inifile.read(filename, "UTF-8")


# 读取
print("db.ip=", inifile.get("db","ip"))
# 第二
print("web.pwd=",inifile.get("web","pwd"))


# 添加新的
inifile["web"]["ip"] = "111.111.111.1"
with open(filename, "w") as configfile:
    inifile.write(configfile)
```

