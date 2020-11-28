# ROG-Zephyrus-Duo-15
All configuration hacks to transform Asus ROG Zephyrus Duo 15 GW550LWS into working developer machine

## Ubuntu/Mint

### Keyboard
There is no mechanical direct keys for Home and End.
I mapped my PtrSc and right Ctrl keys to be Home and End.
Please note with this change you will lose also Sys_Req as a key.

Modify the following lines in your `/usr/share/X11/xkb/symbols/pc` file 

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

## Windows

## Connect Sennheiser PXC 550-II to Mint 20
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
