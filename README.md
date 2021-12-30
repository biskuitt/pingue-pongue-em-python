# pingue-pongue-em-python
Um joguinho de pingue pongue feito em python para treinar algumas técnicas de interface gráfica.


import turtle

# Classes

class Shape(turtle.Turtle):
    def __init__(self, goto):
        turtle.Turtle.__init__(self)
        self.shape("square")
        self.color("white")
        self.penup()
        self.goto(goto, 0)



class Player(Shape):
    def __init__(self, goto):
        Shape.__init__(self, goto)
        self.shapesize(6, 0.7)

    def up(self):
        self.sety(self.ycor() + 20)


    def down(self):
        self.sety(self.ycor() - 20)

class Ball(Shape):
    def __init__(self, goto):
        Shape.__init__(self, goto)
        self.dx = 0.5
        self.dy = 0.5
    def update(self, play_a, play_b):
        self.collision(play_a, play_b)
        self.setx(self.xcor() + self.dx)
        self.sety(self.ycor() + self.dy)

    def collision(self, play_a, play_b):
        if self.ycor() > 290:
            self.sety(290)
            self.dy *= -1
        elif self.ycor() < -290:
            self.sety(-290)
            self.dy *= -1

        if self.xcor() < -340 and self.diff(play_a):
            self.dx *= -1
        if self.xcor() > 340 and self.diff(play_b):
            self.dx *= -1

    def diff(self, player):
        return self.ycor() < player.ycor() + 60 and self.ycor() > player.ycor() - 60

class Score(Shape):
    def __init__(self, goto):
        Shape.__init__(self, goto)
        self.goto(0, goto)
        self.score_a = 0
        self.score_b = 0
        self.hideturtle()
        self.draw()

    def update(self, ball):
        if ball.xcor() > 350:
            self.score_a +=1
            self.clear()
            self.draw()
            ball.goto(0, 0)
        elif ball.xcor() < -350:
            self.score_b += 1
            self.clear()
            self.draw()
            ball.goto(0, 0)

    def draw(self):
        self.write("Jogador A: {}      Jogador B: {}".format(self.score_a, self.score_b),
        font=("Arial", 22, "normal"), align="center")


play_a = Player(-350)
play_b = Player(350)
ball = Ball(0)
score = Score(260)


win = turtle.Screen()
win.title("Pong Game")
win.bgcolor("black")
win.setup(width=800, height=600)
win.tracer(0)

win.listen()
win.onkeypress(play_a.up,"w")
win.onkeypress(play_a.down,"s")

win.onkeypress(play_b.up, "Up")
win.onkeypress(play_b.down,"Down")

while True:
    win.update()
    ball.update(play_a, play_b)
    score.update(ball)
