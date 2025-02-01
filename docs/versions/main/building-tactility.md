# Building Tactility

## Cloning from git

Ensure you clone the repository including the submodules! For example:

```sh
git clone --recurse-submodules -j8 https://github.com/ByteWelder/Tactility.git
```

## Build Requirements

- ESP32 hardware (or variant)
- [ESP-IDF 5.4](https://docs.espressif.com/projects/esp-idf/en/v5.4/esp32/get-started/index.html) or a newer v5.4.x
- Linux, macOS, or Windows via WSL2+Ubuntu (\*)

(\*) Continuously tested with latest Arch (x86-64) and Ubuntu 24.04 (x86-64). Sporadically tested with latest macOS (aarch64).

## Building

Make a copy of `sdkconfig.board.YOUR_BOARD` and name it `sdkconfig`. Copy from `sdkconfig.defaults` if you are setting up a custom board.

```sh
cp sdkconfig.board.<board-name> sdkconfig
```

Build the firmware:

```sh
idf.py build
```

Flash the firmware to a device and monitor the output:

```sh
idf.py -p <port> flash monitor
```

`<port>` is something like `/dev/ttyACM0` or `/dev/ttyUSB0` on Linux/macOS, or `COM3` on Windows.


