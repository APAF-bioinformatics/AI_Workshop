---
name: Gemini AI Workshop Guidelines
description: Core programming guardrails, safety rules, and workspace directory constraints for workshop participants.
---
<!-- APAF Metadata
last_verified_date: 2026-06-10
apaf_approved: true
apaf_version: 1.0.0
apaf_org: APAF Bioinformatics
-->

# Antigravity AI Workshop Guidelines & Guardrails

Welcome to the AI Coding Workshop. This document outlines the mandatory guardrails, standards, and safety practices for building applications with Antigravity and autonomous coding agents.

---

## 1. Respect Workspace & Ignored Paths (CRITICAL)

To prevent data leakage, unauthorized file reading, and pollution of the context window, all AI agents and participants MUST adhere to the following rules regarding ignored files:

*   **Respect `.gitignore` and `.aiignore`**: AI agents MUST respect directories and files listed in `.gitignore` and `.aiignore` by default.
*   **Prohibition on Proactive Access**: Agents must NOT proactively search, discover, or read files in these ignored paths (e.g., files in `data/`, temporary files, local credential logs, or databases) unless the user explicitly instructs them to do so in the active request.
*   **Path Separation**: Always keep sensitive configurations, large dataset directories, and model checkpoint logs under ignored folders (e.g., `data/`) so that automated tools do not parse them during global workspace analysis.

---

## 2. Core Programming Guardrails

### A. Loop Optimization & Functional Programming
To ensure maximum readability, prevent stack overflows, and reduce token usage during recursive runs:
*   **Avoid Imperative Loops**: Avoid classic imperative `for` and `while` loops for data iteration where possible.
*   **JavaScript standard**: Use functional array methods (`forEach`, `map`, `filter`, `reduce`, `every`, `some`).
*   **R standard**: Strictly use the `purrr` family (`map`, `walk`, `imap`) or `lapply()`. **Zero tolerance for standard `for` loops in R script data processing.**

### B. Frame-Rate Independent Physics (Game Dev)
When developing Canvas or rendering loops, never link speed directly to the frame rate:
*   Always calculate the high-precision delta time (`dt = (current - last) / 1000.0`).
*   Multiply all velocities and updates by `dt` (e.g., `x += vx * dt`) to ensure consistent speed across 60Hz, 120Hz, and variable refresh rate monitors.

### C. Snappy Physics & Controls
*   Use high friction coefficients (e.g., `18.0`) and corresponding acceleration logic (`speed * friction * 1.5`) to create highly responsive controls, preventing slippery entity drift on user input.

### D. Safe Web Audio API Practices
*   **Interaction Requirement**: Browsers block audio playback until a user gesture occurs. Always initialize or resume the `AudioContext` only after the first user click (e.g., clicking the start button or selecting an option on the menu).
*   **Procedural Synthesis**: Prefer synthesizing basic sound effects procedurally (using oscillators, gain nodes, and filters) over loading external audio files to ensure the application remains self-contained and fast loading.

---

## 3. SEO & Layout Best Practices

*   **Responsive Scaling**: Set fixed native dimensions on the `<canvas>` element (e.g., `width="900" height="1000"`) and scale it responsively using CSS aspect ratios (`aspect-ratio: 9 / 10`) to keep coordinate space rendering uniform.
*   **Semantic Structure**: Use correct HTML5 elements (`<header>`, `<main>`, `<canvas>`, `<footer>`) to promote accessibility.
*   **Aesthetics**: Wow the user at first glance. Use curated HSL color palettes, neon accents, dark microscopic overlays, and smooth CSS transitions. Avoid default basic colors.

---

## 4. Useful Agent Skills & Integration

To optimize development workflows, workshop participants and autonomous agents should leverage the following standard skills:

*   **`modern-web-guidance`**: Essential search tool for modern web standards. Refer to it first for Layout/CSS queries, Canvas scaling, and advanced Web APIs.
*   **`chrome-devtools`**: Provides real-time page diagnostics, DOM tree inspections, console log auditing, and user event simulation (e.g. click coordinates).
*   **`a11y-debugging`**: Use for checking accessibility structures, semantic markup, focus state cycles, and color contrasts.
*   **`memory-leak-debugging`**: Crucial for canvas-based loop performance. Helps identify rendering memory leaks or unreleased event handlers.

---

<!-- APAF Bioinformatics | Gemini.md | Approved | 2026-06-10 -->
