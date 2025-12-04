Users and Groups
root
- risk of a persistent root shell
- staying logged into root shell has risk of making a critical error.
- actions made in the root shell are not logged under your personal user account
- for these reasons, using ```sudo``` is the preferred method for admin access
 ```
  # This file lists the users and groups who are granted permission to run commands a the superuser
  /etc/sudoers
  ```



/etc/passwd
/etc/shadow
/etc/group
User Management Tools