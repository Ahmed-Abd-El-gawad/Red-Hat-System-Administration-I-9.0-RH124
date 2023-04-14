# Chapter 7. Control Access to Files

| content |
| --- |
| [7.1 Interpret Linux File System Permissions](#7.1) |
| [7.3 Manage File System Permissions from the Command Line](#7.3) |
| [7.5 Manage Default Permissions and File Access](#7.5) |


<a name="7.1"></a>
## 7.1 Interpret Linux File System Permissions

* Effects of Permissions on Files and Directories
  | Permission | Effect on files | Effect on directories |
  | --- | --- | --- |
  | ```r``` (read) | File contents can be read. | Contents of the directory (the file names) can be listed. |
  | ```w``` (write) | File contents can be changed. | Any file in the directory can be created or deleted. |
  | ```x``` (execute) | Files can be executed as commands. | The directory can become the current working directory. You can run the cd command to it, but it also requires read permission to list files there. |
  * The ```ls``` command ```-l``` option shows detailed information about permissions and ownership:
    ```console
    [user@host ~]$ ls -l test
    -rw-rw-r--. 1 student student 0 Mar  8 17:36 test
    ```
  * Use the ```ls``` command ```-d``` option to show detailed information about a directory itself, and not its contents.
    ```console
    [user@host ~]$ ls -ld /home
    drwxr-xr-x. 5 root root 4096 Feb 31 22:00 /home
    ```
    * The first character of the long listing is the file type, and is interpreted as follows:
      * ```-``` is a regular file.
      * ```d``` is a directory.
      * ```l``` is a symbolic link.
      * ```c``` is a character device file.
      * ```b``` is a block device file.
      * ```p``` is a named pipe file.
      * ```s``` is a local socket file.
    * The next nine characters represent the file permissions. These characters are interpreted as three sets of three characters: 
        * The first set are permissions that apply to the file owner. 
        * The second set are for the file's group owner.
        * The last set applies to all other (world) users. 
        * If a set is an rwx string, then that set has all three permissions: read, write, and execute. If a letter is replaced by a ```-``` dash character, then that set does not have that permission.
    * After the second column (the link count)
      * The first name specifies the file owner
      * The second name is the file's group owner.

<a name="7.3"></a>
## 7.3 Manage File System Permissions from the Command Line

| Commands |
| --- |
| [chmod](#chmod) |
| [chown](#chown) |

<a name="chmod"></a>
* ```chmod Who/What/Which file|directory```
  | Who | Set | Description |
  | --- | --- | --- |
  | ```u``` | user | The file owner. |
  | ```g``` | group | Member of the file's group. |
  | ```o``` | other | Users who are not the file owner nor members of the file's group. |
  | ```a``` | all | All the three previous groups. |

  | What | Operation | Description |
  | --- | --- | --- |
  | ```+``` | add | Adds the permissions to the file. |
  | ```-``` | remove | Removes the permissions to the file. |
  | ```=``` | set exactly | Set exactly the provided permissions to the file. |

  | Which | Mode | Description |
  | --- | --- | --- |
  | ```r``` | read | Read access to the file. Listing access to the directory. |
  | ```w``` | write | Write permissions to the file or directory. |
  | ```x``` | execute | Execute permissions to the file. Allows to enter the directory, and access files and subdirectories inside the directory. |
  | ```X``` | special execute | Execute permissions for a directory, or execute permissions to a file if it has at least one of the execute bits set. |

* You can use the ```chmod``` command ```-R``` option to recursively set permissions on the files in an entire directory tree. For example, the next command recursively adds read, write, and execute permissions for the members of the ```myfolder``` directory and the files and directories inside it.
  ```bash
  [user@host ~]$ chmod -R g+rwx /home/user/myfolder
  ```
  
* The following command recursively sets read and write access on the ```demodir``` directory and all its children for their group owner, but only applies group execute permissions to directories and files that already have execute set for user, group, or other.
  ```bash
  [root@host opt]# chmod -R g+rwX demodir
  ```
  
* Octal method
  ```bash
  chmod ### file|directory
  ```
  ![user-perms](https://github.com/Ahmed-Abd-El-gawad/Red-Hat-System-Administration-I-9.0-RH124/blob/main/Chapter%207.%20Control%20Access%20to%20Files/user-perms.svg)
  
<a name="chown"></a>
* Only the ```root``` user can change the user that owns a file.
* grant ownership of the ```app.conf``` file to the student user, use the following command:
  ```console
  [root@host ~]# chown student app.conf
  ```
* The chown command ```-R``` option recursively changes the ownership of an entire directory tree.
  ```console
  [root@host ~]# chown -R student Pictures
  ```
* You can also use the ```chown``` command to change group ownership of a file by preceding the group name with a colon (```:```).
  ```console
  [root@host ~]# chown :admins Pictures
  ```
* You can use the ```chown``` command to change both owner and group at the same time by using the ```owner:group``` syntax.
  ```console
  [root@host ~]# chown visitor:guests Pictures
  ```
* Instead of using the ```chown``` command, some users change the group ownership by using the ```chgrp``` command. This command works similarly to ```chown```, except that you can use it only to change group ownership and the colon (```:```) before the group name is not required.
* You might encounter alternative chown syntax that separates owner and group with a period character instead of a colon:
  ```console
  [root@host ~]# chown owner.group filename
  ```

<a name="7.5"></a>
## 7.5 Manage Default Permissions and File Access

| Contents|
| --- |
| [Effects of Special Permissions on Files and Directories](#special_permissions) |

<a name="special_permissions"></a>
* Effects of Special Permissions on Files and Directories
  
  | Permission|  Effect on files | Effect on directories |
  | --- | --- | --- |
  | ```u+s``` (suid) | File executes as the user that owns the file, not as the user that ran the file. | No effect. |
  | ```g+s``` (sgid) | File executes as the group that owns the file. | Files that are created in the directory have a group owner to match the group owner of the directory. |
  | ```o+t``` (sticky) | No effect. | Users with write access to the directory can remove only files that they own; they cannot remove or force saves to files that other users own. |

* The ```setuid``` permission on an executable file means that commands run as the user that owns that file, rather than as the user that ran the command. One example is the passwd command:
  ```console
  [user@host ~]$ ls -l /usr/bin/passwd
  -rwsr-xr-x. 1 root root 35504 Jul 16  2010 /usr/bin/passwd
  ```
* In a long listing, you can identify the setuid permissions by a lowercase ```s``` character in the place where you would normally expect the ```x``` character (owner execute permissions). If the owner does not have execute permissions, then this character is replaced by an uppercase ```S``` character.

* The ```setgid``` special permission on a directory means that created files in the directory inherit their group ownership from the directory, rather than inheriting group ownership from the creating user. This feature is commonly used on group collaborative directories to automatically change a file from the default private group to the shared group, or if a specific group should always own files in a directory. An example of this behavior is the ```/run/log/journal``` directory:
  ```console
  [user@host ~]$ ls -ld /run/log/journal
  drwxr-sr-x. 3 root systemd-journal 60 May 18 09:15 /run/log/journal
  ```

