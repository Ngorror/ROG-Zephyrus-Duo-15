# ROG-Zephyrus-Duo-15
All configuration hacks to transform Asus ROG Zephyrus Duo 15 `GW550LWS` into working developer machine

## Ubuntu/Mint

### Keyboard
There is no mechanical direct keys for Home and End.
I mapped my PtrSc and right Ctrl keys to be Home and End.
Please note with this change you will lose also Sys_Req as a key.
To get the key code - you can run in your console
```
xev
```

Modify the following lines in your [/usr/share/X11/xkb/symbols/pc](https://github.com/Ngorror/ROG-Zephyrus-Duo-15/blob/main/pc) file 

```
xkb_symbols "pc105" {
  ...
  key <RCTL> {	[ Control_R		]	};
  ...
  modifier_map Control{ Control_L, Control_R };
  ...
}
...
xkb_symbols "editing" {
  key <PRSC> {
    type= "PC_ALT_LEVEL2",
    symbols[Group1]= [ Print, Sys_Req ]
  };
  ...
}
```
to

```
xkb_symbols "pc105" {
  ...
  key <RCTL> {	[ END		]	};
  ...
  modifier_map Control{ Control_L };
  ...
}
...
xkb_symbols "editing" {
  key <PRSC> {
    type= "PC_ALT_LEVEL2",
    symbols[Group1]= [ Home, Print ]
  };
  key <RCTL> {        [  End                  ]       };
  ...
}
```

Please note I changed not only the Asus **[Right Control]** and **[PtrSc]** keys but also my Corsair K70 RGB TKL **[Right Windows]** key.
Now it is also mapped as **[Home]** button

### Connect Sennheiser PXC 550-II to Mint 20
create /etc/bluetooth/audio.conf
```
[General]
Enable=Source,Sink,Media,Socket
```
install the following apps
```
apt install pulseaudio pulseaudio-module-bluetooth pavucontrol blueman bluez 
```

do not forget to [v] Trust this device in blueman

in case of problems to conect to the device after pairing you can run `bluetoothctl` and investigate the device

to enable `a2dp_sink`

```
pacmd list-cards

# pacmd set-card-profile <card index> <profile name> 
pacmd set-card-profile 3 a2dp_sink
```

### Setup Spotify 
install `kdocker` to minimize Spotify. 
```
apt install kdocker
```

Set kdocker as startup program with the following command
```
kdocker -q -i /usr/share/spotify/icons/spotify-linux-24.png spotify %U
```

### Wireless doesn't start on Mint boot
If you have dual boot system, and you go through a cycle of 
- Windows Boot
- Windows Shutdown
- Linux Boot
- No wireless card in Mint

then you need to disable Windows Fast Start in `Control Panel => Power Options => Fast Startup`

It is good idea also to disable Secure Boot in your UEFI Bios too,

### Configure battery threshold
It's not specific to this laptop but [here is the source](https://www.reddit.com/r/linuxhardware/comments/g8kpee/psa_kernel_54_added_the_ability_to_set_a_battery/) and how I configured mine to load charge only to 60% - you have the same functionality in windows - in MyAssus desktop program.
```
$ cat /sys/class/power_supply/BAT0/status
Charging
$ cat /sys/class/power_supply/BAT0/capacity
74
$ echo 60 | sudo tee /sys/class/power_supply/BAT0/charge_control_end_threshold
$ cat /sys/class/power_supply/BAT0/status
Not charging
```
You want to execute this command on every start but it must be executed as root
so add to the root crontab the following row (`crontab -e`)
```
@reboot echo 60 | sudo tee /sys/class/power_supply/BAT0/charge_control_end_threshold
```

### Second screen zoom problems
This is applicable only for version with Full HD (1920x1080 300Hz) primary screen

When I set the Display resolution of the 2nd screen to 1920x550 then it displays twice two small copies of my 2nd screen vertically instead of working properly.

Versions of Mint before 20.3 and nvidia drivers before 470 - it was easy to just upscale the 2nd screen with Fraction scaling to 200%
With Mint 20.3 and nvidia drivers 470 ir newer (I've just installed 510) you can see only 153% in Fraction scaling dropdown

You can use the following command to adjust the 2nd screen image by just upscaling the output

```
xrandr --output DP-0 --scale 0.5x0.5
```

Note: keep your 2nd screen resolution to the native one 3840 x 1100

### Display brightness
I didn't manage to run any of the internet solutions to control my screens brightnes.
Now I'm using the following commands

for the 2nd display (strangely enough my Mint decided to initialize it first)
```
xrandr --output DP-0 --brightness 0.5
```

and the main screeen
```
xrandr --output DP-2 --brightness 0.5
```

If you have external monitors and you want to control them - get their names with
```
xrandr
```

here is an example of my setup now
```
ngoro@duo:~$ xrandr
Screen 0: minimum 8 x 8, current 1920 x 1630, maximum 32767 x 32767
DP-0 connected 1920x550+0+1080 (normal left inverted right x axis y axis) 344mm x 99mm
   3840x1100     60.02*+  48.02  
   1920x550      60.06    48.08  
DP-1 disconnected (normal left inverted right x axis y axis)
HDMI-0 disconnected (normal left inverted right x axis y axis)
DP-2 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 344mm x 193mm
   1920x1080     60.04*+ 300.18  
DP-3 disconnected (normal left inverted right x axis y axis)
DP-4 disconnected (normal left inverted right x axis y axis)

```

### Keyboard shortcut to Toggle Play/Pause in the current media player
in Preferences >> Keyboard >> Shortcut >> Custom Shortcut add
```
xdotool key --clearmodifiers XF86AudioPlay
```
[source](https://askubuntu.com/questions/1156148/add-new-play-pause-shortcut-key-not-spotify)
