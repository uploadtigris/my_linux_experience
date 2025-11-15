Lenovo Pro 9 uses the Intel Meteor Lake-P HD Audio Controller

requires the SOF (Sound Open Firmware) driver
```
sudo dnf install sof-firmware
systemctl --user enable --now pipewire pipewire-pulse
sudo reboot
```

 *Update 11/15/25* subwoofers not working again.
 - Checked on the Arch Wiki for Pulse-Audio
 - Asked ChatGPT some questions
 - Understood that the incorrect topology was loaded ```Topology file: intel/sof-ace-tplg/sof-hda-generic-4ch.tplg```
 - The solution was to run the following commands to update the kernel

```
sudo dnf --enablerepo=updates-testing upgrade kernel kernel-modules kernel-modules-extra
sudo reboot
```
Now audio is working again.

