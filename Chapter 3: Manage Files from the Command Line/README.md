# Chapter 3: Manage files from the Command Line

| content |
| --- |
| [3.3 Specify Files by name](#3.3) |
| [3.5 Manage Files with Command-line Tools](#3.5) |
| [3.7 Make Links Between Files](#3.7) |
| [3.9 Match File Names with Shell Expansions](#3.9) |

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
```console
[user@host Documents]$ ls -R Thesis/
Thesis/:
Chapter1  Chapter2  Chapter3

Thesis/Chapter1:

Thesis/Chapter2:

Thesis/Chapter3:
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

<a name="3.5"></a>
## 3.5 Manage Files with Command-line Tools

| Commands |
| --- | 
| [mkdir](#mkdir) |
| [cp](#cp) |
| [mv](#mv) |
| [rm](#rm) |
| [rmdir](#rmdir) |

<a name="mkdir"></a>
Creates one or more directories or subdirectories.
```console
[user@host ~]$ cd Documents
[user@host Documents]$ mkdir ProjectX ProjectY ProjectZ
[user@host Documents]$ ls
ProjectX  ProjectY ProjectZ
```
The ```mkdir``` command ```-p``` (parent) option creates any missing parent directories for the requested destination.
```console
[user@host Documents]$ mkdir -p Thesis/Chapter1 Thesis/Chapter2 Thesis/Chapter3
[user@host Documents]$ ls -R Thesis/
Thesis/:
Chapter1  Chapter2  Chapter3

Thesis/Chapter1:

Thesis/Chapter2:

Thesis/Chapter3:
```

<a name="cp"></a>
```cp``` command copies a file, and creates a file either in the current directory or in a different specified directory.
```console
[user@host ~]$ cd Videos
[user@host Videos]$ cp blockbuster1.ogg blockbuster3.ogg
[user@host Videos]$ ls -l
total 0
-rw-rw-r--. 1 user user    0 Feb  8 16:23 blockbuster1.ogg
-rw-rw-r--. 1 user user    0 Feb  8 16:24 blockbuster2.ogg
-rw-rw-r--. 1 user user    0 Feb  8 16:34 blockbuster3.ogg
```
* If a file with the same name exists in the target directory, then the existing file is overwritten.
* By default, the cp command does not copy directories; it ignores them.
In the following example, two directories are listed as arguments, the Thesis and ProjectX directories. The last argument, the ProjectX directory, is the target and is valid as a destination. The Thesis argument is ignored by the cp command because it is intended to be copied and it is a directory.
```console
[user@host Documents]$ cp thesis_chapter1.txt thesis_chapter2.txt Thesis ProjectX
cp: -r not specified; omitting directory 'Thesis'
[user@host Documents]$ ls Thesis ProjectX
ProjectX:
thesis_chapter1.txt  thesis_chapter2.txt

Thesis:
Chapter1  Chapter2  Chapter3
```
1. ```-r``` solve this problem, copy directories and their contents
```console
[user@host Documents]$ cd ProjectY
[user@host ProjectY]$ cp -r ../Thesis/ .
[user@host ProjectY]$ ls -lR
.:
total 0
drwxr-xr-x. 5 user user 54 Mar  7 15:08 Thesis

./Thesis:
total 0
drwxr-xr-x. 2 user user 6 Mar  7 15:08 Chapter1
drwxr-xr-x. 2 user user 6 Mar  7 15:08 Chapter2
drwxr-xr-x. 2 user user 6 Mar  7 15:08 Chapter3

./Thesis/Chapter1:
total 0

./Thesis/Chapter2:
total 0

./Thesis/Chapter3:
total 0
```

<a name="mv"></a>
* Moves files from one location to another.
*  If you think of the absolute path to a file as its full name, then moving a file is effectively the same as renaming a file. The content of the files that are moved remain unchanged.
```console
[user@host Documents]$ ls -l
-rw-r--r--. 1 user user  7100 Mar  7 14:37 thesis_chapter1.txt
-rw-r--r--. 1 user user 11431 Mar  7 14:39 thesis_chapter2.txt
...output omitted...
[user@host Documents]$ mv thesis_chapter2.txt thesis_chapter2_reviewed.txt
[user@host Documents]$ ls -l
-rw-r--r--. 1 user user  7100 Mar  7 14:37 thesis_chapter1.txt
-rw-r--r--. 1 user user 11431 Mar  7 14:39 thesis_chapter2_reviewed.txt
...output omitted...
```
1. ```-v``` display a detailed output of the command operations.
```console
[user@host Documents]$ ls Thesis/Chapter1
[user@host Documents]$
[user@host Documents]$ mv -v thesis_chapter1.txt Thesis/Chapter1
renamed 'thesis_chapter1.txt' -> 'Thesis/Chapter1/thesis_chapter1.txt'
[user@host Documents]$ ls Thesis/Chapter1
thesis_chapter1.txt
[user@host Documents]$ ls -l
-rw-r--r--. 1 user user 11431 Mar  7 14:39 thesis_chapter2_reviewed.txt
...output omitted...
```

<a name="rm"></a>
* Removes files.
* By default, ```rm``` does not remove directories. You can use the ```rm``` command ```-r```or ```--recursive``` option to enable the rm command to delete directories and their contents. The ```rm -r``` command traverses each subdirectory first, and individually removes their files before removing each directory.
```console
[user@host Documents]$ ls -l Thesis/Chapter1
-rw-r--r--. 1 user user 7100 Mar  7 14:37 thesis_chapter1.txt
[user@host Documents]$ rm Thesis/Chapter1/thesis_chapter1.txt
[user@host Documents]$ rm Thesis/Chapter1
rm: cannot remove 'Thesis/Chapter1': Is a directory
[user@host Documents]$ rm -r Thesis/Chapter1
[user@host Documents]$ ls -l Thesis
drwxr-xr-x. 2 user user 6 Mar  7 12:37 Chapter2
drwxr-xr-x. 2 user user 6 Mar  7 12:37 Chapter3
```
* You can use the ```rm``` command ```-i``` option to interactively prompt for confirmation before deleting. This option is essentially the opposite of using the ```rm``` command ```-f``` option, which forces the removal without prompting the user for confirmation.
* If you specify both the ```-i``` and ```-f``` options, then the ```-f``` option takes priority and you are not prompted for confirmation before rm deletes files.
```console
[user@host Documents]$ rm -ri Thesis
rm: descend into directory 'Thesis'? y
rm: descend into directory 'Thesis/Chapter2'? y
rm: remove regular empty file 'Thesis/Chapter2/thesis_chapter2.txt'? y
rm: remove directory 'Thesis/Chapter2'? y
rm: remove directory 'Thesis/Chapter3'? y
rm: remove directory 'Thesis'? y
```

<a name="rmdir"></a>
You can also use the ```rmdir``` command to remove empty directories. Use the ```rm``` command ```-r``` option to delete non-empty directories.
```console
[user@host Documents]$ pwd
/home/user/Documents
[user@host Documents]$ rmdir ProjectZ
[user@host Documents]$ rmdir ProjectX
rmdir: failed to remove 'ProjectX': Directory not empty
[user@host Documents]$ rm -r ProjectX
[user@host Documents]$
```

<a name="3.7"></a>
## 3.7 Make Links Between Files

| Content |
| --- | 
| [Hard Links](#hard) |
| [Symbolic Links](#symbolic) |

<a name="hard"></a>
### Hard Links

* The new hard link acts exactly like the original file name. After the link is created, you cannot tell the difference between the new hard link and the original name of the file.
* You can determine whether a file has multiple hard links by using the ```ls -l```
```console
[user@host ~]$ pwd
/home/user
[user@host ~]$ ls -l newfile.txt
-rw-r--r--. 1 user user 0 Mar 11 19:19 newfile.txt
```
* ```ln``` command to create a hard link (another file name) that points to an existing file.
```console
[user@host ~]$ ln newfile.txt /tmp/newfile-hlink2.txt
[user@host ~]$ ls -l newfile.txt /tmp/newfile-hlink2.txt
-rw-rw-r--. 2 user user 12 Mar 11 19:19 newfile.txt
-rw-rw-r--. 2 user user 12 Mar 11 19:19 /tmp/newfile-hlink2.txt
```
* To determine whether two files are hard linked, use the ```ls``` command ```-i``` option to list each file's inode number. If the files are on the same file system and their inode numbers are the same, then the files are hard links that point to the same data file content.
```console
[user@host ~]$ ls -il newfile.txt /tmp/newfile-hlink2.txt
8924107 -rw-rw-r--. 2 user user 12 Mar 11 19:19 newfile.txt
8924107 -rw-rw-r--. 2 user user 12 Mar 11 19:19 /tmp/newfile-hlink2.txt
```
* Even if the original file is deleted, you can still access the contents of the file provided that at least one other hard link exists. Data is deleted from storage only when the last hard link is deleted, making the file contents unreferenced by any hard link.
```console
[user@host ~]$ rm -f newfile.txt
[user@host ~]$ ls -l /tmp/newfile-hlink2.txt
-rw-rw-r--. 1 user user 12 Mar 11 19:19 /tmp/newfile-hlink2.txt
[user@host ~]$ cat /tmp/newfile-hlink2.txt
Hello World
```
* you can use hard links only with regular files. You cannot use the ```ln``` command to create a hard link to a directory or special file.
* you can use hard links only if both files are on the same file system.
* You can use the df command to list the directories that are on different file systems. For example, you might see the following output:
```console
[user@host ~]$ df
Filesystem                   1K-blocks    Used Available Use% Mounted on
devtmpfs                        886788       0    886788   0% /dev
tmpfs                           902108       0    902108   0% /dev/shm
tmpfs                           902108    8696    893412   1% /run
tmpfs                           902108       0    902108   0% /sys/fs/cgroup
/dev/mapper/rhel_rhel9--root  10258432 1630460   8627972  16% /
/dev/sda1                      1038336  167128    871208  17% /boot
tmpfs                           180420       0    180420   0% /run/user/1000
```

<a name="symbolic"></a>
### Symbolic Links

* The ```ln``` command ```-s``` option creates a symbolic link, which is also called a "soft link". 
* A symbolic link is not a regular file, but a special type of file that points to an existing file or directory.
* Symbolic links can link two files on different file systems.
* Symbolic links can point to a directory or special file, not just to a regular file.
```concole
[user@host ~]$ ln -s /home/user/newfile-link2.txt /tmp/newfile-symlink.txt
[user@host ~]$ ls -l newfile-link2.txt /tmp/newfile-symlink.txt
-rw-rw-r--. 1 user user 12 Mar 11 19:19 newfile-link2.txt
lrwxrwxrwx. 1 user user 11 Mar 11 20:59 /tmp/newfile-symlink.txt -> /home/user/newfile-link2.txt
[user@host ~]$ cat /tmp/newfile-symlink.txt
Symbolic Hello World
```
* The first character of the long listing for the ```/tmp/newfile-symlink.txt``` file is ```l``` (letter ```l```) instead of ```-```. This character indicates that the file is a symbolic link and not a regular file.
* When the original regular file is deleted, the symbolic link still points to the file but the target is gone. A symbolic link that points to a missing file is called a "dangling symbolic link".
```console
[user@host ~]$ rm -f newfile-link2.txt
[user@host ~]$ ls -l /tmp/newfile-symlink.txt
lrwxrwxrwx. 1 user user 11 Mar 11 20:59 /tmp/newfile-symlink.txt -> /home/user/newfile-link2.txt
[user@host ~]$ cat /tmp/newfile-symlink.txt
cat: /tmp/newfile-symlink.txt: No such file or directory
```
* A hard link points a name to data on a storage device.
* A symbolic link points a name to another name, which points to data on a storage device.
* If you use ```cd``` to change to the symbolic link, then the current working directory becomes the linked directory. Some tools might track that you followed a symbolic link to get there. For example, by default, cd updates your current working directory by using the name of the symbolic link rather than the name of the actual directory. If you want to update the current working directory by using the name of the actual directory, then you can use the ```-P``` option.
```console
[user@host ~]$ ln -s /etc /home/user/configfiles
[user@host ~]$ cd /home/user/configfiles
[user@host configfiles]$ pwd
/home/user/configfiles
[user@host configfiles]$ cd -P /home/user/configfiles
[user@host etc]$ pwd
/etc
```
* ```cd -P``` command is equivalent to ```cd -L```, which is used to force cd to not follow symbolic links and display the actual path of the directory.

<a name="3.9"></a>
## 3.9 Match File Names with Shell Expansions

| Content |
| --- | 
| [Pattern Matching](#pattern) |
| [Tilde Expansion ```~```](#tilde) |
| [Brace Expansion](#brace) |

<a name="pattern"></a>
### Pattern Matching

| Metacharacters | Matches |
| --- | --- |
| Pattern |	Matches |
| [\*](#*)	| Any string of zero or more characters. |
| [?](#?)	| Any single character. |
| [[abc…​]](#[abc…​])	| Any one character in the enclosed class (between the square brackets). |
| [!abc…​]	| Any one character not in the enclosed class. |
| [^abc…​]	| Any one character not in the enclosed class. |
| [[:alpha:]]	| Any alphabetic character. |
| [[:lower:]]	| Any lowercase character. |
| [[:upper:]]	| Any uppercase character. |
| [[:alnum:]]	| Any alphabetic character or digit. |
| [[:punct:]]	| Any printable character that is not a space or alphanumeric. |
| [[:digit:]]	| Any single digit from 0 to 9. |
| [[:space:]]	| Any single white space character, which might include tabs, newlines, carriage returns, form feeds, or spaces. |

```console
[user@host ~]$ mkdir glob; cd glob
[user@host glob]$ touch alpha bravo charlie delta echo able baker cast dog easy
[user@host glob]$ ls
able  alpha  baker  bravo  cast  charlie  delta  dog  easy  echo
```
<a name="*"></a>
```console
[user@host glob]$ ls a*
able  alpha
[user@host glob]$ ls *a*
able  alpha  baker  bravo  cast  charlie  delta  easy
```
<a name="[abc…​]"></a>
```console
[user@host glob]$ ls [ac]*
able  alpha  cast  charlie
```
<a name="?"></a>
```console
[user@host glob]$ ls ????
able  alpha  cast  easy  echo
[user@host glob]$ ls ?????
baker  bravo  delta
```

<a name="tilde"></a>
### Tilde Expansion ```~```

```console
[user@host glob]$ echo ~root
/root
[user@host glob]$ echo ~user
/home/user
[user@host glob]$ echo ~/glob
/home/user/glob
[user@host glob]$ echo ~nonexistinguser
~nonexistinguser
```

<a name="brace"></a>
### Brace Expansion

```console
[user@host glob]$ echo {Sunday,Monday,Tuesday,Wednesday}.log
Sunday.log Monday.log Tuesday.log Wednesday.log
[user@host glob]$ echo file{1..3}.txt
file1.txt file2.txt file3.txt
[user@host glob]$ echo file{a..c}.txt
filea.txt fileb.txt filec.txt
[user@host glob]$ echo file{a,b}{1,2}.txt
filea1.txt filea2.txt fileb1.txt fileb2.txt
[user@host glob]$ echo file{a{1,2},b,c}.txt
filea1.txt filea2.txt fileb.txt filec.txt
```
```console
[user@host glob]$ mkdir ../RHEL{7,8,9}
[user@host glob]$ ls ../RHEL*
RHEL7 RHEL8 RHEL9
```

