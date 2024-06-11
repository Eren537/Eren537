
import pygame
import sys

# Pygame'i başlat
pygame.init()

# Renk tanımları
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)

# Pencere boyutları
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600

# Pencereyi oluştur
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Zıplamalı Oyun")

# FPS ayarı
clock = pygame.time.Clock()

# Oyuncu özellikleri
player_size = 50
player_x = SCREEN_WIDTH // 2
player_y = SCREEN_HEIGHT - player_size
player_velocity = 5
player_y_velocity = 0
gravity = 0.5
jump_power = -10
on_ground = True

# Platform özellikleri
platforms = [
    pygame.Rect(200, 500, 200, 20),
    pygame.Rect(400, 400, 200, 20),
    pygame.Rect(600, 300, 200, 20)
]

# Oyun döngüsü
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Tuşları kontrol et
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        player_x -= player_velocity
    if keys[pygame.K_RIGHT]:
        player_x += player_velocity
    if keys[pygame.K_SPACE] and on_ground:
        player_y_velocity = jump_power
        on_ground = False

    # Yerçekimini uygula
    player_y_velocity += gravity
    player_y += player_y_velocity

    # Yere temas kontrolü
    if player_y >= SCREEN_HEIGHT - player_size:
        player_y = SCREEN_HEIGHT - player_size
        player_y_velocity = 0
        on_ground = True

    # Platformlarla çarpışma kontrolü
    on_ground = False
    for platform in platforms:
        if player_y + player_size > platform.y and player_y + player_size - player_y_velocity <= platform.y and player_x + player_size > platform.x and player_x < platform.x + platform.width:
            player_y = platform.y - player_size
            player_y_velocity = 0
            on_ground = True

    # Ekranı beyaza boyayarak temizle
    screen.fill(WHITE)

    # Platformları çiz
    for platform in platforms:
        pygame.draw.rect(screen, GREEN, platform)
    
    # Oyuncuyu çiz
    pygame.draw.rect(screen, BLACK, (player_x, player_y, player_size, player_size))
    
    # Ekranı güncelle
    pygame.display.flip()
    
    # FPS'i ayarla
    clock.tick(60)

