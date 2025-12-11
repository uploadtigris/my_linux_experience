/dev directory
- What is the /dev directory?
	- shows each of the files that represent each device connected to the system
- Interacting with Device Files
	- `/dev/null` --> discard output
	- there is no `dev` command -->  us `ls` or `cat` to interact with the dev files
- the evolution of /dev
	- before, a static /dev file would contain all possible hardware devices to be connected to the system
	- now, a system like `udev` manages the /dev linux environment
		- dynamically removes and creates files for devices as they are connected and removed

device types

```bash
ls -l /dev

rwxr-xr-x           - root    8 Dec 11:14  accel  
drwxr-xr-x          - root    9 Dec 22:04  block  
drwxr-xr-x          - root    8 Dec 11:13  bus  
drwxr-xr-x          - root    9 Dec 22:06  char  
drwxr-xr-x          - root    8 Dec 11:13  cpu  
drwxr-xr-x          - root    8 Dec 11:14  disk  
drwxr-xr-x          - root    8 Dec 11:13  dma_heap  
drwxr-xr-x          - root    8 Dec 11:14  dri

#- Permissions
#- Owner
#- Group
#- Major Device Number
#- Minor Device Number
#- Timestamp
#- Device Name

# First letter in Permissions
# c - character
# b - block
# p - pipe
# s - socket
```

- **character devices**
	- transfer data one character at a time
- **block device**
	- transfer data in large, fixed-size blocks
- **pipe devices**
	- FIFOs, allow for inter-process communication
	- Act like character devices but channel their output to another process
- **Socket Devices
	- also facilitate communication b/w processes
	- more versatile than pipe, can support multiple processes, even across a network

- major number --> identifies the driver type (HDD, Floppy, etc)
- minor number --> identifies the instances (1st floppy, 2nd floppy)

Device Names
- SCSI and Modern Storage Devices
	- the SCSI (Small Computer System Interface) subsystem
	- sd --> SCSI disk { alphas are for drives ; numerics are for partitions}
		- /dev/sda --> the first storage drive
		- /dev/sdb --> the second storage drive
		- /dev/sda3 --> the third partition on the first storage drive
- Pseudo-Devices
	- /dev/zero --> Accepts and discards all input. When read, produces NULL bytes.
	- /dev/null --> Accepts and discards all input written to it, and produces no output when read
	- /dev/random --> produces a stream of random numbers generated from environmental noise
- Legacy PATA devices
	- Parallel ATA (PATA) --> uses the hd prefix but otherwise similar to SCSI

sysfs 
- what is sysfs?
	- proposed as a better way to manage devices on a Linux system
	- virtual filesystem --> typically mounted at /sys --> exporting info about kernel objects, hardware devices, and drivers from the kernel's device model to user space
- The role of the Linux /sys directory
	- to provide a structured view of all the devices on your system
	- /sys is for viewing information || /dev is for directly interacting with devices
- sysfs vs. /dev
	- /dev 
		- provides devices nodes --> allow programs to access the device themselves
	- /sys
		- view information about and manage devices

udev
- Before you would need to create a node to connect a device with `

```bash
mknod /dev/sdb1 b 8 3
```
- **udev automatically creates and destroys nodes now**
- udevd --> daemon that is running on the system, and it **listens for messages from the kernel** about devices connected to the system.
- `udevadm info --query=all --name=/dev/sda`

lsusb, lspci, lssci
- lsusb --> scan USB hubs 
- lspci --> scans for internal components connected to the motherboard 
- lssci --> shows **direct** information about the storage devices themselves (SCSI & SATA)

dd
- powerful tool for reading and copying data

```bash
# copies data byte by byte
dd if=/home/pete/backup.img of=/dev/sdb bs=1024

# copies contents of backup.img to the block device /dev/sdb
# does this in blocks of 1024 bytes
```

- essential dd options
	- if=file --> specifies input file
	- of=file --> " " output file
	- bs=bytes --> sets block size. ex. bs=1M or bs 1k or bs 1G
	- count=number --> copies only this # of blocks
- using **bs** and **count** together
	- **count** is useful if you need a certain amount of data
	- ttl data copied will be **bs** multiplied by **count**
- the *power* and *danger* of **dd**
	- You can use it to create backups of entire disk drives, restore disk images, and securely wipe data.
	- ***swapping if and of can result in data loss***
	- 

