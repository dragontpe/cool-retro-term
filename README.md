# cool-retro-term (Apple Silicon Native)

A fork of [Swordfish90/cool-retro-term](https://github.com/Swordfish90/cool-retro-term) built natively for Apple Silicon (ARM64). No Rosetta required.

![screenshot](screenshot.png)

## Why this fork?

The upstream project doesn't ship ARM64 macOS binaries. The Homebrew cask is Intel-only (runs via Rosetta), and the older v1.2.0 ARM64 community builds crash on macOS 14+ due to Qt5's OpenGL context failures on Apple Silicon.

This fork builds the latest Qt6 master branch natively on ARM64. It just works.

## What is cool-retro-term?

A terminal emulator that mimics the look and feel of old cathode ray tube (CRT) screens. It comes with several built-in profiles:

- **Default Futuristic** -- sci-fi terminal vibes
- **Green Phosphor** -- classic Matrix/hacker green
- **Amber Phosphor** -- warm retro amber (shown above running Claude Code)
- **Vintage** -- dusty, flickering, barely-holding-together CRT
- **IBM DOS** -- the beige box experience
- And more...

All the CRT effects are configurable: scanlines, screen curvature, bloom, phosphor glow, flicker, burn-in, jitter, ambient light.

It is a fully functional terminal emulator -- run your shell, editors, CLI tools, whatever. It's not a toy.

## Build from source (macOS Apple Silicon)

### Prerequisites

Install Qt6 via Homebrew:

```bash
brew install qt@6 qt5compat qtshadertools
```

### Build

```bash
git clone --recursive https://github.com/dragontpe/cool-retro-term.git
cd cool-retro-term
qmake6
make -j$(sysctl -n hw.ncpu)
```

### Install

The build produces `cool-retro-term.app` in the project root. To install it properly:

```bash
# Copy the app bundle
cp -R cool-retro-term.app /Applications/cool-retro-term.app

# Bundle the QML terminal widget plugin
mkdir -p /Applications/cool-retro-term.app/Contents/Resources/qml/QMLTermWidget
cp -R qmltermwidget/QMLTermWidget/ /Applications/cool-retro-term.app/Contents/Resources/qml/QMLTermWidget/

# Create a launcher wrapper that sets the QML import path
cd /Applications/cool-retro-term.app/Contents/MacOS
mv cool-retro-term cool-retro-term.bin
cat > cool-retro-term << 'EOF'
#!/bin/bash
DIR="$(cd "$(dirname "$0")" && pwd)"
export QML2_IMPORT_PATH="${DIR}/../Resources/qml"
exec "${DIR}/cool-retro-term.bin" "$@"
EOF
chmod +x cool-retro-term
```

Launch from Applications or Spotlight like any other app.

### Settings

Access settings via the menu bar or **Cmd+,** (standard macOS shortcut). From there you can switch profiles and tweak individual effects.

## Tested on

- MacBook Pro (M4 Max)
- macOS Tahoe (26.2)
- Qt 6.11.0
- Xcode Command Line Tools

## Credits

All credit to [Swordfish90](https://github.com/Swordfish90) for creating cool-retro-term. This fork only exists because the upstream CI doesn't build for ARM64 yet.

## License

GPL-3.0 -- same as upstream.
