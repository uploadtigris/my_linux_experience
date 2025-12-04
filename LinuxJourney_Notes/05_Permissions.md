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

Setgid

Process Permissions

The Stick Bit