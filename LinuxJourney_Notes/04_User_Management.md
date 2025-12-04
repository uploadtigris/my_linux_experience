
root
- risk of a persistent root shell
- staying logged into root shell has risk of making a critical error.
- actions made in the root shell are not logged under your personal user account
- for these reasons, using ```sudo``` is the preferred method for admin access
 ```
  # This file lists the users and groups who are granted permission to run commands a the superuser
  /etc/sudoers
  ```

- To edit the sudoers file --> use the visudo command

/etc/passwd
- this is where the mapping b/w usernames and UIDs is stored
- to view it's contents ``` cat /etc/passwd/```
	- this file lists all the system users, & detailed info about them, each line is a single user account
- there are SEVEN main fields of each entry
	- **Username**: login name
	- **Password**: placeholder for the user's encrypted password
		- x --> encrypted password is in ```/etc/shadow```
		- * --> account is locked and cannot be used for login
		-    --> blank field means the user has no password
	- **User ID (UID)**: unique numerical id for the user. Root always has 0
	- **Group ID (GID):** numerical ID for user's primary group
	- **GECOS Field**: comment field that traditionally holds extra information like the user's full name, phone number, or office location --> comma-delimited
	- **Home Directory**: the absolute path to the user's home directory
	- **Default Shell:** user's default command-line interpreter, executed upon login
- Notice that many of the users are just service running with restricted permissions for security purposes
- **Editing**
	- you CAN edit the passwd file directly w/ the "vipw" command but this is STRONGLY discouraged
	- It is always safer to use the following commands
		- useradd
		- usermod
		- userdel

/etc/shadow
- critical component in Linux systems for storing sensitive user authentication information. 
- requires superuser privileges

```bash
# view the file
sudo cat /etc/shadow
```

```bash
# example output
root:MyEPTEa$6Nonsense:15000:0:99999:7:::
```

- The fields from left to right
	- username
		- duh
	- encrypted password
		- hashed psw: * or ! means the account is locked
	- date of last password change
		- The number of days since January 1,1970 that the password was last changed. ***A value of 0 forces a password change at the next login***.
	- minimum password age
		- the minimum number of days that must pass before the user can change their password again
	- maximum password age
		- the maximum # of days the password is valid. After this period, the user must change it
	- password warning period
		- the # of days before pswd expiration that user will get a message prompting them to change their password
	- password inactivity period
		- the # of days after a password expires 
	- account expiration date
		- days since January 1, 1970, when the user account will be disabled
	- reserved field
		- The field is reserved for future use
- while this file is fundamental, most modern distributions supplement it with other authentication mechanisms, such as **Pluggable Authentications Modules (PAM)**
	- PAM is about authentication
	- we do not edit the configuration files directly, other applications do the modifications for us
	- timestamps for logging in/out logged to the "wtmp" file

/etc/group
- defines the groups on the system and their members

```bash
sudo /etc/group
```

```bash
#example output
root:*:0:pete
```

- fields from left to right
	- group name
		- the unique name of the group
	- group password
		- legacy feature, rarely used
		- modern tools use sudo and other to elevate privileges instead of group passwords
		- typically will see a placeholder like * or x
	- group ID (GID)
		- numerical identifier for the group
		- system often uses the GID internally instead of the group name
	- List of Users
		- command separated list of users in this group

User Management Tools
- adding users
	- **sudo useradd bob**
	- "adduser" script also exsits
- deleting users
	- **sudo userdel bob**
	- may not remove the users home directory -> "-r flag is needed to do this"
	- **sudo userdel -r bob**
- Changing passwords
	- **passwd user1**