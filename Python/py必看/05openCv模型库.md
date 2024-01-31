<center>openCv模型库</center>





[toc]







## openCv模型库

> [openCv](https://github.com/opencv/opencv) 开源计算机视觉库







### 1. 安装

> 安装

```shell
pip install opencv-python
```

> 自带的模型： `venv/Lib/cv2/data` 下







### 2. 使用

> 卧槽，牛皮。如何实现的。这功能。只能感叹

```python
import cv2


# 脸部模型 
cascade_path = "./moduels/haarcascade_frontalface_default.xml"
# cascade_path = "./moduels/haarcascade_frontalface_alt_tree.xml"
# cascade_path = "./moduels/haarcascade_frontalface_alt.xml"



# 眼睛模型
# eye_cascade_path = "./moduels/haarcascade_eye.xml"
eye_cascade_path = "./moduels/haarcascade_eye_tree_eyeglasses.xml"


def run(imgPath=""):
    outputPath = "./output.jpg"

    # 图片文件获取
    image = cv2.imread(imgPath)

    # 灰度化
    image_gray = cv2.cvtColor(image,cv2.COLOR_BGR2GRAY)

    # 分类器获取
    cascade = cv2.CascadeClassifier(cascade_path)
    # 脸部检测
    facerect = cascade.detectMultiScale(image_gray, scaleFactor=1.1, minNeighbors=2, minSize=(30,30))

    if len(facerect) > 0:
        # 画出脸
        for rect in facerect:
            cv2.rectangle(image, tuple(rect[0:2]), tuple(rect[0:2]+rect[2:4]), (0,255, 0), thickness=3)


        # 眼睛检测
        eye_cascade = cv2.CascadeClassifier(eye_cascade_path)
        # 眼睛检测
        eyes = eye_cascade.detectMultiScale(image_gray)
        for (ex, ey, ew, eh) in eyes:
            cv2.rectangle(image, (ex,ey), (ex+ew, ey + eh), (0, 255,0), 2)

        
        #保存
        cv2.imwrite(outputPath, image)
        print("找到脸了")
    else:
        print("对不起，没找到脸部")


run(imgPath="./face2.jpeg")

```

