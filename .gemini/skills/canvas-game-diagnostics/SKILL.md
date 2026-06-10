---
name: Canvas Game Diagnostics and Physics
description: Diagnostics, performance profiling, and physics guardrails for Canvas and Web Audio applications.
---
<!-- APAF Metadata
last_verified_date: 2026-06-10
apaf_approved: true
apaf_version: 1.0.0
apaf_org: APAF Bioinformatics
-->

# Canvas Game Diagnostics & Physics

This skill provides instructions, formulas, and boilerplate patterns for debugging and implementing high-performance, frame-rate independent HTML5 Canvas games.

## 1. Frame-rate Independent Physics (Analytical Solvers)

Standard Euler integration ($v \leftarrow v + a \cdot dt - v \cdot f \cdot dt$) oscillates or overshoots when friction ($f \cdot dt \ge 1$). Use the analytical solver (exponential decay) for damping/friction:

```javascript
// Analytical Physics Update
const alpha = Math.exp(-friction * dt);
velocity = velocity * alpha + (acceleration / friction) * (1 - alpha);
position += velocity * dt;
```

## 2. Bullet/Entity Tunneling Prevention (Swept-volume Collision)

To prevent fast-moving bullets from skipping thin entities:
- Model the bullet's step as a segment from `(x_prev, y_prev)` to `(x_curr, y_curr)`.
- Check the minimum distance from the entity's center point `(px, py)` to the segment.

```javascript
function distToSegment(px, py, x1, y1, x2, y2) {
    const dx = x2 - x1;
    const dy = y2 - y1;
    const lenSq = dx * dx + dy * dy;
    if (lenSq === 0) return Math.hypot(px - x1, py - y1);
    
    // Project point onto segment, clamp parameter t to [0, 1]
    let t = ((px - x1) * dx + (py - y1) * dy) / lenSq;
    t = Math.max(0, Math.min(1, t));
    
    const projX = x1 + t * dx;
    const projY = y1 + t * dy;
    return Math.hypot(px - projX, py - projY);
}
```

## 3. Performance & Memory Profiling Checklist

- **State Stack Leaks**: Ensure every `ctx.save()` is paired with a matching `ctx.restore()`. Imbalances crash or slow down the Canvas rendering context.
- **Garbage Collection (GC) Thrashing**: Avoid instantiating vectors or utility objects inside the main rendering/update loop. Reuse objects or use flat float arrays.
- **Reference Leakage**: Ensure event listeners added to `window` or `document` are removed when shifting states/screens.

## 4. Web Audio Context Gesture Resume Pattern

```javascript
let audioCtx = null;
function initAudio() {
    if (!audioCtx) {
        audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    }
    if (audioCtx.state === 'suspended') {
        audioCtx.resume();
    }
}
// Trigger initAudio() inside a user click/keydown handler
```

<!-- APAF Bioinformatics | SKILL.md | Approved | 2026-06-10 -->
