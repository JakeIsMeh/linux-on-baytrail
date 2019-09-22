# Debian 10.1.0 Buster
### Last updated: 22/9/2019
# Overview

Lenovo 100s:

What's not working out of the box?

| Feature            | Fix? |
|--------------------|------|
| Webcam   | No   |
| Battery  | ?    |
| Wi-Fi    | Yes  |
| Sound    | Yes  |

---
# Installation
## Prerequesites:
1. A >1GB USB Flash drive
2. Debian Buster multiarch netinst iso ([link](https://cdimage.debian.org/debian-cd/10.1.0/multi-arch/iso-cd/))
3. Any computer capable of running [UNetbootin](https://unetbootin.github.io/) (Windows users: see [Rufus](https://rufus.ie/))
4. A phone capable of Wi-Fi tethering over USB/A Linux compatible Wi-Fi adpater

## Instructions:
01. Flash the Debian ISO you downloaded onto the USB flashdrive using UNetbootin/Rufus
02. Plug into laptop, along with your phone/Wi-Fi adpater.
03. Press the Novo button to boot into the UEFI menu.
04. Select your USB Flashdrive in the UEFI boot menu.
05. Select either Graphical Install or Install, both are identical in outcome.
06. As the installer is preparing for setup, you can now enable USB Tethering on your phone.
07. Proceed on with the installation.
08. When the installer asks your to choose a network device, select your Wi-Fi adapter/Tethering Phone.
09. Finish the installation.
10. Reboot, login, and run fixes and any post-install scripts you may have.

# Fixes

## Battery

See the bug report filed [here](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=927163).

Status: Not Release Critical, on wishlist.

## Wi-Fi
Before proceeding, connect a tethering phone/Linux compatible Wi-Fi adapter.

```
$ su -
$ apt install firmware-realtek
$ reboot
```
## Sound
```
$ su -
$ apt install firmware-intel-sound
$ reboot
```
