# RunningCat

## Introduction

**Cat Run** is an engaging and fun endless runner game developed using Python and Pygame. In this game, you control a character called "Catosaur" who must avoid obstacles and keep running as long as possible. The game features dynamic animations, sound effects, and an increasing level of difficulty to keep players challenged and entertained.

## Features

- **Endless Running**: Keep running and avoid obstacles to score points.
- **Dynamic Obstacles**: Encounter a variety of obstacles such as cacti and pterodactyls.
- **Realistic Animations**: Smooth running, jumping, and ducking animations for the Catosaur.
- **Sound Effects**: Immersive sound effects for jumping, scoring points, and game over.
- **Simple Controls**: Easy-to-use controls for jumping and ducking.
- **Score Tracking**: Keep track of your score and lives left.

## Requirements

- Python 3.x
- Pygame library

## Installation

1. **Clone the Repository**:
    ```sh
    git clone https://github.com/your-username/cat-run.git
    cd cat-run
    ```

2. **Install Pygame**:
    ```sh
    pip install pygame
    ```

3. **Download Game Assets**:
   - Ensure all the required assets (images and sounds) are placed in the `assets` directory as specified in the code.

## How to Play

1. **Run the Game**:
    ```sh
    python cat_run.py
    ```

2. **Game Controls**:
    - **Jump**: Press the `Space` bar or `Up` arrow key to make the Catosaur jump.
    - **Duck**: Press the `Down` arrow key to make the Catosaur duck.

3. **Objective**:
    - Avoid obstacles and keep running to increase your score.
    - The game speed increases gradually, making it more challenging.
    - The Catosaur has 3 lives, and each collision with an obstacle will reduce a life.
    - When all lives are lost, the game is over.

## Code Explanation

### Main Game Loop

The main game loop handles events, updates game objects, checks for collisions, and renders the game screen.

```python
while True:
    keys = pygame.key.get_pressed()
    if keys[pygame.K_DOWN]:
        cat.duck()
    else:
        if cat.ducking:
            cat.unduck()
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        if event.type == CLOUD_EVENT:
            current_cloud_y = random.randint(50, 300)
            current_cloud = Cloud(cloud, 1380, current_cloud_y)
            cloud_group.add(current_cloud)
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE or event.key == pygame.K_UP:
                cat.jump()
                if game_over:
                    game_over = False
                    game_speed = 6
                    player_score = 0
    screen.fill("white")
    if pygame.sprite.spritecollide(catosaur_group.sprite, obstacle_group, False):
        if collision_time == 0:
            if count <= 1:
                game_over = True
            else:
                count -= 1
            death_sfx.play()
            collision_time = pygame.time.get_ticks()

    if collision_time != 0 and pygame.time.get_ticks() - collision_time >= collision_cooldown:
        collision_time = 0

    if game_over:
        end_game()

    if not game_over:
        game_speed += 0.0020
        if round(player_score, 1) % 100 == 0 and int(player_score) > 0:
            points_sfx.play()

        if pygame.time.get_ticks() - obstacle_timer >= obstacle_cooldown:
            obstacle_spawn = True

        if obstacle_spawn:
            obstacle_random = random.randint(1, 50)
            if obstacle_random in range(1, 7):
                new_obstacle = Cactus(1260, 340)
                obstacle_group.add(new_obstacle)
                obstacle_timer = pygame.time.get_ticks()
                obstacle_spawn = False
            elif obstacle_random in range(7, 10):
                new_obstacle = Ptero()
                obstacle_group.add(new_obstacle)
                obstacle_timer = pygame.time.get_ticks()
                obstacle_spawn = False

        player_score += 0.1
        player_score_surface = game_font.render(
            str(f"{int(player_score)}  -  life: {count}"), True, ("black"))
        screen.blit(player_score_surface, (850, 10))

        cloud_group.update()
        cloud_group.draw(screen)

        ptero_group.update()
        ptero_group.draw(screen)

        catosaur_group.update()
        catosaur_group.draw(screen)

        obstacle_group.update()
        obstacle_group.draw(screen)

        ground_x -= game_speed

        screen.blit(ground, (ground_x, 360))
        screen.blit(ground, (ground_x + 1280, 360))

        if ground_x <= -1280:
            ground_x = 0

    clock.tick(120)
    pygame.display.update()
```

### Classes

- **Catosaur**: Manages the player's character, including animations, jumping, ducking, and gravity.
- **Cloud**: Manages the clouds in the background.
- **Cactus**: Represents the cactus obstacles.
- **Ptero**: Represents the pterodactyl obstacles.

### Functions

- **end_game**: Handles game over logic, including resetting the game state.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
