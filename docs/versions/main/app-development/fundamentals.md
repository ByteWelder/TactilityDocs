# App Development - Fundamentals

Functions and variables in an external `.elf` file are referred to as "symbols". When you build an external app, it contains all the symbols
of that app itself, but you're likely also using external dependencies such as libc or LVGL.

The app refers to these symbols, but they don't actually exist inside its binary. When the app is loaded onto a device, a Tactility subsystem maps these symbols from the main firmware into the app. This mechanism is comparable to attaching a dynamically linked library where Tactility is the one facilitating the "library" functions.

These are the responsibilities of each component:

## Device Firmware

The device firmware contains symbol mappings:

It exposes a limited set of functions to the external apps. It stores a map that ties a function pointer to a function name. Function names are stripped by the ESP-IDF build system, so we have to manually map them. This mapping is defined in `tt_init.cpp` and is hard-wired into the firmware.

## TactilitySDK

TactilitySDK exposes symbol interfaces/names to the external apps:

It includes headers for TactilityC, LVGL and more. It exposes Tactility/LVGL/libc functionality to external apps. Linking with the app happens dynamically, so its symbols are not embedded statically into the app.

## External apps

Apps only contain the symbols defined by its own source files. All other symbols must be present in the main firmware **and** exposed explicitly by `tt_init.cpp` in `TactilityC`

