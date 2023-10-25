# Dell Vostro 5481 Hackintosh
Hackintosh for DELL Vostro 5481 with OpenCore


# MacOS 14 Sonoma!

> The AppleALC kext is a custom build from my repo BunoBBS/AppleALC
## What works
- Wifi (Airport and airdrop[? theoretically, but not tested])
- Bluetooth
- Ethernet (Gigabit)
- Brightness
- HDMI and DP alt mode over USB-C
- All USBs
- Card reader
- High precision touchpad with gestures
- Battery and percentage
- CPU power management* (EPP 9F and EPB 15[power save], low power apps for the MB Air) 
- Brighness keys
- Sleep and lid wake
- Deep Sleep (aka hybrid sleep, standby) (fixed in a 3 hour timer, can't change)
- Hibernation (if use hibernatemode 25)
- Disabled Nvidia MX130 (saves power ~5w)
- Media keys and volume
- Audio (custom non-official layout at layout 15 for combojack to work)
- ~~Combojack (mic and headphones joined port)~~
  - ~~Use [MicFix](https://github.com/WingLim/MicFix)to alternate automatically~~ (stopped working, I'll create a new codec to try to solve that)
- Keyboard backlight

> * **Power management** 
> With the current configuration, the max clock is 3.7 GHz and 20 Watts for a
> few seconds; after it drops to 15 Watts sustained. In battery save mode, max
> is 2.6 GHz and 10 Watts.

## What doesn't work
- After hibernation, the screen is black and requires to close and reopen lid
- To connect an external monitor via USB-C, if it is not recognized, unplug and plug it back in.
- ~~To connect external monitor via USB-C, the display has to be turned off and on again~~
  - ~~To achieve this, close and reopen the lid or~~
  - ~~Make a shortcut to turn off the display or~~
  - ~~Use the `pmset displaysleepnow` command~~

- The high GPU usage was due to the scaling being enabled, even for the max resolution of the display. To fix this, use a helper application such as [displayplacer](https://github.com/jakehilborn/displayplacer) and use `scaling=off` in the command.
- ~~For some reason using 4k60 external display (via alt mode DP USB-C) the GPU usage goes to 100% on any large refresh of the screen (video or animations) and the CPU throttles aggressively.~~
  - ~~the workaround is to `sudo pkill -9 WindowServer`. After login, th usage will be much less for a while.~~
  - ~~**If you know a fix for this, please open an issue and tell me how!!**~~
