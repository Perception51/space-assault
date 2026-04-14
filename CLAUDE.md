# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

---

## Project Goals

A personal learning and experimentation project for browser game development assisted by Claude Code. The primary game is **Space Assault** — a retro top-down shooter. A second mini-game (**Tic Tac Toe**) also lives in the repo. There is no fixed roadmap; features are added as ideas come.

---

## Architecture Overview

Each game is a **single self-contained HTML file** — no build step, no modules, no separate JS or CSS files. All logic, styles, and markup live together.

### Space Assault (`shooter.html`)

The game runs on an 800×600 `<canvas>` driven by a `requestAnimationFrame` loop:

```
loop() → update() → render()
```

**State machine** — the global `state` string controls both update and render:

| State | Description |
|---|---|
| `'start'` | Title screen; no entities active |
| `'wave_intro'` | Wave/boss banner; `waveTimer` counts down then switches to `'playing'` |
| `'playing'` | Active gameplay |
| `'gameover'` | Score overlay; click restarts |

**Entity model** — all entities are plain objects with `{ x, y, w, h }` used for AABB collision:

| Variable | Role |
|---|---|
| `enemies[]` | Normal wave enemies (types A, B, C) |
| `playerBullets[]` | Bullets fired by the player |
| `enemyBullets[]` | Bullets from Type C enemies and the boss (shared pool) |
| `particles[]` | Short-lived `Particle` instances for death effects |
| `boss` | Single boss object, or `null` |

**Wave progression:**
- `waveConfig(w)` → `{ count, speed, types[] }` for a normal wave
- `spawnWave(w)` populates `enemies[]` in a grid above the canvas
- `isBossWave(w)` — `true` when `w % 10 === 0`
- `spawnBoss(w)` sets `boss`; HP scales as `50 + (w/10 - 1) * 40`
- Wave advances when `enemies.length === 0 && boss === null`

**Sprite system:** enemy sprites are defined as `[col, row]` cell arrays on a 7×5 grid in `ENEMY_SPRITES` and rendered by `drawEnemySprite()`. The boss uses an inline 11×9 cell array inside `drawBoss()`.

---

## Design and Style Guidelines

- **Vanilla JS only** — no frameworks, no npm packages, no CDN imports, ever
- **Single file per game** — all HTML, CSS, and JS stays in one file
- **Performance-conscious game loop** — avoid object allocation inside `update()` and `render()` where possible; reuse arrays by filtering in place
- **Retro / Space Invaders aesthetic** — black background, pixel-art sprites drawn with canvas rects, `Courier New` monospace font, green primary color (`#00ff41`), CRT scanline overlay
- **No comments stating the obvious** — comments should explain *why*, not *what*

---

## Constraints and Policies

- **Must run fully offline** — no external requests. Every asset is inline; no fonts, scripts, or images loaded from the network
- **No external libraries** — zero CDN or npm dependencies, now or in the future
- **Single HTML file per game** — do not split games into separate JS/CSS files

---

## Repo and Git Rules

Commit and push to GitHub after every meaningful unit of work.

### Commit format (Conventional Commits)

```
<type>(<optional scope>): <short subject>

- bullet: what changed and why

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
```

**Types:** `feat`, `fix`, `refactor`, `style`, `chore`, `docs`
Subject line: lowercase, ≤ 72 characters, no trailing period.

### Versioning (Semantic Versioning)

| Change | Bump | Example |
|---|---|---|
| New feature | Minor | `v1.1.0` → `v1.2.0` |
| Bug fix / patch | Patch | `v1.1.0` → `v1.1.1` |
| Breaking change | Major | `v1.1.0` → `v2.0.0` |

After tagging, create a GitHub Release:
```bash
git tag -a vX.Y.Z -m "vX.Y.Z — short description"
git push origin --tags
gh release create vX.Y.Z --title "vX.Y.Z — Title" --notes "..."
```

---

## Frequently Used Commands / Workflows

### Run the game
No build step. Open the file directly in a browser:
```bash
# Windows
start shooter.html
```

### Push a change
```bash
git add <file>
git commit -m "feat(scope): subject"
git push
```

### Tag and release a new version
```bash
git tag -a vX.Y.Z -m "vX.Y.Z — description"
git push origin --tags
gh release create vX.Y.Z --title "vX.Y.Z — Title" --notes "changelog text"
```

### View release history
```bash
gh release list
```

---

## Testing and Build Instructions

There is no build system, bundler, linter, or test suite. Verification is manual:

1. Open `shooter.html` in a browser
2. Exercise the changed feature end-to-end (start screen → gameplay → wave transitions → game over)
3. Confirm no regressions in adjacent features (movement, shooting, wave progression, boss spawning)

---

## Keeping This File Up to Date

**Update this file whenever:**
- A new feature changes the architecture (new entity types, new game states, new files)
- A new game or major mode is added to the repo
- Constraints or style rules are agreed upon or changed
- A project milestone is reached and the goals section no longer reflects reality

The goal is that any new Claude Code session should be able to read this file and immediately understand the project without reading the source code first.
