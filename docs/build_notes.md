# Build Notes - Getting CHIP-8 to Compile and Run

**Date:** December 17, 2025  
**Platform:** Windows with MSYS2/UCRT64

## Problem Overview

The project initially failed to build due to SDL2 library compatibility issues. The local SDL2 libraries in `./lib` were 32-bit versions incompatible with the 64-bit UCRT GCC compiler being used.

## Error Messages Encountered

When attempting to compile with:
```bash
gcc -g -I ./include ./src/main.c -L ./lib -lmingw32 -lSDL2main -lSDL2 -o ./bin/main.exe
```

The linker produced multiple errors:
```
skipping incompatible ./lib/libSDL2main.a when searching for -lSDL2main
skipping incompatible ./lib/libSDL2.dll.a when searching for -lSDL2
cannot find -lSDL2main: No such file or directory
cannot find -lSDL2: No such file or directory
```

## Root Cause

- The project contained 32-bit SDL2 libraries
- The system GCC compiler is 64-bit UCRT (from MSYS2)
- Architecture mismatch prevented linking

## Solution

### Step 1: Install Compatible SDL2 Libraries

Installed the correct SDL2 package for UCRT64 using MSYS2's package manager:

```bash
C:\msys64\usr\bin\bash.exe -lc "pacman -S --noconfirm mingw-w64-ucrt-x86_64-SDL2"
```

This installed:
- `mingw-w64-ucrt-x86_64-SDL2-2.30.9-1`
- `mingw-w64-ucrt-x86_64-vulkan-loader-1.3.296.0-1` (dependency)

### Step 2: Compile Without Local SDL2 Libraries

Removed the `-L ./lib` flag to use system-installed SDL2 libraries:

```bash
gcc -g -I ./include ./src/main.c -lmingw32 -lSDL2main -lSDL2 -o ./bin/main.exe
```

### Step 3: Run the Program

```bash
.\bin\main.exe
```

## Build Command Reference

### Current Working Build Command

```bash
gcc -g -I ./include ./src/main.c -lmingw32 -lSDL2main -lSDL2 -o ./bin/main.exe
```

### Command Breakdown

- `-g` - Include debugging information
- `-I ./include` - Add include directory for headers
- `./src/main.c` - Source file to compile
- `-lmingw32` - Link MinGW32 runtime library
- `-lSDL2main` - Link SDL2 main library (provides WinMain wrapper)
- `-lSDL2` - Link SDL2 core library
- `-o ./bin/main.exe` - Output executable path

## Current Project State

The project is now simplified to a basic SDL2 test program that:
- Creates a 640x320 window titled "Chip8 Window"
- Displays a white 40x40 pixel square on black background
- Handles SDL_QUIT events for clean shutdown

## Prerequisites for Building

1. **MSYS2 installed** at `C:\msys64`
2. **UCRT64 GCC toolchain** installed
3. **SDL2 libraries** installed via:
   ```bash
   pacman -S mingw-w64-ucrt-x86_64-SDL2
   ```

## Future Considerations

- If restoring full CHIP-8 emulator functionality, all `chip8*.c` source files will need to be restored
- The `config.h` header was deleted and would need recreation
- Consider updating Makefile to use system SDL2 libraries instead of local copies
- May want to detect and handle both 32-bit and 64-bit build environments

## Verification

Build successful with exit code 0. Program runs and creates an SDL2 window as expected.
