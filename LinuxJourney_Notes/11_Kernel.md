# Privilege Levels

**Difference between Kernel Mode and User Mode**

- This separation is crucial for protecting the system's hardware and resources from direct, uncontrolled access by applications.
	- Kernel --> complete access
	- User mode --> very limited access to a small, safe portion of memory and CPU resources.
		- cannot directly access devices directly, must have kernel mode handle these operations.

**Protection rings and privileged access**

- two primary levels
	- ring 0
		- where kernel runs
		- highest level of privilege
	- ring 3
		- user-mode applications run here
		- least privileged & no direct access to hardware

**System calls and kernel privileges**

- The bridge between **user mode** and **kernel mode** is the **system call**
- When a user application needs to perform a privileged task, makes a system call to request that kernel performs action
# Systems Calls

**What are system calls?**

- syscalls --> provide a way for user-space processes to request services directly from the kernel.
- Kernel exposes a set of services via the system call API
	- Essential for operations like reading or writing to a file, managing memory, or handling network connections.
- System maintains a `syscall table` where each system call is registered with a unique ID

**The system call mechanism in Linux**

- User inputs `ls` --> 
- Code runs library function (sets up necessary parameters) ; triggers "trap" --> 
- Signals processor to switch to kernel mode & **system call handler takes over** --> uses unique ID to look up ***requested function*** in `syscall table` -->
- After the requested function is executed, context is switch back to user-mode

**Viewing system calls with strace**

```bash
strace ls
```
# Kernel Installation

```bash
# list kernel version
uname -r
```

```bash
# install a different linux kernel version
sudo apt install linux-generic-lts-vivid
```

```bash
# perform upgrade to all packages on system
sudo apt dist-upgrade
```
# Kernel Location

**The /boot directory**

- Contains the kernel
	- Often multiple kernels 

**Key kernel files**

- **vmlinuz**
	- The compressed, executable Linux kernel. "z" == compressed. 
- **intird**
	- The initial Ram disk
	- `initrd` is a temp root filesystem loaded into memory during startup to mount the real root filesystem
- **System.map**
	- holds the Symbol Table
		- maps kernel function names to memory addresses
			- used for debugging kernel panics & other accidents
- **config**
	- details the drivers & features included during the compilation of the kernel

**Managing kernel files**

- use your **package manager** to remove old kernel versions

# Kernel Modules

- car engine == kernel
- roof rack / sound system == kernel modules

- They extend the functionality of the kernel without requiring you to recompile the core kernel or reboot the system

```bash
# list the loaded modules & their dependencies
lsmod
```

```bash
# load a module & dependencies
sudo modprobe bluetooth
```

```bash
# unload a module
sudo modprobe -r bluetooth
```

**Managing Modules**

Making module load at boot by editing config file

```plaintext
# /etc/modprobe.d/peanutbutter.conf

options peanut_butter type=almond
```

Preventing a module from loading at boot (**blacklisting***)

```plaintext
# /etc/modprobe.d/peanutbutter.conf

blacklist peanut_butter
```