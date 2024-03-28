import random
import time
import os

# Function to clear the console screen
def clear_screen():
    os.system('cls' if os.name == 'nt' else 'clear')

# Function to draw the game screen
def draw_game(player_pos, enemy_pos, bullets):
    screen = [' '] * 20
    screen[player_pos] = 'P'
    for bullet in bullets:
        screen[bullet[0]] = '*'
    for enemy in enemy_pos:
        screen[enemy] = 'E'

    print(''.join(screen))

# Function to move player
def move_player(player_pos, move):
    return max(0, min(player_pos + move, 19))

# Function to move bullets and enemies
def move_entities(entities, direction):
    for i in range(len(entities)):
        entities[i] += direction

# Function to check for collisions
def check_collisions(player_pos, bullets, enemy_pos):
    if player_pos in enemy_pos:
        return True
    for bullet in bullets:
        if bullet in enemy_pos:
            enemy_pos.remove(bullet)
            bullets.remove(bullet)
    return False

# Main game function
def main():
    player_pos = 9
    enemy_pos = []
    bullets = []
    score = 0
    game_over = False

    while not game_over:
        clear_screen()
        draw_game(player_pos, enemy_pos, bullets)

        # Player movement
        move = 0
        key = input('Use A and D to move, press SPACE to shoot: ').lower()
        if key == 'a':
            move = -1
        elif key == 'd':
            move = 1
        elif key == ' ':
            bullets.append(player_pos)

        player_pos = move_player(player_pos, move)

        # Add new enemy
        if random.random() < 0.1:
            enemy_pos.append(0)

        # Move bullets and enemies
        move_entities(bullets, 1)
        move_entities(enemy_pos, 1)

        # Check for collisions
        if check_collisions(player_pos, bullets, enemy_pos):
            game_over = True

        # Increase score
        score += 1
        print("Score:", score)
        time.sleep(0.1)

    print('Game Over!')
    print('Your score:', score)

if __name__ == "__main__":
    main()
