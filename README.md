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

