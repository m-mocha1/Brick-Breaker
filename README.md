# Brick Breaker / Arkanoid (Vanilla JS + DOM)

A simple, high-performance Brick Breaker / Arkanoid clone written in plain JavaScript, HTML and CSS—no frameworks, no `<canvas>`—built to run at a solid 60 FPS with smooth controls, performance monitoring, and a pause menu.
![Screenshot 2025-05-16 170556](https://github.com/user-attachments/assets/16ad7f50-eade-44c3-a01a-64c51e181b61)

---

## 🚀 Features

- **144 FPS Game Loop**  
  Uses `requestAnimationFrame` to drive a consistent, jank-free 60 frames-per-second animation loop.  
- **Performance Monitoring**  
  Built-in FPS counter and frame-time measurements (via `performance.now()`) to verify no frame drops.  
- **Pause Menu**  
  - **Continue**: Resume game exactly where you left off, without losing timing or state.  
  - **Restart**: Reset score, timer, lives, and bricks.  
- **Scoreboard HUD**  
  - **Timer / Countdown**: Shows elapsed play time or time remaining.  
  - **Score**: Tracks points (XP) earned by breaking bricks.  
  - **Lives**: Displays remaining lives.  
- **Smooth Keyboard Controls**  
  Hold arrow keys (or A / D) to move paddle continuously; release to stop instantly—no key-spam required.  
- **Minimal DOM Layers**  
  Only a handful of absolutely-positioned elements (paddle, ball, bricks, HUD, menus) to keep repaint/reflow cost low.

## 🔧 Getting Started

### Prerequisites

- A modern desktop browser (Chrome, Firefox, Edge, Safari) with JavaScript enabled.

### Installation & Running

1. **Clone or download** this repository.
2. Open `index.html` in your browser.
3. The game will start automatically—press **Space** to pause.

---

## 🎮 Controls

| Key            | Action                               |
| -------------- | ------------------------------------ |
| ◀️ / ▶️ or A / D | Move paddle left / right (hold to keep moving) |
| **Space**      | Pause / Continue                     |
| **R**          | Restart level                        |

---

## ⚙️ Game Loop & Performance

The heart of the engine lives in **`engine.js`**:

1. **`requestAnimationFrame(gameLoop)`**  
   Schedules each frame, providing the highest-precision timing.  
2. **Delta‐time calculation**  
   ```js
   const now = performance.now();
   const delta = now - lastFrameTime;
   lastFrameTime = now;
   updateAll(delta);
   renderAll();

⏸️ Pause Menu
Triggered by pressing Space.

Overlay DOM element with three buttons:

Continue: Closes overlay and resumes loop.

Restart: Calls resetGame() to reinitialize all state.

Quit (if implemented): Could return to a main menu (future feature).

While paused, the game loop is halted; timers stop, but the overlay itself remains responsive at 60 FPS (minimal repaint cost).

📊 Scoreboard (HUD)
Timer

Counts up from 00:00 showing minutes : seconds played.

Alternatively can be configured as a countdown.

Score

Incremented each time a brick is destroyed.

Shown as a numerical value (“Score: 1200 XP”).

Lives

Starts at 3 (configurable).

Decrements when ball misses the paddle.

When reaching zero, triggers “Game Over” state.

All HUD elements are simple <div>s with position: absolute; so updates only touch text content, avoiding full-layout reflows.

📈 Performance Tips & Tricks
Batch DOM Reads/Writes
Read all positions/styles first, then apply writes to minimize layout thrashing.

Minimal Layer Count
Only generate brick elements for bricks that exist; remove DOM nodes immediately when bricks are destroyed.

Offload Logic
Heavy calculations (collision detection, score logic) happen in JS objects, not in CSS transitions or animations.

