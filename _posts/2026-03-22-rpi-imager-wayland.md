---
title: RPi Imager under Wayland
description: Running RPi Imager as root under wayland can cause issues
tags: linux wayland arch
---

**TL:DR** scroll down to the code fence, run that, it'll be fine.

Running [Raspberry Pi Imager](https://www.raspberrypi.com/software/) under Wayland on Linux can cause issues,
because it needs root permissions.
Sure, its implementation should probably be changed so we don't have to run the entire app as root,
but this is the world we live in.

When running it as root you will often get some seemingly arcane Qt errors,
complaining about failing to initialize any Qt platform plugins.
There is a fairly lengthy discussion about this issue on the relevant [issue tracker](https://github.com/raspberrypi/rpi-imager/issues/1322),
but none of the suggested fixes exactly worked for me.
Even the solution proposed by another Arch and Wayland user didn't work for me,
as pkexec has never really worked under Sway for me.

Luckily, you can just substitute sudo.
The real important bit is passing on the `WAYLAND_DISPLAY` environment.

```sh
sudo env WAYLAND_DISPLAY=$WAYLAND_DISPLAY /usr/bin/rpi-imager
```

