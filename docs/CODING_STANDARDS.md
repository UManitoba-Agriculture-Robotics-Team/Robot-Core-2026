# Coding Standards

This document describes the coding rules for Robot-Core-2026.
Almost every rule listed here is automatically enforced by **clang-format**, so you rarely need to think about formatting by hand. Just save your file (with format-on-save enabled in VS Code) and the tool handles the rest.

If something in this guide feels confusing, ask in the team chat. These rules exist to keep the codebase consistent and to prevent bugs on the robot's microcontrollers.

---

## 1. Formatting (enforced by clang-format)

These are handled automatically. You do not need to memorize them; the formatter will fix them for you every time you save.

| Rule | Setting |
|------|---------|
| Indent with spaces, not tabs | 4 spaces per level |
| Max line length | 100 characters |
| Braces | Same line as the statement |
| Single-line if/for/while bodies | Always wrap in braces |
| Pointer/reference alignment | Left-aligned (`int* ptr`, not `int *ptr`) |

**Example:**

```cpp
// Good (the formatter will produce this automatically)
if (sensorReady) {
    int data = readSensor();
    process(data);
}

// Bad (no braces around single-line body -- the formatter will add them)
if (sensorReady)
    process(readSensor());
```

---

## 2. Naming Conventions

clang-format does NOT enforce naming, so these need to be followed manually. Keep it simple:

| Thing | Style | Example |
|-------|-------|---------|
| Files | snake_case | `motor_driver.cpp`, `motor_driver.hpp` |
| Classes / Structs | PascalCase | `MotorDriver`, `SensorArray` |
| Functions | camelCase | `readSensor()`, `setSpeed()` |
| Variables | camelCase | `motorSpeed`, `sensorValue` |
| Constants & Pins | ALL_CAPS with underscores | `MAX_SPEED`, `MOTOR_PIN_1` |

---

## 3. File Organization

We use standard C++ files (`.cpp` and `.hpp`) for our modules, not standard Arduino sketches (`.ino`), because they are much easier to organize in larger projects.

Every module should have this layout:

```
src/drive/
    motor_driver.hpp      // Public header (declarations)
    motor_driver.cpp      // Implementation
```

**Include guards** -- use `#pragma once` at the top of every header file. 
**Arduino include** -- Always `#include <Arduino.h>` at the top of your `.cpp` and `.hpp` files if you are using Arduino functions like `digitalWrite` or `Serial`.

```cpp
// motor_driver.hpp
#pragma once
#include <Arduino.h>

class MotorDriver {
public:
    void setupPins();
    void setSpeed(int speed);

private:
    int speed_ = 0;
};
```

---

## 4. Arduino-Specific Rules (CRITICAL)

Because microcontrollers have very little RAM and no operating system, you must follow these rules to keep the robot from freezing or crashing:

1. **NO `delay()` calls in the main logic!**
   When you call `delay(1000)`, the entire robot freezes for 1 second. It cannot read sensors, it cannot respond to stop commands, and it cannot talk to the base station. 
   - *Alternative:* Use the `millis()` function to check how much time has passed without pausing the program (often called "blink without delay" pattern).

2. **AVOID the `String` class!**
   Do not use the capitalized `String` type (e.g., `String message = "hello";`). `String` uses dynamic memory allocation. On small microcontrollers, this quickly fragments the limited RAM and causes the robot to silently crash and reboot.
   - *Alternative:* Use C-style character arrays (`char message[20] = "hello";`) or process data byte-by-byte.

3. **Use Constants for Pin Numbers.**
   Never hardcode pin numbers in your functions. Define them at the top of your file or in a config file.
   ```cpp
   // Good
   const int LEFT_MOTOR_PIN = 5;
   digitalWrite(LEFT_MOTOR_PIN, HIGH);
   
   // Bad
   digitalWrite(5, HIGH); 
   ```

4. **Keep `loop()` clean.**
   The `loop()` function in `main.cpp` should be easy to read. It should mostly call other functions.
   ```cpp
   void loop() {
       readSensors();
       calculatePath();
       updateMotors();
   }
   ```

---

## 5. Comments

- Write comments that explain **why**, not **what**. The code already shows what it does.
- Add a brief comment at the top of each file saying what the file is for.

---

## Summary

- **Save often** to let clang-format handle all spacing and braces.
- Follow the naming conventions (`camelCase`, `PascalCase`, `ALL_CAPS`).
- **Do not use `delay()`** so the robot stays responsive.
- **Do not use `String`** to avoid memory crashes.
- Use `#pragma once` for header guards.

If you are ever unsure, ask the team! Consistency matters more than any single rule.
