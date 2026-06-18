# Operation Roomba

![HTML5](https://img.shields.io/badge/HTML5-Canvas-orange) ![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-yellow) ![Web Audio API](https://img.shields.io/badge/Web%20Audio-API-blue) ![No Dependencies](https://img.shields.io/badge/Dependencies-None-brightgreen)

**Operation Roomba** is a top-down stealth cleaning game built entirely in vanilla JavaScript with no dependencies or build step. You play as a Roomba at 3am — navigate five rooms of increasing chaos, clean **80% of the dirt** before dawn, and avoid waking the family at all costs.

Every room is procedurally generated with randomized dirt clusters, furniture, legos, socks, sleeping cats, roaming dogs, and baby monitors. Too much noise fills the owner's patience meter — and when it hits 100%, you're on a countdown to game over.

---

### Table of Contents
- [Features](#features)
- [Code Structure](#code-structure)
- [Architecture Overview](#architecture-overview)
- [How to Play](#how-to-play)
- [Levels](#levels)
- [Difficulty](#difficulty)
- [Installation](#installation)
- [Contributing](#contributing)
- [Contact](#contact)

---

### Features

**Gameplay**
- **Five procedurally generated rooms** — dirt, furniture, and obstacles are randomly placed every run.
- **Noise meter** — collisions, cat hisses, dogs, and baby monitors all raise noise. Hit 100% and the owner wakes with a live countdown timer.
- **Battery system** — drains while moving; return to the glowing dock to recharge with animated charging sparks.
- **Combo multiplier** — consecutive dirty-cell cleans stack up to an 8× score multiplier.
- **Three lives per level** — lost to legos, cats, and dogs; losing all ends the run.
- **Star ratings** — earn 1–3 stars per level based on clean percentage and time remaining.
- **High score** — best score is persisted across sessions via `localStorage`.

**Obstacles & Hazards**
- **Legos** — hidden on the floor; stepping on one costs a life and spikes the noise meter.
- **Socks** — snag the Roomba and stun it for 2 seconds.
- **Cats** — sleeping until disturbed; they wake, hiss, chase you, and actively re-dirty cleaned tiles as they run.
- **Dogs** — always awake, always moving, always hostile.
- **Baby monitors** — raise the noise meter passively while you're within range.

**Power-ups**
- **B (Boost)** — 3× movement speed for 6 seconds.
- **M (Mega)** — doubles the Roomba's cleaning radius for 6 seconds.
- **H (Hush)** — muffles noise build-up and actively lowers the meter for 8 seconds.

**HUD & UI**
- **Full-width progress bar** — pill-shaped bar across the top with a dashed 80% goal marker and live percentage readout.
- **Minimap** — appears on large levels, showing cleaned vs. dirty tiles and your current position.
- **Active-effect timers** — on-screen labels show remaining seconds for Boost, Mega, and Hush.
- **Dock arrow** — directional indicator appears when battery is low and the dock is off-screen.
- **Owner warning overlay** — red tint and flashing countdown when the owner is waking up.
- **Touch D-pad** — on-screen directional controls appear automatically on touch devices.

**Audio**
- Fully synthesized sound effects via the Web Audio API — no audio files.
- Hum while moving, lego crunch, sock slurp, cat hiss, dog bark, baby monitor beep, charging ticks, combo chimes, alarm, and level-up fanfare.
- Global mute toggle (`M` key or on-screen button).

---

### Code Structure

```
OperationRoomba/
└── index.html          # Entire game — markup, styles, and all JavaScript in one file
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
├── Input               # Keyboard events, touch D-pad, mouse tracking
├── Spawn helpers       # Procedural placement of furniture and obstacles
├── Game flow           # startLevel, startGame, nextLevel, shake, popup, burst
├── Collision           # Circle-point, circle-circle, circle-rect, furniture resolution
├── Update              # Main game loop logic (physics, AI, pickups, win/lose checks)
├── World-space draw    # Room tiles, dock, furniture, obstacles, Roomba
├── Screen-space draw   # Darkness gradient, popups, HUD, minimap, overlay screens
└── Main loop           # requestAnimationFrame loop
```

---

### Architecture Overview

- **Game loop** — a single `requestAnimationFrame` loop calls `update()` then `draw()` every frame at 60 fps.
- **Grid system** — the map is divided into 20×20px cells stored in flat `Uint8Array` buffers (`dirty`, `cleaned`). A secondary offscreen canvas (`mmCanvas`) maintains the minimap pixel-by-pixel as cells are cleaned.
- **Camera** — the world is rendered at 1.8× zoom with a scrolling camera that clamps to map bounds. All world-space drawing happens inside a `ctx.scale` + `ctx.translate` block; HUD elements are drawn afterwards in screen space.
- **Darkness** — a radial gradient overlay drawn in screen space creates the flashlight effect, with the inner radius expanding for Mega and Boost power-ups.
- **Procedural generation** — dirt is placed in random Gaussian clusters; furniture, obstacles, and power-ups are placed with a minimum-distance rejection-sampling algorithm so nothing spawns on top of the player start or dock.
- **Audio** — all sound is synthesized on demand using `OscillatorNode` and `AudioBufferSourceNode`. White noise with an exponential decay envelope produces the lego crunch; oscillators with frequency ramps handle all melodic cues.
- **Persistence** — only the high score is stored, using `localStorage`.

---

### How to Play

Clean **80% of the dirt** in each room before the timer hits zero.

**Controls**

| Input | Action |
|-------|--------|
| `WASD` / Arrow keys | Move |
| `P` / `Escape` | Pause |
| `M` | Toggle mute |
| `Space` / `Enter` | Confirm / advance screen |

Touch devices show an on-screen D-pad automatically.

**Obstacles**

| Obstacle | Effect |
|----------|--------|
| **Lego** | Costs a life, spikes the noise meter |
| **Sock** | Stuns the Roomba for 2 seconds |
| **Cat** | Wakes, chases you, and re-dirties cleaned tiles |
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
- **Noise meter** — fills from legos, cats, dogs, and baby monitors. Hush power-up actively drains it. At 100% the owner begins waking — you have a limited countdown before game over.
- **Combo** — cleaning consecutive dirty cells builds a multiplier (up to 8×). It resets if you stop moving.
- **Lives** — 3 lives per level. Legos, cats, and dogs each cost one.

---

### Levels

| # | Room | Size | Highlights |
|---|------|------|------------|
| 1 | The Kitchen | 560 × 380 | Intro size — no cats or dogs |
| 2 | The Living Room | 940 × 620 | First cat and baby monitor appear |
| 3 | The Hallway | 1500 × 960 | Long corridors, dogs join the chaos |
| 4 | The Bedroom | 2100 × 1340 | Multiple cats, dogs, and monitors |
| 5 | The Whole House | 2900 × 1860 | Maximum chaos — minimap essential |

---

### Difficulty

Select at the start screen. Affects time limit, lego count, and owner patience.

| Mode | Time | Legos | Owner patience |
|------|------|-------|----------------|
| **Easy** | +40% | −40% | 5 seconds to escape |
| **Normal** | Base | Base | 3 seconds to escape |
| **Hard** | −30% | +50% | 2 seconds to escape |

Best score is saved locally across sessions.

**Star ratings** per level:

- ⭐ — cleaned 80%+ dirt (level complete)
- ⭐⭐ — cleaned 85%+ dirt
- ⭐⭐⭐ — cleaned 95%+ dirt with at least 35% time remaining

---

### Installation

1. **Clone the repository**:
   ```bash
   gh repo clone mariarodr1136/OperationRoomba
   ```
   Or via HTTPS:
   ```bash
   git clone https://github.com/mariarodr1136/OperationRoomba.git
   ```

2. **Open the game**:
   ```bash
   open OperationRoomba/index.html
   ```
   Or simply double-click `index.html` in any file manager. No server, no build step, no dependencies.

---

### Contributing

Contributions are welcome. To contribute:

1. Fork the repository.
2. Create a new branch:
   ```bash
   git checkout -b feat/your-feature-name
   ```
3. Make your changes and commit with a clear message:
   ```bash
   git commit -m "your commit message"
   ```
4. Push your branch and open a pull request with a clear description of your changes.

---

### Contact

For questions or feedback: mrodr.contact@gmail.com
