---
title: "Disable IR Camera in Ubuntu"
date: 2020-09-22T12:31:54-04:00
categories:
    - blog
tags:
    - ubuntu
    - tweak
draft: false
---

## Problem 
After installing [KDE Neon](https://neon.kde.org) and using [Firefox](https://www.mozilla.org) I tried to call someone with the deceasing Google Hangouts. Firefox was asking for permission to access the microphone and the camera. But unfortunately only the IR camera from the [Thinkpad T480](http://www.thinkwiki.org/wiki/Category:T480) was listed as camera device. The normal - non-IR - camera wasn't listed, not even in a scroll-down menu.

After some searching I found mostly the solution to disable the `uvcvideo` kernel driver module. But this turns off all webcams and not just the IR webcam.

## Solution

The most useful solution was found on [Ask Ubuntu](https://askubuntu.com/a/1187373). First of all, the vendor and product id for the IR camera is required. The command `lsusb` helps to gather these information:

```sh
arthurk@t480:~$ lsusb
[...] 
Bus 001 Device 003: ID 5986:1141 Acer, Inc Integrated IR Camera
Bus 001 Device 015: ID 0451:82ff Texas Instruments, Inc. Integrated IR Camera
[...]
```

We see two IR related devices and both will be disabled. In this case the ids are:

- 5986 and 1141
- 0451 and 82ff

Now create an `udev` rule in `/etc/udev/rules.d` with

```sh
sudo vim /etc/udev/rules.d/40-disable-ir-webcam.rules
```

and the following two lines 

```sh
ATTRS{idVendor}=="5986", ATTRS{idProduct}=="1141", RUN="/bin/sh -c 'echo 0 >/sys/\$devpath/authorized'"
ATTRS{idVendor}=="0451", ATTRS{idProduct}=="82ff", RUN="/bin/sh -c 'echo 0 >/sys/\$devpath/authorized'"
```

to make this change permanent even after a reboot.

The [man page](https://linux.die.net/man/7/udev) for `udev` explains the meaning of the lines in the rules. `ATTRS` searches the device path until all `ATTRS` are matched and `$devpath` is a variable with the actual device path that the system has detected for the specific device. 

## Result

Now the IR camera is disabled when the system is started and Firefox requests permission to access the normal built-in webcam.