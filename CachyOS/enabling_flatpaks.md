# Enabling Flatpaks

Installing Flatpak
```
sudo pacman -S flatpak
```
Enabling Flathub
```
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

## Installing applications
If Flatpak file was downloaded:
```
flatpak install ./md.obsidian.Obsidian.flatpakref
```
If downloading Flatpak file from flathub:
```
flatpak install flathub com.discordapp.Discord
```

Check the file was downloaded
```
flatpak list | grep -i obsidian
```



