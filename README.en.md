# Doom Dungeon (末日地牢)

> A browser-based first-person shooter roguelike — **a single HTML file + local Three.js**. No build step, double-click to play, works fully offline.

**[🎮 Play Online](https://studyguy.github.io/doom-dungeon/)** · **[📦 Source Code](https://github.com/studyguy/doom-dungeon)**

---

## 🕹️ What Is This

Want a quick, satisfying dungeon-shooter run straight from your browser? *Doom Dungeon* is inspired by *Soul Knight*, using a **room-to-room progression**: 3 major levels × (4 combat rooms + 1 supply room + 1 boss room). Clear a room to unlock the exit, collect coins, and take down bosses layer by layer.

The entire game lives in **a single `fps.html`** with a local Three.js fallback — nothing to install. Clone the repo, double-click the file, grab a gun, and you're in combat within 30 seconds. Between waves the map is **regenerated procedurally** from the wave number as a seed, so the same wave is reproducible while every run still feels fresh.

This project was built independently by [studyguy](https://github.com/studyguy) via **Vibe Coding**, for learning and entertainment purposes only.

## ✨ Features

- **Tight movement feel**: pointer-lock mouselook, WASD movement, tap-to-hop / hold-to-jump-high, Shift sprint (costs stamina), C crouch, anti-clipping collision, out-of-combat regeneration, hit direction indicator, low-HP warning, slow-motion death cam
- **4 weapons** (unlock via weapon crates; switch with 1–4 / mouse wheel):
  - 🔫 Pistol: starts with you, infinite ammo — the emergency fallback
  - 🔫 Rifle: all-rounder (34 dmg / 9 rps / 30-round mag)
  - 💥 Shotgun: 8-pellet cone spread + hit knockback for close-range burst
  - 🎯 Sniper: right-click scope, 220 headshot damage for long-range picks
- **Mario-Kart-style item boxes**: glowing `?` crates usable with F — heal / ammo / speed-up / bomb / straight rocket / homing missile / lightning / star invincibility / ghost stealth; **weapon enhancers** (+20% damage, +15% fire rate, +50% magazine, ×0.7 spread) apply instantly, last the whole run, capped at 3 levels each
- **4 enemy types with L4D2-style abilities**: Grunts / Runners / Brutes / Shooters, all telegraphed with a red pre-attack glow; Runners pounce (crouch to dodge), Shooters tongue-grab (shoot the tongue or crouch), Grunts explode on death, Brutes charge into walls and stun themselves, plus a **Tank boss event** every 8 waves
- **Procedural terrain × tactical movement**: every wave rebuilds the map from the wave number as seed (low platforms / jump pads / low walls / archways / energy platform unlock progressively) — jumping and crouching decide your tactical position, with no safe spots
- **Presentation**: real-time shadows, gradient sky, 20+ synthesized sound effects and BGM via Web Audio

## 🎯 Gameplay Reference

### Controls

| Key | Action |
|---|---|
| WASD / Mouse | Move / look (click to lock pointer, sensitivity adjustable) |
| LMB / RMB | Fire (hold for auto) / aim (scope with sniper) |
| Space | Jump (tap = short, hold = high) |
| Shift / C | Sprint (costs stamina) / crouch (dodges shooter projectiles) |
| R / F | Reload / use item |
| 1–4 / Wheel | Switch weapon |
| M / Esc | Mute / pause |

> Stamina: sprint 30/s, jump 5/hit; regenerates at 40/s after 0.8s idle. Sprint locks below 15 stamina. Arrow keys rotate the view when pointer lock is unavailable.

### Enemy Quick Reference

| Type | Unlocks | HP | Attack | Damage | Score |
|---|---|---|---|---|---|
| 🔴 Grunt | Wave 1 | 100 | Swing + lunge; explodes on death | 10 | 100 |
| 🟠 Runner | Wave 3 | 60 | Pounce (crouch-dodgeable) | 6 | 130 |
| ⚫ Brute | Wave 5 | 300 | Heavy hit + knockback; wall-charge self-stun | 22 | 250 |
| 🟣 Shooter | Wave 7 | 80 | Ranged orbs + tongue grab (shootable) | 8 | 160 |

- Attack flow: chase → **red telegraph** (dodgeable) → hit check → lunge → recovery → cooldown
- Scaling: HP ×(1+0.12(w−1)) capped ×3; damage ×(1+0.06(w−1)) capped ×1.8; speed +0.05/wave capped ×1.5
- Player: 100 HP, 0.4s invincibility frames on hit (anti-swarm), heals 6/s after 6s out of combat

### Terrain Mechanics

| Terrain | Tactical meaning |
|---|---|
| Low platform / jump pad (0.6m) | Jumpable; melee still reaches you — a mobility spot, not a bunker |
| Central energy platform (1.2m) | Unlocks at wave 7 — great vantage, but shooters can hit you and you can be surrounded |
| Low wall (1.35m) | Fully hidden while crouched, exposed standing; vaultable; cannot stand on top |
| Archway (1.3m ceiling) | Crouch-through only; dodges shooter lines of sight |

> Map generation guarantees: no overlaps, no dead ends (grid BFS reachability check), broken maps auto-rollback. Between waves you resupply +30% ammo for all unlocked weapons.

## 🚀 Quick Start

| Method | How |
|---|---|
| 🌐 Play online | [studyguy.github.io/doom-dungeon](https://studyguy.github.io/doom-dungeon/) (auto-deployed from main via GitHub Pages) |
| 💻 Play locally | Clone the repo and double-click `fps.html` (local `three.min.js` fallback, works offline) |
| 🖥️ Local server | `python -m http.server 8123`, then open `http://127.0.0.1:8123/fps.html` |

## 🛠️ Technical Notes

- **Three.js r152**: local `three.min.js` first, automatic fallback to jsdelivr / cdnjs / bootcdn / unpkg
- **Object pools**: bullets 60, enemies 26, particles 240, enemy projectiles 40 — no GC hitches
- **Game loop**: `requestAnimationFrame` with a crash guard (a throwing frame never kills the loop)
- **Deterministic terrain**: maps rebuilt from the wave number as seed — same wave, same map
- **Debug console** (F12):

```js
__game.debug.spawnWave(7)                  // jump to wave 7
__game.debug.teleportEnemyNear('brute', 3) // teleport a Brute in front of you
__game.debug.toggleGod()                   // god mode
__game.debug.regenerateMap()               // re-roll terrain
__game.debug.spawnSpecial('tank')          // spawn special enemy
```

## 📁 Project Structure

```
doom-dungeon/
├── fps.html            # The entire game (HTML + CSS + JS in one file)
├── three.min.js        # Local Three.js r152 fallback
├── index.html          # GitHub Pages entry (redirects to fps.html)
├── CHANGELOG.md        # Version history
├── docs/maintenance.md # Multi-machine maintenance handbook
└── .github/workflows/  # Pages auto-deploy
```

## 🔧 Maintenance Conventions

- Commits follow Conventional Commits (`feat` / `fix` / `docs` / `chore` / `refactor` / `test` + Chinese description)
- `git pull --rebase` before starting; large changes go on a `feature/xxx` branch — see [docs/maintenance.md](docs/maintenance.md)

## 📜 Disclaimer

- This project was built independently by [studyguy](https://github.com/studyguy) via **Vibe Coding**, for learning and entertainment only. Any resemblance to other projects is purely coincidental.
