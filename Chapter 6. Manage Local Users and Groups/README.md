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

<a name="id"></a>
* Use the id command to show information about the currently logged-in user:
```console
[user01@host ~]$ id
uid=1000(user01) gid=1000(user01) groups=1000(user01) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```
* To view basic information about another user, pass the username to the id command as an argument:
```console
[user01@host ~]$ id user02
uid=1002(user02) gid=1001(user02) groups=1001(user02) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```
<a name="-l"></a>
* Use the ls -l command to view the owner of a file. 
```console
[user01@host ~]$ ls -l mytextfile.txt
-rw-rw-r--. 1 user01 user01 0 Feb  5 11:10 mytextfile.txt
```
<a name="-ld"></a>
* Use the ls -ld command to view the owner of a directory, rather than the contents of that directory. In the following output, the third column shows the username.
```console
[user01@host]$ ls -ld Documents
drwxrwxr-x. 2 user01 user01 6 Feb  5 11:10 Documents
```


<a name="6.3"></a>
## 6.3 Gain Superuser Access



<a name="6.5"></a>
## 6.5 Manage Local User Accounts



<a name="6.7"></a>
## 6.7 Manage Local Group Accounts



<a name="6.9"></a>
## 6.9 Manage User Passwords


