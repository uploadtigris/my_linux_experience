## CachyOS Maintenance (Arch-Based)

**Update system (weekly):**
```bash
sudo pacman -Syu
```

**Clean package cache:**
```bash
sudo paccache -r
```

**Remove orphaned packages:**
```bash
sudo pacman -Qtdq | sudo pacman -Rns -
```

**AUR update (if using yay/paru):**
```bash
yay -Syu
```
