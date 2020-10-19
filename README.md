# super_shifted_caps

## What?

This is a custom
[XKB](https://wiki.archlinux.org/index.php/X_keyboard_extension) symbol for the
Caps Lock key. It changes the behaviour of the Caps Lock key to act as a Super
(Windows) key when pressed on its own, while preserving its original function
when pressing it in combination with Shift.

- CapsLock -> Super
- Shift + CapsLock -> CapsLock

It closely mimics the following [AutoHotkey](https://www.autohotkey.com/) script for Windows:

```autohotkey
+Capslock::Capslock
Capslock::LWin
```

This autohotkey script is included in this repo as well as `SuperShiftedCaps.ahk`.

## Why?

This is intended for people who use a keyboard that does not have Super keys,
but still want to use it as a modifier for shortcuts and who also don't need the
Caps Lock functionality so often that it has to be accessible through a single
dedicated key.

People who don't need Caps Lock functionality at all already can remap this key
to be Super using the following command:

```
setxkbmap -option caps:super
```

However, the original Caps Lock functionality is then lost.

### I need Super+Shift for some of my shortcuts!

That should not be too much of a problem. If you first press Shift and then Caps
Lock, the Caps Lock functionality will be executed. If you first press Caps Lock
and then Shift, X will interpret that as Super+Shift and you can still use your
shortcuts that way.

## Installation

1. Download the `customcaps` file from this repository.
2. Move it to the `/usr/share/X11/xkb/symbols` directory
3. Activate this custom xkb symbol:
   1. Print your current xkbmap, add the custom symbol to the xkb_symbols line,
      and save it to a file like this:
      ```
      setxkbmap -print | sed '/xkb_symbols/ {s/"/+customcaps(super_shifted_capslock)"/2 }' > caps.xkb
      ```

   2. Your file should look something like this:
      ```
      xkb_keymap {
              xkb_keycodes  { include "evdev+aliases(qwerty)" };
              xkb_types     { include "complete"      };
              xkb_compat    { include "complete"      };
              xkb_symbols   { include "pc+us(altgr-intl)+inet(evdev)+customcaps(super_shifted_capslock)"  };
              xkb_geometry  { include "pc(pc105)"     };
      };
      ```
   3. Load this xkbmap: `xkbcomp caps.xkb $DISPLAY`

You can load this xkbmap automatically after your X session started using the
mentioned `xkbcomp` command.

Alternatively, you can always generate this map on the fly through a temporary
file like this:
```
setxkbmap -print | sed '/xkb_symbols/ {s/"/+customcaps(super_shifted_capslock)"/2 }' > /tmp/caps.xkb && xkbcomp /tmp/caps.xkb $DISPLAY
```
