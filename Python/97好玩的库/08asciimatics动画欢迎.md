<center>asciimatics艺术动画</center>





[toc]







## asciimatics艺术动画

> asciimatics艺术动画: [github](https://github.com/peterbrittain/asciimatics)一个跨平台包，用于执行类似诅咒的操作，加上更高级别的 API 和小部件来创建文本 UI 和 ASCII 艺术动画











### 1. 安装

> **终端样式**： [asciinema](https://asciinema.org/)

```shell
pip install asciimatics
```









### 2. 使用

> python hellloworld

```python
from random import randint
from asciimatics.screen import Screen

def demo(screen):
    while True:
        screen.print_at('Hello world!',
                        randint(0, screen.width), randint(0, screen.height),
                        colour=randint(0, screen.colours - 1),
                        bg=randint(0, screen.colours - 1))
        ev = screen.get_key()
        if ev in (ord('Q'), ord('q')):
            return
        screen.refresh()

Screen.wrapper(demo)
```



> 其他

```python
from asciimatics.effects import Cycle, Stars
from asciimatics.renderers import FigletText
from asciimatics.scene import Scene
from asciimatics.screen import Screen

def demo(screen):
    effects = [
        Cycle(
            screen,
            FigletText("ASCIIMATICS", font='big'),
            int(screen.height / 2 - 8)),
        Cycle(
            screen,
            FigletText("ROCKS!", font='big'),
            int(screen.height / 2 + 3)),
        Stars(screen, 200)
    ]
    screen.play([Scene(effects, 500)])

Screen.wrapper(demo)
```

