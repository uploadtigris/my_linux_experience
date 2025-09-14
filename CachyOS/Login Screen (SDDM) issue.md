# Login Screen (SDDM) issue

The SDDM screen looked super wimpy. This was the non default setting, needed to run the following to fix it:

```
sudo systemctl enable sddm --force
sudo systemctl set-default graphical.target
```
This brought up an error:
```
configuration file "/var/lib/sddm/.config/sddm-greeter/qt6rc" not writable
```
To solve this we do the following:
```
# Check ownership and permissions of the parent directories
ls -ld /var/lib/sddm
ls -ld /var/lib/sddm/.config
ls -ld /var/lib/sddm/.config/sddm-greeter
```
```
sudo -u sddm touch /var/lib/sddm/.config/sddm-greeter/qt6rc
```
```
sudo systemctl restart sddm
```
