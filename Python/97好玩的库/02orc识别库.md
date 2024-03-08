<center>验证码识别库</center>





[toc]







## 验证码处理

> 验证码识别库





### 1. ddddocr

> ddddocr: [gihtub](https://github.com/sml2h3/ddddocr)



* 安装

```shell
pip install ddddocr
```

* orc识别部分

```python
import ddddocr


# beta 模型 更好识别
ocr = ddddocr.DdddOcr(beta=True)
# ocr = ddddocr.DdddOcr()

with open("test.jpg", 'rb') as f:
    image = f.read()

res = ocr.classification(image)
print(res)
```

* 目标检测部分(难点)

```shell
pip install opencv-python
```

```python
import ddddocr
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    # 启动 chromium 浏览器
    browser = p.chromium.launch(headless=False,slow_mo=1000)
    # 打开一个标签页
    page = browser.new_page()
    # 打开百度
    page.goto("http://127.0.0.1:8008/#/login")
    # 打印当前页面title
    print(page.title())
    #定位验证码，截图验证码
    page.locator(".s-canvas").screenshot(path="example.png")
    #定位账号输入框，输入账号
    page.get_by_placeholder("账号").fill("admin")
    #定位密码输入框，输入密码
    page.get_by_placeholder("密码").fill("Admin@123")
    #实例化验证码识别的库
    ocr = ddddocr.DdddOcr()
    #打开截图的验证码
    with open('example.png', 'rb') as f:
        img_bytes = f.read()
    result = ocr.classification(img_bytes)
    print(result)
    #输入验证码
    page.get_by_placeholder("验证码").fill(result)
    #定位登录，点击登录
    page.get_by_role("button", name="登录").click()
    #登录后截图
    page.screenshot(path="example2.png")
    #关闭浏览器
    browser.close()

```

