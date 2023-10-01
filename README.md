# hello
import pygame
import time
from random import randint

pygame.init()
window = pygame.display.set_mode((500, 500))
back = (140, 140, 200)
win_fonk = (94, 235, 164)

clock = pygame.time.Clock()

class Area():
    def __init__(self, x=0, y=0, width=10, height=10, color=back):
        self.rect = pygame.Rect(x, y, width, height)
        self.fill_color = color
    def colliderect(self, rect):
        return self.rect.colliderect(rect)
    def color(self, new_color):
        self.fill_color = new_color

    def fill(self):
        pygame.draw.rect(window, self.fill_color, self.rect)

    def outline(self, frame_color, thickness):
        pygame.draw.rect(window, frame_color, self.rect, thickness)

    def collidepoint(self, x, y):
        return self.rect.collidepoint(x, y)
    
class Picture(Area):
    def __init__(self, filename, x=0, y=0, width=0, height=0):
        Area.__init__(self, x=x, y=y, width=width, height=height)
        self.image = pygame.image.load(filename)

    def draw(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Label(Area):
    def set_text(self, text, fsize=15, text_color=(0, 0, 0)):
        self.image = pygame.font.SysFont("Arial", fsize).render(text, True, text_color)
    def draw(self, shift_x=0, shift_y=0):
        self.fill()
        window.blit(self.image, (self.rect.x + shift_x, self.rect.y + shift_y))


enemy_0 = Area(x=randint(10, 490), y=randint(10, 490), width=10, height=10, color=(255, 0, 0))
enemy_1 = Area(x=randint(10, 490), y=randint(10, 490), width=10, height=10, color=(255, 0, 0))
enemy_2 = Area(x=randint(10, 490), y=randint(10, 490), width=10, height=10, color=(255, 0, 0))
enemy_3 = Area(x=randint(10, 490), y=randint(10, 490), width=10, height=10, color=(255, 0, 0))
enemy_4 = Area(x=randint(10, 490), y=randint(10, 490), width=10, height=10, color=(255, 0, 0))
enemies = [enemy_0, enemy_1, enemy_2, enemy_3, enemy_4]

goal_area = Area(x=randint(100, 400), y=randint(100, 400), width=100, height=100, color=(72, 232, 195))
ball = Picture("теребонька.png", x=randint(60, 440), y=randint(40, 460), width=100, height=80)
i = 0
points = Label(x=130, y=0, width=0, height=0)
points.set_text(text=i, fsize=50, text_color=(168, 235, 52))

score = Label(x=0, y=0, width=0, height=0)
score.set_text(text= "SCORE:", fsize=50, text_color=(168, 235, 52))

start_time = time.time()
cur_time = start_time

time1 = 60
ciklov = 0
time_text = Label(240, 0, 0, 0)
time_text.set_text(text="TIME LEFT:", fsize=50, text_color=(168, 235, 52))


timer = Label(420, 0, 0, 0)
timer.set_text(text=str(time1), fsize=50, text_color=(168, 235, 52))


n = 30
a = -1
b = -1

move_left = False
move_right = False
move_down = False
move_up = False
running = True
while running:
    window.fill(back)
    goal_area.fill()
    ball.draw()
    score.draw()
    points.draw()
    time_text.draw()
    timer.draw()

    for enemy in enemies:
        enemy.fill()
        if enemy.colliderect(ball.rect):
            if i > 0:
                i -= 1
                points.set_text(text=i, fsize=50, text_color=(168, 235, 52))
    

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            break
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_a:
                move_left = True

            if event.key == pygame.K_d:
                move_right = True
            
            if event.key == pygame.K_w:
                move_up = True

            if event.key == pygame.K_s:
                move_down = True

        elif event.type == pygame.KEYUP:
            if event.key == pygame.K_a:
                move_left = False
            if event.key == pygame.K_d:
                move_right = False
            if event.key == pygame.K_w:
                move_up = False
            if event.key == pygame.K_s:
                move_down = False

    if move_left == True:
        ball.rect.x -= 3
    if move_right == True:
        ball.rect.x += 3
    if move_up == True:
        ball.rect.y -= 3
    if move_down == True:
        ball.rect.y += 3
    if ball.rect.x > 400:
        move_right = False    
    if ball.rect.x < 0:
        move_left = False



    if n == 0:
        n = 20
        a = randint(0, 3)
        b = randint(0, 3)
    else:
        if a == 0 and goal_area.rect.x < 200:
            goal_area.rect.x += 3
            
        if a == 1 and goal_area.rect.x > 0:
            goal_area.rect.x -= 3
            
        if a == 2 and goal_area.rect.y > 0:
            goal_area.rect.y -= 3
            
        if a == 3 and goal_area.rect.y < 300:
            goal_area.rect.y += 3
            
        if b == 0:
            for enemy in enemies:
                if enemy.rect.x < 200:
                    enemy.rect.x += 3
                else:
                    enemy.rect.x = randint(0, 490)
                    enemy.rect.y = randint(0, 490)
        if b == 1:
            for enemy in enemies:
                if enemy.rect.x > 0:
                    enemy.rect.x -= 3
                else:
                    enemy.rect.x = randint(0, 490)
                    enemy.rect.y = randint(0, 490)
        if b == 2:
            for enemy in enemies:
                if enemy.rect.y > 0:
                    enemy.rect.y -= 3
                else:
                    enemy.rect.x = randint(0, 490)
                    enemy.rect.y = randint(0, 490)
        if b == 3:
            for enemy in enemies:
                if enemy.rect.y < 300:
                    enemy.rect.y += 3
                else:
                    enemy.rect.x = randint(0, 490)
                    enemy.rect.y = randint(0, 490)
        n -= 1
    

    if ball.colliderect(goal_area.rect):
        i += 1
        points.set_text(text=i, fsize=50, text_color=(168, 235, 52))



        if i == 1000:
            window.fill((0, 0, 0))
            win = Label(x=200, y=200, width=0, height=0)
            win.set_text(text= "WIN", fsize=80, text_color=(win_fonk))
            win.draw()
            score.draw()
            points.draw()
            time_text.draw()
            timer.draw()
            running = False
    
    new_time = time.time()
    if new_time - start_time >= time1:
        win = Label(0, 0, 500, 500, (0, 0, 0))
        win.set_text("TIME IS OVER!!!", 60, (255, 0, 0))
        win.draw()
    
    if time1 - int(new_time - start_time) > 0:

        if int(new_time) - int(cur_time) == 1:
            timer.set_text(text=str(time1 - int(new_time - start_time)), fsize=50, text_color=(168, 235, 52))
            timer.draw()
            cur_time = new_time

   

    
    pygame.display.update()
    clock.tick(144)
