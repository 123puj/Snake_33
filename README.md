# Snake_33
import pygame
import time
import random

# Initialize pygame
pygame.init()

# Set display width and height
WIDTH = 600
HEIGHT = 400

# Define colors
WHITE = (255, 255, 255)
BLUE = (50, 150, 255)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# Set up display
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Snake Game")

# Snake variables
snake_block = 10
snake_speed = 15

# Font for score
font = pygame.font.SysFont(None, 35)

# Function to display the score
def show_score(score):
    value = font.render(f"Your Score: {score}", True, WHITE)
    screen.blit(value, [10, 10])

# Main game function
def gameLoop():
    game_over = False
    game_close = False

    x = WIDTH / 2
    y = HEIGHT / 2
    dx = 0
    dy = 0

    snake = []
    length_of_snake = 1

    # Generate random food
    food_x = round(random.randrange(0, WIDTH - snake_block) / 10.0) * 10.0
    food_y = round(random.randrange(0, HEIGHT - snake_block) / 10.0) * 10.0

    clock = pygame.time.Clock()

    while not game_over:

        while game_close:
            screen.fill(BLUE)
            msg = font.render("YOU Lost! Press Q-Quit or C-Play Again", True, RED)
            screen.blit(msg, [WIDTH / 6, HEIGHT / 3])
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_r:
                        gameLoop()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    dx = -snake_block
                    dy = 0
                elif event.key == pygame.K_RIGHT:
                    dx = snake_block
                    dy = 0
                elif event.key == pygame.K_UP:
                    dy = -snake_block
                    dx = 0
                elif event.key == pygame.K_DOWN:
                    dy = snake_block
                    dx = 0

        # Boundaries check
        if x >= WIDTH or x < 0 or y >= HEIGHT or y < 0:
            game_close = True

        # Move the snake
        x += dx
        y += dy
        screen.fill(BLUE)
        pygame.draw.rect(screen, WHITE, [food_x, food_y, snake_block, snake_block])

        snake_head = []
        snake_head.append(x)
        snake_head.append(y)
        snake.append(snake_head)

        if len(snake) > length_of_snake:
            del snake[0]

        # Check collision with itself
        for segment in snake[:-1]:
            if segment == snake_head:
                game_close = True

        # Draw the snake
        for segment in snake:
            pygame.draw.rect(screen, GREEN, [segment[0], segment[1], snake_block, snake_block])

        # Check if food is eaten
        if x == food_x and y == food_y:
            food_x = round(random.randrange(0, WIDTH - snake_block) / 10.0) * 10.0
            food_y = round(random.randrange(0, HEIGHT - snake_block) / 10.0) * 10.0
            length_of_snake += 1

        show_score(length_of_snake - 1)

        pygame.display.update()
        clock.tick(snake_speed)

    pygame.quit()
    quit()

# Run the game
gameLoop()
