# Nexus-HelloWorld

A boilerplate addon project for Guild Wars 2 Nexus, designed to serve as a starting point for developing addons in the Nexus ecosystem.

![image](https://github.com/user-attachments/assets/8aa51d15-5738-4656-ac2d-07c1344e2295)

## Overview

This template provides a basic structure for creating Nexus addons, implementing essential features like:

- Basic addon lifecycle management (load/unload)
- Settings management with JSON persistence
- ImGui rendering setup
- Keybind handling
- Event system integration
- Mumble Link integration
- Quick access menu integration

## Features

- **Settings Management**: JSON-based settings system with automatic saving/loading
- **UI Components**: Basic ImGui window implementation with:
  - Toggle visibility
  - Lock/unlock position
  - Auto-resize functionality
- **Integration Points**:
  - Mumble Link data access
  - Nexus Link data access
  - Window resize event handling
  - Keybind processing
  - Quick access menu entries

## Project Structure

```
Nexus-HelloWorld/
├── src/
│ ├── entry.cpp # Main addon entry point and core functionality
│ ├── settings.cpp # Settings management implementation
│ ├── shared.cpp # Shared resources and globals
│ ├── settings.h # Settings declarations
│ └── shared.h # Shared header declarations
├── scripts/
│ ├── generate_version.ps1 # Version generation for Windows
│ └── generate_version.sh # Version generation for Linux
└── CMakeLists.txt # CMake build configuration
```

## Building

This project can be built on both Windows and Linux, but will always produce a Windows DLL as the final output since Nexus addons are Windows DLLs.

### Prerequisites

- CMake 3.8 or higher
- C++20 compatible compiler
- On Linux: MinGW toolchain (x86_64-w64-mingw32-gcc-posix and x86_64-w64-mingw32-g++-posix)

### Windows

#### Using Visual Studio

1. Open the project folder in Visual Studio
2. Select your preferred configuration (Debug/Release)
3. Build the solution

#### Using Command Line

Debug build:

```cmd
cmake --preset x64-debug && cmake --build --preset x64-debug
```

Release build:

```cmd
cmake --preset x64-release && cmake --build --preset x64-release
```

### Linux

#### Prerequisites

On Debian/Ubuntu:

```bash
sudo apt install build-essential cmake ninja-build mingw-w64
```

On Arch Linux:

```bash
sudo pacman -S base-devel cmake ninja mingw-w64-gcc
```

On Fedora:

```bash
sudo dnf install cmake ninja-build mingw64-gcc-c++ mingw64-winpthreads-static
```

#### Building

Debug build:

```bash
cmake --preset linux-debug && cmake --build --preset linux-debug
```

Release build:

```bash
cmake --preset linux-release && cmake --build --preset linux-release
```

The compiled addon will be output as `Nexus-HelloWorld.dll` in the `out/build/linux-debug/bin` or `out/build/linux-release/bin` directory.

Note: When building on Linux, the project uses MinGW for cross-compilation to produce a Windows DLL. The build system automatically configures:

- Cross-compilation toolchain (x86_64-w64-mingw32)
- Static linking of runtime libraries
- Windows headers and libraries

If you get "windows.h not found" errors, make sure you have the MinGW development packages installed properly for your distribution.

## Getting Started

1. Clone this repository (including submodules):

   ```
   git clone https://github.com/yourusername/Nexus-HelloWorld.git --recurse-submodules
   ```

2. Rename the project:

   - Update `CMakeLists.txt` project name
   - Update addon name in `entry.cpp`
   - Update relevant paths and identifiers

3. Build the project using the instructions above

4. Copy the resulting DLL to your Nexus addons folder

## Development Notes

### Key Integration Points

#### Keybinds

    APIDefs->InputBinds.RegisterWithString("KB_HW_TOGGLEVIS", ProcessKeybind, "(null)");

#### Events

    APIDefs->Events.Subscribe("EV_WINDOW_RESIZED", OnWindowResized);
    APIDefs->Events.Subscribe("EV_MUMBLE_IDENTITY_UPDATED", OnMumbleIdentityUpdated);

#### Rendering

    APIDefs->Renderer.Register(ERenderType_Render, AddonRender);
    APIDefs->Renderer.Register(ERenderType_OptionsRender, AddonOptions);

### Settings

- Settings are automatically saved to `{AddonDirectory}/settings.json`
- Uses JSON format for easy modification
- Includes built-in serialization and deserialization

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

MIT License

## Acknowledgments

- Guild Wars 2 Nexus team for the addon framework @ https://github.com/RaidcoreGG/Nexus
- ImGui for the UI framework @ https://github.com/ocornut/imgui
