<center>Beautiful库</center>





[toc]







## Beautiful库

> [Beautiful Soup](https://beautiful-soup-4.readthedocs.io/en/latest/)是一个用于从 HTML 和 XML 文件中提取数据的 Python 库。







### 1. 安装

> 安装

```shell
python3 -m venv venv

# 安装
pip install beautifulsoup4
```







### 2. 网站的取图片的

> [图片] 可以自己改造

```python
import sys, os , time
import urllib.request, urllib.error, urllib.parse
import pprint as pprint
from bs4 import BeautifulSoup


# 代理头
userAgent = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"


def download_img(url, dst_path):
    try:
        req = urllib.request.Request(url, headers={'User-Agent': userAgent})
        res = urllib.request.urlopen(req)
        img_data = res.read()
        with open(dst_path, mode='wb') as f:
            f.write(img_data)
    except urllib.error.URLError as ex:
        print(ex)

    if(len(sys.argv) < 2):
        print("\n", "请使用：py main.py [url]")
        exit(0)


# 获取url
url = sys.argv[1]

# url请求
req = urllib.request.Request(url, headers={'User-Agent': userAgent})
html = urllib.request.urlopen(req)

# 解析获取的html
soup = BeautifulSoup(html,"html.parser")
# print(soup.html)
# print(soup.text)

# 取处所有图片标记 数组
img_list = soup.find_all("img")
print(img_list)


# 整理url为 完全的格式
url_list = []
for img in img_list:
    if img.get("src") == None:
        continue
    tmp = img["src"].strip()
    if tmp.startswith("https://") or tmp.startswith("http://"):
        pass
    else:
        tmp = urllib.parse.urljoin(url, tmp)
    
    if not tmp in url_list:
        url_list.append(tmp)

# pprint.pprint(url_list)


# 建立下载文件
download_dir = "./out"
if not os.path.exists(download_dir):
    os.mkdir(download_dir)

# 下载处理
digits_width = len(str(len(url_list)))
count = 0
for url in url_list:
    count = count + 1

    # 文件名称
    fmt_url = url
    if fmt_url.find("?") > -1:
        fmt_url = fmt_url[0: fmt_url.find("?")]
    
    file, ext = os.path.splitext(fmt_url)
    filename = str(count).zfill(digits_width) + ext;

    if len(ext) == 0:
        filename = filename + ".png"
    
    dst_path = os.path.join(download_dir,filename)

    print(url)
    print("", "->", dst_path)

    # time.sleep(0.1)
    download_img(url, dst_path)
```

