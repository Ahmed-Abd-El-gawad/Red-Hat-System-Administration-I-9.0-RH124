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

* 

<a name="7.3"></a>
## 7.3 Manage File System Permissions from the Command Line

| Commands |
| --- |
| [](#) |


<a name="7.5"></a>
## 7.5 Manage Default Permissions and File Access

| Commands |
| --- |
| [](#) |
