# Change Keyboard Layout

The keyboard layout settings enable the user to control the layout used on the text console and graphical user interfaces.

## Displaying the Current Settings

As mentioned before, you can check your current keyboard layout configuration with the following command:

```bash
localectl status
```

In the following output, you can see the keyboard layout configured for the virtual console and for the X11 window system.

```bash
~]$ localectl status
System Locale: LANG=en_US.utf8
VC Keymap: us
X11 Layout: us
```

## Listing Available Keymaps

To list all available keyboard layouts that can be configured on your system, type:

```bash
localectl list-keymaps
```

You can use grep to search the output of the previous command for a specific keymap name. There are often multiple keymaps compatible with your currently set locale. For example, to find available Czech keyboard layouts, type:

```bash
localectl list-keymaps | grep cz
cz
cz-cp1250
cz-lat2
cz-lat2-prog
cz-qwerty
cz-us-qwertz
sunt5-cz-us
sunt5-us-cz
```

## Setting the Keymap


To set the default keyboard layout for your system, use the following command as root:

```bash
sudo localectl set-keymap map
```

Replace map with the name of the keymap taken from the output of the localectl list-keymaps command. Unless the --no-convert option is passed, the selected setting is also applied to the default keyboard mapping of the X11 window system, after converting it to the closest matching X11 keyboard mapping. This also applies in reverse, you can specify both keymaps with the following command as root:

```bash
sudo localectl set-x11-keymap map
```

If you want your X11 layout to differ from the console layout, use the --no-convert option.

```bash
localectl --no-convert set-x11-keymap map
```

With this option, the X11 keymap is specified without changing the previous console layout setting.

Imagine you want to use German keyboard layout in the graphical interface, but for console operations you want to retain the US keymap. To do so, type as root:

```bash
sudo localectl --no-convert set-x11-keymap de
```

Then you can verify if your setting was successful by checking the current status:

```bash
localectl status
System Locale: LANG=de_DE.UTF-8
VC Keymap: us
X11 Layout: de
```

Apart from keyboard layout (map), three other options can be specified:

```bash
localectl set-x11-keymap map model variant options
```

Replace model with the keyboard model name, variant and options with keyboard variant and option components, which can be used to enhance the keyboard behavior. These options are not set by default. For more information on X11 Model, X11 Variant, and X11 Options see the kbd(4) man page.