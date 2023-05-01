# Chapter 13. Archive and Transfer Files

| content |
| --- |
| [13.1 Manage Compressed tar Archives](#13.1) |
| [13.3 Transfer Files Between Systems Securely](#13.3) |
| [13.5 Synchronize Files Between Systems Securely](#13.5) |


<a name="13.1"></a>
## 13.1 Manage Compressed tar Archives

| Content |
| --- |
| [Common Options of the tar Utility](#options) |
| [Create an Archive](#-c) |
| [List Archive Contents](#-t) |
| [Extract Archive Contents](#-x) |
| [Create a Compressed Archive](#compression) |

<a name="options"></a>
* One of the following ```tar``` command actions is required to perform a ```tar``` operation:
  
  | Option | purpose |
  | --- | --- |
  | ```-c``` or ```--create``` | Create an archive file. |
  | ```-t``` or ```--list``` | List the contents of an archive. |
  | ```-x``` or ```--extract``` | Extract an archive. |
* The following ```tar``` command general options are commonly included:
  
  | Option | purpose |
  | --- | --- |
  | ```-v``` or ```--verbose``` | Show the files being archived or extracted during the ```tar``` operation. |
  | ```-f``` or ```--file``` | Follow this option with the archive file name to create or open. |
  | ```-p``` or ```--preserve-permissions``` | Preserve the original file permissions when extracting. |
  | ```--xattrs``` | Enable extended attribute support, and store extended file attributes. |
  | ```--selinux``` | Enable SELinux context support, and store SELinux file contexts. |
* The following ```tar``` command compression options are used to select an algorithm:
  
  | Option | purpose |
  | --- | --- |
  | ```-a``` or ```--auto-compress``` | Use the archive's suffix to determine the algorithm to use. |
  | ```-z``` or ```--gzip``` | Use the ```gzip``` compression algorithm, resulting in a ```.tar.gz``` suffix. |
  | ```-j``` or ```--bzip2``` | Use the ```bzip2``` compression algorithm, resulting in a ```.tar.bz2``` suffix. |
  | ```-J``` or ```--xz``` | Use the ```xz``` compression algorithm, resulting in a ```.tar.xz``` suffix. |
  | ```-Z``` or ```--compress``` | Uses an ```LZ-variant``` algorithm, resulting in a ```.tar.Z``` suffix. |

<a name="-c"></a>
* To create an archive with the ```tar``` command, use the create and file options with the archive file name as the first argument, then followed by a list of files and directories to include in the archive.
  ```console
  [user@host ~]$ tar -cf mybackup.tar myapp1.log myapp2.log myapp3.log
  [user@host ~]$ ls mybackup.tar
  archive.tar
  ```
* The ```tar``` command recognizes absolute and relative file name syntax.
  * By default, ```tar``` removes the leading forward slash (```\```) character from absolute file names, so that files are stored internally with relative path names.
  * This technique is safer, because extracting absolute path names always overwrites existing files. With files archived with relative path names, files can be extracted to a new directory without overwriting existing files.
* A user must have read permissions on the target files being archived. For example, ```root``` privileges are required archive the ```/etc``` directory and its contents, because only privileged users can read all ```/etc``` files An unprivileged user can create an archive of the ```/etc``` directory, but the archive would not contain files the user cannot read, and directories for which the user lacks the read and execute permissions.
  ```console
  [root@host ~]# tar -cf /root/etc-backup.tar /etc
  tar: Removing leading `/' from member names
  ```

<a name="-t"></a>
* Use the ```tar``` command ```t``` option to list the file names from within the archive specified with the ```f``` option. The files list with relative name syntax, because the leading forward slash (```/```) was removed during archive creation.
  ```console
  [root@host ~]# tar -tf /root/etc.tar
  etc/
  etc/fstab
  etc/crypttab
  etc/mtab
  ...output omitted...
  ```

<a name="-x"></a>
* Extract a ```tar``` archive into an empty directory to avoid overwriting existing files. When the ```root``` user extracts an archive, the extracted files preserve the original user and group ownership. If a regular user extracts files, then the user becomes the owner of the extracted files.
  ```console
  [root@host ~]# mkdir /root/etcbackup
  [root@host ~]# cd /root/etcbackup
  [root@host etcbackup]# tar -tf /root/etc.tar
  etc/
  etc/fstab
  etc/crypttab
  etc/mtab
  ...output omitted...
  [root@host etcbackup]# tar -xf /root/etc.tar
  ```
* When you extract files from an archive, the current ```umask``` is used to modify each extracted file's permissions. Instead, use the ```tar``` command ```p``` option to preserve the original archived permissions for extracted files. The ```--preserve-permissions``` option is enabled by default for a superuser.
  ```console
  [user@host scripts]# tar -xpf /home/user/myscripts.tar
  ...output omitted...
  ```

<a name="compression"></a>
* The ```tar``` command supports these compression methods, and others:
  * ```gzip``` compression is the legacy, fastest method, and is widely available across platforms.
  * ```bzip2``` compression creates smaller archives but is less widely available than ```gzip```.
  * ```xz``` compression is newer, and offers the best compression ratio of the available methods.
  * **compress** is a legacy LZ algorithm variation, and is widely available across platforms.
  ```console
  [root@host ~]# tar -czf /root/etcbackup.tar.gz /etc
  tar: Removing leading `/' from member names
  ```
  ```console
  [root@host ~]$ tar -cjf /root/logbackup.tar.bz2 /var/log
  tar: Removing leading `/' from member names
  ```
  ```console
  [root@host ~]$ tar -cJf /root/sshconfig.tar.xz /etc/ssh
  tar: Removing leading `/' from member names
  ```
* The ```tar``` command can automatically determine which compression was used, so it is not necessary to specify the compression option. If you do include an incorrect compression type, ```tar``` reports that the specified compression type does not match the file's type.
* The ```gzip```, ```bzip2```, ```xz```, and ```compress``` algorithms are also implemented as standalone commands for compressing individual files without creating an archive. These commands do not allow you to create a single compressed file of multiple files, such as a directory. As previously discussed, to create a compressed archive of multiple files, use the ```tar``` command with your preferred compression option. To uncompress a single compressed file or a compressed archive file without extracting its contents, use the ```gunzip```, ```bunzip2```, ```unxz```, and ```uncompress``` standalone commands.

  The ```gzip``` and ```xz``` commands provide an ```-l``` option to view the uncompressed size of a compressed single or archive file. Use this option to verify sufficient space is available before uncompressing or extracting a file.
  ```console
  [user@host ~]$ gzip -l file.tar.gz
           compressed        uncompressed  ratio uncompressed_name
            221603125           303841280  27.1% file.tar
  [user@host ~]$ xz -l file.xz
  Strms  Blocks   Compressed Uncompressed  Ratio  Check   Filename
      1       1    195.7 MiB    289.8 MiB  0.675  CRC64   file.xz
  ```


<a name="13.3"></a>
## 13.3 Transfer Files Between Systems Securely

| Content |
| --- |
| [sftp](#sftp) |
| [scp](#scp) |

<a name="sftp"></a>
* The <sub>OpenSSH</sub> suite is useful for securely running shell commands on remote systems. Use the ***Secure File Transfer Program (SFTP)*** to interactively upload to or download files from an SSH server. This program is part of the <sub>OpenSSH</sub> suite. A session with the ```sftp``` command uses the secure authentication mechanism and encrypted data transfer to and from the SSH server.
  ```console
  [user@host ~]$ sftp remoteuser@remotehost
  remoteuser@remotehost's password: password
  Connected to remotehost.
  sftp>
  ```
* The interactive ```sftp``` session accepts various commands that work the same way on the remote file system as in the local file system, such as the ```ls```, ```cd```, ```mkdir```, ```rmdir```, and ```pwd``` commands. 
  ```console
  sftp> help
  Available commands:
  bye                                Quit sftp
  cd path                            Change remote directory to 'path'
  chgrp [-h] grp path                Change group of file 'path' to 'grp'
  chmod [-h] mode path               Change permissions of file 'path' to 'mode'
  chown [-h] own path                Change owner of file 'path' to 'own'
  ...output omitted...
  ```
* In an ```sftp``` session, you might want to run some commands on your local host. For most available commands, add the ```l``` character before the command. For example, the ```pwd``` command prints the current working directory on the remote host. To print the current working directory on your local host, use the ```lpwd``` command.
  ```
  sftp> pwd
  Remote working directory: /home/remoteuser
  sftp> lpwd
  Local working directory: /home/user
  ```
* The ```put``` command uploads a file to the remote system. 
  ```console
  sftp> mkdir hostbackup
  sftp> cd hostbackup
  sftp> put /etc/hosts
  Uploading /etc/hosts to /home/remoteuser/hostbackup/hosts
  /etc/hosts                                 100%  227     0.2KB/s   00:00
  ```
* The ```get``` command downloads a file from the remote system. 
  ```console
  sftp> get /etc/yum.conf
  Fetching /etc/yum.conf to yum.conf
  /etc/yum.conf                              100%  813     0.8KB/s   00:00
  sftp> exit
  [user@host ~]$
  ```
* The ```exit``` command exits the sftp session.
* To copy a whole directory tree recursively, use the ```sftp``` command ```-r``` option. The following example recursively copies the ```/home/user/director```y local directory to the remotehost machine.
  ```console
  sftp> put -r directory
  Uploading directory/ to /home/remoteuser/directory
  Entering directory/
  file1                                      100%    0     0.0KB/s   00:00
  file2                                      100%    0     0.0KB/s   00:00
  sftp> ls -l
  drwxr-xr-x    2 student  student        32 Mar 21 07:51 directory
  ```
* To get a remote file with the ```sftp``` command on a single command line, without opening an interactive session, use the following syntax. You cannot use single command line syntax to put files on a remote host.
  ```console
  [user@host ~]$ sftp remoteuser@remotehost:/home/remoteuser/remotefile
  Connected to remotehost.
  Fetching /home/remoteuser/remotefile to remotefile
  remotefile                                                       100%    7    15.7KB/s   00:00
  ```

<a name="scp"></a>
* The scp command, which system administrators widely use to copy files to and from remote systems, is based on a historical rcp protocol that was not designed with security considerations in mind. 
* The scp command has a known code injection issue that could grant an attacker the ability to execute arbitrary commands on the remote server.


<a name="13.5"></a>
## 13.5 Synchronize Files Between Systems Securely

| Content |
| --- |
| [rsync](#rsync) |

<a name="rsync"></a>
* The ```rsync``` command is another way to copy files from one system to another system securely.
* The tool uses an algorithm that minimizes the amount of copied data by synchronizing only the changed portions of files. 
  * If two files or directories are similar between two servers, then the rsync command copies only the differences between the file systems.
* An advantage of the ```rsync``` command is that it copies files between a local system and a remote system securely and efficiently.

* Use the ```rsync``` command ```-n``` option for a ***dry run***. A dry run simulates what happens when the command is executed. The dry run shows the changes that the ```rsync``` command would perform when executing the command. Perform a dry run before the actual ```rsync``` command operation to ensure that no critical files are overwritten or deleted.

* The ```rsync``` command ```-a``` or ```--archive``` option enables "archive mode". This option enables recursive copying and turns on many valuable options to preserve most characteristics of the files. Archive mode is the same as specifying the following options:
  | Option | Description |
  | --- | --- |
  | ```-r```, ```--recursive``` | Synchronize the whole directory tree recursively |
  | ```-l```, ```--links``` | Synchronize symbolic links |
  | ```-p```, ```--perms``` | Preserve permissions |
  | ```-t```, ```--times``` | Preserve time stamps |
  | ```-g```, ```--group``` | Preserve group ownership |
  | ```-o```, ```--owner``` | Preserve the owner of the files |
  | ```-D```, ```--devices``` | Preserve device files |




