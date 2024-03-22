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





### 3. 页面元素定位

> 提供多种定位策略

```shell
from selenium.webdriver.common.by import By

# 定位元素并点击
element = driver.find_element(by=By.ID, value='myId')
element.click()

# 获取元素文本
text = driver.find_element(by=By.ID, value='myId').text

# name
input_field = driver.find_element(by=By.NAME, value='myInput')
```





### 4. 元素交互

> 交互

```shell
# 键盘发送
element.send_keys('Hello, Selenium!')
```

```shell
# 点击
element.click()
```

```shell
# 清楚元素内容
element.clear()
```

```shell
# 获取文本
text = element.text
```

```shell
# 获取元素属性
attribute_value = element.get_attribute('href')
```





### 5. 浏览器操作

> 浏览器

```shell
# 刷新页面
driver.refresh()

# 回退一页
driver.back()

# 前进一页
driver.forward()

# 切换标签页
driver.switch_to.window(handle)

# 执行JavaScript
driver.execute_script(script, args)

# 关闭浏览器
driver.quit()
```

