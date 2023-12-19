<center>WechatRobot</center>





[toc]







## 微信机器人

> 一个基于 [WeChatFerry](https://github.com/lich0821/WeChatFerry) 的微信机器人示例。 [github](https://github.com/lich0821/WeChatRobot)







### 1. 安装

> 官方文档很详细了

1. 安装 Python>=3.9（Python 12 需要自己编译依赖，慎选），例如 [3.10.11](https://www.python.org/ftp/python/3.10.11/python-3.10.11-amd64.exe)
2. 安装微信 `3.9.2.23`，下载地址在 [这里](https://github.com/lich0821/WeChatFerry/releases/latest)；也可以从 [WeChatSetup](https://gitee.com/lch0821/WeChatSetup) 找到。
3. 克隆项目

```shell
git clone https://github.com/lich0821/WeChatRobot.git

# 如果网络原因打不开，可以科学上网，或者使用gitee
git clone https://gitee.com/lch0821/WeChatRobot.git
```

4. 安装依赖

```shell
python.exe -m venv wechat

# 进入虚拟环境

# 升级 pip
python -m pip install -U pip
# 安装必要依赖
pip install -r requirements.txt
# ChatGLM 还需要安装一个 kernel
ipython kernel install --name chatglm3 --user
```

5. 运行

> 我们需要运行两次 `main.py` 第一次是生成配置文件 `config.yaml`, 第二次是真正跑你的机器人。

```shell
python main.py

# 需要停止按 Ctrl+C
```

6. 启动

```shell
python main.py
```

7. 查看其他配置

```shell
# 查看帮助
python main.py -h

# 例: 我想运行选择chatgpt的机器人
python main.py -c 2
```





### 2. 配置

> `config.yaml` 是配置项 配置ai模型

```shell
groups:
  enable: [] # 允许响应的群 roomId，大概长这样：2xxxxxxxxx3@chatroom, 多个群用 `,` 分隔
  
groups:
  enable: []  # 允许响应的群 roomId，大概长这样：2xxxxxxxxx3@chatroom

news:
  receivers: ['']  # 定时新闻接收人（roomid 或者 wxid）

report_reminder:
  receivers: ['']  # 定时日报周报月报提醒（roomid 或者 wxid）

```







### 3. 扩展

> `base.func_maren.py` 骂人服务

```python
import random

from chinese_calendar import is_workday
from robot import Robot

class MaRen:
    @staticmethod
    def ma(robot: Robot) -> None:

        receivers = robot.config.REPORT_REMINDERS
        if not receivers:
            receivers = ["filehelper"]
        #  骂人业务
        for receiver in receivers:

            arr = ['大傻春', '大傻柱', '白傻子']

            robot.sendTextMsg(random.choice(arr), receiver)

```

> `main.py`

```python
from base.func_maren import MaRen


# 四秒发送
robot.onEverySeconds(4, MaRen.ma, robot=robot)
```

> 哦耶哦耶。牛皮。哈哈







### 4. 其他接口

> 骂人： [fxkyou](https://my.wulvxinchen.cn/fxxkyou/)

```shell
    @staticmethod
    def fxxkyou(robot: Robot) -> None:
        receivers = robot.config.REPORT_REMINDERS
        if not receivers:
            receivers = ["filehelper"]
        #  骂人业务
        for receiver in receivers:

            # 骂人等级
            level = [ 'max', 'min']

            url = "https://my.wulvxinchen.cn/fxxkyou/api.php?level=" +  level[0]

            resp = requests.get(url)

            if resp.status_code == 200:
                robot.sendTextMsg(resp.text, receiver)

```

