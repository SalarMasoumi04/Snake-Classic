# ایمپورت کتابخانه‌ها
import random
from OpenGL.GL import *
from OpenGL.GLU import *
from OpenGL.GLUT import *

# اندازه شبکه (تعداد خانه‌ها) و سرعت بازی بر حسب میلی‌ثانیه
GRID = 20
SPEED = 130

# وضعیت بازی
snake = [(10, 10), (9, 10), (8, 10)]   # لیست خانه‌های مار (اولی سر است)
direction = (1, 0)                      # جهت حرکت فعلی
next_direction = direction              # جهت درخواستی بازیکن
food = (15, 10)                         # مختصات غذا
game_over = False

# ساخت غذا در خانه‌ای که روی مار نباشد
def new_food():
    while True:
        pos = (random.randint(0, GRID - 1), random.randint(0, GRID - 1))
        if pos not in snake:
            return pos

# رسم یک خانه (مربع) در مختصات شبکه
def draw_cell(x, y):
    glBegin(GL_QUADS)
    glVertex2f(x + 0.05, y + 0.05)
    glVertex2f(x + 0.95, y + 0.05)
    glVertex2f(x + 0.95, y + 0.95)
    glVertex2f(x + 0.05, y + 0.95)
    glEnd()

# تنظیمات اولیه
def init():
    glClearColor(0.1, 0.1, 0.1, 1)
    glMatrixMode(GL_PROJECTION)
    glLoadIdentity()
    # هر خانه شبکه یک واحد است
    gluOrtho2D(0, GRID, 0, GRID)

# تابع نمایش
def display():
    glClear(GL_COLOR_BUFFER_BIT)
    # غذا قرمز
    glColor3f(1, 0, 0)
    draw_cell(*food)
    # سر مار سبز روشن و بدن سبز تیره
    for i, (x, y) in enumerate(snake):
        glColor3f(0, 1, 0) if i == 0 else glColor3f(0, 0.6, 0)
        draw_cell(x, y)
    glFlush()

# حرکت مار؛ هر SPEED میلی‌ثانیه اجرا می‌شود
def update(value):
    global snake, direction, food, game_over
    if not game_over:
        direction = next_direction
        head = (snake[0][0] + direction[0], snake[0][1] + direction[1])
        # برخورد با دیوار یا بدن
        if not (0 <= head[0] < GRID and 0 <= head[1] < GRID) or head in snake:
            game_over = True
            print("Game Over!! Your Score: ", len(snake) - 3)
        else:
            snake.insert(0, head)
            if head == food:
                food = new_food()   # غذا خورده شد، دم حذف نمی‌شود
            else:
                snake.pop()         # حرکت عادی، دم حذف می‌شود
    glutPostRedisplay()
    glutTimerFunc(SPEED, update, 0)

# کلیدهای جهت‌دار (مار نمی‌تواند مستقیم برعکس برگردد)
def keys(key, x, y):
    global next_direction
    if key == GLUT_KEY_UP and direction != (0, -1):
        next_direction = (0, 1)
    elif key == GLUT_KEY_DOWN and direction != (0, 1):
        next_direction = (0, -1)
    elif key == GLUT_KEY_LEFT and direction != (1, 0):
        next_direction = (-1, 0)
    elif key == GLUT_KEY_RIGHT and direction != (-1, 0):
        next_direction = (1, 0)

glutInit()
glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB)
glutInitWindowSize(500, 500)
glutCreateWindow(b"Snake")
init()
glutDisplayFunc(display)
glutSpecialFunc(keys)
glutTimerFunc(SPEED, update, 0)
glutMainLoop()
