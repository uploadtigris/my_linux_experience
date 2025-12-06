ps (Processes)
- The programs running on your machine. 
- The Linux Kernel manages them.
- Each process is assigned a unique number called the process ID (PID) --> PIDs are typically assigned sequentially

```bash
ps

# output
PID        TTY     STAT   TIME          CMD
41230    pts/4    Ss        00:00:00     bash
51224    pts/4    R+        00:00:00     ps
```

PID: The unique Process ID
TTY: The controlling terminal for the process
STAT: The current status of the process
TIME: The total CPU time the process has used
CMD: The command that started the process

```bash
# BSD-Style Options
ps aux 
# a --> displays all processes for all users
# u --> Provided a detailed, user-oriented format
# x --> includes processes not attached to any terminal
```

```bash
# System V style (**many users prefer this over ps aux)
ps -ef
# -e --> selects every process on the system
# -f --> displays a "full-format" listing, including details like UID, PPID (parent process ID), C (CPU utilizaiton), and STIME (start time)
```

- real time monitoring with the top command

Controlling Terminal

```bash
ps
# output
   PID TTY          TIME CMD  
 63166 pts/2    00:00:00 fish  
 63505 pts/2    00:00:00 ps
```

- TTY --> indicates the **controlling terminal** that executed the command
	- comes from "Teletype", historical device for interacting w/ computer
	- Terminal Devices vs. Pseudo-Terminals
		- Terminal Devices
			- enter TTY --> Ctrl Alt F1
			- return to graphical session --> Ctrl Alt F7
		- Pseudo-Terminals (PTS)
			- emulate a terminal within a window
			- If you look at `ps tty` output it will be lsited as pts/*
- The role of controlling terminal
	- **most processes are bound to the terminal session that started it**
	- ex. if you run "find" in the terminal windows and then close the window, the find process will also be terminated.
- processes w/o a controlling terminal
	- **daemons** --> designed to run in the background and manage system services
		- start when the system boots and stop only when it shuts down
		- daemons are not attached to a controlling terminal
		- *? in the TTY column shows process d/n have a controlling terminal*

Process Details
- "a process is a program in execution"
- it is an **instance** of a running program to which the computer has allocated resources
- The **kernel** manages the processes, acting as a scheduler

Process Creation
- The Fork and Exec Model
	- *the primary mechanism for process creation in Linux*
	- Fork
		- existing process clones itself, creating it's own Process ID (PID) original becomes a Parent Process (with PPID)
	- Exec
		- execve --> system call used to load and run a new program
- Observing Parent-Child Relationships
	- `ps l` --> shows the long format view about running processes
- The Init Process
	- the common ancestor
	- when the system boots, the kernel creates init as the very first user-space process, assigning it a PID of 1
	- runs with root privileges to manage the system

Process Termination
- The Termination Process
	- `_exit`--> process typically terminates by calling this
		- Although, this doesn't immediately erase the process. 
			- parent processes much acknowledge it's child's termination by using the `wait` status. 
		- Upon exiting, the process provides a termination status to the kernel with is an integer value
			- ***0 --> successful execution***
			- ***non-zero --> error***
- Orphan Processes
	- 

Signals

kill (Terminate)

niceness

Process States

/proc filesystem

Job Control