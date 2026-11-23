# gd2nds

**gd2nds** is an ahead-of-time compiler that turns a [Godot 4.7](https://godotengine.org/) 2D project into a Nintendo DS `.nds` ROM.

It reads a Godot project folder, translates every GDScript file to C, converts scenes and assets to DS-native binary data, and links everything against a small C runtime with [BlocksDS](https://blocksds.skylyrac.net/). There is no Godot engine on the DS and no GDScript interpreter: your scripts become native ARM9 code.

!!! warning "Early development"
    gd2nds is being designed and built now. The pages here describe the plan; commands and features marked *planned* do not exist yet. Follow progress in the [main repository](https://github.com/Kliiyu/gd2nds).

## Key decisions

- **Toolchain: BlocksDS.** A modern, maintained libnds fork with ARM GCC, `grit`, `mmutil`/Maxmod, NitroFS and `ndstool`. It installs easily in CI through its Docker image and is MIT/zlib licensed.
- **GDScript compiles to C, not LLVM.** C is easy to read and debug, GCC for ARM9 already optimises well, and the runtime is C anyway.
- **The host compiler is written in Rust** and targets Godot 4.7. It ships as one static binary.
- **Static typing is expected.** Typed GDScript (`var x: int`, `func f(a: Vector2) -> void`) compiles to plain C types. Untyped code falls back to a tagged `Variant`, which is slower but still works.
- **Fixed-point maths.** The ARM9 has no FPU, so `float`, `Vector2` and `Transform2D` default to 20.12 fixed point, with a project option for soft-float.
- **2D first.** Sprites map to OAM, `TileMapLayer` to tiled backgrounds, and `Camera2D` to background scroll and sprite offsets.

## Where to go next

- [Getting started](getting-started.md): what you need and how a build will work.
- [Architecture](architecture.md): the pipeline, the GDScript-to-C compiler, the runtime and asset conversion.
- [Roadmap](roadmap.md): MVP scope and milestones.
