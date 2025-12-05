File Permissions
- ***ls -l Desktop/*** --> shows the read/write permissions of a file
- Decoding the permissions string

```bash
# example output
drwxr-xr-x

# separate the string into these sections 
d | rwx | r-x | r-x
```

- r --> read permission
- w --> write permission
- x --> execute permission
- - --> no permission granted

- The meaning of these will differ slightly if the subject is a Directory or a File
	- Example
		- if file --> x --> executable
		- if dir --> x --> allows you to enter it

- The four sections
	- first, lists the type of object dir/file
	- Second set: applies to the User (Owner) | ex. rwx --> read/write/execute 
	- Third set: applies to the group associated with the file/dir
	- Fourth set: applies to all OTHER users on the system

Modifying Permissions
- When you need to ***modify file or directory access rights***, the primary tool you'll use is the **chmod**
- using symbolic mode
	- u --> user/owner
	- g --> group
	- o --> others
	- a --> all: user, groups, and others
	- example:
		- chmod u+x myfile
			- adds executable permission for the user "u" on "myfile"
			- removing the permission would just replace + with -
		- chmod ug+x myfile
			- modify multiple users at once
- user numerical (Octal) mode
	- allows you to set all permissions for the user, group, and other simultaneously with a three-digit number
		- 4: read (r)
		- 2: write (w)
		- 1: execute (x)
			- add them up

```bash
# Octal mode example
chmod 755 myfile

# in this example
7 (user) --> 4 + 2 + 1 --> user gets read, write, and execute permissions
5 (Group) --> 4 + 0 + 1 --> The group gets read and execute perms only
5 (Others) --> 4 + 0 + 1 --> all other users get read + execute permissions
```

- **Always apply the principle of least privilege, granting only the permissions that are strictly necessary.**

Ownership Permissions
- changing user ownership
	- chown --> transfer the ownership of a file to a different user

```bash
# change ownership of myfile to the user patty
sudo chown patty myfile
```

- changing group ownership
	- chgrp --> change the group associated with a file

```bash
# allow all members of the new group to have access based on the group's linux permissions
sudo chgrp whales myfile
```

- changing both user and group
	- chown --> By separating the user and group name with a color, you can update both attributes simultaneously
	- this is the most common method for managing Linux file ownership

```bash
# assigns user ownership to patty and group ownership to whales for the file myfile
sudo chown patty:whales myfile
```

Umask
- every file comes with a default set of permissions, which can be changed by "default"
- umask uses the 3-bit permission set we see in numerical permissions. Instead of adding, **we take away these permissions**

```bash
# users --> access to everything
# groups --> take away their write permissions (2)
# others --> take away their executable permission (1)
umask 021
```

- default unmask on most distros is 002 --> full user access, no write access for groups and other others
- If you want the new default to persist, you'll have to modify your startup file (.profile)

Setuid
- Used when a normal user needs elevated access to do stuff. The ***Set User ID (SUID)*** allows a user to run a program as the owner of the program file rather than as themselves.

```bash
# command
ls -l /usr/bin/passwd

#output
-rwsr-xr-x 1 root root 47032 Dec 1 11:45 /usr/bin/passwd
```

- Notice the "s" in the user's section of the permissions
	- "***s" is the SUID*** --> allows the users who launched the program to get the file owner's permission as well as execution permission, in this case, root

- Modifying SUID
	- symbolic way
```bash
sudo chmod u+s myfile

# S --> this means that it still does the same thing as 's' but it does not have execute permissions.
```
	- numerical way
```bash
sudo chmod 4755 myfile
```
		 
Setgid
- allows a program to run as if it were a member of that group

```bash
# output
-rwxr-sr-x 1 root tty 19024 Dec 14 11:45 /usr/bin/wall

# s is now in the permission bit in the group permission set
```

```bash
#modifying SGID
sudo chmod g+s myfile
sudo chmod 2555 myfile
```

Process Permissions
- Even though adding s allows you to run the program as root, you cannot do other things as root. 
- There are three UIDs (User ID) associated with every process
	- **effective user ID**
		- ID the kernel uses to determine access rights
		- "Whose permissions am I acting with right now?"
	- **real user ID**
		- ID of the user that launched the process
		- "'Who launched me?"
	- **saved user ID**
		- Allows a process to switch between the effective UID and real UID
		- Store previous effective UID so the process can switch back
		- "My backup identity so I can switch back later"

The Sticky Bit
- A configuration option that can be applied to directories / files
- When set, that directory can only be deleted or renamed by the file's owner, the directory's owner, or the root user.
- Useful for shared directories where multiple users need to create and manage their own files w/o interfering with others.
```bash
# the global /tmp folder is a great example of this
ls -ld /tmp

# notice the 't' at the end --> this is the "stick bit"
drwxrwxrwt 17 root root 4096 Dec 15 11:45 /tmp
```

Setting up the sticky bit (Symbolic Mode)
```bash
chmod +t my_shared_dir
```

Setting up the sticky bit (Symbolic Mode)
```bash
chmod 1755 my_shared_dir
```

