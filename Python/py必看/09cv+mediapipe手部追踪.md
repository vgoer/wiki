<center>cv+mediapipe手部追踪</center>



[toc]









## cv+mediapipe手部追踪

> 手部追踪： [youtube](https://www.youtube.com/watch?v=x4eeX7WJIuA)







### 1. 安装

> 一定使用虚拟环境

```python
pip install opencv-python

pip install mediapipe
```





### 2. 使用

> code

```python
import cv2
import mediapipe as mp
import time


# 调用默认摄像头
cap = cv2.VideoCapture(0)
# 如果希望调用其他摄像头，可以尝试不同的索引号
# cap = cv2.VideoCapture(1)

# 使用mp模型 手部模型
mpHands = mp.solutions.hands
hands = mpHands.Hands()
# 画
mpDraw = mp.solutions.drawing_utils
# 点线样式
handLmsStyle = mpDraw.DrawingSpec(color=(0,0,255), thickness=5)
handConStyle = mpDraw.DrawingSpec(color=(0,255,0), thickness=2)


# fps
cTime = 0
Ptime = 0

while True:
    ret, img = cap.read()  # 读取视频帧

    if ret:

        imgRGB = cv2.cvtColor(img, cv2.COLOR_RGB2BGR)
        result = hands.process(imgRGB)

        # print(result.multi_hand_landmarks)

        # 视窗宽高
        imgHeight = img.shape[0]
        imgWidth = img.shape[1]

        if result.multi_hand_landmarks:
            # 所有的手
            for handLms in result.multi_hand_landmarks:
                # 写入点和线
                mpDraw.draw_landmarks(img, handLms, mpHands.HAND_CONNECTIONS,handLmsStyle, handConStyle)

                # 21个 坐标
                for i,lm in enumerate(handLms.landmark):
                    # print(i, lm.x, lm.y)
                    xPos = int(lm.x * imgWidth)
                    yPos = int(lm.y * imgHeight)

                    # 坐标点
                    cv2.putText(img,str(i), (xPos-25, yPos+5), cv2.FONT_HERSHEY_SIMPLEX,0.4, (0,0,255), 2)

                    # 单独标出 大拇指
                    if i==4:
                        cv2.circle(img, (xPos,yPos), 10, (0,0,255), cv2.FILLED)

                    print(i, xPos, yPos)

        cTime = time.time()
        fps = 1/(cTime-Ptime)
        Ptime = cTime
        cv2.putText(img, f"FPS: {int(fps)}", (20,50), cv2.FONT_HERSHEY_SIMPLEX, 1, (255,0,0))

        cv2.imshow('Camera', img)  # 在窗口中显示视频帧

    if cv2.waitKey(1) & 0xFF == ord('q'):  # 按下'q'键退出循环
        break

cap.release()  # 释放摄像头资源
cv2.destroyAllWindows()  # 关闭所有窗口
```

