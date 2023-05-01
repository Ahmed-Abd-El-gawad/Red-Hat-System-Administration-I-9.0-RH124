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




<a name="13.3"></a>
## 13.3 Transfer Files Between Systems Securely

| Content |
| --- |

<a name=""></a>
* 


<a name="13.5"></a>
## 13.5 Synchronize Files Between Systems Securely

| Content |
| --- |

<a name=""></a>
* 



