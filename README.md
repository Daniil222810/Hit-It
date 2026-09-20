# Hit-It
Игра Hit It где нужно пройти препятствия три раза
from turtle import Turtle

class Sprite(Turtle):
    def __init__(self, shape, color, step = 10, x = 0, y = -100):
        super().__init__()
        self.penup()
        self.shape(shape)
        self.color(color)
        self.step = step
        self.goto(x, y)
    def move_left(self):
        self.goto(self.xcor() - self.step, self.ycor())
    def move_right(self):
        self.goto(self.xcor() + self.step, self.ycor())
    def move_up(self):
        self.goto(self.xcor(), self.ycor() + self.step)
    def move_down(self):
        self.goto(self.xcor(), self.ycor() - self.step)
    def is_collide(self, sprite_2):
        dist = self.distance(sprite_2.xcor(), sprite_2.ycor())
        if dist < 30:
            return True
        else:
            return False
    def set_move(self, x_end, y_end, x_start, y_start):
        self.x_start = x_start
        self.y_start = y_start
        self.x_end = x_end
        self.y_end = y_end
        self.goto(x_start, y_start)
        angle = self.towards(x_end, y_end)
        self.setheading(angle)
        
    def make_step(self):
        self.forward(self.step)
        if self.distance(self.x_end, self.y_end) < self.step:
            self.set_move(self.x_end, self.y_end, self.x_start, self.y_start)
sprite_2 = Sprite('triangle', 'green', 10, 0, 180)
sprite = Sprite('circle', 'yellow', 10, 0, -100)
enemy_1 = Sprite('square', 'red', 25, -200, 70)
enemy_1.set_move(200, 70, -200, 70)
enemy_2 = Sprite('square', 'red', 25, 200, 0)
enemy_2.set_move(-200, 0, 200, 0)
total = 0
while total < 3:
    scr = sprite.getscreen()
    scr.listen()
    scr.onkey(sprite.move_left, 'Left')
    scr.onkey(sprite.move_right, 'Right')
    scr.onkey(sprite.move_down, 'Down')
    scr.onkey(sprite.move_up, 'Up')
    if sprite.is_collide(sprite_2):
        sprite.goto(0, -100)
        total += 1
    if sprite.is_collide(enemy_1) or sprite.is_collide(enemy_2):
        sprite_2.hideturtle()
        break
    enemy_1.make_step()
    enemy_2.make_step()
enemy_1.hideturtle()
enemy_2.hideturtle()
