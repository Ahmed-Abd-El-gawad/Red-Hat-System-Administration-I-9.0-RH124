# Chapter 3: Manage files from the Command Line

| content |
| --- |
| [3.3 Specify Files by name](#3.3) |

<a name="3.3"></a>
## 3.3 specify Files by name

| Commands |
| --- | 
| [pwd](#pwd) |
| [ls](#ls) |
| [cd](#cd) |
| [touch](#touch) |

<a name="pwd"></a>
Display the full path name of the current working directory.
```console
[user@host ~]$ pwd
/home/user
```

<a name="ls"></a>
Lists directory contents for the specified directory or, if no directory is given, for the current working directory.
```console
[user@host ~]$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```
```ls``` command has multiple options for displaying attributes on files.
1. ```-l``` (long listing format)
```console
[user@host ~]$ ls -l
total 0
drwxr-xr-x. 2 user user 6 Mar  2 02:45 Desktop
drwxr-xr-x. 2 user user 6 Mar  2 02:45 Documents
drwxr-xr-x. 2 user user 6 Mar  2 02:45 Downloads
drwxr-xr-x. 2 user user 6 Mar  2 02:45 Music
drwxr-xr-x. 2 user user 6 Mar  2 02:45 Pictures
drwxr-xr-x. 2 user user 6 Mar  2 02:45 Public
drwxr-xr-x. 2 user user 6 Mar  2 02:45 Templates
drwxr-xr-x. 2 user user 6 Mar  2 02:45 Videos
```
2. ```-a``` (all files, including hidden files)
* One dot ```.``` refers to the current directory.
* Two dots ```..``` refer to the parent directory.
* File names that begin with a dot ```.``` indicate hidden files.
```console
[user@host ~]$ ls -la
total 40
drwx------. 17 user user 4096 Mar  2 03:07 .
drwxr-xr-x.  4 root root   35 Feb 10 10:48 ..
drwxr-xr-x.  4 user user   27 Mar  2 03:01 .ansible
-rw-------.  1 user user  444 Mar  2 04:32 .bash_history
-rw-r--r--.  1 user user   18 Aug  9  2021 .bash_logout
-rw-r--r--.  1 user user  141 Aug  9  2021 .bash_profile
-rw-r--r--.  1 user user  492 Aug  9  2021 .bashrc
drwxr-xr-x.  9 user user 4096 Mar  2 02:45 .cache
drwxr-xr-x.  9 user user 4096 Mar  2 04:32 .config
drwxr-xr-x.  2 user user    6 Mar  2 02:45 Desktop
drwxr-xr-x.  2 user user    6 Mar  2 02:45 Documents
...output omitted...
```
3. ```-R``` (recursive, to include the contents of all subdirectories)

<a name="cd"></a>
Change your shell's current working directory. 
```console
[user@host ~]$ pwd
/home/user
[user@host ~]$ cd Videos
[user@host Videos]$ pwd
/home/user/Videos
[user@host Videos]$ cd /home/user/Documents
[user@host Documents]$ pwd
/home/user/Documents
[user@host Documents]$ cd
[user@host ~]$ pwd
/home/user
```
The ```cd``` command has many options.
1. ```-``` changes to the previous directory.
```console
[user@host ~]$ cd Videos
[user@host Videos]$ pwd
/home/user/Videos
[user@host Videos]$ cd /home/user/Documents
[user@host Documents]$ pwd
/home/user/Documents
[user@host Documents]$ cd -
[user@host Videos]$ pwd
/home/user/Videos
[user@host Videos]$ cd -
[user@host Documents]$ pwd
/home/user/Documents
[user@host Documents]$ cd -
[user@host Videos]$ pwd
/home/user/Videos
[user@host Videos]$ cd
[user@host ~]$
```
2. ```..``` command uses the ```..``` hidden directory to move up one level to the parent directory, without needing to know the exact parent name.
3. The other hidden directory ```.``` specifies the current directory on commands where the current location is either the source or destination argument, and avoids the need to type the directory's absolute path name.
```console
[user@host Videos]$ pwd
/home/user/Videos
[user@host Videos]$ cd .
[user@host Videos]$ pwd
/home/user/Videos
[user@host Videos]$ cd ..
[user@host ~]$ pwd
/home/user
[user@host ~]$ cd ..
[user@host home]$ pwd
/home
[user@host home]$ cd ..
[user@host /]$ pwd
/
[user@host /]$ cd
[user@host ~]$ pwd
/home/user
[user@host ~]$
```

<a name="touch"></a>
Updates the time stamp of a file to the current date and time without otherwise modifying it. This command is useful for creating empty files.
```console
[user@host ~]$ touch Videos/blockbuster1.ogg
[user@host ~]$ touch Videos/blockbuster2.ogg
[user@host ~]$ touch Documents/thesis_chapter1.odf
[user@host ~]$ touch Documents/thesis_chapter2.odf
```

