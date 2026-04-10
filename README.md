# Space Assault

A retro-style top-down shooter playable entirely in the browser. No dependencies, no install — just open `shooter.html` and play.

![Space Assault](https://img.shields.io/badge/version-v1.1.0-orange) ![License](https://img.shields.io/badge/license-MIT-green)

---

## Gameplay

Survive endless waves of alien enemies that grow faster and more numerous each round. Every 10th wave a giant boss arrives — kill it to advance.

### Controls

| Input | Action |
|---|---|
| `Arrow Keys` | Move ship |
| `Mouse` | Aim |
| `Hold Left Click` | Fire |

---

## Features

- **Progressive waves** — enemy count, speed, and homing intensity increase each wave
- **3 enemy types** unlocked over time:
  - **Type A** (grey) — basic, 1 HP
  - **Type B** (purple) — armored, 2 HP, introduced at wave 3
  - **Type C** (red) — shooter, 2 HP, fires back at the player from wave 5
- **Boss waves** every 10 levels — a giant enemy with scaled HP, radial bullet sprays, and a full-width HP bar
- **3 lives** with invincibility frames after each hit
- **Persistent hi-score** tracked across rounds
- CRT scanline overlay and particle death effects

---

## Running the Game

No build step required.

1. Clone or download the repository
2. Open `shooter.html` in any modern browser

```bash
git clone https://github.com/Perception51/space-assault.git
cd space-assault
# open shooter.html in your browser
```

---

## Versioning

Releases follow [Semantic Versioning](https://semver.org/). See the [Releases](https://github.com/Perception51/space-assault/releases) page for the full changelog.

| Version | Summary |
|---|---|
| `v1.1.0` | Boss waves every 10 levels |
| `v1.0.0` | Initial release |
