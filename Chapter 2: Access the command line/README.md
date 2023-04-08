# Chapter 2: Access the command line
## Basic Command Syntax

Which user is running 
```console
[user@host ~]$ whoami
user
```

Typing more than one command in a single line
```console
[user@host ~]$ command1 ; command2
command1 output
command2 output
```

Get the current time
```console
[user@host ~]$ date
Sun Feb 27 08:32:42 PM EST 2022
[user@host ~]$ date +%R
20:33
[user@host ~]$ date +%x
02/27/2022
```

Change current user password
```console
[user@host ~]$ passwd
Changing password for user user.
Current password: old_password
New password: new_password
Retype new password: new_password
passwd: all authentication tokens updated successfully.
```
