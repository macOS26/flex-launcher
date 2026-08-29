This is a fork of Flex Launcher adapted for macOS M1/M4 Arm64 and Raspberry Pi5 Arm64 Linux running SuperBox64OS. SuperBox64OS is based on Ubuntu 23.10 with redesigned desktop and dock icons for a Mac-like experience.

Additions by Heisenburg:
Added native Arcade Spinner support plus enhanced Trackball and Classic Arcade game controller support. Flex Launcher serves as the primary Arcade game emulator launcher for SuperBox64OS, also created by Heisenburg.

Technical Notes:
SDL2 video changes support Metal graphics on macOS. The Mac version was used for development and testing the Linux counterpart. Mac development ceased after Linux completion, with a new native Swift launcher called "Game Changer" created separately from scratch. 

Flex Launcher is programmed in C and optimized for ARM64 architectures with specialized gaming hardware support. Todd has patched, updated and compiled over a dozen game C based emulators to run natively on the SuperBox64OS. Most of his work required a deep understanding of the SDL video framework.

This Mac version demo screen features an early POC for SuperBox64OS:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5a94b388-e2a4-4139-91ef-397347e61a79" />

macOS Game Changer UI for comparision, produced later and inspired by Flex Launcher:
```
https://github.com/search?q=owner%3ASuperBox64+game+changer+repo%3ASuperBox64%2FGameChanger
```
<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/ceac2dc9-7e62-4acb-85ab-8c8b4fa55d2f" />


install macOS dependencies via homebrew:
```
brew install cmake libsdl2 libsdl2_image libsdl2_ttf pkg-config
```

mac build:
```
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make
./flex-launcher
```

configure the config.ini file:
```
config.ini
```

SuperBox64 Website:
```
https://superbox64.com
```

# Flex Launcher
</div>
<details open>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about">About</a>
    </li>
    <li>
      <a href="#screenshots">Screenshots</a>
    </li>
    <li>
      <a href="#installation">Installation</a>
      <ul>
        <li><a href="#windows">Windows</a></li>
        <li><a href="#linux">Linux</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
      <ul>
        <li><a href="#controls">Controls</a></li>
        <li><a href="#debugging">Debugging</a></li>
      </ul>
    <li><a href="#development-status">Development Status</a></li>
    <li><a href="#documentation">Documentation</a></li>
    <li><a href="#credits">Credits</a></li>
  </ol>
</details>

## About
Flex Launcher is a customizable application launcher and front end designed with a TV-friendly [10 foot user interface](https://en.wikipedia.org/wiki/10-foot_user_interface), intending to mimic the look and feel of a streaming box or game console. Flex Launcher allows you to launch applications on your HTPC or couch gaming PC entirely by use of a TV remote or a gamepad. No keyboard or mouse required!

Flex Launcher is compatible with both Windows and Linux (including Raspberry Pi devices).

## Screenshots
![Screenshot 1](docs/assets/screenshots/screenshot1.png "Screenshot 1")

https://user-images.githubusercontent.com/95071366/208355237-11f00cbb-9cc3-436b-98f7-8de350e584a7.mp4

## Installation
Executables are available for Windows 64 bit, Linux x86-64, and Raspberry Pi. You can also compile the program yourself using the [compilation guide](docs/compilation.md).

### Windows
Download the win64 .zip file from the [latest release](https://github.com/complexlogic/flex-launcher/releases/latest) and extract the contents to a directory of your choice. Flex Launcher should be run on an up-to-date Windows 10 system, or Windows 11.

### Linux
Binary packages are available on the [release page](https://github.com/complexlogic/flex-launcher/releases) for APT and pacman based distributions. You may use the commands below to install.

#### APT-based x86-64 Distributions (Debian, Ubuntu, etc.)
This package is compatible with Debian Bullseye and later, Ubuntu 21.04 and later.
```Shell
wget https://github.com/complexlogic/flex-launcher/releases/download/v2.1/flex-launcher_2.1_amd64.deb
sudo apt install ./flex-launcher_2.1_amd64.deb
```
#### Pacman-based x86-64 Distributions (Arch, Manjaro, etc.)
```Shell
wget https://github.com/complexlogic/flex-launcher/releases/download/v2.1/flex-launcher-2.1-1-x86_64.pkg.tar.zst
sudo pacman -U flex-launcher-2.1-1-x86_64.pkg.tar.zst
```
#### Raspberry Pi
This package is compatible with Raspbian Bullseye and later, 64 bit only.
```Shell
wget https://github.com/complexlogic/flex-launcher/releases/download/v2.1/flex-launcher_2.1_arm64.deb
sudo apt install ./flex-launcher_2.1_arm64.deb
```
#### Copying Assets to Home Directory
The Linux packages install a default config file and assets to `/usr/share/flex-launcher`. It is strongly recommended to *not* edit this config file directly, as it will be overwritten if you upgrade to a later version of Flex Launcher. Instead, copy these files to your home directory and edit it there.
```Shell
cp -r /usr/share/flex-launcher ~/.config
sed -i "s|/usr/share/flex-launcher|$HOME/.config/flex-launcher|g" ~/.config/flex-launcher/config.ini
```

## Usage
Flex Launcher uses an INI file to configure the menus and settings. Upon  startup, the program will search for a file named `config.ini` in the following locations in order:
1. The current working directory
2. The directory containing the `flex-launcher` executable
3. Linux only: `~/.config/flex-launcher`
4. Linux only: `/usr/share/flex-launcher`

If your config file is in one of the above locations, Flex Launcher can be started simply by double clicking the executable file or adding it to autostart. If your config file is in a non-standard location, you must specify the path via command line argument:
```Shell
flex-launcher -c /path/to/config.ini
```
Flex Launcher ships with a default config file which is intended strictly for demonstration purposes. If you try to start one of the applications, it is possible that nothing will happen because the install path is different on your system, or you don't have the application installed at all. See the [configuration file documentation](docs/configuration.md#configuring-flex-launcher) for instuctions on how to change the menus and settings.

### Controls
The keyboard arrow keys move the highlight cursor left and right. Enter selects the current entry, backspace goes back to the previous menu (if applicable), and Esc quits the program. 

#### TV Remotes
Flex Launcher does not feature built-in decoding of IR or CEC signals. If you plan to use a TV remote to control the device, it is assumed that these signals are decoded by the OS or another program and mapped to keyboard presses, which can then be received by Flex Launcher.

#### Gamepads
Gamepad controls are built-in to the program, but are disabled by default. To enable them, open your configuration file and, under the "Gamepad" section, change the "Enabled" setting from false to true. After that, the gamepad controls should "Just Work" for most users. If your gamepad is not recognized automatically, or you want to change the default controls, see the [gamepad controls documentation](docs/configuration.md#gamepad-controls).

### Debugging
Flex Launcher has a debug mode which may be enabled as follows:
```Shell
flex-launcher -d
```
This will output a logfile named `flex-launcher.log` in the same directory as `flex-launcher.exe` on Windows, and in `~/.local/share/flex-launcher` on Linux. 

## Development Status
Flex Launcher has reached a mature state, and there are currently no feature releases planned for the future. I've started a [new HTPC launcher project](https://github.com/complexlogic/big-launcher) which is similar in nature to Flex Launcher, but aims to provide a more advanced, Smart TV-like user interface. My future development effort will be focused on that new project, but I will still maintain Flex Launcher for bugfixes and dependency updates.

## Documentation
Here is a list of available documentation:
- [Configuration](docs/configuration.md#configuring-flex-launcher)
- [General Setup Guide](docs/setup.md#setup-guide)
  - [Windows-specific Setup Guide](docs/setup_windows.md#windows-setup-guide)
  - [Linux-specific Setup Guide](docs/setup_linux.md#linux-setup-guide)
- [Compilation Guide](docs/compilation.md#compilation-guide)

## Credits
Flex Launcher is made possible by the following projects:
- [SDL](https://github.com/libsdl-org/SDL), including the subprojects:
  - [SDL_image](https://github.com/libsdl-org/SDL_image)
  - [SDL_ttf](https://github.com/libsdl-org/SDL_ttf)
- [Nanosvg](https://github.com/memononen/nanosvg)
- [inih](https://github.com/benhoyt/inih)
- [Numix icons](https://github.com/numixproject)

The design of Flex Launcher was strongly influenced by the excellent desktop application launcher [xlunch](https://github.com/Tomas-M/xlunch).
