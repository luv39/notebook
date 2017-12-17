# hellopygame
```python
#!/usr/bin/env python

import pygame, sys
from pygame.locals import *

pygame.init()
displaysurf = pygame.display.set_mode((400, 300))
pygame.display.set_caption("hello world")
while True:  # main game loop
    for event in pygame.event.get():
        if event.type == QUIT:
            pygame.quit()
            sys.exit()
    pygame.display.update()
```
* 初始化pygame `pygame.init()`
* 设置窗口宽度和高度 `pygame.display.set_mode((400, 300))`, 传入一个元组，将会创建一个宽400，高300的窗口，返回一个`pygame.Surface`对象。
* 设置窗口顶部显示的标题文本 `pygame.display.set_caption("hello world")`
* main game loop:1.处理事件 2.更新游戏状态 3.在屏幕上绘制游戏状态
* 获得事件 `pygame.event.get()`
* 

