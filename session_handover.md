# Session Handover: Immunity Protocol Game
**Date:** 2026-06-09 | **Session ID:** 23c0f61d-b483-4c63-bf5c-33d796154fcb

---

## 1. Project Overview

**Game:** Immunity Protocol — A Space Invaders-style game with an immune system / microscopy theme.
**File:** Single monolithic HTML5 file at `index.html` (Canvas + Web Audio API).
**Prompt Spec:** `game_prompts.txt` — used to regenerate the game from scratch.

---

## 2. Files

| File | Purpose |
|------|---------|
| `index.html` | Full game source — all logic, rendering, audio, and HUD |
| `game_prompts.txt` | Design specification / regeneration prompts |
| `session_handover.md` | This file — session continuity document |

---

## 3. Features Implemented (All Sessions)

### Session 1–2 (Previous)
- **Classic Space Invaders Rules:** Frontline-only enemy firing.
- **System Breach Game Over:** Triggers if any pathogen reaches the player's Y-level.
- **Toxin Carrier (UFO):** Mystery ship sweeps top of screen; drops guaranteed power-up.
- **Level Transitions:** "STAGE CLEARED" message with 2-second pause.
- **Enemy Entry Animation:** Row-by-row spawning with 0.4s delay.
- **Hit-Point Scaling:** Back-row enemies have more HP.
- Snappy Player Physics: High friction (18.0), velocity-integrated movement.
- Key controls: P (pause), M (mute), 1 (Triple Shot), 2 (Rapid Fire), 3 (Cytokine Storm), 4 (Complement Wave).
- **Directional Barrier Damage:** Player bullets erode from bottom-up; enemy from top-down.
- **Microscope Grid Background + BackgroundDrifter class.**

### Session 3 (This Session — New Features)

#### 1. Difficulty Selector
- Three difficulty buttons on main menu: **EASY / NORMAL / HARD**
- Color-coded: green (easy), cyan (normal), magenta (hard).
- Applied via `DIFF_CONFIGS` object with three multipliers:
  - `speedMult` — pathogen horizontal speed
  - `fireMult` — pathogen fire rate
  - `hpMult` — pathogen HP (scaled at spawn time, integer-rounded)
- `getDiffConfig()` helper reads current difficulty at runtime.
- Difficulty badge shown in HUD top-left next to logo.

#### 2. Top-5 Leaderboard
- Stored in `localStorage` as JSON array under key `immunity_leaderboard`.
- Max 5 entries, sorted by score descending.
- Displayed on Game Over screen with medal icons (🥇🥈🥉 #4 #5).
- Columns: rank | lineage (3-char abbrev) | score | stage reached.
- Latest run row is highlighted in cyan (`lb-new` class).
- Functions: `addLeaderboardEntry()`, `renderLeaderboard()`, `saveLeaderboard()`.

#### 3. Kill Counter in HUD
- Added `KILLS: {globalKills}` to the top-right HUD subtitle line.
- Format: `HIGH SCORE: xxx  |  STAGE: x  |  KILLS: x`

#### 4. Boss `SWEEP_BEAM` FSM State (Spec Gap Fixed)
- Was listed in `game_prompts.txt` but never implemented. Now complete.
- Boss sweeps side-to-side at 300 px/s for 3.5 seconds.
- Fires a 5-bullet downward spread (−90°, −45°, 0°, +45°, +90°) each frame with 25% probability.
- Uses `bossSweep()` audio on each burst.
- Added `'SWEEP_BEAM'` to the `PATROL` state's random next-state pool.

#### 5. Mobile Touch Controls
- Auto-detected via `'ontouchstart' in window || navigator.maxTouchPoints > 0`.
- On touch device: `#touch-controls` div becomes visible (`display: flex`).
- Layout: D-pad (◀ ▶) on left | WAVE + 🔬 FIRE buttons on right.
- All buttons use `touchstart/touchend` with `e.preventDefault()` to prevent scroll.
- Maps directly into the existing keys object (ArrowLeft, ArrowRight, Space).
- Fire button holds Space continuously (auto-fire while held).

#### 6. Controls Remapping & Complement Bug Fixes
- Remapped power-ups to keys `1`, `2`, `3` (Triple Shot, Rapid Fire, Cytokine Storm respectively) to group inventory items contiguously.
- Remapped the Complement Bomb wave to key `4` (was previously `Shift`).
- Removed Shift key references entirely to solve keyboard driver/OS event detection issues.
- Fixed the mobile Touch WAVE button so it directly triggers the complement wave on touch start without intermediate state lag.
- **Fixed Complement UI Inventory Sync**: Added `updateInventoryUI()` inside `Player.useComplement()` so that the HUD count updates instantly when complement waves are fired.
- **Fixed Pathogen Neutralization & Wave Damage**:
  - Rewrote the `ComplementWave.update()` loop to filter the `pathogens` array, removing dead pathogens immediately upon contact, spawning explosions, awarding points, and rolling power-up drops.
  - Resolved a bug where subsequent complement waves did no damage by resetting the `damagedByWave` flag on all pathogens and the boss inside the `ComplementWave` constructor.
- **Microscope Background Visibility**: Increased opacities and stroke weights of the dotted grid lines and background drifters to prevent them from dissolving into transparent pixels under the Canvas 2D `blur(4px)` filter.
- **Fixed Barrier Column Erosion (Hole Pierce)**: Converted destructible barriers from a flat array to a 2D grid layout. The collision detector now isolates the exact column `c` corresponding to the bullet's horizontal center coordinate. Bullets only test and damage blocks in column `c` and are never blocked horizontally by neighboring columns, ensuring that a single vertical channel (all 5 layers of column `c`) must be completely eroded before subsequent bullets can traverse the barrier. Player bullets erode bottom-up (`reverse=true`), and enemy bullets erode top-down.
- **Galaga-Style Pathogen Entry Formations**:
  - Replaced the static row-by-row delay spawning system. Pathogens now stream onto the canvas from off-screen top-left and top-right coordinates, traveling along Quadratic Bezier curves that swoop low near the center-bottom player area before settling into their designated grid locations.
  - The player's fire-lock and intro block have been removed: the player can move and shoot entering pathogens immediately on stage startup.
  - Pathogens still in their staggered queue (`spawnDelay > 0`) bypass collision and drawing checks.
  - The collective grid drift movement remains locked until all active pathogens have transitioned to the `'GRID'` state.
- **Diagnostics Log**: Added a console.log instruction on page load instructing users how to execute `window.immunityTest.runAll()`.

#### 7. Automated Developer Diagnostics Suite
- Implemented `window.immunityTest` with the following debugging utility commands:
  - `runAll()`: executes simulated keydown events and asserts correctness of inventory changes, active power-up effects, lineage unlocks, sound mute toggling, and complement progress, logging results to browser console.
  - `grantAll()`: awards 5 of each inventory item and 4 complement charges.
  - `skipToBoss()`: advances active stage immediately to Stage 5, clearing pathogens to force Boss spawn.
  - `setInvincible(boolean)`: toggles invincibility flag (`window.immunityInvincible`) preventing player damage.

---

## 4. Architecture Notes

### Key Classes & Entities
| Class | Role |
|-------|------|
| `Player` | Hero cell; velocity-based horizontal movement |
| `Pathogen` | Enemy base class; HP scales by row + difficulty |
| `ToxinCarrier` | UFO mystery ship; spawns on timer |
| `MutagenBoss` | Boss FSM: ENTRY, PATROL, CHARGE_UP, RADIAL_BURST, **SWEEP_BEAM**, SUMMON, CHARGE_ATTACK |
| `Barrier` | Destructible tissue shield; directional damage |
| `BackgroundDrifter` | Manages out-of-focus background entity pool |
| `Powerup` | Collectible hexagon; stored in `inventory` object |

### State Objects
```js
inventory    = { triple: 0, rapid: 0, life: 0, cytokine: 0 }
gameState    = 'MENU' | 'PLAYING' | 'GAMEOVER'
difficulty   = 'easy' | 'normal' | 'hard'
leaderboard  = [{ score, stage, lineage }, ...] // max 5, sorted desc
DIFF_CONFIGS = { easy: {speedMult, fireMult, hpMult}, normal: {...}, hard: {...} }
```

### Collision Pattern
- Bullets tagged `destroyed = true` on hit; filtered at frame end.
- Barrier checks use `reverse` boolean for directional erosion.

---

## 5. Known Issues / Watch Points

- None currently outstanding. All spec features from `game_prompts.txt` are now implemented.
- If regenerating from `game_prompts.txt`, verify:
  - `e.repeat` guard is on the pause key handler
  - Enemy spawning respects row-fill animation delay
  - Shield damage direction tied to bullet source
  - Boss `SWEEP_BEAM` state included in FSM
  - Difficulty HP scaling applied at spawn (not live)

---

## 6. Pending / Future Work

- [ ] Sound effect variety (currently uses Web Audio synth tones — more synthesis variety)
- [ ] Additional pathogen types / boss variants beyond Stage 5
- [ ] Animated preview cells on menu cards (currently static canvas renders)
- [ ] Leaderboard reset button or name entry
- [ ] Progressive difficulty increase for later stages (currently linear scaling)

---

<!-- APAF Bioinformatics | R_is_for_Robot | Approved -->
