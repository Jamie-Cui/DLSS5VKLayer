# Repository Guidelines

## Project Structure & Module Organization

DLSS5VKLayer forwards Linux Vulkan frames to a Windows NGX helper running under Wine/Proton.

- `layer_linux/src/`: Vulkan interception, capture, composition, scaling, and shaders.
- `helper/` and `core/`: Windows helper, NGX integration, and guards; `windows/meson.build` defines Windows targets.
- `common/`: shared-memory protocol and runner discovery; `gui/`: Qt 6 controls.
- `test_gui/`: binder regression tests; `test_layer/`: Vulkan smoke program.
- `tools/`: build, shader-generation, and diagnostic utilities; `packaging/`: distribution scripts.
- `standalone_runner/third_party/`: vendored headers. Generated build artifacts belong in `build/`; packages go in `dist/`.

## Build, Test, and Development Commands

Install Clang/LLVM, Meson, Ninja, Qt 6 development packages, MinGW, and 32-bit/static runtime libraries; see `README.md` for dependencies.

- `tools/meson-build.sh`: build native Linux, 32-bit Linux, and Windows targets.
- `meson compile -C build/native`: rebuild native targets after setup.
- `meson test -C build/native --print-errorlogs`: run registered tests; Meson sets Qt's offscreen platform.
- `tools/meson-build.sh analyze`: run Clang static analysis; requires `scan-build`.
- `./build/native/gui/dlssnr_gui`: launch the GUI.
- `./dlssnr-helper doctor`: check runtime configuration and dependencies.

Builds do not install the Vulkan manifest. Install a package before integration testing.

## Coding Style & Naming Conventions

Use C++17, four-space indentation, and same-line opening braces; Meson files use two spaces. Match surrounding names: `ShmBinder` types, `kShmVersion` constants, and existing function casing. No repository-wide formatter configuration is present; avoid unrelated reformatting.

Regenerate embedded shader headers with `tools/gen_*_spv.sh` when changing their sources. Coordinate shared-memory layout changes across the layer, helper, and GUI, including protocol versioning.

## Testing Guidelines

`binder_test.cpp` uses a standalone Qt executable with explicit checks; no coverage threshold is configured. Add focused regressions beside relevant tests and register new executables with Meson; follow the existing `*_test.cpp` naming where applicable.

For rendering changes, run `build/windows/windows/smoke.exe` through configured Wine/Proton using the README recipe. Record GPU, driver, runner, and helper logs; verify original-frame presentation when the helper is unavailable.

## Commit & Pull Request Guidelines

History favors short, action-oriented subjects, occasionally prefixed with `fix:` or a component name. Use focused commits. PRs should describe behavior changes, link relevant issues, and report validation; include screenshots for GUI or rendering changes.

Keep proprietary NGX DLLs and generated build/package outputs out of commits.
