# Roadmap

## MVP

The MVP is done when a small platformer-style Godot test project (one scene, a player sprite moved by the D-pad, an animated enemy, a tilemap level and a following camera) builds with one command and runs at 60 fps in melonDS and on real hardware.

**In the MVP:** `Node`, `Node2D`, `Sprite2D`, `AnimatedSprite2D`, `TileMapLayer`, `Camera2D`, `Timer`; typed GDScript with classes, functions, `if`/`for`/`while`/`match`, `Vector2` maths, signals with constant names, `Input` actions, `get_node` with constant paths, `preload`, `queue_free`, `change_scene_to_file`; one screen.

**Not in the MVP:** physics bodies, audio, UI controls, `await`, shaders, particles, `Tween`, `AnimationPlayer`, 3D, saving and dual-screen layouts.

## Milestones

Each milestone ends in a ROM that runs.

1. **Hello ROM.** Repository, BlocksDS Docker build, and a CLI that drops a hand-written C `main` into a template and produces a `.nds` showing a sprite. Proves the toolchain end to end.
2. **Scenes and assets.** Parse `project.godot` and `.tscn`, convert PNGs and `SpriteFrames`, and show the main scene's sprites at their editor positions, with no scripts.
3. **GDScript core.** Lexer, parser, type checker and C emitter for the MVP subset. `_ready`, `_process`, `Input` and `position` work, and the test project's player moves.
4. **Tilemaps and camera.** `TileMapLayer` to backgrounds, `Camera2D` scrolling with limits, z-order sorting. The MVP test project runs.
5. **Hardening.** Error messages with file and line, a golden-output test suite of small `.gd` files run headlessly in an emulator, and a size and VRAM report after every build.

## After the MVP

In rough order:

- Collisions and `CharacterBody2D`
- Audio with Maxmod
- `Label` and fonts
- `AnimationPlayer` for 2D properties
- `await` on timers
- Both screens
- 3D through the DS 3D engine, as a separate project
