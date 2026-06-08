# ⚽ PIXEL KICK — Retro Arcade Soccer

> *A first-person pixel art penalty shootout with full arcade polish, CRT aesthetics, and cinematic kick animation. Zero dependencies. One HTML file.*

---

## Screenshot / Preview

```
┌─────────────────────────────────────────────────────────────┐
│  🌙  ★  ·  ★  ·  ·  ★         PIXEL KICK        ★  ·  ★  │
│  [crowd][crowd][crowd][crowd][crowd][crowd][crowd][crowd]    │
│  ╔═══════════════════════════════════════════════════════╗  │
│  ║  . . . . . . NET . . . . . . . . . . . . . . . . .  ║  │
│  ║  │                    [GK]                         │  ║  │
│  ╚══╧═══════════════════════════════════════════════════╧══╝ │
│     ┌──────────────────────────────────────────────────┐    │
│     │              ⚽  [ball in flight]                 │    │
│     └──────────────────────────────────────────────────┘    │
│                    [LEFT LEG]  [RIGHT LEG]                   │
│  ░░░░░░░░░░░░░░░ CRT SCANLINES OVERLAY ░░░░░░░░░░░░░░░░░░  │
└─────────────────────────────────────────────────────────────┘
```

---

## About

**PIXEL KICK** is a browser-based, single-file retro arcade game built entirely with vanilla HTML5 Canvas and JavaScript — no libraries, no frameworks, no build tools. It captures the look and feel of a late-1980s arcade cabinet: chunky pixel art, glowing phosphor fonts, scanline overlays, screen shake, and chiptune-adjacent visual feedback.

You take penalties from the spot. You have **5 shots**. Make as many goals as you can.

---

## Features

### 🎮 Gameplay
- **5-shot penalty round** per game with score and combo tracking
- **Hold & release** shooting mechanic — hold Space to charge power, release to fire
- **Aim freely** with the mouse or arrow keys before each shot
- **Combo multiplier** for consecutive goals (displayed on screen)
- **Performance rating** at full time based on goals scored
- **Pixel trophy** awarded for 4+ goals

### ⚽ Shot Outcomes (randomised each kick)
| Outcome | Probability | Description |
|---|---|---|
| **Goal** | 40% | Ball flies into the net |
| **Save** | 22% | Goalkeeper dives and stops it |
| **Post** | 12% | Ball rattles the woodwork |
| **Miss** | 8% | Wide or over the bar |
| **Curved Shot** | 18% | Rare banana-ball that bends in |

### 🦵 First-Person Visuals
- Fully animated **pixel-art legs and cleats** always visible at the bottom of screen
- Three-phase **kick animation**: windup → strike → follow-through
- **Camera bobbing** while aiming to simulate the player's weight shift
- **Ball spin and arc** during flight with perspective scaling (shrinks as it travels away)
- Animated **ball shadow** on the pitch surface

### 🥅 Stadium & Atmosphere
- **Night stadium** with starfield, crescent moon, and 4 floodlight towers with animated light beams
- **4-row pixel crowd** that bobs and reacts to goals
- **Goalkeeper** in orange who dives to the correct side on saves, with gloves and idle sway
- Detailed **goal structure** with crossbar, posts, depth lines, and a semi-transparent net grid
- Painted **pitch markings**: penalty box, 6-yard box, penalty arc, penalty spot, corner flags

### ✨ Arcade Polish
- **Particle explosions** on goals (80 confetti stars + 30 crowd throw-ins)
- **Post spark particles** when the ball hits the woodwork
- **Save spark particles** on goalkeeper stops
- **Screen shake** scaled to outcome intensity (goals shake hardest)
- **CRT scanline overlay** via CSS repeating gradient
- **Vignette** darkening toward screen edges
- **Random screen flicker** simulating phosphor instability
- **Animated moving scanline** sweep effect
- **Power charge arc** drawn around the crosshair as you hold Space
- Glowing **Press Start 2P** and **VT323** retro fonts throughout

---

## Controls

| Input | Action |
|---|---|
| **Mouse move** | Aim the crosshair |
| **Arrow keys** | Aim the crosshair (no page scroll) |
| **Hold Space** | Charge shot power (bar fills up) |
| **Release Space** | Fire the shot |
| **Click** | Start game / advance after result / restart |
| **Space / Enter** | Start game / advance after result / restart |

> **Tip:** A fully charged shot (100% power) travels faster and hits harder — but the outcome is still randomised, so timing your charge is about feel, not fate.

---

## How to Run

No installation required. Just open the file in any modern browser:

```bash
# Option 1 — double-click
open retro_soccer.html

# Option 2 — local server (avoids any CORS quirks)
python3 -m http.server 8080
# then visit http://localhost:8080/retro_soccer.html

# Option 3 — VS Code Live Server extension
# Right-click retro_soccer.html → "Open with Live Server"
```

**Browser compatibility:** Chrome, Firefox, Safari, Edge — any browser with HTML5 Canvas support (effectively all browsers since 2012).

**Internet connection:** Required on first load to fetch the two Google Fonts (`Press Start 2P` and `VT323`). The game runs fully offline once fonts are cached.

---

## File Structure

Everything lives in a single file:

```
retro_soccer.html
├── <style>        — CRT overlay, scanline animation, HUD, title screen, fonts
├── <canvas>       — 640×480 game viewport (pixelated rendering)
├── HUD overlays   — Score, shots remaining, outcome text (CSS-positioned divs)
└── <script>
    ├── State machine       (title → aiming → kicking → result → gameover)
    ├── Input handling      (keyboard + mouse, no-scroll prevention)
    ├── Game logic          (shoot, processResult, nextShot, initGame)
    ├── Particle system     (goal confetti, post sparks, save sparks, crowd throws)
    ├── Scene rendering     (sky, crowd, pitch, goal, goalkeeper, ball, legs)
    ├── Kick animation      (windup / strike / follow-through phases)
    ├── Camera bob          (sinusoidal vertical offset while aiming)
    ├── Screen shake        (random offset with exponential decay)
    └── CRT effects         (flicker, vignette — canvas-side complement to CSS)
```

---

## State Machine

```
[title] ──SPACE/CLICK──► [aiming] ──SPACE hold/release──► [kicking]
                              ▲                                 │
                              │                          ball animation
                              │                                 │
                         [result] ◄─────── processResult() ────┘
                              │
                    (auto after 2s, or SPACE/CLICK)
                              │
                    shotsLeft > 0 → back to [aiming]
                    shotsLeft = 0 → [gameover]
                              │
                         SPACE/CLICK
                              │
                           [aiming]  (new game)
```

---

## Scoring & Ratings

| Goals / 5 shots | Rating |
|---|---|
| 5 | WORLD CLASS |
| 4 | GREAT PLAYER 🏆 |
| 3 | DECENT SHOT |
| 2 | NEEDS PRACTICE |
| 1 | MISSED THE NET |
| 0 | MISSED THE NET |

---

## Technical Notes

- Canvas resolution is fixed at **640×480** — the classic VGA resolution and a nod to arcade monitor standards of the era.
- All drawing uses `Math.round()` on coordinates to keep pixels crisp and avoid sub-pixel blurring.
- The `image-rendering: pixelated` CSS property ensures the canvas scales without interpolation on high-DPI displays.
- `requestAnimationFrame` drives the game loop; no fixed timestep is used (runs at display refresh rate).
- The outcome is determined at the moment of shooting — the ball animation plays out the pre-determined result cinematically.

---

## Possible Extensions

- **Sound effects** via the Web Audio API (crowd roar, boot thud, post clang, net swish)
- **Hi-score persistence** with `localStorage`
- **Difficulty modes** adjusting outcome weights (Easy: more goals; Hard: more saves)
- **Multiple camera angles** — side-on or isometric view as an unlock
- **Multiplayer** alternating shots on the same device
- **Mobile touch controls** — tap and hold for charge, swipe direction for aim

---

## Credits

Built with vanilla HTML5 Canvas, CSS, and JavaScript.
Fonts: [Press Start 2P](https://fonts.google.com/specimen/Press+Start+2P) and [VT323](https://fonts.google.com/specimen/VT323) via Google Fonts.

*Insert coin to continue.*
