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
install `kdocker` to minimize Spotify. Set kdocker as startup program with the following command
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
