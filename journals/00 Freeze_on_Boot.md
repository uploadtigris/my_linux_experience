✅ **Completed**
## Date
07/16/2025
## Issue
Stuck on logo boot screen ~ Fedora 42 KDE Plasma Desktop (installed 07/16/25 via automatic download, Fedora Media Writer)
## Description
- Lenovo Yoga 9 Laptop freezes on the Logo screen with the spinning icon frozen in place. Nothing happens after 10+ minutes.
## Attempts
1. Tried to add "nomodeset" to the boot parameters via grub. Many variations were attempted in method which mostly resulted in a black screen.
2. Live boot media with Fedora 24 KDE Plasma Desktop.
   
## Solution

#### Live boot media with Fedora 24 KDE Plasma Desktop.

** Open terminal **

``` bash
# Create mount point
sudo mkdir /mnt/system
```
``` bash
# Mount your system
sudo mount -t btrfs -o subvol=root /dev/nvme0n1p3 /mnt/system
```
``` bash
# create directories
sudo mkdir -p /mnt/system/dev
sudo mkdir -p /mnt/system/proc
sudo mkdir -p /mnt/system/sys
```
``` bash
# mount system
sudo mount --bind /dev /mnt/system/dev
sudo mount --bind /proc /mnt/system/proc
sudo mount --bind /sys /mnt/system/sys
```
``` bash
# chroot into system
sudo chroot /mnt/system
```
``` bash
# removing drivers and performing regen
dnf remove -y nvidia-driver nvidia-driver-libs akmod-nvidia
dracut --force --regenerate-all
systemctl set-default multi-user.target
```
``` bash
# exit 
exit
```
``` bash
# reboot
sudo reboot
```
- reboot into tty mode (no GUI; should do this automatically)... forgot username.
- Reboot while hold 'Esc'
- pressed 'e' on Fedora entry I am fixing
- added 'systemd.unit=emergency.target' to the line starting with 'Linux'
- Booted by pressing 'Ctrl+X'
- Now, in emergency mode, ran the following command to retreive my username

```
cat /etc/passwd | grep 1000
```

grabbed this username output. Exited.

*continuing on*

Updated the system in the root terminal after loging in with my credentials.
** There were almost 2 GB of updates to be done with the following command **
```
sudo dnf udpate
```

Reinstalling the GUI
```
sudo dnf install akmod-nvidia
sudo akmods --force
sudo dracut --force
sudo systemctl set-default graphical.target
sudo reboot
```

Temporarily reenable the GUI
```
sudo systemctl start display-manager
```

Installed the newer NVIDIA drivers
``` 
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```
``` 
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda
```

build drivers and boot image
```
sudo akmods --force
sudo dracut --force
sudo reboot
```

At system reboot, the system was going to TTY as a default.
Thus, I logged in with my credentials again and rain the following code to reset the defaults.
```
sudo systemctl start display-manager
```
```
sudo systemctl set-default graphical.target
```

Grepping for the current driver ~ verifying I am using an NVIDIA driver
```
lspci -k | grep -A 2 -i nvidia
```

* System works! YAY! *
