#DisplayLink Drivers

## The Problem

Originally tried these commands from [this reddit post](https://www.reddit.com/r/cachyos/comments/1p7q1mt/external_display_through_usb_c_dock_j5create/)
```
pacman -S linux-headers
yay -S displaylink
systemctl enable displaylink.service
```
Unfortunately, evdi DKMS failed to build on the CachyOS kernels:
```
Error! Bad return status for module build on kernel: 6.17.9-2-cachyos
Error! Bad return status for module build on kernel: 6.12.59-2-cachyos-lts
==> ERROR: Missing 6.17.9-arch1-1 kernel modules tree
```
CachyOS does not ship the matching Arch kernel build tree required for DKMS to build evdi, which the DisplayLink driver depends on.
This means DisplayLink cannot currently compile on the CachyOS kernels.

This is a known limitation on kernels that add custom patches (like CachyOS, Xanmod, Liquorix, etc).

## The Fix
Installing the vanilla arch kernel and headers
```
sudo pacman -S linux linux-headers
```
Rebooted and selected Linux (Arch) kernel from the bootloader
```
# verifying that the driver changed
uname -r
```
should see something like --> 6.17.9-arch1-1

Reinstalling the DisplayLink drivers
```
yay -S displaylink evdi-dkms
```
Enabling DisplayLink service on start & rebooting
```
sudo systemctl enable --now displaylink.service
sudo reboot
```
Success 🎊
