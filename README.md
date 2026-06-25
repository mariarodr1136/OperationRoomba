# Operation Roomba 🤖

![HTML5](https://img.shields.io/badge/HTML5-Canvas-orange) ![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-yellow) ![Web Audio API](https://img.shields.io/badge/Web%20Audio-API-blue) ![No Dependencies](https://img.shields.io/badge/Dependencies-None-brightgreen) ![Render](https://img.shields.io/badge/Deploy-Render-46E3B7)

**Operation Roomba** is a top-down stealth cleaning game built entirely in vanilla JavaScript with no dependencies or build step. You play as a Roomba at 3am — navigate five rooms of increasing chaos, clean **80% of the dirt** before dawn, and avoid waking the family at all costs.

Every room is procedurally generated with randomized dirt clusters, furniture, legos, socks, sleeping cats, roaming dogs, and baby monitors. Too much noise fills the owner's patience meter — and when it hits 100%, you're on a countdown to game over.

Live Application: https://operationroomba.onrender.com

---

https://github.com/user-attachments/assets/INSERT_VIDEO_ASSET_ID_HERE

---

## Table of Contents
- [Features](#features)
- [Languages & Frameworks Used](#languages--frameworks-used)
- [Code Structure](#code-structure)
- [How to Play](#how-to-play)
- [Levels](#levels)
- [Installation](#installation)
- [Requirements](#requirements)
- [Inspiration](#inspiration)
- [Contributing](#contributing)
- [Contact](#contact)

---

## Features

**Gameplay**
- **Five procedurally generated rooms** — dirt, furniture, and obstacles are randomly placed every run.
- **Noise meter** — collisions, cat hisses, dogs, and baby monitors all raise noise. Hit 100% and the owner wakes with a live countdown timer.
- **Battery system** — drains while moving; return to the glowing dock to recharge with animated charging sparks.
- **Combo multiplier** — consecutive dirty-cell cleans stack up to an 8× score multiplier.
- **Three lives per level** — lost to legos, cats, and dogs; losing all three ends the run.
- **High score** — best score is persisted across sessions via `localStorage`.

**Obstacles & Hazards**
- **Legos** — hidden on the floor; stepping on one costs a life and spikes the noise meter.
- **Socks** — snag the Roomba and stun it for 3 seconds.
- **Cats** — sleeping until disturbed; they wake, hiss, chase you, and actively re-dirty cleaned tiles as they run.
- **Dogs** — always awake, always moving, always hostile.
- **Baby monitors** — raise the noise meter passively while you're within range.

**Power-ups**
- **B (Boost)** — 3× movement speed for 6 seconds.
- **M (Mega)** — doubles the Roomba's cleaning radius for 6 seconds.
- **H (Hush)** — muffles noise build-up and actively lowers the meter for 8 seconds.

**Pause Menu**
- Pause at any time with `P`, `Escape`, or `Space`.
- **Quit to Title** button in the pause overlay — opens a confirmation dialog before abandoning your run.
- Hover effects and cursor change on all interactive pause buttons.
- Distinct sound effects for pausing, resuming, opening the quit dialog, confirming, and cancelling.

**HUD & UI**
- **Segmented progress bar** — spans the top with a dashed 80% goal marker and live percentage readout.
- **Minimap** — appears on large levels, showing cleaned vs. dirty tiles and your current viewport.
- **Active-effect timers** — on-screen labels show remaining seconds for Boost, Mega, and Hush.
- **Dock arrow** — directional indicator appears when battery is low and the dock is off-screen.
- **Owner warning overlay** — red tint and flashing countdown when the owner is waking up.
- **Touch D-pad** — on-screen directional controls appear automatically on touch devices.

**Audio**
- Fully synthesized sound effects via the Web Audio API — no audio files.
- Hum while moving, lego crunch, sock slurp, cat hiss, dog bark, baby monitor beep, charging ticks, combo chimes, alarm, and level-up fanfare.
- Pause / resume chimes and menu interaction sounds.
- Global mute toggle (`M` key or on-screen button).

---

## Languages & Frameworks Used

### Frontend
- **HTML5 Canvas**: All game rendering — world tiles, sprites, HUD, overlays, and minimap drawn via 2D canvas API
- **CSS**: Title screen, pause overlay, and HUD layout styling
- **JavaScript (ES6+)**: Entire game engine — game loop, physics, procedural generation, AI, input, audio, and persistence

### Audio
- **Web Audio API**: All sound synthesized on demand using `OscillatorNode` and `AudioBufferSourceNode` — no audio files required

### Deployment
- **Render**: Static site deployed via `render.yaml` — no build step required

### Version Control
- **Git / GitHub**: Source hosting & issue tracking

---

## Code Structure

```
OperationRoomba/
├── index.html      # Entire game — markup, styles, and all JavaScript in one file
└── render.yaml     # Render static site deployment config
```

The JavaScript inside `index.html` is organized into self-contained sections:

```
index.html
├── Levels              # Level definitions (map size, obstacle counts, time limits)
├── Grid                # Dirt cell generation, cleaning, minimap canvas
├── Audio               # Web Audio API — all sound synthesis functions
├── Game state          # Global variables (score, battery, noise, lives, timers)
├── Camera              # Viewport scroll and zoom logic
├── Roomba              # Player object and movement constants
├── Input               # Keyboard, touch D-pad, mouse tracking, pause menu interaction
├── Spawn helpers       # Procedural placement of furniture and obstacles
├── Game flow           # startLevel, startGame, nextLevel, shake, popup, burst
├── Collision           # Circle-point, circle-circle, circle-rect, furniture resolution
├── Update              # Main game loop logic (physics, AI, pickups, win/lose checks)
├── World-space draw    # Room tiles, dock, furniture, obstacles, Roomba
├── Screen-space draw   # Darkness gradient, popups, HUD, minimap, overlay screens
└── Main loop           # requestAnimationFrame loop
```

**Architecture highlights:**
- **Game loop** — a single `requestAnimationFrame` loop calls `update()` then `draw()` every frame at ~60 fps.
- **Grid system** — the map is divided into 20×20 px cells stored in flat `Uint8Array` buffers (`dirty`, `cleaned`). A secondary offscreen canvas (`mmCanvas`) maintains the minimap pixel-by-pixel as cells are cleaned.
- **Camera** — the world is rendered at 1.8× zoom with a scrolling camera that clamps to map bounds. All world-space drawing happens inside a `ctx.scale` + `ctx.translate` block; HUD elements are drawn afterwards in screen space.
- **Darkness** — a radial gradient overlay drawn in screen space creates the flashlight effect, with the inner radius expanding for Mega and Boost power-ups.
- **Procedural generation** — dirt is placed in random Gaussian clusters; furniture, obstacles, and power-ups are placed with a minimum-distance rejection-sampling algorithm so nothing spawns on top of the player start or dock.
- **Audio** — all sound is synthesized on demand using `OscillatorNode` and `AudioBufferSourceNode`. White noise with an exponential decay envelope produces the lego crunch; oscillators with frequency ramps handle all melodic cues.
- **Persistence** — only the high score is stored, using `localStorage`.

---

## How to Play

Clean **80% of the dirt** in each room before the timer hits zero.

**Controls**

| Input | Action |
|-------|--------|
| `WASD` / Arrow keys | Move |
| `P` / `Escape` | Pause / resume |
| `Space` / `Enter` | Confirm / advance screen |
| `M` | Toggle mute |

Touch devices show an on-screen D-pad automatically.

**Obstacles**

| Obstacle | Effect |
|----------|--------|
| **Lego** | Costs a life, spikes the noise meter |
| **Sock** | Stuns the Roomba for 3 seconds |
| **Cat** | Wakes, hisses, chases you, and re-dirties cleaned tiles |
| **Dog** | Wanders constantly — always hostile |
| **Baby monitor** | Passively raises noise while you're within range |

**Power-ups**

| Symbol | Name | Effect |
|--------|------|--------|
| **B** | Boost | 3× speed for 6 seconds |
| **M** | Mega | Doubled cleaning radius for 6 seconds |
| **H** | Hush | Muffles and reduces noise for 8 seconds |

**Mechanics**
- **Battery** — drains while moving, recharges at the glowing dock. A directional arrow appears on-screen when battery is low and the dock is off-camera.
- **Noise meter** — fills from legos, cats, dogs, and baby monitors. At 100% the owner begins waking — you have a short countdown before game over.
- **Combo** — cleaning consecutive dirty cells builds a multiplier (up to 8×). It resets if you stop moving.
- **Lives** — 3 lives per level. Legos, cats, and dogs each cost one.

---

## Levels

| # | Room | Map size | Legos | Cats | Dogs |
|---|------|----------|-------|------|------|
| 1 | The Kitchen | 900 × 600 | 20 | 1 | 1 |
| 2 | The Living Room | 1500 × 980 | 38 | 2 | 2 |
| 3 | The Hallway | 2400 × 1540 | 64 | 3 | 3 |
| 4 | The Bedroom | 3300 × 2100 | 100 | 5 | 4 |
| 5 | The Whole House | 4500 × 2900 | 150 | 7 | 6 |

---

## Installation

### 1. Clone
```bash
git clone https://github.com/mariarodr1136/OperationRoomba.git
cd OperationRoomba
```

### 2. Open the game
```bash
open index.html
```
Or double-click `index.html` in any file manager. No server, no build step, no dependencies.

## Requirements
- Modern browser (Chrome, Firefox, Safari, Edge)
- No Node.js, no build tools, no dependencies required

---

## Inspiration

Operation Roomba is inspired by classic stealth games — the tension of moving quietly through a space, the risk-reward of pushing further into hostile territory, and the inevitability of eventually waking someone up. Reimagining that formula through the lens of a Roomba at 3am makes every mundane household obstacle feel genuinely threatening. The procedurally generated rooms ensure that no two runs are alike, keeping the cleaning loop fresh while escalating the chaos across five levels.

---

## Contributing

Contributions welcome — new obstacle types, level layouts, power-ups, or audio improvements.

1. Fork the repo
2. Create a branch:
   ```bash
   git checkout -b feat/my-feature
   # or
   git checkout -b fix/issue-###
   ```
3. Make your changes and commit with a clear message:
   ```bash
   git commit -m "feat: add <short description>"
   ```
4. Push your branch and open a pull request with a description of your changes.

## Contact
If you have any questions or feedback, feel free to reach out at [mrodr.contact@gmail.com](mailto:mrodr.contact@gmail.com).
