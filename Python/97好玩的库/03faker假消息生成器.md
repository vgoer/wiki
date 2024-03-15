<center>python假消息生产器</center>





[toc]







## faker

> Faker [github](https://github.com/joke2k/faker) 是一个为你生成假数据的 Python 包。





### 1. 安装

```shell
pip install Faker
```





### 2. 列子

```python
from faker import Faker
from pprint import pprint



# 本地化 en_US   it_IT ....
fake = Faker("zh_CN")

# 姓名
print(fake.name())

# 邮箱
print(fake.email())

# 国家
print(fake.country())

# 个人信息
pprint(fake.profile())

```

