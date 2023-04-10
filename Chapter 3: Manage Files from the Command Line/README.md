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

