# Create An External App

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

## Install and test your app

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

## TactilityC development

When making changes to `TactilityC`, you'll need to build your own SDK locally:

1. Open a terminal and navigate to the Tactility project root folder.
2. Build the firmware for the relevant target (e.g. ESP32 S3) with `idf.py build`
3. Make sure that the `release/` directory either doesn't exist or doesn't have a previous SDK in it.
4. Run `Buildscripts/release-sdk-current.sh`
5. Set environment variable `TACTILITY_SDK_PATH` - for example: `export TACTILITY_SDK_PATH=/path/to/Tactility/release/TactilitySDK`
6. Run `tactility.py build esp32s3 --local-sdk` (change "esp32s3" to the relevant hardware target where applicable)
