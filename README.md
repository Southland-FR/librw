librw
=====

This library is supposed to be a re-implementation of RenderWare graphics,
or a good part of it anyway.

It is intended to be cross-platform in two senses:
support rendering on different platforms similar to RW;
supporting all file formats for all platforms at all times and provide
way to convert to all other platforms.

Supported file formats are DFF and TXD for PS2, D3D8, D3D9 and Xbox.
Not all pre-instanced PS2 DFFs are supported.
BSP is not supported at all.

For rendering we have D3D9 and OpenGL (>=2.1, ES >= 2.0) backends.
Rendering some things on the PS2 is working as a test only.

# Uses

librw can be used for rendering [GTA](https://github.com/gtamodding/re3).

The Southland-FR fork also maintains separate integration branches for its
downstream projects:

| Branch | Consumer | Purpose |
| --- | --- | --- |
| `master` | General use | Current upstream plus portable fixes shared by all consumers |
| `ariane` | [Ariane](https://github.com/Dryxio/ariane) | Ariane-specific asset compatibility |
| `sa-reversed` | `gta-reversed-dryxio` standalone executable | RenderWare 3.6 ABI and D3D9/CMake integration |
| `vc3insa` | `reVC` `vcinsa` | Legacy external-device/render-target integration |

Downstream build automation should pin an exact tested commit from its branch,
not a moving branch name. Keep project-specific changes off `master` so that one
consumer cannot silently change another consumer's ABI or renderer behavior.

# Building

### CMake

Choose the rendering backend explicitly, then build:

```sh
cmake -S . -B build -DLIBRW_PLATFORM=GL3 -DLIBRW_GL3_GFXLIB=GLFW \
  -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release
```

`LIBRW_PLATFORM` supports `D3D9`, `GL3`, `NULL` and the platform-specific
targets listed in the top-level CMake file.

### Premake

Generate a configuration with Premake 5 and build the `librw` target from the
generated `build` directory. For example, on Linux x64:

```sh
premake5 gmake2 --gfxlib=glfw
make -C build -j2 config=release_linux-amd64-gl3 librw
```

On Apple Silicon, use `config=release_macos-arm64-gl3`. On Windows D3D9, run
`premake5 vs2019` and build the `librw` target for `win-amd64-d3d9`.
