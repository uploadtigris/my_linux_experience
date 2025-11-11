# Date: 8/3/25
# Topic: I need to configure system for virtualization.

I need to allow the user (me) to be able to run virtual machines (VMs), as show in [this video](https://www.youtube.com/watch?v=rKH8XhPfJjE&t=4m1s)
```bash
sudo usermod -aG libvirt $(whoami) && sudo usermod -aG kvm $(whoami)
```
