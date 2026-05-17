# Robot-Core-2026

This repo contains the C++ code for all sub-modules that run on the actual robot's microcontrollers (Arduino).
We use **PlatformIO** (via VS Code) to manage libraries, build the code, and flash it to the board.
Everything is formatted automatically with `clang-format` so the entire team writes code that looks the same.

---

## Table of Contents

1. [Project Structure](#project-structure)
2. [Prerequisites](#prerequisites)
3. [Getting Started](#getting-started)
4. [Building and Flashing](#building-and-flashing)
5. [Code Formatting](#code-formatting)
6. [Coding Standards](#coding-standards)
7. [Useful Links](#useful-links)

---

## Project Structure

```
Robot-Core-2026/
|-- platformio.ini          # PlatformIO config (board, framework, libraries)
|-- .clang-format           # Formatter rules (do not delete)
|-- .editorconfig           # Consistent whitespace across editors
|-- .vscode/                # VS Code workspace settings
|   |-- settings.json
|   +-- extensions.json
|-- docs/
|   +-- CODING_STANDARDS.md # Naming conventions, Arduino style rules, etc.
|-- src/
|   |-- main.cpp            # Entry point (setup and loop)
|   |-- drive/              # Motor and drivetrain control
|   |-- sensors/            # Sensor drivers and data processing
|   |-- navigation/         # Path planning and localization
|   +-- utils/              # Shared helpers and utilities
+-- tests/                  # Unit tests (if applicable)
```

Each folder under `src/` is a self-contained module for different robot functionalities.

---

## Prerequisites

You need the following tools installed on your machine before you can build or contribute to this project.

| Tool | What It Does |
|------|--------------|
| **VS Code** | Our recommended code editor |
| **PlatformIO Extension** | VS Code extension to build/flash Arduino code and manage dependencies |
| **clang-format** | Automatically formats C++ code (can be installed via VS Code extension) |
| **Git** | Version control |

---

## Getting Started

1. **Clone the repo**

   ```bash
   git clone https://github.com/UManitoba-Agriculture-Robotics-Team/Robot-Core-2026.git
   cd Robot-Core-2026
   ```

2. **Open in VS Code**

   Open the `Robot-Core-2026` folder in VS Code.

3. **Install Recommended Extensions**

   VS Code should show a pop-up in the bottom right corner recommending extensions for this repository. Click **"Install All"**.
   (If you don't see the prompt, open the Extensions tab on the left sidebar, search for `@recommended`, and install the PlatformIO IDE and Clang-Format extensions).

4. **Wait for PlatformIO to Initialize**

   The first time you open a PlatformIO project, it will take a few minutes to download the necessary Arduino toolchains and frameworks. Let it finish.

---

## Building and Flashing

With PlatformIO installed, you will see a little **Alien head icon** on the left sidebar, and a row of icons on the bottom blue status bar of VS Code.

- **To Build (Compile):** Click the **Checkmark** (✓) icon on the bottom status bar.
- **To Upload (Flash to Arduino):** Connect your Arduino via USB and click the **Right Arrow** (→) icon on the bottom status bar.
- **Serial Monitor:** Click the **Plug** icon on the bottom status bar to view text printed via `Serial.println()`.

*(Note: If you change the specific Arduino board we are using, you will need to update the `board` property in the `platformio.ini` file).*

---

## Code Formatting

### What is clang-format?

`clang-format` is a tool (some code written by a group of very nice and smart people) that automatically formats C/C++ code. We give it a set of rules (stored in the `.clang-format` file at the repo root folder), and it rewrites your code to follow those rules exactly. Things like indentation, brace placement, and spaces are handled for you.

### Why do we use it?`

Without a formatter, every person on the team writes code in their own style. That makes pull requests noisy and the codebase harder to read. With `clang-format`, everyone's code looks identical regardless of personal preference.

### How to use it

**The easy way: format on save**
We have configured VS Code to run the formatter every time you save a file! Just write your code, hit `Ctrl+S` (or `Cmd+S` on Mac), and it will automatically clean up your spacing and braces.

You never have to argue about tabs vs. spaces again.

---

## Coding Standards

The full coding standards document lives here:

**[docs/CODING_STANDARDS.md](docs/CODING_STANDARDS.md)**

Here is the short version:

- **Formatting** is handled entirely by clang-format on save.
- **Naming**: files use `snake_case`, classes use `PascalCase`, functions and variables use `camelCase`, constants/pins use `ALL_CAPS`.
- **Arduino Specifics**: Do not use the `String` class (it crashes Arduinos), and avoid `delay()` (it freezes the robot).
- **Headers**: use `#pragma once` instead of traditional include guards.

Read the full document before writing your first pull request.

---

## Useful Links

| Resource | Link |
|----------|------|
| Coding Standards (this repo) | [docs/CODING_STANDARDS.md](docs/CODING_STANDARDS.md) |
| Arduino Reference | [www.arduino.cc/reference/en/](https://www.arduino.cc/reference/en/) |
| PlatformIO Documentation | [docs.platformio.org](https://docs.platformio.org/) |
| clang-format Style Options | [clang.llvm.org/docs/ClangFormatStyleOptions.html](https://clang.llvm.org/docs/ClangFormatStyleOptions.html) |
