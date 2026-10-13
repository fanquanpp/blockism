[中文](README.md) · **English**

# BLOCKISM

> A constructivist 2D platformer and a homage to *Thomas Was Alone*. Minimal geometry, sharp edges, a red/black/paper palette — built in Godot 4.7 / GDScript.

Repo name `blockism` (formerly `geometric-construct`). The game was previously titled 《几何构成 / Geometric Construct》.

## What it is

BLOCKISM is a single-player-first 2D platformer with an optional two-player race mode. The player switches between the geometries on the roster, each defined by a single verb — speed, elasticity, or inversion. The 16 campaign levels unfold across four acts, with a tutorial level and a dedicated two-player race level.

- Engine: Godot 4.7 (standard) / GDScript 2.0
- Viewport: 1280×720, `canvas_items` stretch + `expand` aspect; minimum desktop window 960×540; mobile portrait/landscape supported
- Platforms: PC (keyboard & mouse / gamepad) and Android (touch)
- Art: fully procedural (`_draw` recipes plus a few `canvas_item` shaders), colours sourced only through `data/palette.tres`

## Art direction

Minimalism + constructivism + geometry + sharp edges:

- No rounded corners, no gradients, no soft shadows
- Flat colour blocks, thin lines, hard-edged right-angle shadows, large numerals
- Red / black / paper as the primary visual triad

Palette (`data/palette.tres`): `ink #101216`, `paper #EDEAE0`, `red #E0492F`, `yellow #E8B33A`, `blue #4E86D8`, `orange #E07E2E`, plus the mechanism semantic slots `buff #4E9E58` (buff / safety) and `cool #45B0BE` (cold environment).

## The four geometries

The design calls for four geometries on an attribute scale of 0.0–2.0, with a weight order of yellow > red > orange > blue. The current roster ships three (the orange ball was retired during development).

| Name | Shape | Trait | Weight |
|---|---|---|---|
| 疾 (now "Red") | red square | Speed type: sprint (hold Shift, target speed 1.5× ≈ 4.5 cells/s), double jump (2.0 cells per jump), wall climb (up to 2.0 cells per climb) | 1.0 |
| 簧 (now "Yellow") | yellow tall rectangle | Elastic type: 100% bounce (holding jump fires a higher bounce), allies who jump off her top get ×2 jump height | 1.5 |
| 坠 (now "Blue") | blue inverted square | Inversion type: press jump to flip between ceiling and floor, horizontal inertia conserved; base speed 0.5, up to 2.0× through a speed gate | 0.2 |
| 转 (orange ball) | orange circle | Rolling type: moves by rolling — **retired, not in the current roster** | — |

> Naming history: 疾 / 簧 / 坠 / 转 → 疾 / 跃 / 逆 / 圆 → 红 / 黄 / 蓝 (roster fixed at three). The circle and a fifth geometry (a dual-body triangle) were retired in turn; the roster now holds three geometries.

## Mechanics

Mechanisms (source slugs):

- Speed gate `speed_gate` — raises the sprint cap while inside
- Timed bridge `timed_bridge` — collapses when crossed, returns on a timer
- Resonance pedal `resonance_pedal` — an inactive ally holds the pedal to reveal a paper bridge for the active body
- Piano tile `piano_tile` — sounds when stepped or rolled on; pitch maps inversely to height
- Ski patch `ski_patch` — a speed-boost strip
- Mover `mover` — moving platform
- Checkpoint beacon `checkpoint_beacon`
- Exit door `exit_door`

World props also include hint markers `hint_marker` and spawn markers `spawn_marker`.

## Acts and levels

Four acts, four levels each — 16 campaign levels in total — plus a tutorial level, a two-player race level, and a developer gate-probe level.

| Act | Title | Theme colour | Level folder |
|---|---|---|---|
| Act I · On Separate Paths | Red and Yellow's four introductory stages; three doors stand side by side at the finale | blue | `levels_native/act1/` |
| Act II · The Fork | Three solo paths converge at the crossing | yellow | `levels_native/act3/` |
| Act III · Metamorphosis | Three paths that run against the grain | orange | `levels_native/act4/` |
| Act IV · The Truth of the Marks | Three doors return to place; curtain | red | `levels_native/act5/` |

> Level folder numbers do not match act order: the old `act2/` was retired and the acts shifted up (Act I = `act1/`, Act II = `act3/`, Act III = `act4/`, Act IV = `act5/`). `levels_native/` also holds `tutorial/`, `duel/`, `dev/`, and `_template.tscn` as a level-authoring template.

Music in `data/music/`: prologue + act1–4 (five tracks). Backdrops in `data/backdrop/`: act1–4 + menu.

## Controls

| Action | Keyboard | Gamepad | Touch |
|---|---|---|---|
| Move | A / D, ← / → | left stick / D-pad | bottom-left wheel pad (nudge to walk, push fully to accelerate) |
| Jump · double jump | Space / W / ↑ (tap for a short hop, hold for full height) | A button | tap for a short hop, hold for full height |
| Invert (Blue) | Space | A button | tap the screen |
| Sprint | Shift (hold) | X button | — |
| Wall climb | Space (hold while on a wall) | — | hold while on a wall |
| Switch geometry | Tab / E / Q | RB / LB | tap the party chips, top-left |
| Direct roster select | 1–3 | — | — |
| Recall | R | Back (View / Select) | top-right button row |
| Pause | Esc / P | Start | top-right button row |

Interface navigation: main menu `1–4` to pick an act, `K` for the key guide, `C` for the geometry archive, `S` for settings, `Esc` to quit; `Enter` / `Space` to confirm, `Esc` to go back (gamepad A confirms, B goes back). Every touch screen has tappable buttons / tabs, with no keyboard dependency. The wheel-pad layout can be switched between "fixed" and "floating" in the settings panel.

## How to run

Prerequisite: Godot 4.7 (standard). Replace `godot` below with your Godot 4.7 executable.

```bash
# 1) First-time import (registers class_names, builds the .godot/ cache)
godot --headless --path . --import

# 2) Run the game (main scene res://scenes/Main.tscn)
godot --path .

# 3) Headless smoke test (exit code is authoritative)
godot --headless --path . res://tests/native_check.tscn
```

> Note: `project.godot` registers the autoload `_mcp_game_helper` → `res://addons/godot_ai/runtime/game_helper.gd`. That addon is a local AI / MCP helper and is **not tracked in the repository** (see the third-party-plugin section of `.gitignore`). A fresh clone must install that addon separately, or remove that autoload line, before the game will start.

## Project structure

```
addons/editor_kit/     first-party editor plugin (mechanism dock)
assets/
  brand/               app icons (192 / 432; foreground / background / monochrome)
  editor/              mechanism SVG icons for the editor
  fonts/               Noto Sans SC variable font + OFL licence
  tiles/               ground / platform / decor / special tile atlases
data/
  backdrop/            act1-4 + menu backdrop resources
  characters/          dash / spring / fall geometry .tres files
  music/               prologue + act1-4
  sfx/                 sound-effect .tres files
  tiles/               native_tileset.tres
  tuning/              movement_default.tres
  palette.tres         colour palette
levels_native/         level .tscn files (act1 / act3 / act4 / act5 + tutorial + duel + dev) + _template
scenes/                Main.tscn plus core / entities / world / ui / net / fx / tutorial / art scenes
scripts/               GDScript (art / core / data / dev / entities / fx / net / tutorial / ui / world)
shaders/               light_streaks / speed_lines / tile_theme
tests/                 headless gate scenes
tools/                 audit and generation scripts
project.godot  export_presets.cfg  icon.png  CHANGELOG.md
```

## Editor addon

`addons/editor_kit` — a first-party Godot editor plugin (`@tool EditorPlugin`) shipped with the repo:

- A dock panel: the mechanism placement table (`dock_panel.gd` + `placement_table.gd`)
- In-viewport overlay drawing: `_forward_canvas_draw_over_viewport` draws mover travel arrows and speed-gate sensor dashed boxes

The project registers and enables it in `project.godot`.

Separately: `addons/godot_ai` is a local AI / MCP helper addon (the `_mcp_game_helper` autoload). It is for development only, is not tracked in the repository, and does not ship with the game.

## Testing and gates

There is **no CI pipeline** in the repo (no `.github/` workflows). Quality gates are a set of headless Godot scenes judged by their text assertions and exit code:

- `tests/`: `flow_check` (level-completion chain), `native_check` (per-level load), `stats_check`, `focus_check`, `trait_check`, `ambience_check`, `transition_check`, `replay_transition_check`, `mech_resonance`, `tutorial_check`, plus the screenshot tools `shot_all` / `win_shot`
- `tools/`: level audit, door-placement audit, hot-path audit, keybind reconciliation, tile-data physics check, font-coverage check, tile / brand-icon generation, waste cleanup, and more

## Known limitations

- The roster is three geometries; the designed fourth, the orange ball 转, was retired.
- The game title in `project.godot` / `scripts/core/version.gd` is still 《几何构成 / GEOMETRIC CONSTRUCT》; the repo is being renamed to `blockism` (BLOCKISM).
- Same-screen two-player / race mode needs a keyboard or gamepads; touch devices cannot do same-screen two-player.
- The story / dialogue system, the roguelike mode, and haptic vibration were all retired in earlier versions and are not in the current build.
- The only published artifact is a debug Android APK (see Releases, latest tag `v0.70.0`); there is no signed store release.
- Running the project depends on the untracked third-party addon `addons/godot_ai` (an autoload); a fresh clone must install it or remove that autoload line.
- There is no `LICENSE` file at the repo root (see below).

## Licence

There is no `LICENSE` file at the repository root, so all rights are reserved by default unless the repository owner states otherwise.

The bundled font `assets/fonts/NotoSansSC-VF.ttf` (Noto Sans SC Variable) is licensed under the **SIL Open Font License 1.1**; see `assets/fonts/LICENSE.txt`.
