# dell-vostro-5481-hackintosh
Hackintosh for DELL Vostro 5481 with OpenCore

The AppleALC kext is a custom build from my repo BunoBBS/AppleALC

# MacOS Ventura 13.4

## What works
- Wifi (Airport and airdrop[? theoretically, but not tested])
- Bluetooth
- Ethernet (Gigabit)
- Brightness
- HDMI and DP over USB-C
- All USBs
- Card reader
- High precision touchpad with gestures
- Battery and percentage
- CPU power management* (EPP 9F and EPB 15[power save], low power apps for the MB Air) 
- Brighness keys
- Sleep and lid wake
- Deep Sleep (aka hybrid sleep, standby) (fixed in a 3 hour timer, can1t change)
- Hibernation (if use hibernatemode 25)
- Disabled Nvidia MX130 (saves power ~5w)
- Media keys and volume
- Audio (custom non-official layout at layout 15 for combojack to work)
- Combojack (mic and headphones joined port)
  - Use [MicFix](https://github.com/WingLim/MicFix) to alternate automatically
- Keyboard backlight

> * **Power management** 
> With the current configuration, the max clock is 3.7 GHz and 20 Watts for a
> few seconds; after it drops to 15 Watts sustained. In battery save mode, max
> is 2.6 GHz and 10 Watts.

## What doesn't work
- After hibernation, the screen is black and requires to close and reopen lid
- To connect external monitor via USB-C, the display has to be turned off and on again
  - To achieve this, close and reopen the lid or
  - Make a shortcut to turn off the display or
  - Use the `pmset displaysleepnow` command
- For some reason using 4k60 external display (via alt mode DP USB-C) the GPU usage goes to 100% on any large refresh of the screen (video or animations) and the CPU throttles aggressively.
  - the workaround is to `sudo pkill -9 WindowServer`. After login, th usage will be much less for a while.
  - **If you know a fix for this, please open an issue and tell me how!!**
