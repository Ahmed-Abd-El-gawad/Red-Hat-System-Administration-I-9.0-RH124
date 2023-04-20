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
| [Identifying Remote Users ```w```](#w) |
| [Keys](#keys) |

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

<a name="w"></a>
* ```console
  [developer1@host ~]$ ssh developer1@hosta
  developer1@hosta's password: redhat
  [developer1@hosta ~]$ w
   16:13:38 up 36 min,  1 user,  load average: 0.00, 0.00, 0.00
  USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
  developer2   pts/0    172.25.250.10    16:13    7:30   0.01s  0.01s -bash
  developer1   pts/1    172.25.250.10    16:24    3.00s  0.01s  0.00s w
  [developer2@hosta ~]$
  ```

<a name="keys"></a>
* The ```ssh``` command asks for confirmation to log in when the client does not have a copy of the public key in its known hosts file. The copy of the public key is saved in the ```~/.ssh/known_hosts``` file to confirm the server's identity for the future automatically.
  ```console
  [developer1@host ~]$ ssh hostb
  The authenticity of host 'hosta (172.25.250.12)' can't be established.
  ECDSA key fingerprint is SHA256:qaS0PToLrqlCO2XGklA0iY7CaP7aPKimerDoaUkv720.
  Are you sure you want to continue connecting (yes/no)? yes
  Warning: Permanently added 'hostb,172.25.250.12' (ECDSA) to the list of known hosts.
  developer1@hostb's password: redhat
  ...output omitted...
  [developer1@hostb ~]$
  ```
  
* Verify the fingerprint of the server's SSH host key by using the following command.
  ```console
  [developer1@hostb ~]$ ssh-keygen -l -f /etc/ssh/ssh_host_ecdsa_key.pub
  256 SHA256:qaS0PToLrqlCO2XGklA0iY7CaP7aPKimerDoaUkv720 root@server (ECDSA)
  ```

* If a server's public key is changed because the key was lost due to hard drive failure or it was replaced for some legitimate reason, then for successful login, you must edit the known hosts file to replace the old public key entry with the new public key.
  
  The ```/etc/ssh/ssh_known_hosts``` file stores the public key file for each user on the SSH client. Each key consists of one line, where the first field is the list of hostnames and IP addresses that share the public key. The second field is the encryption algorithm that is used for the key. The last field is the key itself.
  ```console
  [developer1@host ~]$ cat ~/.ssh/known_hosts
  hosta,172.25.250.11 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOsEi0e+FlaNT6jul8Ag5Nj+RViZl0yE2w6iYUr+1fPtOIF0EaOgFZ1LXM37VFTxdgFxHS3D5WhnIfb+68zf8+w=
  ```

* Each remote SSH server that you connect to stores its public key in a file with the .pub extension in the ```/etc/ssh``` directory.
  ```console
  [developer1@hosta ~]$ ls /etc/ssh/*key.pub
  /etc/ssh/ssh_host_ecdsa_key.pub  /etc/ssh/ssh_host_ed25519_key.pub  /etc/ssh/ssh_host_rsa_key.pub
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

