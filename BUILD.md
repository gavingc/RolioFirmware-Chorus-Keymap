# ZMK Build

TL;DR This repo is also built using Github Actions. See Actions for the latest public firmware builds: click on the latest successful build, and at the bottom of the page under Artifacts download the firmware.zip file.

https://github.com/gavingc/RolioFirmware-Chorus-Keymap/actions

The detailed information that follows is only for building using a native, ZMK configured, local Zephyr toolchain.

https://zmk.dev/docs/development/local-toolchain/build-flash

The motivation for local builds may be outright preference, and/or to more rapidly try out and learn different things. Then commit and push, and hopefully see a successful Actions build too.

## Use venv

Don't forget to activate the virtual environment in the ZMK checkout dir:

```bash
cd zmk/
source .venv/bin/activate
```

## Use App Directory

All actions below are performed in the app/ subdirectory of the ZMK checkout:

```bash
cd app/
```

## Clean/Pristine

When building for a new board enable the pristine `-p` argument for the first clean build:

```bash
west build -p -b new_board...
```

This argument may be left enabled if the build times are satisfactory, it doesn't seem to make too much difference on an i7 powered laptop.

## Faster Repeat Builds

After the initial clean build, the ZMK docs suggest to omit all arguments except the build directory:

```bash
west build -d build/left
```

This command didn't seem to work in testing on this repo, there may be no benefit in pursuing it further if the build times are satisfactory.

## ZMK Studio Snippet

Add the `-S` argument to include the `studio-rpc-usb-uart` snippet:

```bash
west build -d build/left -S studio-rpc-usb-uart ...
```

## Build Commands

These build commands assume that the external ZMK module RolioFirmware-Chorus-Keymap (this repo) has been cloned to the same directory as zmk. Such that it is 2 directories up from the zmk/app/ directory. If the RolioFirmware-Chorus-Keymap directory is in another location then specify the full absolute path in the commands.

Once the .venv has been activated, and the current directory has been changed to app/ then the following build commands may be used.

Left:

```bash
west build -p -d build/left -b nice_nano_v2 -S studio-rpc-usb-uart -- -DSHIELD="rolio_left vista508" \
    -DZMK_EXTRA_MODULES="./../../RolioFirmware-Chorus-Keymap/" \
    -DZMK_CONFIG="./../../RolioFirmware-Chorus-Keymap/config"
```

Firmware file location after a successful build: `zmk/app/build/left/zephyr/zmk.uf2`

Right:

```bash
west build -p -d build/right -b nice_nano_v2 -S studio-rpc-usb-uart -- -DSHIELD="rolio_right vista508" \
    -DZMK_EXTRA_MODULES="./../../RolioFirmware-Chorus-Keymap/" \
    -DZMK_CONFIG="./../../RolioFirmware-Chorus-Keymap/config"
```

Firmware file location after a successful build: `zmk/app/build/right/zephyr/zmk.uf2`


