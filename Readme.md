# Roadii

Automatic Linux Wii guitar remapping using `evsieve`

## Requirements

This is built with SteamOS as its primary consumer. Specifics will differ per distro. I develop for SteamOS in an Arch Linux container using [Distrobox](https://distrobox.it), and for Bazzite in a Fedora toolbox container using [toolbx](https://containertoolbx.org).

- Rust
- `libudev`
- [`evsieve`](https://github.com/KarsMulder/evsieve)
  - `libevdev`
- `pkg-config`

### One-Liners

#### Arch Linux

```bash
sudo pacman -S rust systemd-libs libevdev pkgconf
```

#### Fedora

```bash
sudo dnf install cargo systemd-devel libevdev libevdev-devel
```

### Hardware

This is tested with a Wii MotionPlus Wiimote, and a Guitar Hero World Tour guitar. It should work with the other Guitar Hero Wii guitars (which have a slot for a Wiimote), and other Wiimotes, as long as both are recognised by the Linux kernel, though they have not been tested.

## Compiling / installing
```bash
# Download source & extract
wget https://github.com/ticky/roadii/archive/refs/heads/main.tar.gz -O roadii-main.tar.gz
tar -xzf roadii-main.tar.gz

# Enter directory & compile
cd roadii-main
cargo build --release

# Install roadii to /home/deck/bin
sudo install -m 755 -t /home/deck/bin target/release/roadii
# The previous line can probably be replaced with chmod && cp if need be
```

## Setup

Provided in the `etc` folder are an example udev rule and systemd service to automatically run Roadii when a supported Wii guitar controller is connected. This presumes you are using SteamOS, adapting to other Linux systems is left as an exercise for the reader.

1. Install `roadii` and `evsieve` executables to `/home/deck/bin`
2. Copy `etc/systemd/system/roadii@.service` to `/etc/systemd/system`, and `etc/udev/rules.d/99-roadii.rules` to `/etc/udev/rules.d`.
3. Reload the udev rules with `sudo udevadm control --reload`

Now you're ready to connect your Wii guitar via Bluetooth!

## Usage

With the udev rules and systemd service configured, the guitar will appear as several devices; ignore any which mention Nintendo, as `evsieve` has taken exclusive access to them - the one you care about now is simply called "Wiitar".

It's configured to match a PlayStation 3 guitar controller as closely as possible, providing a reasonble mapping for both navigating SteamOS, emulators, and your game of choice.

## Caveats

- Only one connected Wii guitar controller is supported
