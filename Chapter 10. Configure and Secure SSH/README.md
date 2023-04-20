# Chapter 10. Configure and Secure SSH

| content |
| --- |
| [10.1 Access the Remote Command Line with SSH](#10.1) |
| [10.3 Configure SSH Key-based Authentication](#10.3) |
| [10.5 Customize OpenSSH Service Configuration](#10.5) |


<a name="10.1"></a>
## 10.1 Access the Remote Command Line with SSH

| Content |
| --- |
| [Ways to ```ssh```](#ways) |

<a name="ways"></a>
* The following ```ssh``` command logs you in on the hosta remote server using the same user name as the current local user.
  ```console
  [developer1@host ~]$ ssh hosta
  developer1@hosta's password: redhat
  ...output omitted...
  [developer1@hosta ~]$
  ```
* Use the ```exit``` command to log out of the remote system.
  ```console
  [developer1@hosta ~]$ exit
  logout
  Connection to hosta closed.
  [developer1@host ~]$
  ```
* The following ```ssh``` command logs you in on the hosta remote server with the ```developer2``` user name.
  ```console
  [developer1@host ~]$ ssh developer2@hosta
  developer2@hosta's password: shadowman
  ...output omitted...
  [developer2@hosta ~]$
  ```
* The following ```ssh``` command runs the ```hostname``` command on the hosta remote system as the ```developer2``` user without accessing the remote interactive shell.
  ```console
  [developer1@host ~]$ ssh developer2@hosta hostname
  developer2@hosta's password: shadowman
  hosta.lab.example.com
  [developer1@host ~]$
  ```



<a name="10.3"></a>
## 10.3 Configure SSH Key-based Authentication

| Content |
| --- |

* 

<a name="10.5"></a>
## 10.5 Customize OpenSSH Service Configuration

| Content |
| --- |

* 

