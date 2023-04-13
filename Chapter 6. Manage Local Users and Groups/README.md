# Chapter 6. Manage Local Users and Groups

| content |
| --- |
| [6.1 Describe User and Group Concepts](#6.1) |
| [6.3 Gain Superuser Access](#6.3) |
| [6.5 Manage Local User Accounts](#6.5) |
| [6.7 Manage Local Group Accounts](#6.7) |
| [6.9 Manage User Passwords](#6.9) |


<a name="6.1"></a>
## 6.1 Describe User and Group Concepts

| Commands |
| --- |
| [id](#id) |
| [ls -l](#-l) |
| [ls -ld](#-ld) |
| [ps](#ps) |
| [cat /etc/passwd](#/etc/passwd) |
| [cat /etc/group](#/etc/group) |

<a name="id"></a>
* Use the ```id``` command to show information about the currently logged-in user:
```console
[user01@host ~]$ id
uid=1000(user01) gid=1000(user01) groups=1000(user01) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```
* To view basic information about another user, pass the username to the ```id``` command as an argument:
```console
[user01@host ~]$ id user02
uid=1002(user02) gid=1001(user02) groups=1001(user02) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```
<a name="-l"></a>
* Use the ```ls -l``` command to view the owner of a file. 
```console
[user01@host ~]$ ls -l mytextfile.txt
-rw-rw-r--. 1 user01 user01 0 Feb  5 11:10 mytextfile.txt
```
<a name="-ld"></a>
* Use the ```ls -ld``` command to view the owner of a directory, rather than the contents of that directory. In the following output, the third column shows the username.
```console
[user01@host]$ ls -ld Documents
drwxrwxr-x. 2 user01 user01 6 Feb  5 11:10 Documents
```
<a name="ps"></a>
* Use the ```ps``` command to view process information. The default is to show only processes in the current shell. Use the ps command ```-a``` option to view all processes with a terminal. Use the ps command ```-u``` option to view the user that is associated with a process. In the following output, the first column shows the username.
```console
[user01@host ~]$ ps -au
USER     PID %CPU %MEM    VSZ   RSS TTY    STAT START  TIME COMMAND
root    1690  0.0  0.0 220984  1052 ttyS0  Ss+  22:43  0:00 /sbin/agetty -o -p -- \u --keep-baud 1
user01  1769  0.0  0.1 377700  6844 tty2   Ssl+ 22:45  0:00 /usr/libexec/gdm-x-session --register-
user01  1773  1.3  1.3 528948 78356 tty2   Sl+  22:45  0:03 /usr/libexec/Xorg vt2 -displayfd 3 -au
user01  1800  0.0  0.3 521412 19824 tty2   Sl+  22:45  0:00 /usr/libexec/gnome-session-binary
user01  3072  0.0  0.0 224152  5756 pts/1  Ss   22:48  0:00 -bash
user01  3122  0.0  0.0 225556  3652 pts/1  R+   22:49  0:00 ps -au
```
<a name="/etc/passwd"></a>
* Each line in the ```/etc/passwd``` file contains information about one user. The file is divided into seven colon-separated fields.
  ```console
  [user01@host ~]$ cat /etc/passwd
  ...output omitted...
  user01:x:1000:1000:User One:/home/user01:/bin/bash
  ```
  Consider each part of the code block, separated by a colon:
  * ```user01``` : The username for this user.
  * ```x``` : The user's encrypted password was historically stored here; this is now a placeholder.
  * ```1000``` : The UID number for this user account.
  * ```1000``` : The GID number for this user account's primary group. Groups are discussed later in this section.
  * ```User One``` : A brief comment, description, or the real name for this user.
  * ```/home/user01``` : The user's home directory, and the initial working directory when the login shell starts.
  * ```/bin/bash``` : The default shell program for this user that runs at login. Some accounts use the ```/sbin/nologin``` shell to disallow interactive logins with that account.
<a name="/etc/group"></a>
* Each line in the ```/etc/group``` file contains information about one group. Each group entry is divided into four colon-separated fields.
  ```console
  [user01@host ~]$ cat /etc/group
  ...output omitted...
  group01:x:10000:user01,user02,user03
  ```
  Consider each part of the code block, separated by a colon:
  * ```group01``` : Name for this group.
  * ```x``` : Obsolete group password field; this is now a placeholder.
  * ```10000``` : The GID number for this group (```10000```).
  * ```user01,user02,user03``` : A list of users that are members of this group as a secondary group.


<a name="6.3"></a>
## 6.3 Gain Superuser Access



<a name="6.5"></a>
## 6.5 Manage Local User Accounts



<a name="6.7"></a>
## 6.7 Manage Local Group Accounts



<a name="6.9"></a>
## 6.9 Manage User Passwords


