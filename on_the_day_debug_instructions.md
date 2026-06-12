# On-the-Day Debugging Instructions

Welcome to the hands-on debugging session for **Immunity Protocol**. Below is the quick start guide and your main tasks for the workshop.

---

## 1. Quick Start: Setting Up the Repository

- [ ] **Download the Repository:** Get the workshop files from the [GitHub Repository](https://github.com/APAF-bioinformatics/AI_Workshop).
- [ ] **Extract Files:** Unzip the downloaded repository files on your local machine.
- [ ] **Open in IDE:** Open the workspace folder in your **Antigravity IDE** (or preferred editor) and begin exploring the codebase.

---

## 2. Setting Up the Starter Code

> [!NOTE]
> As it takes a significant number of tokens and time to build the entire codebase from scratch, a starter template has been prepared for you.

- [ ] **Move Starter Code:** Copy `data/index_to_debug.html` out of the `data/` directory, place it in the root directory (up one level), and rename it to `index.html`.
- [ ] **Open index.html:** Open the newly renamed `index.html` file and read through its structure.
- [ ] **Inspect Configuration Files:**
  - **`.gitignore`:** Standard configuration preventing Git from tracking unwanted logs, local settings, or temp files.
  - **`.aiignore`:** Custom file that blocks autonomous coding agents (like Antigravity) from parsing sensitive, temporary, or excessively large files to conserve context window tokens.

---

## 3. Reviewing Workspace Skills

Antigravity has already provisioned a helper skill in your local `.gemini/skills/canvas-game-diagnostics/` folder. It provides:
1. **Frame-Rate Independent Physics:** Damping and friction updates using exponential decay equations.
2. **Tunneling Prevention:** Minimum distance calculation formulas to prevent high-speed projectiles from bypassing collision boundaries.
3. **Performance Auditing:** Patterns for matching `ctx.save()` and `ctx.restore()`, and minimizing inner-loop garbage collection.
4. **Web Audio Gesture Handlers:** Templates to resume the browser's `AudioContext` only after explicit user clicks.

---

## 4. Debugging & Implementation Tasks

Complete the following programming challenges in `index.html`:

### Task A: Smooth Player Movement (Ref: Prompt 5)
* **Location:** Locate the movement update code for the player cell.
* **Problem:** The horizontal movement is very jerky, lacks fine control, and jumps position erratically.
* **Goal:** Implement linear interpolation (lerp) or damping to make player movement smooth and viscous.

### Task B: Complement Power-Up UI Alignment (Ref: Prompt 14)
* **Location:** Locate the HUD / bottom HUD status panel rendering code.
* **Problem:** The complement status text at the bottom overflows past the right edge of the canvas bounding box.
* **Goal:** Recalculate alignments/margins to ensure the complement charge meter fits cleanly on all aspect ratios.

### Task C: Layer-by-Layer Barrier Shield Damage (Ref: Prompts 4 & 13)
* **Location:** Identify the bullet-versus-barrier collision handler.
* **Problem:** The barrier blocks currently take damage out of order (starting on row 2 instead of the edge closest to the shooter), and bullets can pass through early.
* **Goal:**
  * Player bullets must damage the bottom-most row first (row 5 up to row 1).
  * Pathogen bullets must damage the top-most row first (row 1 down to row 5).
  * Projectiles should **never** pass through a barrier column to hit cells behind it if active blocks still remain in that column.

### Task D: Resolve Boss Stage Hang & Add Shortcuts (Ref: Prompts 11 & 21)
* **Location:** Locate the stage advancement loop and the boss spawn triggers.
* **Goal:**
  * **Fix Boss Freeze:** Address the stage freeze that happens when spawning the stage 5 boss.
  * **Add Test Shortcut:** Bind the `8` key to immediately jump to the next boss wave (Stage 5, 10, 15, etc.) for testing.
  * **Clean Up:** Once boss stages are fully tested and verified, remove the shortcut key event listener.

### Task E: Cell Selection Unlock Logic (Ref: Prompt 15)
* **Location:** Locate the Lineage Selector screen render/event functions.
* **Goal:** Lock the **Natural Killer** and **Neutrophil** cell lineages initially. Enable selection only when the player's persistent high score exceeds the unlock threshold.

---

## 5. Testing Notes: Managing Cell Unlocks & High Scores

Use your browser's Developer Tools Console (`F12` or `Cmd+Option+I` on Mac) to manage local storage:

* **Clear High Score Only:**
  ```javascript
  localStorage.removeItem('immunity_highscore');
  ```

* **Reset Everything (High Score & Leaderboard):**
  ```javascript
  localStorage.clear();
  ```

* **Instant Selection Unlock (Set Score to 15,000):**
  ```javascript
  localStorage.setItem('immunity_highscore', '15000');
  ```
  *(Note: Run this command in the console and then refresh the page to instantly unlock the Natural Killer and Neutrophil cells without playing through the stages.)*

---

## 6. Optional Advanced Polish & Extension Tasks

### Prompt 16: Design System & Motion Guidelines (Impeccable)
> To ensure the game feels premium and avoids the generic "AI slop" aesthetic, load and reference the custom `impeccable-design` skill in `.gemini/skills/impeccable-design/` during code generation:
> 1. **Motion Design:** Use natural easing curves (such as cubic-bezier deceleration: `cubic-bezier(0.22, 1, 0.36, 1)`) and precise timing (100–300ms for micro-interactions/transitions) for all animations. Avoid linear reveals or elastic bounces.
> 2. **Color & Contrast:** Use the perceptually uniform OKLCH color space for palette definitions. Apply tinted neutrals matching the primary brand hue rather than generic raw grays.
> 
> **Design Guidelines Map:**
> - Motion Design: [animate.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/animate.md) (controls easing curves, timing, and physics velocity damping)
> - Color & Contrast: [colorize.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/colorize.md) (perceptually uniform OKLCH spaces and tinted neutrals)

---

### Prompt 17: Dotted Microscope Grid & Blurred Drifting Background
> Draw a faint static microscope grid on the 2D Canvas using TWO layers of dotted lines:
> - Primary grid (100px spacing): `rgba(0, 242, 254, 0.22)`, dash pattern `[3, 15]`.
> - Secondary finer grid (50px spacing): `rgba(0, 242, 254, 0.08)`, dash pattern `[2, 24]`.
> This creates a realistic reticle-style microscope eyepiece effect.
>
> Implement a depth-of-field effect with a drifting background layer: floating out-of-focus red blood cells, platelets, and faint pathogens (spiked viruses, green rod bacteria, and branched fungi) drifting slowly downward at visible opacity (0.12 to 0.26). Render these background drifters with `ctx.filter = 'blur(4px)'` before drawing and reset to `'none'` after, to simulate specimen that is outside the focal plane of the microscope objective.
>
> **Design Guidelines Map:**
> - Grid Spacing & Layers: [layout.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/layout.md) (rules for geometric alignment, screen layout structure, and padding grid columns)
> - Colors & Transparencies: [colorize.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/colorize.md) (rules for alpha values, layering colors, and contrast balance)
> - Drifting Easing: [animate.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/animate.md) (rules for smooth, non-snapping drifting movement)
> - Canvas Performance Optimization: [optimize.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/optimize.md) (rules for minimizing filter overhead and layout recalculations)

---

### Prompt 18: Dynamic Screen Shake & Episafe Life-Loss Blink
> Implement a screen shake system that offsets the canvas context view matrix briefly on key events (player hit, pathogen lysis, boss alarms). Tracked via `shakeTimer` and `shakeIntensity`.
>
> Implement a life-loss blink effect. When the player loses a life, trigger a soft red screen flash (`blinkTimer = 0.55s`). In the draw loop, compute `alpha = sin(ratio * π) * 0.2` where `ratio = blinkTimer / 0.55`, and fill the canvas with `rgba(220, 30, 30, alpha)`. This creates a smooth bell-curve fade that is visible but will NOT trigger photosensitive epilepsy.
>
> **Design Guidelines Map:**
> - Screen Shake "Juice": [delight.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/delight.md) (rules for satisfying visual responses and sensory feedback to user actions)
> - Life-Loss Bell-Curve Fade: [animate.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/animate.md) (rules for mathematically modeled natural easing transitions over time)

---

### Prompt 19: Fluid Bullet Physics
> Implement fluid bullet physics. Both player bullets and enemy bullets must simulate movement through a viscous biological medium. Each bullet instance has:
> - `age` (seconds elapsed)
> - `driftAmp` (random 18–32px for player, 12–22px for enemy)
> - `driftFreq` (random 4–7 Hz for player, 3–6 Hz for enemy)
> - `driftPhase` (random)
> In each `update(dt)`, `lateral drift = driftAmp * cos(age * driftFreq + driftPhase) * dt`, added to x position. This makes bullets curve sinusoidally as if buffeted by cytoplasmic flow.
>
> **Design Guidelines Map:**
> - Cytoplasmic Sine Movement: [animate.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/animate.md) (rules for fluid, non-linear physical motion simulation)
> - Update Optimization: [optimize.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/optimize.md) (guidelines for low-overhead position updates of multiple entities)

---

### Prompt 20: Lightweight Particle Emitter System
> Create a lightweight particle engine handling custom position, count, spread angle, speed, color, velocity decay, gravity, and lifespan, allowing particles to fade out smoothly on destruction.
>
> **Design Guidelines Map:**
> - Particle Feedback & Juiciness: [delight.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/delight.md) (rules for rich procedural visual effects and particle burst feel)
> - Memory & Draw Calls Optimization: [optimize.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/optimize.md) (guidelines for recycling particle arrays and reducing allocations)

---

### Prompt 21: Boss FSM Advanced States — Summon & Sweep Beam
> Extend the Mutagen Boss FSM with two additional states:
> - `SWEEP_BEAM`: sweeps side-to-side at 300px/s for 3.5s while streaming 5-bullet downward fan spreads (−90°, −45°, 0°, +45°, +90°) with 25% probability per frame. Then returns to `PATROL`.
> - `SUMMON`: spawns Virus minions (up to 8 on-screen), then returns to `PATROL`. Update PATROL's random transition table to also include `SUMMON` and `SWEEP_BEAM`. Ensure that when the boss is defeated, all remaining spawned minions in the pathogens list are also cleared/lysed immediately so that the stage can be successfully cleared without hanging.
>
> **Design Guidelines Map:**
> - FSM State Safeguards & Cleanups: [harden.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/harden.md) (rules for handling edge cases, state transitions, and memory leaks or hangs)
> - Sweep Speeds & Motion: [animate.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/animate.md) (rules for timing and speed tuning during boss attacks)

---

### Prompt 22: Conga Line Entry Formation
> Implement a dynamic Conga Line entry formation when a stage is initialized:
> - Group and order the pathogens sequentially by columns to form a single-file queue. The center columns must fill first, ordering the columns from center to outward: Columns 3 and 4 first (top-to-bottom), then Columns 2 and 5, then Columns 1 and 6, and finally Columns 0 and 7.
> - Pathogens must enter the screen in a closely spaced, flowing train (one after another); a pathogen begins its swoop flight when the previous pathogen in the queue is slightly ahead on the flight path (e.g., when the predecessor has reached 8% of its flight completion), allowing multiple pathogens to follow each other mid-flight along the curve.
> - Flight path: Pathogens swoop from offscreen (top-left for even columns, top-right for odd columns) down toward the center-bottom, execute a full 360-degree circular loop (radius ~100px) around a center point near (450, 600), and then glide smoothly from the loop to their home grid slots.
> - The player cell is fully active (unlocked) and can move/shoot pathogens mid-flight immediately upon stage start.
> - Keep collective grid movement (side-to-side drift and descent) inactive as long as any pathogen is still swooping or hasn't arrived at its home slot.
>
> **Design Guidelines Map:**
> - Entry Choreography & Swoop Paths: [animate.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/animate.md) (principles of stage staging, entrance motion, and path interpolation)
> - Home Slots & Spacing Coordinates: [layout.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/layout.md) (structural arrangement rules for final grid layouts)

---

### Prompt 23: Variant Pathogen Grid Formations
> Introduce variety to non-boss stages by implementing three distinct structural formations that cycle dynamically based on the stage number:
> - **Formation 1 (Alternating Concentric Circles):** Arrange pathogens in three concentric circles (outer radius 180px, middle 120px, inner 60px) rotating around a central point (450, 250). Symmetrically alternate rotation directions (e.g., outer and inner circles rotate clockwise, middle rotates counter-clockwise) at a constant angular speed of 0.5 rad/s.
> - **Formation 2 (Pulsating Star Shape):** Arrange pathogens in a 5-pointed star layout. The entire formation drifts side-to-side while continuously pulsating in size (scaling its coordinates dynamically between 85% and 115% of its native size using a sine-wave function: `scale = 1.0 + 0.15 * sin(time * 2.0)`).
> - **Formation 3 (Undulating Wave Shape):** Arrange pathogens in a double-sine wave layout. As the formation drifts horizontally, each column undulates vertically based on elapsed time and its horizontal position (`y = base_y + 40 * sin(time * 3.0 + colIndex * 0.5)`), creating a fluid wave animation across the screen.
> - Each of these formations will still have to move down over time as per the rectangular formation.
>
> **Design Guidelines Map:**
> - Layout Geometry (Circles, Stars, Waves): [layout.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/layout.md) (rules for geometric alignment, centering, and symmetric spacing)
> - Pulsation, Wave & Rotation Math: [animate.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/animate.md) (guidelines for fluid trigonometric motion design)

---

### Prompt 24: Custom Enhanced Boss Encounters
> Design and implement a custom advanced boss stage (e.g., at Stage 10) featuring a multi-phase boss ('Bio-Carrier Overlord'):
> - **Phase 1 (Shielded):** The boss is surrounded by 4 orbiting shield-generator nodes (each having 10 HP). The boss itself is immune to all damage until all 4 orbital nodes are destroyed.
> - **Phase 2 (Enraged):** Once the shield is broken, the boss glows deep magenta, increases its movement speed by 40%, and changes its firing pattern to fire rapid homing bullets targeting the player cell directly.
> - Add dynamic status text and particle bursts indicating shield collapse and phase transitions. Ensure all spawned minions are also cleared/lysed immediately on the boss's defeat to prevent game hangs.
>
> **Design Guidelines Map:**
> - Boss Phase State Transitions: [harden.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/harden.md) (safe multi-phase lifecycle management and crash prevention)
> - Color Shift & Visual Feedback: [colorize.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/colorize.md) (using contrast shifts and warning colors like magenta to represent state change)
> - Feedback Banners & Particles: [delight.md](file:///Users/ignatiuspang/Workings/2026/AI_Workshop_Preparations/.gemini/skills/impeccable-design/reference/delight.md) (using text banners and dramatic particle bursts for dramatic stage progress)

---

<!-- APAF Bioinformatics | on_the_day_debug_instructions.md | Approved | 2026-06-12 -->
