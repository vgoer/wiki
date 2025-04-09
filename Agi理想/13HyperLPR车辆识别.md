<center>HyperLPR车辆识别</center>





[toc]







## HyperLPR车辆识别

> HyperLPR 高性能开源中文车牌识别框架 
>
> [gitee](https://gitee.com/zeusees/HyperLPR) [github](https://github.com/szad670401/HyperLPR)
>
> 







### 1. 安装

```shell
python -m pip install hyperlpr3


# 快速测试
# image url
lpr3 sample -src https://koss.iyong.com/swift/v1/iyong_public/iyong_2596631159095872/image/20190221/1550713902741045679.jpg

# image path
lpr3 sample -src images/test_img.jpg -det high
```

> 代码使用

```python
# import opencv
import cv2
# import hyperlpr3
import hyperlpr3 as lpr3

# Instantiate object
catcher = lpr3.LicensePlateCatcher()
# load image
image = cv2.imread("images/test_img.jpg")
# print result
print(catcher(image))
```

> 启动web服务

```shell
# start server
lpr3 rest --port 8715 --host 0.0.0.0
```

> 写入到图片中
>
> 字体：[ttf](https://fonts.google.com/selection?lang=zh_Hans)

```python
# coding:utf-8
import hyperlpr3 as lpr3
import cv2
from PIL import Image, ImageDraw, ImageFont
import numpy as np


def drawRectBox(image, rect, addText, fontC):
    x1, y1, x2, y2 = map(int, (rect[0], rect[1], rect[2], rect[3]))

    # 绘制车牌方框
    cv2.rectangle(image, (x1, y1), (x2, y2), (0, 0, 255), 2)

    # 计算文字背景位置
    text_bg_x1 = max(x1 - 1, 0)
    text_bg_y1 = max(y1 - 25, 0)
    text_bg_x2 = min(x1 + 150, image.shape[1])  # 适当加宽背景

    # 方案一：使用getbbox（推荐）
    bbox = fontC.getbbox(addText)
    text_width = bbox[2] - bbox[0]
    text_height = bbox[3] - bbox[1]

    # 方案二：使用textlength（需要指定方向）
    # text_width = int(fontC.getlength(addText, mode="L"))  # LTR水平方向

    # 文字居中计算
    text_x = x1 + (150 - text_width) // 2 if (x1 + 150) < image.shape[1] else x1

    # 绘制半透明背景
    overlay = image.copy()
    cv2.rectangle(overlay, (text_bg_x1, text_bg_y1),
                  (text_bg_x2, y1), (0, 0, 255), -1)
    cv2.addWeighted(overlay, 0.6, image, 0.4, 0, image)

    # 转换格式绘制文字
    img_pil = Image.fromarray(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
    draw = ImageDraw.Draw(img_pil)
    draw.text((text_x, text_bg_y1 + (25 - text_height) // 2),  # 垂直居中
              addText, (255, 255, 255), font=fontC)

    return cv2.cvtColor(np.array(img_pil), cv2.COLOR_RGB2BGR)


# 读取图片并转换色彩空间
image = cv2.imread('car.png')
if image is None:
    raise FileNotFoundError("图片文件未找到或无法读取")

# 初始化识别器
catcher = lpr3.LicensePlateCatcher(detect_level=lpr3.DETECT_LEVEL_HIGH)

# 获取识别结果
results = catcher(image)

# 加载字体（添加备用字体处理）
try:
    fontC = ImageFont.truetype("NotoSans.ttf", 20)
except:
    fontC = ImageFont.load_default()
    print("警告：指定字体未找到，使用默认字体")

# 遍历所有识别结果
for result in results:
    license_plate, confidence, _, bounding_box = result
    print(f"识别到车牌: {license_plate}, 置信度: {confidence:.2f}")
    # image = drawRectBox(image, bounding_box, f"{license_plate} {confidence:.2f}", fontC)
    image = drawRectBox(image, bounding_box, f"{license_plate}", fontC)

# 显示结果（添加窗口自适应）
cv2.namedWindow('RecognitionResult', cv2.WINDOW_NORMAL)
cv2.imshow('RecognitionResult', image)
cv2.resizeWindow('RecognitionResult', 1024, int(1024 * image.shape[0] / image.shape[1]))
cv2.waitKey(0)
cv2.destroyAllWindows()
```

> 优化： 预处理增强，字符矫正等功能。

```python
# coding:utf-8
import hyperlpr3 as lpr3
import cv2
import numpy as np
from PIL import Image, ImageDraw, ImageFont


# --------------------- 预处理函数（修复通道问题）---------------------
def preprocess_image(image):
    # 保持原始彩色通道结构
    hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)

    # 仅对亮度通道进行增强
    h, s, v = cv2.split(hsv)
    clahe = cv2.createCLAHE(clipLimit=3.0, tileGridSize=(8, 8))
    v = clahe.apply(v)

    # 合并通道并转回BGR
    enhanced_hsv = cv2.merge([h, s, v])
    enhanced = cv2.cvtColor(enhanced_hsv, cv2.COLOR_HSV2BGR)

    # 高斯模糊去噪（保持3通道）
    blurred = cv2.GaussianBlur(enhanced, (5, 5), 0)

    # 边缘检测优化（保持3通道）
    gray = cv2.cvtColor(blurred, cv2.COLOR_BGR2GRAY)
    edges = cv2.Canny(gray, 50, 150)
    kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (3, 3))
    closed = cv2.morphologyEx(edges, cv2.MORPH_CLOSE, kernel)

    # 将单通道转换为3通道
    return cv2.cvtColor(closed, cv2.COLOR_GRAY2BGR)


# ------------------- 字符校正函数 --------------------
def correct_license_plate(text):
    confusion_map = {"S": "5", "5": "S", "B": "8", "8": "B", "0": "O", "O": "0"}
    return "".join([confusion_map.get(c, c) for c in text])


# ------------------- 绘制优化函数 --------------------
def drawRectBox(image, rect, addText, fontC):
    x1, y1, x2, y2 = map(int, rect)
    cv2.rectangle(image, (x1, y1), (x2, y2), (0, 0, 255), 2)

    # 计算文字区域
    text_bg_x1 = max(x1 - 1, 0)
    text_bg_y1 = max(y1 - 25, 0)
    text_bg_x2 = min(x1 + 150, image.shape[1])

    # 半透明背景
    overlay = image.copy()
    cv2.rectangle(overlay, (text_bg_x1, text_bg_y1),
                  (text_bg_x2, y1), (0, 0, 255), -1)
    cv2.addWeighted(overlay, 0.6, image, 0.4, 0, image)

    # 文字绘制
    img_pil = Image.fromarray(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
    draw = ImageDraw.Draw(img_pil)
    bbox = fontC.getbbox(addText)
    text_w = bbox[2] - bbox[0]
    text_x = x1 + (150 - text_w) // 2 if (x1 + 150) < image.shape[1] else x1
    draw.text((text_x, text_bg_y1 + 2), addText, (255, 255, 255), font=fontC)

    return cv2.cvtColor(np.array(img_pil), cv2.COLOR_RGB2BGR)


# ------------------- 主流程 --------------------
# 读取图片
image = cv2.imread('car.png')

# 预处理（输出3通道图像）
processed = preprocess_image(image)

# 初始化识别器（兼容最新API）
catcher = lpr3.LicensePlateCatcher()  # 使用默认参数

# 识别处理（传递原始彩色图像）
results = catcher(image)  # 注意这里改为使用原始图像

# 加载字体
try:
    fontC = ImageFont.truetype("NotoSans.ttf", 20)
except:
    fontC = ImageFont.load_default()

# 过滤并绘制结果
MIN_CONFIDENCE = 0.75
for result in results:
    license, conf, _, box = result
    if conf < MIN_CONFIDENCE:
        continue

    license = correct_license_plate(license)
    print(f"识别到车牌: {license}, 置信度: {conf:.2f}")
    image = drawRectBox(image, box, f"{license}", fontC)

# 显示结果
cv2.namedWindow('Result', cv2.WINDOW_NORMAL)
cv2.imshow('Result', image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

> 但是，在生活中，我们更多用到的是从视频中对车牌进行识别，因此我们只需要对视频的每一帧图片进行车牌识别检测，然后将检测信息绘制上去即可，核心代码如下：

```python
# coding:utf-8
# 导入包
import hyperlpr3 as lpr3
# 导入OpenCV库
import cv2
from PIL import Image, ImageDraw, ImageFont
import numpy as np

# 定义画图函数
def drawRectBox(image, rect, addText, fontC):
    """
    车牌识别，绘制矩形框与结果
    :param image: 原始图像
    :param rect: 矩形框坐标
    :param addText:车牌号
    :param fontC: 字体
    :return:
    """
    # 绘制车牌位置方框
    cv2.rectangle(image, (int(round(rect[0])), int(round(rect[1]))),
                  (int(round(rect[2]) + 15), int(round(rect[3]) + 15)),
                  (0, 0, 255), 2)
    # 绘制字体背景框
    cv2.rectangle(image, (int(rect[0] - 1), int(rect[1]) - 25), (int(rect[0] + 120), int(rect[1])), (0, 0, 255), -1,
                  cv2.LINE_AA)
    img = Image.fromarray(image)
    draw = ImageDraw.Draw(img)
    draw.text((int(rect[0] + 1), int(rect[1] - 25)), addText, (255, 255, 255), font=fontC)
    imagex = np.array(img)
    return imagex


# 读取摄像头
cap = cv2.VideoCapture(0, cv2.CAP_DSHOW)
# 车牌标注的字体
fontC = ImageFont.truetype("NotoSans.ttf", 20, 0)

while True:
    ref, frame = cap.read()
    if ref:
        # 识别车牌
        catcher = lpr3.LicensePlateCatcher()
        all_res = catcher(frame)
        if len(all_res) > 0:
            lisence, conf,_, boxes = all_res[0]
            frame = drawRectBox(frame, boxes, lisence, fontC)
        cv2.imshow("RecognitionResult", frame)
        if cv2.waitKey(10) & 0xFF == ord('q'):
            break  # 退出
    else:
        break

```

