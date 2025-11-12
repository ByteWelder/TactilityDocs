# Guide

This page explains how to create a new device implementation.

## Update Kconfig

Define the new device in `Firmware/Kconfig`.

## Create project folder

Make an empty new folder in `Boards/`: the name should match board identifier that was set in the sdkconfig file.

## Create device.properties

The `sdkconfig` is generated from `device.properties` file in `Boards/your-device-id/`

See the [device.properties documentation](device-development/device-properties.md) for more info.

Create a `sdkconfig.board.yourboard` file. It's good to use one of the other boards of a similar architecture as reference.

Devices with a large flash ROM (e.g. `16 MB`) will take long to flash. Consider making making a copy of `sdkconfig.board.yourboard` named `sdkconfig.board.yourboard.dev` with a `4 MB` partition table. The `.dev` files are ignored by git.

## Implementation

Look at other board projects to see how they are set up. The LilyGO T-Deck is one of the better reference implementations.
Keep in mind that other boards might have the same or similar hardware, so you can possibly copy parts of their implementations. (e.g. the display and/or touch driver)

Make sure there's a `extern const tt::hal::Configuration hardwareConfiguration = { .. }` variable declared. It needs to be declared exactly like this. Place it in a file named `Configuration.cpp`

Re-use existing drivers from:

- Tactility at `Drivers/`
- [components.espressif.com](https://components.espressif.com/) and write your own wrappers. Look at Tactility's `Drivers/` for examples.

Alternatively, you can implement your own or refactor another open source driver.

If you find `esp_lcd` or `esp_lcd_touch` drivers, you can implement them more easily with `Drivers/EspLcdCompat/` functionality:

- `class EspLcdTouch` will assist with a touch driver implementation
- `class EspLcdSpiDisplay` is the recommended base class for SPI display drivers
- `class EspLcdDisplayV2` is the recommended base class for non-SPI display drivers
- `class EspLcdDisplay` can be used in more complex driver scenarios

## Continuous Integration

Update the board matrix in `.github/workflows/build-firmware.yml`
