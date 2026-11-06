# Wave Spectrum Lab

Inspect a 16-bit PCM WAV with a native Qt spectrum image and PNG export.

[![Build and verify](https://github.com/netcodermike/wave-spectrum-lab/actions/workflows/verify.yml/badge.svg)](https://github.com/netcodermike/wave-spectrum-lab/actions/workflows/verify.yml)

**Language:** C++ · **Platform:** Windows, macOS, Linux desktop · **App type:** WAV frequency analysis and visual export

First-release learning implementation, with explicit scope below. This is one independently runnable app, not a placeholder in a combined repository.

## Features

- Mono/stereo PCM import
- 4096-sample analysis window
- Logarithmic spectrum display
- PNG chart export

## Requirements

C++17, CMake 3.16+, and Qt 6 Widgets. Ubuntu: sudo apt-get install qt6-base-dev. On other systems install the matching Qt compiler kit and set CMAKE_PREFIX_PATH.

## Quick start

```sh
git clone https://github.com/netcodermike/wave-spectrum-lab.git
cd wave-spectrum-lab
cmake -S . -B build
cmake --build build --config Release
# Linux/macOS: ./build/app
# Windows MSVC: .\build\Release\app.exe
```

Commands run from the repository root. Lines marked for a particular operating system apply only there. Language tools are prerequisites, not installed by the app. Close desktop windows normally; terminal/server apps stop with Ctrl+C unless their usage notes say otherwise.

## Example workflow

Enter a short 16-bit PCM WAV path, render, and export PNG. Compare a low sine tone with a higher one.

## Architecture, configuration, and data

main.cpp parses RIFF fmt/data chunks with bounds checks, averages stereo channels, applies a Hann window, and evaluates 256 frequency bins using a direct complex DFT at k/512 normalized frequency. The image is drawn with QPainter. Qt file-path input and PNG export wrap this model. Self-tests recover a synthetic sine peak and reject invalid RIFF data.

## Verification

Build with the quick-start command where needed, then run:

```sh
ctest --test-dir build -C Release --output-on-failure
```

Manual acceptance: Enter a short 16-bit PCM WAV path, render, and export PNG. Compare a low sine tone with a higher one. Also test malformed inputs and an unwritable output destination.

See [validation record](docs/VALIDATION.md) for observed checks and their limits. CI results verify only what the workflow runs; they do not establish native GUI correctness or production readiness.

## Limitations and next steps

Not a DAW or calibrated scientific analyzer. No streaming/audio playback, frequency-axis labels, automatic sample-rate scaling, FFT acceleration, phase display, or float/compressed WAV support. Only the first 4096 frames and 32 MiB files are considered. Rendering is synchronous.

## Troubleshooting

Use the exact platform/toolchain above. Preserve originals and use synthetic fixtures first. Errors are displayed in the UI or stderr. A headless machine can run the model tests but cannot verify native GUI/audio behavior. Output paths are chosen explicitly; keep backups of important data.

## Source map

- `CMakeLists.txt`
- `main.cpp`

## Development and license

Read [contributing](CONTRIBUTING.md) before changing the app, and [security/privacy](SECURITY.md) before supplying private data. Use synthetic fixtures for tests. No telemetry, paid API key, or account is required by the app itself. Dependencies, when present, are stated in Requirements.

[MIT license](LICENSE), copyright 2026 netcodermike.
