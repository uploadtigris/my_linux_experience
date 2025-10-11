✅ **Completed (temporary fix; need to rerun the last command sometimes)**

## Date: 07/16/25

## Issue: Yoga Pro 9 16IMH9 are not playing audio through the subwoofers on Fedora Linux 42 KDE Plasma Edition 

## Solution: 

[Credit is from the top comment in this article](https://askubuntu.com/questions/1487745/bass-speakers-not-working-on-lenovo-yoga-pro-9-14irp8-ubuntu-22-04)

1. Install i2c-tools
```
sudo apt install i2c-tools
```
2. "Get the i2c-bus number on which the TIAS2781 component is connected..." To find the bus number: ```ls -l /sys/bus/i2c/devices/``` and check which of i2c-# you get for TIAS2781. For me, it was the last line in the command response. Mine is i2c-2.
i.e.
```
tmendez@fedora:~$ ls -l /sys/bus/i2c/devices/
total 0
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-0 -> ../../../devices/pci0000:00/0000:00:15.0/i2c_designware.0/i2c-0
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-1 -> ../../../devices/pci0000:00/0000:00:15.1/i2c_designware.1/i2c-1
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-10 -> ../../../devices/pci0000:00/0000:00:02.0/i2c-10
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-11 -> ../../../devices/pci0000:00/0000:00:02.0/i2c-11
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-12 -> ../../../devices/pci0000:00/0000:00:02.0/i2c-12
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-13 -> ../../../devices/pci0000:00/0000:00:02.0/i2c-13
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-14 -> ../../../devices/pci0000:00/0000:00:02.0/i2c-14
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-15 -> ../../../devices/pci0000:00/0000:00:02.0/drm/card1/card1-eDP-1/i2c-15
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-16 -> ../../../devices/pci0000:00/0000:00:02.0/drm/card1/card1-DP-1/i2c-16
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-17 -> ../../../devices/pci0000:00/0000:00:02.0/drm/card1/card1-DP-2/i2c-17
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-18 -> ../../../devices/pci0000:00/0000:00:02.0/drm/card1/card1-DP-3/i2c-18
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-19 -> ../../../devices/pci0000:00/0000:00:02.0/drm/card1/card1-DP-4/i2c-19
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-2 -> ../../../devices/pci0000:00/0000:00:15.2/i2c_designware.2/i2c-2
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-20 -> ../../../devices/i2c-20
lrwxrwxrwx. 1 root root 0 Oct 10 17:57 i2c-21 -> ../../../devices/pci0000:00/0000:00:1f.4/i2c-21
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-3 -> ../../../devices/pci0000:00/0000:00:15.3/i2c_designware.3/i2c-3
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-4 -> ../../../devices/pci0000:00/0000:00:19.0/i2c_designware.4/i2c-4
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-5 -> ../../../devices/pci0000:00/0000:00:19.1/i2c_designware.5/i2c-5
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-6 -> ../../../devices/pci0000:00/0000:00:02.0/i2c-6
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-7 -> ../../../devices/pci0000:00/0000:00:02.0/i2c-7
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-8 -> ../../../devices/pci0000:00/0000:00:02.0/i2c-8
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-9 -> ../../../devices/pci0000:00/0000:00:02.0/i2c-9
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-GXTP7936:00 -> ../../../devices/pci0000:00/0000:00:15.3/i2c_designware.3/i2c-3/i2c-GXTP7936:00
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-LTCN0001:00 -> ../../../devices/pci0000:00/0000:00:15.1/i2c_designware.1/i2c-1/i2c-LTCN0001:00
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-SYNA2BA6:00 -> ../../../devices/pci0000:00/0000:00:15.0/i2c_designware.0/i2c-0/i2c-SYNA2BA6:00
lrwxrwxrwx. 1 root root 0 Oct 10 17:56 i2c-TIAS2781:00 -> ../../../devices/pci0000:00/0000:00:15.2/i2c_designware.2/i2c-2/i2c-TIAS2781:00
```
Look at the last line near the furthest right side ```i2c-2/i2c-TIAS2781:00```. This indicates that i2c-2 is the one to specify.

4. Download [this script](https://bugzilla.kernel.org/attachment.cgi?id=304763). *Replace only THE LAST "2" for your specific i2c number for TIA2781*
```
sudo bash ./2pa-byps.sh 2
```
For me, the second I ran the last command the subwoofers came on.

On every reboot, the script needs to be run. I could use either Systemd or create a cron job for these tasks. I will opt to use Cron as my task is simple and doesn't require advanced features.

```
# Move your script from Downloads to /usr/local/bin/
sudo mv ~/Downloads/2pa-byps.sh /usr/local/bin/subwoofer-fix```
```
```
# verify that the script is in the system wide 'usr' folder
ls /usr/local/bin/ | grep subwoofer-fix
```
```
# make the script executable
sudo chmod +x /usr/local/bin/subwoofer-fix
```
```
# Test it with the correct bus number
sudo /usr/local/bin/subwoofer-fix 2
```
```
# Open the root crontab
sudo crontab -e
```
enter this line into the file.
```
@reboot /usr/local/bin/subwoofer-fix 2 >> /var/log/subwoofer-fix.log 2>&1
```
if using vi, then press Esc & enter ```:wq``` , then ENTER
```
# verify the cron job was added
# Make sure the cron job is listed as in the cron tab file
sudo crontab -l
```
*note: Any output or errors will be logged to ```/var/log/subwoofer-fix.log```*
