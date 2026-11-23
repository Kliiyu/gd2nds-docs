# Getting started

!!! note "Planned usage"
    gd2nds has no release yet. This page describes how the tool is designed to be used, so you can prepare a Godot project for it now.

## Requirements

- A **Godot 4.7** project saved in text formats (`.tscn`, `.tres`), which is Godot's default. Binary `.scn` and `.res` files are not supported.
- **BlocksDS**, the Nintendo DS SDK gd2nds builds with. The easiest route is the official BlocksDS Docker image.
- An emulator such as **melonDS** for testing, or real hardware with a flashcart.

## Building a ROM

One command builds the whole project:

```sh
gd2nds build path/to/project
```

The generated C code goes to `<project>/.gd2nds/build/`, so your Godot project is never modified and you can inspect what each script became. The ROM is written next to it as a `.nds` file.

After every build gd2nds prints a size and VRAM report, and it fails with a clear message (naming the file, line or asset) when a project exceeds what the DS can do.

## Preparing your project

The DS is far smaller than a PC, so a few habits make a project compile cleanly:

- **Type your GDScript.** Use `var speed: float = 120.0` and `func move(dir: Vector2) -> void`. Typed code becomes fast C; untyped code goes through a slower `Variant`.
- **Keep sprites small.** DS sprites are 8 to 64 px per side. Larger textures are split into several hardware sprites, and frame sizes are padded to the nearest DS shape (24x24 becomes 32x32).
- **Mind the budgets.** At most 128 sprites per screen and 656 KB of VRAM shared by all graphics.
- **Avoid dynamic tricks.** `call()` or `get()` by string on your own methods, `load()` of runtime-computed paths, and `@tool` scripts are compile errors.
- **Use supported audio.** `.wav` for sound effects and `.it`, `.xm`, `.mod`, `.s3m` for music. `.ogg` and `.mp3` are rejected.

## Mapping input

`Input.is_action_pressed("jump")` compiles to a button bitmask test, because action names are known at build time. When the default mapping from your `InputMap` doesn't fit, add a `[gd2nds]` section to `project.godot` (or a `gd2nds.toml` file) mapping actions to DS buttons: D-pad, A, B, X, Y, L, R, Start and Select. The touch screen is exposed as mouse events.

## What works first

The first release (the MVP) supports `Node`, `Node2D`, `Sprite2D`, `AnimatedSprite2D`, `TileMapLayer`, `Camera2D` and `Timer` on one screen. See the [roadmap](roadmap.md) for the full scope.
