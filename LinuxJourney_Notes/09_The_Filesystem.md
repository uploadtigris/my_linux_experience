# Filesystem Hierarchy

**The Root Directory**

/ --> Every single file and directory on your system is located under this directory.

**Essential System Directories**

**/bin** --> contains essential CLI programs (ls, cp, mv)
**/sbin** --> contains essential system binaries, intended for sys admin, can only be run by root user
**/etc** --> core system config directory. contains config files for OS and installed apps, should NOT contains executable binaries.
**/lib** --> contains essential shared library files that system binaries in **/bin** and **/sbin** depend on to function correctly
**/boot** --> stores files required for the system's boot process, including the Linux kernel and the boot loader files.

**User and Application Data**

**/home** --> where personal documents, application settings, and other personal files are 
**/root** --> separate home directory for root user even is /home is unavailable
**/opt** --> reserved for 3rd party application software packages
**/usr** --> does NOT hold user's home files. 
	/usr/bin --> non-essential user binaries
	 /user/local --> software compiled from source

**Dynamic and Temporary Data**

**/var** --> files that are exp. to change in size and content, such as sys logs /var/log, caches, and spool files
**/tmp** --> world-writable space for storing temp files. (often deleted on reboot)
**/run** --> contains info about the running system since last boot, such as process IDs (PIDs)

**Device and Mount Points**

**/dev** --> special device files: HDDs, terminals, and input devices
**/media** --> mount point for removable media
**/mnt** --> generic mount point for temp mounting filesystems

**System Information**

**/proc** --> virtual filesystem for real-time info about currently running processes and kernel parameters
**/srv** --> site-specific data
# Filesystem Types

**The Role of the Virtual File System**
- The VFS is an abstraction layer in the Linux kernel that sits between applications and the various filesystems.

**Journaling for Data Integrity**
- "journals" the intended changes in a special log file.
- Once the operation is successfully completed, the journal is updated to mark the task as finished.

**Common Linux File System Types**
- ext4
	- default for many distributions
	- b/w compatible with ext2 and ext3
	- supports very large disk volumes (up to 1 exabyte) & file size (up to 16 terabytes)
	- std choice for most use cases
- Btrfs
	- modern
	- built-in snapshots, incremental backups, and improved performance
	- still under active development (although considered stable)
- XFS
	- high-performance journaling filesystem that excels at handling large files and parallel I/O operations
	- Great for systems managing large amounts of data
- NTFS and FAT
	- windows filesystem types
	- Linux provides full support for reading/writing
- HFS+
	- primary filesystem of macOS
	- Linux has read-only support by default
	- additional tools needed for write support

```bash
# view the filesystems in use on machine
df -T
```
# Anatomy of a Disk

**The Partition Table**
- The component of the disk that tells the OS how the disk is partitioned.
- specifies where each partition begins and ends, which partitions are bootable, and what sectors of the disk are allocated to each partition
- Two partition schemes
	- Master Boot Record (MBR)
		- traditional
		- supports primary, extended, and logical partitions
		- has a limit of 4 primary partitions
	- GUID Partition Table (GPT)
		- one type of partition, you can create a large number of them
		- each partition is assigned a Globally Unique Identifier (GUID)
		- GPT is commonly used with UEFI-based booting systems

**Filesystem Structure**
- Boot block
	- the "block" contains information used to boot the operating system. 
	- One boot block per OS
	- Other partitions may have unused boot blocks
- Superblock
	- single block following the boot block
	- contains data about filesystems (like inode table, size of logical blocks, and ttl size of the filesystem)
- Inode table
	- DB that manages files and directories
	- ea. file or directory has a unique entry in the **inode table** (covered later in it's own lesson)
- Data block
	- Where the content of files and directories is stored
# Disk Partitioning

tools
- fdisk --> a basic command-line partitioning tool; d/n support GPT
- parted --> CMD tool supports MBR and GPT
- gparted --> graphical version of parted. 
- gdisk --> similar to fdisk, only supports GPT

Listing Existing Partitions
```bash
#lists the partiiton tables for all connected block devices
sudo parted -l

# helps you find the correct name
```

Launching Interactive Mode
```bash
sudo parted
```

Selecting a Device
```bash
select /dev/sdb
```

Viewing the Partition Table
```bash
(parted) print 

Model: ATA VBOX HARDDISK (scsi) 
Disk /dev/sdb: 10.7GB Sector size (logical/physical): 512B/512B 
Partition Table: msdos 
Disk Flags: 

Number Start End Size Type File system Flags 
1 1049kB 10.7GB 10.7GB primary ext4 boot
```

Creating a Partition
```bash
# mkpart command creates a new partition
mkpart primary ext4 1MB 5000MB
```

Resizing a Partition
```bash
# need the partition number and the new end point
resizepart 1 8000MB
```
# Creating Filesystems

- AFTER partitioning a disk --> next step is creating a filesystem
	- often called **formatting**, this process organizes the partition so it can store files and directories.

**the mkfs commands**

```bash
sudo mkfs -t ext4 /dev/sdb2

# sudo --> executes command w/ admin privileges (req. for disk management)
# mkfs --> command to create a file system
#-t ext4 --> -t flag specifies filesystem type
# /dev/sdb2 --> target partition where fs will be created
```

**a word of caution**
- ***You should only create a filesystem on a newly created partition or on a disk you intend to {completely erase.}***
- It will ***permanently delete all existing data, and you will likely corrupt the filesystem*** if you attempt to create a new one on top of an existing one without proper preparation.
# mount and umount

**how to mount a filesystem**

```bash
#1 Make a mount point
sudo mkdir /mydrive
```

```bash
#2 Attached your device
sudo mount -t ext4 /dev/sdb2 /mydrive
```

**how to unmount a filesystem in Linux**

```bash
# best practice is to use sudo umount. to detached device & cleanly remove
sudo umount /dev/sdb2

# You CANNOT umount a device if it is currently in use
```

**using UUIDs for Stable Mounting**

- The kernel names the devices **in the order it discovers them**
- This means the names can change between reboots
- To avoid issues, use the universally unique ID (UUID)

```bash
# view UUIDs for block devices
sudo blkid
```

```bash
# mount device
sudo mount UUID=130b882f-7d79-436d-a096-1e594c92bb76 /mydrive
```

# /etc/fstab

When you want to automatically mount filesystems at startup, you configure them in a special configuration file located at `/etc/fstab`

**The fstab file structure**
- **Device Identifier**
	- specifies the device to mount. modern systems use UUID
- **Mount Point**
	- directory in filesystem where device will be mounted
- **Filesystem Type**
	- self explanatory
- **Options**
	- consult mount manpage
- **Dump**
	- the field used by the dump utility to determine if a filesystem needs to be backed up
- **Pass**
	- used by fsck to determine order for checking filesystems at boot time
		- / --> 1
		- others --> 2
		- not checked --> 0

**how to edit**
- use nano with root to edit `/etc/fstab`
- **Be extremely careful when editing this file; an incorrect entry in the fstab can prevent your system from booting correctly.**
- ALWAYS BACKUP THE FILE
- test --> `sudo mount -a`
	- which mounts all filesystems listed in `/etc/fstab`
# swap

```bash
mkswap /dev/sdb2 --> initialize swap areas

swapon /dev/sdb2 --> will enable the swap device

to have swap partition persist --> add entry to /etc/fstab (using sw filesystem)

remove swap: swapoff /dev/sdb2
```

generally, you should allocate 2x of swap space as you have memory.
# Disk Usage

Checking Filesystem Space with df (disk free)
```bash
df -h
```

Analyzing Inode Usage
```bash
# show the inodes --> storing metadata about files
df -i
```

Summarizing Directory Usage with du
```bash
# shows the disk usage for each subdirectory in your current location
du -h
```

df --> check how much disk is free
du --> check the disk usage of specific files and directories
# Filesystem Repair

fsck (filesystem check)
- used to check consistency of filesystem and can even try to repair it for us
- when you boot, fsck will run before disk is mounted
- make sure to do this from somewhere you can access your filesystem w/o it being mounted

```bash
sudo fsck /dev/sda
```
# Inodes

**what is it?**

It is a database that holds data on
- file type
- owner
- group
- access permissions
- timestamps (of diff sorts)
- number of hard links to the file
- size of the file
- number of blocks allocated to the file
- points to the file's data blocks (most important!)

**Inode creation and allocation**

- When a filesystem is created, space for inodes is also allocated

```bash
# see how many inodes are left on your system
df -i
```

**viewing inode information**

```bash
# the left most number is the "inode ID"
ls -li
```

```bash
# view detailed info about a file's "i node"
stat ~/Desktop
```

**how an I-node points to data**

- Inodes point to the actual data blocks of your files.
- typical inodes have 15 pointers
	- first 12 --> data blocks
	- 13 --> block with more pointers
	- 14 & 15 --> blocks of nested pointers
- small files can be accessed quickly using the direct pointers
- larger files are located through the nested pointers
# symlinks

**The Link Count**

- The total number of **hard links** the file has

**Understanding Symlinks**

- Windows == Shortcuts
- Linux == Symbolic Link / Symlink

**Understanding Hard Links**

-  Another entry that points directly to the same inode as the original file

**Creating Symlinks and Hard Links**

```bash
# symbolic link
ln -s /path/to/original /path/to/link
```

```bash
# hard link
ln /path/to/original /path/to/link
```



