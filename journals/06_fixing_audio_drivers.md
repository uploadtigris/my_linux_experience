Lenovo Pro 9 uses the Intel Meteor Lake-P HD Audio Controller

requires the SOF (Sound Open Firmware) driver
```
sudo dnf install sof-firmware
systemctl --user enable --now pipewire pipewire-pulse
sudo reboot
```
