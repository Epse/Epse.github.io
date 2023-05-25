---
title: Belux Plugin Gate Alerts
description: The Belux Euroscope plugin alerts controllers on gate change, but this is sometimes intrusive. I go about changing this.
tags: vatsim euroscope cpp
---

The Belux Euroscope plugin now has configurable alerts whenever an assigned gate changes.
This post will serve as a combination of release notes and manual for this new feature.
A future post will contain more information on the actual implementing.

## TL;DR: The Release Notes

Starting with Belux v1.1.4, the alerts on gate change have received thee states: `blink`, `quiet` or `none`.
Older versions behave like the `blink` setting, which is also the default when no config options are provided. This version is downloadable <a href="/assets/artifacts/Belux_1_1_5.dll" download="Belux.dll">here</a>.

`quiet` still gives gate change messages, but does not mark them as urgent, meaning they do not get the blinking blue background.
They will be marked as unread (meaning blue header text) untill you read them, unless you are marked as busy.
If this is the first time you're hearing of the Euroscope [`.busy`](https://www.euroscope.hu/wp/command-line-reference/) command, have a look, you might like it.

Finally, the `none` setting does what you expect:
no messages will be shown on gate change.

If you use the Arrival List, the gate text colour will still indicate a recent change.

### How do I change this?

There are two ways to change this mode, one that's temporary and local to your current Euroscope instance, and one that's stored and global.

The temporary value involves using `.belux alert_gates <blink/quiet/none>`, where you only provide one of those three values, skipping the angle brackets.

For permanent setting, you can modify the `belux_config.json` file,
which will be located next to the dll in your Plugins folder.
You can introduce the `on_gate_change` value there, as follows.
You can of course pick any value you like.
The other function values are picked as per the default Belux config in this example.

```json
{
    "API_timeout" : 700,
    "debug_mode" : false,
	"on_gate_change": "quiet",
    "functionalities" : {
        "fetch_gates" : true,
        "set_initial_climb" : true,
        "mach_visualisation" : false,
        "rwy_sid_assigner" : false
    }
}
```
