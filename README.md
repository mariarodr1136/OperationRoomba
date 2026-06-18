# Operation Roomba

A top-down stealth cleaning game. You are a Roomba at 3am — clean the house before dawn without waking anyone up.

## Play

Open `index.html` in any modern browser. No build step, no dependencies.

## How to Play

Clean **80% of the dirt** in each room before the timer runs out. Every obstacle makes noise, and too much noise wakes the owner.

### Controls

| Input | Action |
|-------|--------|
| `WASD` / Arrow keys | Move |
| `P` / `Escape` | Pause |
| `M` | Toggle mute |
| `Space` / `Enter` | Confirm / advance screen |

Touch devices show an on-screen D-pad automatically.

### Obstacles

| Obstacle | Effect |
|----------|--------|
| **Lego** | Costs a life and spikes noise |
| **Sock** | Stuns the Roomba for 2 seconds |
| **Cat** | Wakes up and chases you — re-dirties cleaned tiles as it runs |
| **Dog** | Wanders constantly, always hostile |
| **Baby monitor** | Raises noise meter while you're nearby |

### Power-ups

| Icon | Name | Effect |
|------|------|--------|
| **B** | Boost | 3× speed for 6 seconds |
| **M** | Mega | Doubled cleaning radius for 6 seconds |
| **H** | Hush | Reduces and slows noise build-up for 8 seconds |

### Mechanics

- **Battery** — drains while moving, recharges at the glowing dock. Watch for spinning sparks when charging.
- **Noise meter** — fills from collisions and baby monitors. At 100% the owner wakes and you have a countdown to escape.
- **Combo** — cleaning consecutive dirty cells multiplies your score (up to 8×).
- **Lives** — 3 lives per level. Losing all lives ends the run.

## Levels

| # | Room | Notes |
|---|------|-------|
| 1 | The Kitchen | Tutorial-sized, no cats or dogs |
| 2 | The Living Room | First cat and baby monitor |
| 3 | The Hallway | Long corridors, dogs join |
| 4 | The Bedroom | Multiple cats, dogs, and monitors |
| 5 | The Whole House | Maximum chaos |

## Difficulty

Choose at the start screen. Affects time limit, lego count, and owner patience.

- **Easy** — 40% more time, 40% fewer legos, owner takes 5s to wake
- **Normal** — default balance
- **Hard** — 30% less time, 50% more legos, owner wakes in 2s

Best score is saved locally across sessions.

## Star Ratings

Each level awards 1–3 stars on completion:

- ⭐ — cleared 80% of dirt
- ⭐⭐ — cleared 85%+ dirt
- ⭐⭐⭐ — cleared 95%+ dirt with at least 35% time remaining

## Tech

Single `index.html` file — HTML5 Canvas, vanilla JavaScript, Web Audio API for all sound effects. No frameworks, no build tools.
