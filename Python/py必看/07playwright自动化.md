<center>playwright自动化</center>





[toc]







## playwright自动化

> Playwright 是专门为了满足端到端测试的需求而创建的。[py](https://playwright.dev/python/docs/intro)







### 1. 安装

> 安装

```shell
pip install pytest-playwright

# 下载 浏览器
playwright install
```







### 2. 使用

> 截图

```python
from playwright.sync_api import sync_playwright


# 打开浏览器，截图
with sync_playwright() as p:
    # 无头模式
    browser = p.webkit.launch()
    # 有头模式
    # browser = p.webkit.launch(headless=False, slow_mo = 100)

    page = browser.new_page()

    page.goto("https://www.baidu.com")

    # 修正 path 参数为关键字参数
    page.screenshot(path="pyout.png")
    
    browser.close()
```

> 定制 webview

```python
# 定制viewport
with sync_playwright() as p:
    browser = p.webkit.launch(headless=False, slow_mo = 100)
    context = browser.new_context(
        viewport = {"width": 700, "height": 1300}
    )

    page = context.new_page()

    page.goto("https://baidu.com")
    # 修正 path 参数为关键字参数
    page.screenshot(path="pyout.png")

    browser.close()
```

> 使用其他浏览器：

```python
chromium
webkit
firefox
```



> wc，可以直接生成代码。  执行这个命令。牛皮。 卧槽。
>
> [文档](https://playwright.dev/python/docs/codegen)

```python
python.exe -m playwright codegen <url>

# 模拟 ip13
playwright codegen --device="iPhone 13" playwright.dev

# 模拟地理位置和语言
playwright codegen --timezone="Europe/Rome" --geolocation="41.890221,12.492348" --lang="it-IT" bing.com/maps
```

```shell
wc
wc
wc
wc
牛皮。
```





