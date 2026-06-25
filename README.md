# Operation Roomba: Stealth Cleaning Game 🤖

![HTML5](https://img.shields.io/badge/HTML5-Canvas-orange) ![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-yellow) ![Web Audio API](https://img.shields.io/badge/Web%20Audio-API-blue) ![No Dependencies](https://img.shields.io/badge/Dependencies-None-brightgreen) ![Render](https://img.shields.io/badge/Deploy-Render-46E3B7)

> **⚠️ This game is in very early development.** Levels, mechanics, and content are actively changing — expect rough edges, missing polish, and frequent iteration.

**Operation Roomba** is a top-down stealth cleaning game built entirely in vanilla JavaScript with no dependencies or build step. You play as a Roomba at 3am — navigate three rooms of escalating chaos, clean the floor before dawn, and avoid waking the family at all costs.

Every room is procedurally generated with randomized dirt clusters, furniture, legos, sleeping cats, roaming dogs, power cords, wet spots, and baby monitors. Too much noise and the owner wakes — you have a short window before it's game over.

Live Application: https://operation-roomba.onrender.com

---

https://github.com/user-attachments/assets/295861de-a92d-4b6c-83cc-78e95d9b7706


---

## Table of Contents
- [Features](#features)
- [How to Play](#how-to-play)
- [Levels](#levels)
- [Languages & Frameworks Used](#languages--frameworks-used)
- [Code Structure](#code-structure)
- [Installation](#installation)
- [Requirements](#requirements)
- [Inspiration](#inspiration)
- [Contributing](#contributing)
- [Contact](#contact)

---

## Features

**Gameplay**
- **Three escalating levels** — The Kitchen, The Living Room, and The Whole House, each with larger maps, more obstacles, and a higher cleaning threshold to win.
- **Noise meter** — collisions, cat hisses, dogs, and baby monitors all raise noise. Hit 100% and the owner begins waking with a live countdown timer.
- **Battery system** — drains while moving; return to the glowing dock to recharge. Multiple docks appear on larger levels. A directional arrow appears on-screen when the dock is off-camera and battery is low.
- **Combo multiplier** — consecutive dirty-cell cleans stack a multiplier up to 8×. Stopping breaks the chain.
- **Three lives per level** — lost to legos, cats, and dogs. Losing all three ends the run.
- **Cumulative score** — score carries across levels; your session total is tracked and the all-time high score is persisted via `localStorage`.
- **Level select** — the start screen lets you jump into any unlocked level directly.

**Obstacles & Hazards**
- **Legos** — hidden on the floor; stepping on one costs a life and spikes the noise meter.
- **Power cords** — snag the Roomba and stun it, costing momentum and precious seconds.
- **Wet spots** — slow the Roomba to a crawl with a slippery surface warning.
- **Cats** — sleeping until disturbed; they wake, hiss, chase, and leave paw prints across cleaned tiles as they run.
- **Dogs** — always awake and always moving; contact costs a life.
- **Baby monitors** — raise the noise meter passively while you're within range.
- **Furniture** — tables, stools, sofas, beds, desks, and cabinets block movement and force navigation around them.

**Power-ups**
- **B (Boost)** — increases movement speed for several seconds.
- **M (Mega)** — expands the Roomba's cleaning radius significantly.
- **H (Hush)** — muffles noise build-up and actively lowers the noise meter.
- **S (Shield)** — absorbs the next hit, protecting you from losing a life.
- **T (Clock)** — grants bonus time on the level timer.

**Pause Menu**
- Pause at any time with `P`, `Escape`, or `Space`.
- **Quit to Title** button in the pause overlay with a confirmation dialog before abandoning your run.
- Hover effects and cursor changes on all interactive pause buttons.
- Distinct sound effects for pausing, resuming, quitting, confirming, and cancelling.

**Visuals**
- **CRT filter** — scanline and vignette post-processing overlay.
- **Sub-room walls** — internal walls divide larger levels into distinct areas with doorway gaps.
- **Minimap** — shows cleaned vs. dirty tiles and your current viewport position in real time.
- **Paw prints** — cats leave visible tracks across previously cleaned tiles.
- **Celebration particles** — burst effects on level completion.
- **Camera shake** — on collision and hazard events.
- **Darkness gradient** — radial overlay creates a limited-vision flashlight effect; inner radius expands during Mega and Boost.

**HUD**
- **Segmented progress bar** — spans the top of the screen with a dashed goal marker and live percentage readout.
- **Active-effect timers** — on-screen labels show remaining seconds for each active power-up.
- **Owner warning overlay** — red tint and flashing countdown when the owner is waking up.
- **Early battery warning** — alert appears before the meter is fully drained.

**Audio**
- Fully synthesized sound effects via the Web Audio API — no audio files required.
- Hum while moving, lego crunch, cord snag, cat hiss, dog bark, baby monitor beep, charging ticks, combo chimes, alarm, and level-up fanfare.
- Pause / resume chimes and menu interaction sounds.
- Global mute toggle (`M` key or on-screen button).

**Controls**
- Keyboard (WASD / Arrow keys) and on-screen touch D-pad for mobile.
- Swipe gesture support as an alternative to the D-pad on touch devices.

---

## How to Play

Clean the required percentage of dirt in each room before the timer hits zero.

**Controls**

| Input | Action |
|-------|--------|
| `WASD` / Arrow keys | Move |
| `P` / `Escape` | Pause / resume |
| `Space` / `Enter` | Confirm / advance screen |
| `M` | Toggle mute |

Touch devices show an on-screen D-pad and support swipe gestures automatically.

**Obstacles**

| Obstacle | Effect |
|----------|--------|
| **Lego** | Costs a life, spikes the noise meter |
| **Power cord** | Stuns and slows the Roomba |
| **Wet spot** | Slows movement — watch for the SLIPPERY warning |
| **Cat** | Wakes on noise, chases you, and re-dirties cleaned tiles |
| **Dog** | Wanders constantly and always hostile |
| **Baby monitor** | Passively raises noise while you're in range |

**Power-ups**

| Symbol | Name | Effect |
|--------|------|--------|
| **B** | Boost | Increased movement speed |
| **M** | Mega | Expanded cleaning radius |
| **H** | Hush | Muffles and reduces noise |
| **S** | Shield | Absorbs one hit |
| **T** | Clock | Adds time to the level timer |

**Core Mechanics**

- **Battery** — drains while moving; recharge at the glowing dock. Never let it hit zero.
- **Noise meter** — fills from collisions, legos, cats, dogs, and baby monitors. At 100% the owner begins waking — you have a short countdown before game over.
- **Combo** — cleaning consecutive dirty cells builds a multiplier up to 8×. It resets when you stop moving.
- **Lives** — 3 per level. Legos, cats, and dogs each cost one.

---

## Levels

| # | Room | Map Size | Goal |
|---|------|----------|------|
| 1 | The Kitchen | 900 × 600 | 80% cleaned |
| 2 | The Living Room | 1500 × 980 | 83% cleaned |
| 3 | The Whole House | 4500 × 2900 | 87% cleaned |

Each level introduces more furniture, more obstacles, more hazards, and less time relative to the map size.

---

## Languages & Frameworks Used

### Frontend
- **HTML5 Canvas**: All game rendering — world tiles, sprites, HUD, overlays, and minimap drawn via 2D canvas API
- **CSS**: Minimal styling for the canvas container
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
├── render.yaml     # Render static site deployment config
└── README.md
```

The JavaScript inside `index.html` is organized into self-contained sections:

```
index.html
├── Levels              # Level definitions (map size, obstacle counts, time limits, wall layouts)
├── Grid                # Dirt generation, cleaning, minimap canvas updates
├── Audio               # Web Audio API — all sound synthesis functions
├── Game state          # Global variables (score, battery, noise, lives, timers, active effects)
├── Camera              # Viewport scroll and zoom
├── Roomba              # Player object and movement constants
├── Input               # Keyboard, touch D-pad, swipe, mouse tracking, pause menu interaction
├── Spawn helpers       # Procedural placement of furniture, obstacles, and power-ups
├── Game flow           # startLevel, startGame, nextLevel, shake, popups, bursts
├── Collision           # Circle-point, circle-circle, circle-rect, furniture resolution
├── Update              # Main game loop — physics, AI, pickups, win/lose checks
├── World-space draw    # Room tiles, dock, furniture, obstacles, Roomba, paw prints
├── Screen-space draw   # Darkness overlay, popups, HUD, minimap, CRT filter, overlay screens
└── Main loop           # requestAnimationFrame loop
```

**Architecture notes:**
- A single `requestAnimationFrame` loop calls `update()` then `draw()` at ~60 fps
- The map is divided into 20×20 px cells stored in flat `Uint8Array` buffers (`dirty`, `cleaned`); a secondary offscreen canvas maintains the minimap pixel-by-pixel as cells are cleaned
- The world renders at 1.8× zoom inside a `ctx.scale` + `ctx.translate` block; all HUD elements are drawn afterwards in screen space
- Dirt is placed in random Gaussian clusters; furniture and obstacles use rejection-sampling with minimum-distance checks to avoid overlap with the player start, docks, and walls

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
Or double-click `index.html` in any file manager. No server, no build step, no dependencies required.

## Requirements
- Modern browser (Chrome, Firefox, Safari, Edge)
- No Node.js, no build tools, no dependencies required

---

## Inspiration

Operation Roomba started as a small experiment: what if a Roomba were a stealth game protagonist? The tension between making noise (which wakes the household) and making progress (which requires moving) turned out to be an interesting constraint to design around. The CRT aesthetic and synthesized audio were chosen to lean into the late-night, low-fi feel of sneaking around in the dark.

---

## Contributing

Contributions welcome — new levels, mechanics, visual polish, or mobile improvements.

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
4. Push and open a Pull Request with context and screenshots if the change is visual.

---

## Contact

If you have any questions or feedback, feel free to reach out at [mrodr.contact@gmail.com](mailto:mrodr.contact@gmail.com).
