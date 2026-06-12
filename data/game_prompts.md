# Recreating "Immunity Protocol" — Detailed Prompt Breakdown

**Language Stack:** Single-file HTML5 web application containing embedded Vanilla CSS and modern Vanilla JavaScript, using the HTML5 Canvas 2D API and Web Audio API.

---

## Core Game Prompts

### Prompt 1: HTML & CSS Canvas Layout
> Create a single-file HTML5 web application. Implement a responsive, fixed-aspect-ratio Canvas element with a native resolution of 900x1000. The canvas must be centered on the screen. Apply a dark microscopic theme using Vanilla CSS for styling (body background, layout, canvas sizing).

---

### Prompt 2: Procedural Audio Synthesizer
> Implement a procedural Web Audio system in Vanilla JavaScript using OscillatorNodes, GainNodes, and BiquadFilterNodes (no external audio files or libraries allowed). Configure the following audio synthesizer functions:
> - `shoot`: high-frequency square wave sweeping upward quickly.
> - `hit`: low triangle wave blending into noise decay.
> - `lyse` (explosion): sawtooth frequency drop with a white noise burst.
> - `powerup`: ascending major triad arpeggio.
> - `levelUp`: three-note rising melody.
> - `playerHit`: low sawtooth rumble with lowpass filtered noise.
> - `gameOver`: descending sawtooth sweep.
> - `bossWarn`: repeating alarm square wave tones.
> - `complement`: rising triangle sweeps mixed with high-frequency noise.
> - `bossSweep`: white noise sweeps mixed with low sawtooth tones.

---

### Prompt 3: Core Controller and Audio Interactive Hotkeys
> Add keyboard event listeners:
> - `M` to toggle audio mute state.
> - `P` to toggle the game pause state.
> - `1`, `2`, `3` to activate collected power-ups from the player cell's inventory (Triple Shot [1], Rapid Fire [2], Cytokine Storm [3] respectively).
> Display these keys and their active inventory counts in the UI.
> Additionally, display the Complement Charge count with key `4` as the activation key in the same panel.
> *Note:* Extra Life (+1) power-ups do NOT use a key — they are auto-applied on contact.

---

### Prompt 4: Destructible Barriers Design & Structure
> Implement four organic barriers positioned above the player. Construct each barrier using a 2D grid of 5 rows and 10 columns (block size 6x6px with 1px padding). When a block takes damage, decrease its health and color intensity/opacity.

---

### Prompt 5: Player Movement Controller
> Design the player input controller:
> - **Movement/Controls:** Left/Right movement via Arrow Keys or A/D.
> - **Snappy physics:** Friction (e.g. 10.0 to 18.0) and high acceleration (speed * friction * 1.5) to allow precise positioning of the cell without slidy horizontal drift.
> - **Smooth control:** Smooth the raw horizontal input using linear interpolation (lerp) before accelerating (e.g. `targetDir += (moveDir - targetDir) * dt * 15.0`) to give the player cell a luxurious, fluid movement feel simulating motion through a viscous medium.

---

### Prompt 6: Playable Lineage - Macrophage, Natural Killer, and Neutrophil
> Design the Macrophage (Balanced Phagocyte) lineage:
> - Cyan color
> - HP max 5, start with three
> - Standard speed (360px/s)
> - Fire rate one bullet per 0.28s
> - 3 starting lives (max 5)
> - Y-shaped antibody bullets
> - Single-lobed shifting nucleus
>
> Design the Natural Killer Cell (high fire power) lineage:
> - Magenta color
> - Projects massive perforin rings piercing up to three pathogens.
> - HP max 4, start with three
> - Speed 320 px/s
> - Fire Rate: three bullets per 0.3s
> - Ring shaped perforin bullets
>
> Design the Neutrophil (high speed) lineage:
> - Rapid-responder granulocyte. Launches rapid stream of yellow granule bullets
> - HP (max 3), start with three
> - Speed 470 px/s
> - Fire Rate: One bullet per 0.23s
> - Dot shaped granule bullets

---

### Prompt 7: Complement Bomb Wave System
> Implement a Complement Bomb system: pressing key `4` uses 1 charge (calling `updateInventoryUI()` immediately) to generate an expanding golden wavefront starting at the player, clearing all hostile bullets, neutralizing standard pathogens (dealing 10 damage), and damaging the boss (dealing 15 damage). Accumulate progress on kills, awarding 1 charge every 45 kills up to a maximum of 4 charges. Display complement charge count in the inventory HUD panel with label 'Complement: X/4 [4]' in gold colour. Update the panel immediately when a new charge is earned or used, and ensure that the wave resets the 'damagedByWave' state of active pathogens and the boss at instantiation so subsequent waves damage them properly.

---

### Prompt 8: Standard Pathogen Grid Formation
> Create the standard pathogen grid formation and movement:
> - Arrange 4 rows × 8 columns of pathogens in an invaders-like grid.
> - At the start of every stage (including stage 2 onwards), reset the grid to start at Y = 110px from the top of the canvas so pathogens always begin high on screen regardless of where the previous stage ended.
> - Arrange rows so that the toughest pathogens (3 HP Spike Virus) occupy the top rows, medium (2 HP Bacteria) in the middle, and weakest (1 HP Virus) at the bottom rows nearest the player.
> - The grid moves sideways and descends by 16px each time it hits a canvas side bound (increasing downward pressure per bounce).
> - Grid horizontal speed scales up as pathogen numbers decrease.
> - Only the frontline pathogen (the lowest active one in each column) can fire.

---

### Prompt 9: Pathogen Combat Rules
> Implement pathogen combat restrictions:
> - Restrict pathogen random firing so that only the frontline pathogens (the lowest active pathogen in each column) can fire.
> - If standard pathogens breach the defensive line and reach the player's Y coordinate level, trigger Game Over.

---

### Prompt 10: Pathogen Types Specification
> Implement the following pathogen types:
> - **Virus (1 HP):** Red capsid core with 8 vibrating outer spikes.
> - **Bacterium (2 HP):** Green rod-shaped capsule with tail flagella and ciliated surface. Fires green bullets.
> - **Spike Virus (3 HP):** Amber spherical core with yellow receptor nodes. Fires amber spread bullets.

---

### Prompt 11: Mutagen Boss FSM State Machine
> Implement a Mutagen Boss ('Mutagen Cell') that spawns every 5 stages. Give the Boss 60 HP, a segmented HP bar (thirds), and a finite state machine (FSM) with the following states:
> - `ENTRY`: enters from top center.
> - `PATROL`: horizontal drift while firing triple shots. Randomly transitions to one of: `CHARGE_UP` or `CHARGE_ATTACK`.
> - `CHARGE_UP`: visual charging effect (boss glows gold). After 1.8s, plays `bossWarn()` and transitions to `RADIAL_BURST`.
> - `RADIAL_BURST`: fires bullets in 16 radial directions, then returns to `PATROL`.
> - `CHARGE_ATTACK`: locks player position, swoops down vertically for 1.2s, then returns up to y=220.

---

### Prompt 12: Power-Ups & Drop System
> Implement power-ups dropping on death with a low (5%) chance from enemies. Power-up types and collection rules:
> - **T-Helper (T):** Gold hexagon. Stored in inventory. Key `1` activates triple-shot weapon behavior for 12 seconds.
> - **Memory Cell (M):** Cyan hexagon. Stored in inventory. Key `2` reduces weapon cooldown by 45% (rapid fire) for 12 seconds.
> - **Plasma Cell (+1):** Green hexagon. AUTO-APPLIED IMMEDIATELY on contact — adds 1 life if below max capacity. Does NOT enter the inventory and has NO key binding. The player sees green particles burst and life count increase instantly.
> - **Cytokine Storm (C):** Magenta hexagon. Stored in inventory. Key `3` instantly damages all active pathogens and shakes screen. Ensure that if the Cytokine Storm clears the final active pathogen, the game engine correctly detects this and advances the player to the next stage or spawns the boss.

---

### Prompt 13: Collision Matrix
> Implement full collision handlers for:
> - Player bullets vs Barriers and Pathogens. Ensure player bullets correctly damage the organic barrier blocks.
> - Enemy bullets and body contact vs Barriers and Player.
> - Player contact with Power-ups: life power-ups auto-apply; all others add to inventory.

---

### Prompt 14: HUD & Bottom Control Panel UI
> Develop the HUD & bottom overlay layouts:
> - Top overlay containing Game Logo (with difficulty badge), Selected Lineage Name, Score, High Score, Stage number, Kill counter, Life icons (drawing mini-cells), and Complement charges (4 gold squares).
> - Bottom area displays active power-up timers.
> - Include a full-width translucent control panel along the bottom of the canvas showing: Sound state [M] and Pause state [P] on the left, and a power-up legend using coloured hexagon symbols (matching in-game drop appearance) next to each entry:
>   - Gold hexagon labelled 'T' → TRIPLE SHOT [1] with current inventory count
>   - Cyan hexagon labelled 'M' → RAPID FIRE [2] with current inventory count
>   - Magenta hexagon labelled 'C' → CYTOKINE STORM [3] with current inventory count
>   - Green hexagon labelled '+1' → EXTRA LIFE (auto-applied on pickup, no activation key)
>   - Complement charge count with key [4] label shown in gold, accompanied by 4 small fill squares indicating current charges.

---

### Prompt 15: Game State Navigation, Pause, & Leaderboard
> Implement transitions and overlays:
> - Difficulty selector on menu: EASY / NORMAL / HARD buttons. `DIFF_CONFIGS` provides `speedMult`, `fireMult`, `hpMult`. `getDiffConfig()` helper reads current difficulty. Difficulty badge shown in HUD.
> - Top-5 persistent leaderboard in `localStorage` (shown on Game Over screen) with medals, lineage, score, stage.
> - Add a 2-second level transition delay showing a 'STAGE CLEARED' text overlay before initializing the next wave or spawning the boss.
> - Add a game pause overlay when `P` is pressed showing 'BIOME DEFENSE PAUSED'.
> - Menu displays lineage cards with unlock progression. Game Over shows run stats and leaderboard.

---

<!-- APAF Bioinformatics | game_prompts.md | Approved | 2026-06-12 -->
