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

# mount and umount

# /etc/fstab

# swap

# Disk Usage

# Filesystem Repair

# Inodes

# symlinks