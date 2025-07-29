✅ **Completed (temporary fix; need to rerun the last command sometimes)**

## Date: 07/16/25

## Issue: Yoga Pro 9 16IMH9 are not playing audio through the subwoofers on Fedora Linux 42 KDE Plasma Edition 

## Solution: 

[Top Comment (& Credit) from this article](https://askubuntu.com/questions/1487745/bass-speakers-not-working-on-lenovo-yoga-pro-9-14irp8-ubuntu-22-04)

1. Install i2c-tools
```
sudo apt install i2c-tools
```
2. "Get the i2c-bus number on which the TIAS2781 component is connected..." To find the bus number: ```ls -l /sys/bus/i2c/devices/``` and check which of i2c-# you get for TIAS2781. For me, it was the last line in the command respons. Mine is i2c-2.
3. Download [this script](https://bugzilla.kernel.org/attachment.cgi?id=304763). *Replace only THE LAST "2" for your specific i2c number for TIA2781*
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
# Test it with the correct bus number (2, as you discovered)
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
