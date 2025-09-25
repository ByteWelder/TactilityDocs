# External Apps

## Fundamentals

Functions and variables in an external `.elf` file are referred to as "symbols". When you build an external app, it contains all the symbols
of that app itself, but you're likely also using external dependencies such as libc or LVGL.

The app refers to these symbols, but they don't actually exist inside its binary. When the app is loaded onto a device, a Tactility subsystem maps these symbols from the main firmware into the app. This mechanism is comparable to attaching a dynamically linked library where Tactility is the one facilitating the "library" functions.

These are the responsibilities of each component:

### Device Firmware

The device firmware contains symbol mappings:

It exposes a limited set of functions to the external apps. It stores a map that ties a function pointer to a function name. Function names are stripped by the ESP-IDF build system, so we have to manually map them. This mapping is defined in `tt_init.cpp` and is hard-wired into the firmware.

### TactilitySDK

TactilitySDK exposes symbol interfaces/names to the external apps:

It includes headers for TactilityC, LVGL and more. It exposes Tactility/LVGL/libc functionality to external apps. Linking with the app happens dynamically, so its symbols are not embedded statically into the app.

### External apps

Apps only contain the symbols defined by its own source files. All other symbols must be present in the main firmware **and** exposed explicitly by `tt_init.cpp` in `TactilityC`

## How to expose new symbols

### Exposing a libc or LVGL symbol

1. Add the symbol to `tt_init.cpp`.
2. Build a new device firmware and flash it.

Note: You don't need to publish and use a new `TactilitySDK`, as the libc function is available via an existing header file that is available to any external app project.

### Exposing a new Tactility symbol

1. Expose the symbol in `TactilityC` via a `.c` and `.h` file: this will wrap the C++ function into a C function and make it available to the apps.
2. Add the new `TactilityC` symbol to `tt_init.cpp`.
3. Build a new device firmware and flash it.
4. Build `TactilitySDK`
5. Build your external app with the new SDK.

## Create An External App

[App Fundamentals](app-fundamentals.md) will teach you the basics.

Example apps can be found in the [TactilityApps](https://github.com/ByteWelder/TactilityApps) project.

Build the app:
```bash
# Either build for all supported hardware:
python tactility.py build
# Or do a quicker build for just 1 target during development:
python tactility.py build esp32s3
```

Documentation for this command can be found [here](https://github.com/ByteWelder/TactilityTool).

### Install and test your app

First, make sure to set up the Tactility device:
- Wi-Fi should be on
- Development service should be enabled (via Settings -> Development)

Install the app to your ESP32S3 device as follows:

```bash
python tactility.py install 192.168.1.123
```

Make sure you target the right hardware and IP address!

Once the app is installed, you can start it remotely:

```bash
python tactility.py run 192.168.1.123
```

Also here you have to make sure the IP address is correct.

If you want to do build, install and run with one command:

```bash
# Either run (b)uild-(i)nstall-(r)un:
python tactility.py bir 192.168.1.123
# Or just "app goes brrr":
python tactility.py brrr 192.168.1.123
# Whatever floats your boat
```

## Debugging external apps

This hasn't been investigated yet, but the [approach from Zephyr](https://docs.zephyrproject.org/latest/services/llext/debug.html) might work.
In the case of Tactility, the mentioned breakpoint could be placed at `esp_elf_relocate()`

**Warning:** This is untested and might not work!

## TactilityC development

When making changes to `TactilityC`, you'll need to build your own SDK locally:

1. Open a terminal and navigate to the Tactility project root folder.
2. Build the firmware for the relevant target (e.g. ESP32 S3) with `idf.py build`
3. Make sure that the `release/` directory either doesn't exist or doesn't have a previous SDK in it.
4. Run `Buildscripts/release-sdk-current.sh`
5. Set environment variable `TACTILITY_SDK_PATH` - for example: `export TACTILITY_SDK_PATH=/path/to/Tactility/release/TactilitySDK`
6. Run `tactility.py build esp32s3 --local-sdk` (change "esp32s3" to the relevant hardware target where applicable)
