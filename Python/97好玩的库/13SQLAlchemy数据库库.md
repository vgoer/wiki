<center>SQLAlchemy数据库</center>









[toc]







## SQLAlchemy数据库

> SQLAlchemy [github](https://github.com/sqlalchemy/sqlalchemy) 是一个强大的 Python 库，用于数据库操作。









### 1. 安装

> blog: [blog](https://blog.csdn.net/js010111/article/details/119844734)
>
> 在使用SQLAlchemy前要先给Python安装MySQL驱动，由于MySQL不支持和Python3，因此需要使用PyMySQL与SQLAlchemy交互。

```shell
pip install sqlalchemy
pip install pymysql
```

> 连接数据库

```python
from sqlalchemy import create_engine
engine = create_engine("mysql+pymysql://user:password@localhost:3306/database",echo=True)

engine = create_engine("数据库类型+数据库驱动://数据库用户名:数据库密码@IP地址:端口号/数据库?编码...", 其它参数)
```

