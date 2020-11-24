# first-game
my first game
import pygame
import random
import math

# initializing the pygame
pygame.init()

# creating the screen
width = 800
height = 600
RGB = (0, 0, 0)
screen = pygame.display.set_mode((width, height))

# entering the name
user_text = ''
user_input = ''
input_rect = pygame.Rect(300, 240, 200, 100)
rect_color = (255, 0, 255)
def entername():
    entername_style = pygame.font.Font(None, 32)
    entername_display = entername_style.render('Enter your Name and hit ENTER: ', True, (255, 255, 255))
    screen.blit(entername_display, (220, 200))
    user_font = pygame.font.Font(None, 32)
    user_display = user_font.render(user_input, True, (255, 255, 255))
    screen.blit(user_display,(input_rect.x +10, input_rect.y +10))
    input_rect.w = max(190, user_display.get_width() + 10)



# title and screen
pygame.display.set_caption("Car Smasher")
icon = pygame.image.load('crash.png')
pygame.display.set_icon(icon)

# score
score_value = 0
score_RGB = (255, 255, 255)
font = pygame.font.Font('freesansbold.ttf', 32)
textx = 10
texty = 10

def showscore(x, y):
    score = font.render("score: " + str(score_value), True, score_RGB)
    screen.blit(score, (x, y))

# player
playerimg = pygame.image.load('tractor (1).png')
playerx = 650
playery = 250
playery_change = 0
playerx_change = 0

def player(x, y):
    screen.blit(playerimg, (x, y))


# Enemy
enemyimg = pygame.image.load('racing (1).png')
enemyx = 0
enemyy = random.randint(70, 536)

def enemy(x, y):
    screen.blit(enemyimg, (x, y))


# collision detection

def iscollision(playerx, playery, enemyx, enemyy):
    distance = math.sqrt((math.pow(playerx - enemyx, 2)) + math.pow(playery - enemyy, 2))
    if distance < 64:
        return 1

# pause the program for 1 sec

def pause():
    pygame.time.delay(1000)


# starting Game loop
running = True
while running:
    screen.fill(RGB)
 #   pygame.draw.rect(screen, rect_color, input_rect, 5)
 #   entername()
# control inputs
    for event in pygame.event.get():
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_BACKSPACE:
                user_input = user_input[:-1]
            else:
                user_input += event.unicode




# control inputs of pygame
        if event.type == pygame.QUIT:
            running = False

    # if key is pressed
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP:
                playery_change = -1
            if event.key == pygame.K_DOWN:

                playery_change = +1
            if event.key == pygame.K_RIGHT:

                playerx_change = +1
            if event.key == pygame.K_LEFT:

                playerx_change = -1
        if event.type == pygame.KEYUP:
            if event.key == pygame.K_UP or event.key == pygame.K_DOWN or event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                playery_change = 0
                playerx_change = 0






    enemyx += 2
    enemyy += 0

    playery += playery_change

    # player borders
    if playery <= 0:
        playery = 0
    elif playery >= 500:
        playery = 500
    playerx += playerx_change
    if playerx <= 0:
        playerx = 0
    elif playerx >= 650:
        playerx = 650

    # enemy boundary
    if enemyx >= width:
        enemyx = 0
        enemyy = random.randrange(0, 500, 50)

    # coliision detection
    collision = iscollision(playerx, playery, enemyx, enemyy)
    if collision == 1:
        score_value += 1
        pause()
        enemyx = 0
        enemyy = random.randrange(0, 500, 50)
    enemy(enemyx, enemyy)
    player(playerx, playery)
    showscore(textx, texty)



    pygame.display.update()



