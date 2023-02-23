# PingPong
import pygame
import random

# Initialize Pygame
pygame.init()

# Set the screen dimensions
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Ping Pong Game")

# Set the colors
black = (0, 0, 0)
white = (255, 255, 255)

# Set the paddle and ball dimensions
paddle_width = 100
paddle_height = 20
ball_width = 20
ball_height = 20

# Set the paddle and ball positions
player_paddle_x = (screen_width - paddle_width) // 2
player_paddle_y = screen_height - 50
computer_paddle_x = (screen_width - paddle_width) // 2
computer_paddle_y = 30
ball_x = random.randint(0, screen_width - ball_width)
ball_y = screen_height // 2

# Set the ball speed
ball_x_speed = 5
ball_y_speed = 5

# Set the score
player_score = 0
computer_score = 0

# Create the fonts
font = pygame.font.Font(None, 36)

# Set the game loop
game_over = False
while not game_over:

    # Check for events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True

    # Move the player paddle
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        player_paddle_x -= 5
    if keys[pygame.K_RIGHT]:
        player_paddle_x += 5

    # Move the computer paddle
    if ball_y < screen_height // 2:
        computer_paddle_x = ball_x - paddle_width // 2
    else:
        computer_paddle_x = player_paddle_x

    # Move the ball
    ball_x += ball_x_speed
    ball_y += ball_y_speed

    # Check for collisions
    if ball_x <= 0 or ball_x >= screen_width - ball_width:
        ball_x_speed = -ball_x_speed
    if ball_y <= 0:
        ball_y_speed = -ball_y_speed
    if ball_y >= screen_height - ball_height:
        if ball_x >= player_paddle_x and ball_x <= player_paddle_x + paddle_width:
            ball_y_speed = -ball_y_speed
            player_score += 1
            ball_x_speed += random.randint(-2, 2)
        else:
            computer_score += 1
            ball_x_speed += random.randint(-2, 2)
            ball_x = random.randint(0, screen_width - ball_width)
            ball_y = screen_height // 2

    # Draw the screen
    screen.fill(black)
    pygame.draw.rect(screen, white, (player_paddle_x, player_paddle_y, paddle_width, paddle_height))
    pygame.draw.rect(screen, white, (computer_paddle_x, computer_paddle_y, paddle_width, paddle_height))
    pygame.draw.ellipse(screen, white, (ball_x, ball_y, ball_width, ball_height))
    player_text = font.render("Player Score: " + str(player_score), True, white)
    computer_text = font.render("Computer Score: " + str(computer_score), True, white)
    screen.blit(player_text, (10, 10))
    screen.blit(computer_text, (10, 30))
    pygame.display
