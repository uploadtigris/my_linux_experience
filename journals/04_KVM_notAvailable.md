Date: 8/19/25

Issue: “Warning: KVM is not Available” in VIRT-MANAGER. 

## Solution:

[I used this website. Number 2 was the culprit](https://bobcares.com/blog/virt-manager-warning-kvm-is-not-available/). 

I navigated and selected (on a MSI B350 TOMAHAWK motherboard): 

BIOS/UEFI Menu --> OC (Overclocking) --> Advanced CPU Configuration --> SVM Mode ; set to [Enabled]
