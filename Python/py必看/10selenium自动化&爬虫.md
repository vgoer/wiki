<center>selenium自动化&爬虫</center>





[toc]





## selenium自动化&爬虫

> Selenium [doc](https://www.selenium.dev/documentation/) [中文](https://selenium-python-zh.readthedocs.io/en/latest/index.html) 是一系列工具和库的总括项目，可实现和支持 Web 浏览器的自动化。





### 1. 安装

> 下载 webdriver: [web](https://chromedriver.chromium.org/downloads) [blog](https://blog.csdn.net/Z_Lisa/article/details/133307151)

```shell
pip install selenium

# 项目中 需要将它添加到 requirements.txt 文件中:
selenium==4.17.2
```





### 2. 使用

```python
from selenium import webdriver
import time


# 打开google  Chrome  Firefox  Edge
driver = webdriver.Chrome()

# 打开标签
driver.get("https://www.youtube.com/")

# 标题
# print(driver.title)
search = driver.find_element("name", "search")
search.send_keys("hack")

# 退出
# driver.quit()


time.sleep(5)
```

