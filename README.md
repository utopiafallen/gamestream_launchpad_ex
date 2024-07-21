# GameStream Launchpad 🚀

![](demo.gif)

GameStream Launchpad (GSLP) offers a configurable environment for NVIDIA GameStream sessions with your PC from NVIDIA Shield hardware and/or [Moonlight](https://github.com/moonlight-stream) clients. The tool is made with popular launchers in mind and is extensible to any use case through custom config files. By running GSLP as a "game" through Moonlight or SHIELD instead of adding games or launchers directly, it improves the GameStream experience by doing the following when the session opens:

 1. Sets a **custom display resolution** on the host only while the session is open.
 2. Opens and maximizes **your favorite game launcher**, which allows you to do things such as display games from multiple stores (IE: Playnite for Steam, Origin, Epic, Xbox GamePass, etc).
 3. Launches **your background programs** which close at the end of the session (such as a controller remapping program you like to use when streaming only).
 4. Automatically **ends the GameStream session** and reverts the host resolution when the launcher window is closed.
 
Configurations are included for **[Playnite](https://github.com/JosefNemec/Playnite)** fullscreen mode, **[RetroArch](https://github.com/libretro/RetroArch)**, **[GOG Galaxy 2](https://www.gog.com/galaxy)**, and a general purpose remote desktop.

## Basic Setup
 1. Install [Playnite](https://github.com/JosefNemec/Playnite) (recommended, supports controllers) or [GOG Galaxy 2](https://www.gog.com/galaxy) and configure it to your liking.
 2. Download the latest [release](https://github.com/cgarst/gamestream_launchpad/releases/) and extract the files somewhere.
 3. Open GeForce experience and navigate to Settings > SHIELD > ADD.
 4. In the file picker, select the `.bat` script with the launcher and resolution you want your computer to have during the GameStream.
 
## Customization
There are two types of configurable files:
 1. A `.ini` file that describes the launcher's path, window name, your desired background programs, and other settings.
 2. A `.bat` file which launches GSLP describing which `.ini` file and which host resolution to use.
 
A different launcher or custom location can be defined in a new `.ini` config file like the included ones. New resolutions can be added by making a new `.bat` file like the examples. The config's included example launches [JoyToKey](https://joytokey.net/en/), which would allow you to map additional button mapping/combo functions. Some ideas for controller remapping would be to map to Shift-Tab for Steam overlay, Win-G for Xbox gamebar, Win-Alt-PrintScr for screenshot, or Win key for cutscene pauses.

The config file supports an unlimited number of background processes. Paths can be defined with environment variables, or just regular absolute paths. A debug mode can be enabled here which will leave the console window running to check the output after a session. You can also specify if you want the session to end when the game launcher window is closed (default), or not until the process is totally killed.

Example config for Playnite:
```
[LAUNCHER]
# The path to your Playnite.FullscreenApp.exe
launcher_path = %%LOCALAPPDATA%%\Playnite\Playnite.FullscreenApp.exe
# Name of the window to watch to close the session when it's gone
launcher_window_name = Playnite

[BACKGROUND]
# List as many exe's or bat's as you want here. They will run at the start of the GameStream session and be killed at the end.
# background_exe_1 = C:\Program Files (x86)\JoyToKey\JoyToKey.exe
# background_exe_2 = C:\WINDOWS\system32\mspaint.exe

[SETTINGS]
# Set debug = 1 to leave a window running after gamestream to see error messages from GSLP
debug = 0
# Set sleep_on_exit to 1 to put the computer to sleep after the session
sleep_on_exit = 0
# Set close_watch_method to "process" to wait for the launcher process to totally die to exit (can be problematic if it closes to tray), or "window" to just wait the the window to close
close_watch_method = window
```

## Resolution Recommendations
In most cases, the ideal resolution is the one closest to your client system running Moonlight or the NVIDIA app. Make sure to set the client-side streaming quality option at or above the resolution used by GSLP. A resolution can only be set when your system already supports it. However, custom resolutions can be added through the NVIDIA Control Panel's "Change resolution" settings. Additionally, you can still run a 1440p or 4k stream on a system with a 1080p monitor by enabling DSR Factors in the NVIDIA Control Panel's "Manage 3D Settings" globals.

If your client device's native resolution isn't 16:9 then you will likely need to enable a client-side option to stretch the video to fit your screen. The Moonlight client offers this option in it's settings.

## Development

### Developer Dependencies
 1. Install [Python 3.11](https://www.python.org/) for Windows, ensuring that you select "Add Python to PATH" during installation.
 2. Install [pipenv](https://pypi.org/project/pipenv/) via `pip install pipenv`.

### Developer Setup
 1. Clone this repo.
 2. In the repo directory, run `pipenv.exe install` to setup the Python environment.
 3. Ensure that things are working by running `pipenv.exe run gamestream_launchpad.py 1280 720 gamestream_playnite.ini` and closing Playnite.
 4. Build the exe with `build.bat`

