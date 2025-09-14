# Login Screen (SDDM) issue

The SDDM screen looked super wimpy. This was the non default setting, needed to run the following to fix it:

```
sudo systemctl enable sddm --force
sudo systemctl set-default graphical.target
```
