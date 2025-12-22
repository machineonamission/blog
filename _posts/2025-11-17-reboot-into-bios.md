---
layout: post
title: "How to reboot into BIOS from Windows"
date: 2025-11-17 10:10:00 -0600
---

```batch
shutdown /r /fw /t 0
````

**The command requires admin.** Start your terminal as administrator on Windows, or
use [sudo if you enabled it](https://learn.microsoft.com/en-us/windows/advanced-settings/sudo/).

# Add permanent alias

To add a permanent alias to Powershell that the command `bios` reboots into bios, run

```batch
notepad $PROFILE
```

Then in the notepad window that opened, add this

```batch
function bios {
    sudo shutdown /r /fw /t 0
}
```

**This version requires [sudo](https://learn.microsoft.com/en-us/windows/advanced-settings/sudo/)**. If you don't want
to enable it, remove the `sudo` from the command, like so

```batch
function bios {
    shutdown /r /fw /t 0
}
```

but then you'll have to open powershell/terminal as administrator every time to use it.

Replace `bios` with any command you want (with no spaces) to set your own alias.