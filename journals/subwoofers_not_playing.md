✅ **Completed:**

## Date: 07/16/25

## Issue: Yoga Pro 9 16IMH9 are not playing audio through the subwoofers on Fedora Linux 42 KDE Plasma Edition 

## Solution: 

[Top Comment (& Credit) from this article](https://askubuntu.com/questions/1487745/bass-speakers-not-working-on-lenovo-yoga-pro-9-14irp8-ubuntu-22-04)

1. Install i2c-tools
```
sudo apt install i2c-tools
```
2. "Get the i2c-bus number on which the TIAS2781 component is connected..." To find the bus number: ```ls -l /sys/bus/i2c/devices/``` and check which of i2c-# you get for TIAS2781. For me, it was the last line in the command respons. Mine is i2c-2.
3. Download [this script](https://bugzilla.kernel.org/attachment.cgi?id=304763). *Replace 2 for your i2c number for TIA2781*
```
sudo bash ./2pa-byps.sh 2
```
For me, the second I ran the last command the subwoofers came on. Good luck!
