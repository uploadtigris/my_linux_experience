# Overview

**Stage 1 BIOS**

- BIOS (Basic Input/Output system) runs the Power-On Self-Test (POST) to initialize and verify system hardware.
- Once the hardware is verified, **BIOS's primary duty is to locate & load the bootloader from Storage**.

**Stage 2 Bootloader**

- Takes over from BIOS. Main responsibility is to load the Linux kernel into memory.
	- example bootloader = GRUB (GRand Unified Bootloader)
		- Gives menu for choosing operating system or kernel version to boot.
- After a selection (or timeout) --> loads the kernel and initial RAM disk (initrd) into memory --> then passes control to the kernel.

**Stage 3 Kernel**

- Kernel takes control of the system
- Start decompressing. Initializing core hardware and memory management.
- Kernel mounts the root filesystem, which contains all the system files
- *final & most critical task in the boot process is to execute the first user-space program:* **the init process**

**Stage 4 Init**

- The FIRST process started by the kernel and is the ancestor of all other processes on the system.
	- Its primary job is to bring the system to a usable state by starting essential services and background processes
- example of init --> systemd
# Boot Process: BIOS

**The role of BIOS in Linux**

- Its primary function is to initialize and test the system hardware, such as the CPU, memory, and hard drives.
- The main goal of the **bios Linux** process is to locate and **hand off control to the system bootloader.**

**How BIOS Finds the Bootloader**

- BIOS initializes hardrive
- searches for boot block --> for instructions on how to start OS
	- The location is checks depends on the disk's partitioning scheme:
		- Master Boot Record (MBR)
			- First 512 bytes of the hardrive
			- Contains initial boot code and the partition table
		- GUID Partition Table (GPT)

**The rise of UEFI** (Unified Extensible Firmware Interface)

- stores all startup information in an `.efi` file located on a dedicated EFI System Partition (ESP) --> this partition contains the bootloader for the installed operating system.
- benefits over BIOS
	- faster boot times & support for larger hard drives
# Boot Process: Bootloader

**What is a Bootloader**

- A **bootloader in Linux** is a small program that loads the operating system's kernel into memory and then executes it.
- bridge b/w system's firmware and the Linux kernel

**The role of the Linux Boot Loader**

- **Operating System Selection**: It can present a menu to boot into various operating systems, including non-Linux systems, if you have a multi-boot setup.
- **Kernel Selection**: It allows you to choose which version of the Linux kernel to load, which is useful for troubleshooting or testing.
- **Passing Kernel Parameters**: It specifies crucial parameters that the kernel needs to start correctly.

- (other alternatives exists like LILO, SYSLINUX, and Coreboot exist)

**Common Kernel *Parameters* in GRUB**

- initrd --> specifies the location of the initial RAM disk --> temporary root filesystem loaded into memory
- BOOT_IMAGE --> defines the path to the kernel image file that should be loaded
- root --> **points to the location of the actual root filesystem.** The kernel uses this path to find the init process. represented by a device name
- ro --> std param that instructs kernel to mount the root filesystem in **read-only mode** initially
- quiet --> This parameter suppresses most of the detailed boot messages, providing a cleaner, less verbose startup screen.
- splash --> Enables a **graphical splash screen to be displayed** during the boot process instead of text messages.
# Boot Process: Kernel

**Kernel Initialization and the Initramfs**

- bootloader loads kernel into memory --> kernel takes control of the system
- challenge == during boot-up kernel needs drivers that are on a storage device, which the kernel cannot access. 
	- to solve this, Linux uses a temporary root filesystem.
	- The `initramfs` contains just the essential modules the kernel needs to access the actual `boot partition` and other hardware.

**Mounting the Boot Root Filesystem**

- With drivers loaded from `initramfs`, key now locates and mounts the main `boot root` filesytem (whose location is passed as a parameter by the bootloader).
- First, kernel mounts `boot root` partition in read-only mode. 
	- This is a safety measure that allows the `fsck` (file system check) utility to run and verify the integrity of the filesystem w/o risking data corruption.
	- AFTER completes successfully, the kernel remounts the filesystem in read-write mode.
- With the root filesystem fully available, kernel starts the very first user-space program: `init`.
	- `init` --> responsible for bringing the rest of the system online.
# Boot Process: Init

**System V Init**

- traditional init system for Linux
- follows a sequential startup procedure defined by scripts
- system's state is managed through `runlevels`

**Upstart**

- event-based init system
- starts and stops services (called jobs) in response to system events (such as network device becoming available)

**systemd**

- It is a goal-oriented system that manages dependencies aggressively.
- Define a target state & systemd works to satisfy all dependencies.