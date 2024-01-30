<center>autoGui库</center>





[toc]







## AtuoGui库

> auto: [doc](https://pyautogui.readthedocs.io/en)PyAutoGUI 让您的 Python 脚本控制鼠标和键盘，以自动与其他应用程序交互。







### 1. 安装

> 安装

```shell
python3 -m venv venv

# 3. 进入虚拟环境
# On Windows:
.\venv\Scripts\activate
# On macOS and Linux:
source venv/bin/activate

#4. 安装依赖
pip install -r requirements.txt

#5. 升级pip
python.exe -m pip install --upgrade pip

#6. 安装auto
pip install pyautogui
```





### 2. 获取基本信息

> 屏幕，鼠标位置等

```python
import pyautogui

# 屏幕大小
size = pyautogui.size()
print(size)


# 鼠标位置
mouse_pos = pyautogui.position()
print(mouse_pos)

# 判断点是否在屏幕内
print(pyautogui.onScreen(100, 200))
```



> 鼠标移动

```python
import pyautogui

# 屏幕大小
size = pyautogui.size()

# 鼠标移动到 0 0 位置， 周期 1秒
pyautogui.moveTo(0,0,duration=1)


# 把鼠标移动到画面中央， 周期0.5秒
pyautogui.moveTo(size.width/2, size.height/2, duration=0.5)

# 鼠标相对位置移动，周期1秒
pyautogui.moveRel(100, None, duration=1)
```

> 鼠标实时位置

```python
import pyautogui

last_pos = pyautogui.position()

try:
    while True:
        new_pos = pyautogui.position()
        if last_pos != new_pos:
            print(new_pos)
            last_pos = new_pos
            
except KeyboardInterrupt:
    print("\nExit.;")
```

> 鼠标移动，点击，拖动

```python
pip install opencv-python
# 需要截图
```

```python
import pyautogui
import time
# 系统准备时间
time.sleep(1)

# 获取图标位置   confidence 相似程度。
try:
    we_pos = pyautogui.locateOnScreen('we.png', confidence=0.8)
    print('Image found at:', we_pos)
except pyautogui.ImageNotFoundException:
    print('Image not found')

goto_pos   = pyautogui.center(we_pos)

# 移动鼠标
pyautogui.moveTo(goto_pos, duration=1)

# 点击
pyautogui.click()

# 相对移动
pyautogui.moveRel(-100,None, duration=1)

# 点击
pyautogui.click()
```



> 键盘输入： 重点

```python

import pyautogui
import time

time.sleep(2)


# 点击左键
pyautogui.click(button="left")

# 输入
pyautogui.typewrite(" i like python !")

# 写 一个字符停顿时间
pyautogui.typewrite("\n I LIKE PYTHON TOO.", 0.20)

# 修改字符 good  Good.
pyautogui.typewrite(['enter', 'g', 'o', 'o', 'd','left','left','left','backspace','G','end','.'],0.25)
```





> 快捷键：重点

```python
import pyautogui
import time


time.sleep(2)

# 每个动作间隔 0.5
pyautogui.PAUSE = 0.5
# 异常退出
# pyautogui.FAILSAFE = True

# 记事本打出时间
pyautogui.press('f5')

# 打出内容
pyautogui.typewrite("\n hello")
pyautogui.typewrite("\n hello")
pyautogui.typewrite("\n hello")


# 按下ctrl键
pyautogui.keyDown('ctrl')

# ctrl + a 全选
pyautogui.press('a')

# c
pyautogui.press('c')

# 松开ctrl
pyautogui.keyUp('ctrl')

# 点击下方
pyautogui.click(800,800)

# 输入空行
pyautogui.typewrite("\n\n")


# 粘贴
pyautogui.hotkey('ctrl', 'v')
```





