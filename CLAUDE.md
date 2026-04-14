# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the game

No build step. Open `shooter.html` directly in a browser. There are no tests, no linter, and no package manager.

---

## Architecture — `shooter.html`

The entire game is a single self-contained HTML file (~580 lines). All logic, rendering, and styles live inside it. The canvas is 800×600.

### Game loop

```
requestAnimationFrame → loop()
  update()   — advance state, move entities, check collisions
  render()   — clear canvas, draw everything, overlay scanlines
```

### State machine

The global `state` string drives both `update()` and `render()`:

| Value | Meaning |
|---|---|
| `'start'` | Title screen — no entities active |
| `'wave_intro'` | "WAVE X / INCOMING!" or boss banner; `waveTimer` counts down to `'playing'` |
| `'playing'` | Active gameplay |
| `'gameover'` | Score overlay; click restarts via `initGame()` |

### Entity model

All entities are plain objects with `{ x, y, w, h }` for AABB collision (`aabb(a, b)`). There are four live arrays plus one singleton:

| Variable | Contents |
|---|---|
| `enemies[]` | Normal wave enemies (types A, B, C) |
| `playerBullets[]` | Bullets fired by the player |
| `enemyBullets[]` | Bullets fired by Type C enemies and the boss (shared pool) |
| `particles[]` | Short-lived `Particle` instances for death effects |
| `boss` | Single boss object or `null` |

### Wave / boss progression

- `waveConfig(w)` returns `{ count, speed, types[] }` for a normal wave.
- `spawnWave(w)` populates `enemies[]` in a grid formation above the canvas.
- `isBossWave(w)` — `true` when `w % 10 === 0`.
- `spawnBoss(w)` sets `boss`; HP = `50 + (w/10 - 1) * 40`.
- Wave advances when `enemies.length === 0 && boss === null`.

### Sprite system

Enemy sprites are pixel-art defined as `[col, row]` cell arrays on a 7×5 grid in `ENEMY_SPRITES`. `drawEnemySprite(type, cx, cy, alpha)` scales and renders them. The boss uses an inline 11×9 cell array drawn directly in `drawBoss()`.

### Key functions at a glance

| Function | Role |
|---|---|
| `initGame()` | Resets all state and starts wave 1 |
| `updatePlayer()` | Arrow-key movement, shoot-cooldown, fires `playerBullets` |
| `updateEnemies()` | Drift + homing movement, Type C shooting |
| `updateBoss()` | Wall-bounce drift, sine oscillation, radial bullet bursts |
| `checkCollisions()` | Bullets→enemies, bullets→boss, enemies/boss→player |
| `hitPlayer()` | Decrements lives, sets `iframes`, or triggers game over |
| `burst(x, y, color, n)` | Spawns `n` `Particle` instances |
| `drawHUD()` | Score, wave number, lives hearts |
| `drawBoss()` | Boss sprite + full-width HP bar |
| `renderWaveIntro()` | Wave/boss banner with fade-out via `waveTextAlpha` |

---

## Git & GitHub

Commit and push after every meaningful unit of work.

### Commit message format

```
<type>(<optional scope>): <short subject>

<body — bullet points: what changed and why>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
```

**Types:** `feat`, `fix`, `refactor`, `style`, `chore`, `docs`  
Subject line: lowercase, ≤ 72 chars, no trailing period.

### Versioning

Semantic versioning via git tags + `gh release create`:

| Change | Bump | Example |
|---|---|---|
| New feature | Minor | `v1.1.0` → `v1.2.0` |
| Bug fix / patch | Patch | `v1.1.0` → `v1.1.1` |
| Breaking change | Major | `v1.1.0` → `v2.0.0` |

```bash
git tag -a vX.Y.Z -m "vX.Y.Z — short description"
git push origin --tags
gh release create vX.Y.Z --title "vX.Y.Z — Title" --notes "..."
```
