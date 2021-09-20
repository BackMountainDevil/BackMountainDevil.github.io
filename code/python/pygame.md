# Pygame
- date: 2020-11-19
- lastmod: 2020-11-19

# Install
```bash
$ python3 -m pip install -U pygame --user
# 检测是否正确安装，正确安装的话会运行一个游戏
$ python3 -m pygame.examples.aliens
```
## Basic
```bash
import pygame

width = 920  # 画板宽度
height = 640  # 画版高度

pygame.init()
screen = pygame.display.set_mode((width, height))

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            return 0
    pygame.display.update()
sys.exit()
```
# Output
## 
## image

## music

# Input
## keyboard
                 

## mouse
获取位置和鼠标按键状态
```python
if event.type == pygame.MOUSEBUTTONDOWN:
            print("MOUSE CLicked! position: ", event.pos, "mouse state:",
                  pygame.mouse.get_pressed())
``` 
# References
- [GettingStarted — wiki](https://www.pygame.org/wiki/GettingStarted)
- [Pygame Doc](https://www.pygame.org/docs/)
- [手把手教你使用Pygame制作飞机大战小游戏，4万字超详细讲解！](https://cloud.tencent.com/developer/article/1651631)
- [从零开发一个小游戏：PyGame 入门](https://codingpy.com/article/pygame-a-primer-by-real-python/)
- [用Python和Pygame写游戏-从入门到精通（目录）](https://eyehere.net/2011/python-pygame-novice-professional-index/)
- []()
- []()