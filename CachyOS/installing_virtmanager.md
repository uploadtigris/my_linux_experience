# Installing Virtual Machine Manager

Install Flatpak
```
sudo pacman -S flatpak
```

Install Virt-manager via Flathub
```
flatpak install flathub org.virt_manager.virt-manager
```

Install all required virtualization packages
```
sudo pacman -S qemu-full virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat iptables-nft
```

Enable and start system services
```
sudo systemctl enable --now libvirtd.service
sudo systemctl enable --now virtlogd.service
```

Add your user to the libvirt group
```
sudo usermod -aG libvirt $USER
```

Enable KVM kernel modules
```
lsmod | grep kvm
```

Verify virtualizaiton support
```
LC_ALL=C lscpu | grep Virtualization
```

verify libvirt is working
```
# if this returns a table (even if empty), we are good
sudo virsh list --all
```
