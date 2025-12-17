# CHIP-8 Emulator

A CHIP-8 emulator written in C with SDL2 for graphics rendering. CHIP-8 is an interpreted programming language developed in the 1970s for simple video games.

## Features

- Full CHIP-8 instruction set implementation
- 64x32 pixel monochrome display
- SDL2-based graphics rendering with 10x window multiplier (640x320 resolution)
- 16-key hexadecimal keyboard input
- Sound timer support with system beep
- Delay timer implementation
- Memory management with 4KB RAM
- Stack-based subroutine calls (16 levels deep)

## Requirements

- GCC compiler (MinGW for Windows)
- SDL2 library
- Windows OS (current implementation uses Windows-specific functions)

## Building

The project uses a Makefile for compilation. To build the emulator:

```bash
make
```

This will compile all source files and create the executable at `./bin/main.exe`.

To clean build artifacts:

```bash
make clean
```

## Usage

Run the emulator with a CHIP-8 ROM file:

```bash
./bin/main.exe <path_to_rom>
```

Example with included ROMs:

```bash
./bin/main.exe ./bin/pong.rom
./bin/main.exe ./bin/INVADERS
```

## Controls

The CHIP-8 uses a 16-key hexadecimal keypad (0-F). The keys are mapped as follows:

```
CHIP-8 Keypad:          Keyboard Mapping:
+-+-+-+-+               +-+-+-+-+
|1|2|3|C|               |1|2|3|4|
+-+-+-+-+               +-+-+-+-+
|4|5|6|D|               |Q|W|E|R|
+-+-+-+-+    →          +-+-+-+-+
|7|8|9|E|               |A|S|D|F|
+-+-+-+-+               +-+-+-+-+
|A|0|B|F|               |Z|X|C|V|
+-+-+-+-+               +-+-+-+-+
```

Keyboard mapping:
- `0-9`: Keys 0-9
- `A-F`: Keys A-F

## Project Structure

```
chip8/
├── bin/                    # Executable and ROM files
│   ├── INVADERS           # Space Invaders ROM
│   ├── pong.rom           # Pong ROM
│   └── SDL2.dll           # SDL2 dynamic library
├── include/               # Header files
│   ├── chip8.h            # Main emulator structure
│   ├── chip8keyboard.h    # Keyboard handling
│   ├── chip8memory.h      # Memory management
│   ├── chip8registers.h   # CPU registers
│   ├── chip8screen.h      # Display handling
│   ├── chip8stack.h       # Stack operations
│   ├── config.h           # Configuration constants
│   └── SDL2/              # SDL2 headers
├── lib/                   # SDL2 libraries
├── src/                   # Source files
│   ├── main.c             # Entry point and SDL event loop
│   ├── chip8.c            # Core emulator logic
│   ├── chip8keyboard.c    # Keyboard implementation
│   ├── chip8memory.c      # Memory implementation
│   ├── chip8screen.c      # Screen implementation
│   └── chip8stack.c       # Stack implementation
├── docs/                  # Documentation
├── Makefile              # Build configuration
└── LICENSE               # MIT License
```

## Technical Details

- **Memory**: 4096 bytes (0x000-0xFFF)
- **Program Start Address**: 0x200
- **Display**: 64x32 pixels, monochrome
- **Registers**: 16 general-purpose 8-bit registers (V0-VF)
- **Stack**: 16 levels for subroutine calls
- **Timers**: 60Hz delay and sound timers
- **Character Set**: Built-in sprites for hexadecimal digits (0-F)

