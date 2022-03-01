# ball-odraz
import time
import tkinter
from random import choice, randint


WIDTH = 800
HEIGHT = 600
BACKGROUND = 'white'
BALL_SIZE = 40
BALL_COLOR = "RED"
BALL_DIR = {'x': -1, 'y': 1}
BALL_DIR2 = {'x': -1, 'y': 1}
BALL_SHIFT = 10
REFRESH = 20
INITIAL_COORDS = {'x': WIDTH//2, 'y': HEIGHT//2}
INITIAL_COORDS2 = {'x': WIDTH//2, 'y': HEIGHT//2}
COUNT = 1

BALL_COLORS= ["red", "blue", "yellow", "green", "purple", "pink"]

canvas = tkinter.Canvas(width=WIDTH, height=HEIGHT, background=BACKGROUND)
canvas.pack()


def uncenter(x: int, y: int) -> list[int]:
    global BALL_SIZE
    return [x-BALL_SIZE//2,
            y-BALL_SIZE//2,
            x+BALL_SIZE//2,
            y+BALL_SIZE//2]


def center(corner_coords: list) -> list[int]:
    return [corner_coords[0] + abs(corner_coords[0]-corner_coords[2])//2,
            corner_coords[1] + abs(corner_coords[1]-corner_coords[3])//2]


def animation():
    global WIDTH, BALL_COLOR, coords, COUNT, BALL_SIZE, BALL_COLORS, coords2
    coords = center(canvas.coords('BALL'))
    coords2 = center(canvas.coords('BALL2'))
    # x
    coords[0] += BALL_SHIFT*BALL_DIR['x']
    coords2[0] += BALL_SHIFT*BALL_DIR2['x']
    # y
    coords[1] += BALL_SHIFT*BALL_DIR['y']
    coords2[1] += BALL_SHIFT*BALL_DIR2['y']
    coords = uncenter(coords[0], coords[1])
    coords2[1] += BALL_SHIFT*BALL_DIR2['y']
    coords2 = uncenter(coords2[0], coords2[1])
    if coords[0] <= 0:
        BALL_DIR['x'] = 1
        BALL_COLOR=choice(BALL_COLORS)

    if coords[1] <= 0:
        BALL_DIR['y'] = 1
        BALL_COLOR=choice(BALL_COLORS)

    if coords[2] >= WIDTH:
        BALL_DIR['x'] = -1
        BALL_COLOR=choice(BALL_COLORS)

    if coords[3] >= HEIGHT:
        BALL_DIR['y'] = -1
        BALL_COLOR=choice(BALL_COLORS)

    if coords2[0] <= 0:
        BALL_DIR2['x'] = 1
        BALL_COLOR=choice(BALL_COLORS)

    if coords2[1] <= 0:
        BALL_DIR2['y'] = 1
        BALL_COLOR=choice(BALL_COLORS)

    if coords2[2] >= WIDTH:
        BALL_DIR2['x'] = -1
        BALL_COLOR=choice(BALL_COLORS)

    if coords2[3] >= HEIGHT:
        BALL_DIR2['y'] = -1
        BALL_COLOR=choice(BALL_COLORS)

    canvas.delete('BALL')
    canvas.delete('BALL2')
    canvas.create_oval(coords[0], coords[1], coords[2], coords[3], fill=BALL_COLOR, tags='BALL')
    canvas.create_oval(coords2[0], coords2[1], coords2[2], coords2[3], fill=BALL_COLOR, tags='BALL2')
    canvas.after(REFRESH, animation)


coords = uncenter(INITIAL_COORDS['x'], INITIAL_COORDS['y'])
canvas.create_oval(coords[0], coords[1], coords[2], coords[3], fill=BALL_COLOR, tags='BALL')
coords2 = uncenter(INITIAL_COORDS2['x'], INITIAL_COORDS2['y'])
canvas.create_oval(coords2[0], coords2[1], coords2[2], coords2[3], fill=BALL_COLOR, tags='BALL2')

animation()
canvas.mainloop()
