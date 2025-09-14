# Create An External App

[App Fundamentals](app-fundamentals.md) will teach you the basics.

An example app can be found in the main repository in the [ExternalApps](https://github.com/ByteWelder/Tactility/tree/main/ExternalApps) folder. Make a copy and use it as a template to build your own.

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
