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
	- *the 
primary mechanism for process creation in Linux*
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
			- *parent processes much acknowledge it's child's termination by using the `wait` status.* 
		- Upon exiting, the process provides a termination status to the kernel with is an integer value
			- ***0 --> successful execution***
			- ***non-zero --> error***
- Orphan Processes
	- If the parent terminates before its child --> the child becomes and "orphan"
	- The orphan process is immediately adopted by a special system process, typically "init"
		- init assumes the role of parents, periodically calling "wait" to obtain the termination status of its adopted children.
- Zombie Processes
	- When a child process terminates, but its parent has not yet called **wait**
		- kernel releases resources, but it keeps an entry in the process table
	- The zombie is already dead, so they don't consume CPU time.
	- You cannot kill the zombie with signals as they are not running.
	- **The process of the parent calling *wait* to clean up a zombie is called "reaping"** --> if the parent process never calls wait, the zombies can accumulate. 
	- Too many zombie can fill the process table and PREVENT new processes from being create.
	- If the parent process also terminates, **init** will adopt and **reap** the zombie
	
- ***Zombie vs. Orphans***
	- ***Orphan***
		- ***active, running process whose parent has died. It is adopted by init and continues to execute until finished***
	- ***Zombie***
		- ***dead process that has completed execution but still has an entry in the process table. Waiting for parent process to read its exit status***


***In short, an orphan is alive but parent-less, while a zombie is dead but not yet fully reaped by its parent.***

Signals
- purpose
	- user interaction
		- ex: ctrl+C --> interrupt or suspend foreground processes
	- kernel notifications
		- kernel can send signal to a process to notify it of hardware or software issues, such as an illegal memory access (SIGSEGV)
	- process management
		- sysadmins use signals to manage life-cycle of other processes, such as requesting termination

- the signal life-cycle
	- ignore the signal
		- process discard signal, continues execution
	- catch the signal
		- process executes a custom function called "signal handler"
	- perform the default action
		- if not caught or ignored, the default action is taken. (usually means terminating the process)
	- block the signal
		- if the signal is in the process's signal mask, it remains pending until it is unblocked

- Common Linux Process Signals
	- SIGHUP (1): Hangup. often used to tell a daemon to reload its configuration
	- SIGINT (2): Interrupt. set by ctrl+c. a *request* to terminate the process
	- SIGKILL (9): Kill. ***immediate, forceful termination.*** process cannot catch, ignore, or block this signal.
	- SIGSEGV (11): segmentation fault. indicates the process made an invalid memory reference.
	- SIGTERM (15): Termination. This is the polite way to ask a process to terminate. ***the default signal sent by the kill command***. Often referred to as **signal 15 Linux**
	- SIGSTOP:


kill (Terminate)
- default termination with kill sigterm
	- **kill** , despite it's name, can send various signals
		- kill sigterm process_ID --> shutdown cleanly
		- kill -15 process_ID --> equivalent to the command above
- other useful signals
	- SIGHUP
		- kill sighup --> used to tell daemon processes to reload their configuration files
	- SIGINT
		- the interrupt signal 2 sent when you press ctrl + c. requests the process to interrupt its current operation
	- SIGSTOP
		- the signal 19 pauses a process without temrinating it. the process can be resumed later with the SIGCONT signal


Niceness

Process States

/proc file-system

Job Control
























































Signals

kill (Terminate)

niceness

Process States

/proc filesystem

Job Control