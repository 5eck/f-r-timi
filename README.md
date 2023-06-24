# f-r-timi

import pygame
import random

# Initialisierung
pygame.init()

# Bildschirmgröße
width = 800
height = 600

# Farben
black = (0, 0, 0)
white = (255, 255, 255)
red = (255, 0, 0)
green = (0, 255, 0)

# Spielfeldgröße
grid_size = 20
grid_width = width // grid_size
grid_height = height // grid_size

# FPS (Frames pro Sekunde)
fps = 10

# Erstellung des Bildschirms
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Snake Game")

# Game Over Text
font = pygame.font.Font("freesansbold.ttf", 50)

# Funktion zum Zeichnen der Schlange
def draw_snake(snake):
    for segment in snake:
        pygame.draw.rect(screen, green, (segment[0] * grid_size, segment[1] * grid_size, grid_size, grid_size))

# Funktion zum Zeichnen des Apfels
def draw_apple(apple_pos):
    pygame.draw.rect(screen, red, (apple_pos[0] * grid_size, apple_pos[1] * grid_size, grid_size, grid_size))

# Funktion zum Anzeigen des Game Over-Textes
def show_game_over():
    text = font.render("Game Over", True, white)
    text_rect = text.get_rect()
    text_rect.center = (width // 2, height // 2)
    screen.blit(text, text_rect)

# Hauptspiel-Schleife
def game_loop():
    # Position der Schlange
    snake = [(grid_width // 2, grid_height // 2)]
    direction = "right"

    # Position des Apfels
    apple_pos = (random.randint(0, grid_width - 1), random.randint(0, grid_height - 1))

    # Spiel läuft?
    game_over = False

    while not game_over:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP and direction != "down":
                    direction = "up"
                elif event.key == pygame.K_DOWN and direction != "up":
                    direction = "down"
                elif event.key == pygame.K_LEFT and direction != "right":
                    direction = "left"
                elif event.key == pygame.K_RIGHT and direction != "left":
                    direction = "right"

        # Bewegung der Schlange
        if direction == "up":
            new_head = (snake[0][0], snake[0][1] - 1)
        elif direction == "down":
            new_head = (snake[0][0], snake[0][1] + 1)
        elif direction == "left":
            new_head = (snake[0][0] - 1, snake[0][1])
        elif direction == "right":
            new_head = (snake[0][0] + 1, snake[0][1])

        # Game Over, wenn die Schlange aus dem Spielfeld geht
        if new_head[0] < 0 or new_head[0] >= grid_width or new_head[1] < 0 or new_head[1] >= grid_height:
            game_over = True

        # Game Over, wenn die Schlange mit sich selbst kollidi
