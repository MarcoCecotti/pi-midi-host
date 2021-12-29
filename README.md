# :notes: pi-midi-host

> Setup a Raspberry Pi as a headless MIDI USB host, with auto-connection and MIDI merging of all sources.

Tested with a RPi 3B and Zero 2W, but should work with any model. MIDI Bluetooth will only work on model 3/4 and Zero 2W as earlier models don't have an onboard bluetooth chip (though it might be possible to used an external BT dongle).

## Usage

1. Download latest [Raspberry Pi OS lite image](https://downloads.raspberrypi.org/raspios_lite_armhf/images/) and install it on your SD card with [Raspberry Pi Imager](https://www.raspberrypi.com/software/)
1. Create a new file named `ssh` in `/boot` folder of the SD card to enable SSH access.
1. SSH to your RPi with `ssh pi@<IP_ADDRESS>` (default password is `raspberry`)
1. Run this command: `bash <(curl -Ls https://raw.githubusercontent.com/sinedied/pi-midi-host/main/setup.sh)`
1. Reboot

> Note: the filesystem is switched to read-only at the end of the setup, to avoid SD card corruption when powering off. To switch it back on and off, use the `rw` and `ro` commands.

### Aliases

Use these alias to quickly manage your midi setup:
- `midi`: show connected midi devices
- `connect`: reconnect all midi devices

## How to connect bluetooth devices

1. Disable SSP mode (if needed): `sudo hciconfig hci0 sspmode 0`
1. Turn on your bluetooth device and put it in pairing mode
1. Run `sudo bluetoothctl -a`
1. `default-agent`
1. `pair <BT_ADDRESS>`
1. `trust  <BT_ADDRESS>` to allow auto reconnection
1. `connect <BT_ADDRESS>` if connection did not work
1. `exit`

## Network access

You can SSH to the device without knowing its IP address using `ssh pi@midihub.local` after initial setup.

## Credits

Most of this work was based on instructions from [this post](https://neuma.studio/rpi-midi-complete.html).

Additions:
- All-in-one script to setup a new Pi
- MIDI in/out filters for specific devices
- Bluetooth device connection instructions
- Commands aliases
- Drivers for M-Audio MIDISPORT 2x2
