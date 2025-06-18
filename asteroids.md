**The Meta-Prompt:**
(the prompt that creates the prompt)

"I want to add a classic arcade game to my website but I don't know how to explain all 
the details a developer would need. Please ask me about 6 questions to help you understand 
what I want, then write me a comprehensive prompt I can give to a coding AI."


-------


**The Questions that the AI asked:**

1. Which specific classic arcade game are you trying to recreate?
2. What should the visual style look like - retro vector graphics, pixel art, or modern styling?
3. How should the player control the game - keyboard, mouse, touch, or gamepad?
4. What happens when the player wins, loses, or completes a level?
5. Do you want features like score tracking, high scores, multiple lives, or difficulty progression?
6. Should it feel exactly like the original or do you want any modern improvements or variations?

**My Example User Answers:**

1. "The classic Asteroids game from 1979 - the one with the triangle spaceship shooting at floating rocks."

2. "I want it to look exactly like the original - simple white lines on a black background, no fancy graphics or colors. 
Just clean vector-style line drawings like the old arcade machines."

4. "Keyboard controls - arrow keys to rotate and move the ship, spacebar to shoot bullets."

5. "When you hit an asteroid with your ship, you die and get a game over screen with your score. 
When you destroy all the asteroids, new ones should appear to keep the game going. 
No traditional 'winning' - just try to get a high score."

6. "Yes, definitely score tracking. Give more points for smaller asteroids since they're harder to hit. 
Save the high score if the person is logged into my site. No multiple lives - one hit and you're dead like the original."

7. "I want it to feel authentic to the 1979 original but with smooth modern performance. 
Keep the same physics where your ship drifts after you stop thrusting, and make sure everything wraps around the screen edges properly."


---------- this was the md file the above conversation created ----------


# Prompt for Building a Classic Asteroids Game in React

## 1. High-Level Objective

Build a fully functional, pixel-perfect clone of the classic 1979 Atari arcade game "Asteroids" as a self-contained React component.

## 2. Project Overview

You are to create a reusable React component that encapsulates the entire Asteroids game. 
The component should manage its own state and game loop, rendering everything onto a single HTML5 `<canvas>` element.

The final product must be a faithful recreation of the original's iconic vector graphics style: 
crisp white lines and shapes on a pure black background. 
The gameplay, physics, and difficulty progression should feel exactly like the 1979 original.

## 3. Core Gameplay Mechanics & Features

### Player Ship
-   **Appearance**: A simple, sharp-edged isosceles triangle.
-   **Physics & Movement**: The ship has inertia. It does not move directly but is propelled by a thruster.
    -   **Rotation**: The ship can rotate left and right on its central axis.
    -   **Thrust**: Applying thrust pushes the ship forward in the direction it's pointing. The ship will continue to drift based on its accumulated momentum, even when thrust is not active.
    -   **Screen Wrapping**: If the ship goes off any edge of the screen, it should immediately reappear on the opposite edge, maintaining its velocity and trajectory.
-   **Shooting**:
    -   Fires small, white, dot-like projectiles from the ship's apex.
    -   There is a strict limit of 4 bullets on-screen at any given time.
-   **Hyperspace (Emergency Escape)**:
    -   A high-risk maneuver. When activated, the ship instantly disappears and reappears at a random location on the screen.
    -   There is a small but significant chance that the ship will be destroyed upon re-entry (e.g., reappearing inside an asteroid). This risk should discourage constant use.
-   **Destruction**: The ship is destroyed upon collision with an asteroid, a UFO, or a UFO projectile.

### Asteroids
-   **Appearance**: Jagged, irregular polygons with varying numbers of vertices. They must not be perfect circles or ovals.
-   **Lifecycle & Fragmentation**:
    -   **Large Asteroids**: Break into two Medium Asteroids when shot.
    -   **Medium Asteroids**: Break into two Small Asteroids when shot.
    -   **Small Asteroids**: Are completely destroyed when shot.
-   **Movement**: Asteroids drift across the screen at various random speeds and on random trajectories. They also wrap around the screen edges.

### UFOs (Saucers)
-   UFOs should appear periodically to increase the challenge.
-   **Large UFO**:
    -   Appears occasionally.
    -   Moves strictly horizontally across the screen.
    -   Fires projectiles in random directions. Low accuracy.
-   **Small UFO**:
    -   Appears less frequently than the large one, especially in later levels.
    -   Is smaller, faster, and more agile.
    -   Fires projectiles with high accuracy, aimed directly at the player's ship.
-   **Destruction**: Both UFO types are destroyed by a single player shot.

### Scoring
-   Large Asteroid Destroyed: **20 points**
-   Medium Asteroid Destroyed: **50 points**
-   Small Asteroid Destroyed: **100 points**
-   Large UFO Destroyed: **200 points**
-   Small UFO Destroyed: **1000 points**
-   **Extra Life**: Award the player one extra life for every 10,000 points scored.

### Lives & Game State
-   **Initial State**: The player starts with 3 lives.
-   **Losing a Life**: When the ship is destroyed, pause the game for 1-2 seconds. The ship then respawns in the center of the screen and should be invincible for 2-3 seconds to give the player a moment to orient themselves.
-   **Game Over**: The game ends when the player has zero lives remaining. Display a "GAME OVER" message prominently on the screen.
-   **UI Display**: The current score and remaining lives (represented as small ship icons) must be displayed at the top of the game screen at all times.

### Levels & Difficulty
-   The game is level-based. A level is cleared when all asteroids (and any on-screen UFOs) are destroyed.
-   The next level should begin immediately, starting with a greater number of large asteroids than the previous level.

## 4. Controls

Implement keyboard controls for all player actions.
-   `ArrowUp`: Apply Thrust
-   `ArrowLeft`: Rotate Ship Left (Counter-Clockwise)
-   `ArrowRight`: Rotate Ship Right (Clockwise)
-   `Spacebar`: Fire Projectile
-   `Shift` (or `h`): Activate Hyperspace

## 5. Art & Visual Style

This is critical for achieving the authentic retro feel.
-   **Canvas Background**: Solid black (`#000000`).
-   **All Game Objects**: Rendered as aliased (pixelated, not smooth) white (`#FFFFFF`) lines and outlines only. **Do not use any fills, colors, or anti-aliasing.** This is essential to replicate the look of a vector-scan monitor.
-   **Text/Font**: Use a simple, blocky, or pixelated font for the score, lives counter, and "GAME OVER" message to match the retro aesthetic.

## 6. Sound Effects (Optional but Recommended)

For full authenticity, include the classic sound effects, triggered by game events:
-   **Player Fire**: A sharp "pew" sound.
-   **Player Thrust**: A low, rumbling hum that plays only when the thruster is active.
-   **Ship Destruction**: A loud, crashing explosion.
-   **Asteroid Explosion**: A lower-pitched, booming explosion.
-   **UFO Siren**: A looping, two-tone siren sound that plays while a UFO is on screen.
-   **Extra Life Awarded**: A short, positive notification chime.

## 7. Technical Implementation Details

-   **Framework**: React
-   **Rendering Engine**: HTML5 `<canvas>`.
-   **Game Loop**: Use `requestAnimationFrame` for the main game loop to ensure smooth, performant animations tied to the browser's refresh rate.
-   **Component Structure**: Encapsulate all logic within a single, self-contained `<AsteroidsGame />` component. This component will handle its own state, user input (via `useEffect` for event listeners), and rendering logic.
-   **Physics & Collision**: Implement simple 2D physics for object movement and inertia. Bounding-box or bounding-circle collision detection is sufficient. 
