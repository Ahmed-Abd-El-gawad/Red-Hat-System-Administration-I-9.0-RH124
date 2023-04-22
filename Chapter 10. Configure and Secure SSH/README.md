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
| [passwordless](#passwordless) |
| [ssh-agent](#ssh-agent) |
| [Basic SSH Connection Troubleshooting](#troubleshooting) |
| [SSH Client Configuration](#client_config) |

<a name="passwordless"></a>
* To prepare your account, generate a cryptographically-related pair of key files. One key is private and held only by you, while the second is your related public key that is not secret. The private key acts as your authentication credential and it must be stored securely. The public key is copied to your account on servers that you will remotely access, and verifies your use of your private key.
  
  1. SSH Keys Generation
    
    Use the ```ssh-keygen``` command to create a key pair. By default, the ```ssh-keygen``` command saves your private and public keys in the ```~/.ssh/id_rsa``` and ```~/.ssh/id_rsa.pub``` files, but you can specify a different name.
    ```console
    [user@host ~]$ ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/user/.ssh/id_rsa): Enter
    Created directory '/home/user/.ssh'.
    Enter passphrase (empty for no passphrase): Enter
    Enter same passphrase again: Enter
    Your identification has been saved in /home/user/.ssh/id_rsa.
    Your public key has been saved in /home/user/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:vxutUNPio3QDCyvkYm1 user@host.lab.example.com
    The key's randomart image is:
    +---[RSA 2048]----+
    |                 |
    |   .     .       |
    |  o o     o      |
    | . = o   o .     |
    |  o + = S E .    |
    | ..O o + * +     |
    |.+% O . + B .    |
    |=*oO . . + *     |
    |++.     . +.     |
    +----[SHA256]-----+
    ```
    * You can choose to provide a passphrase to ```ssh-keygen```, which is used to encrypt your private key. Using a passphrase is recommended, so that your private key cannot be used by someone who gains access to it. If you set a passphrase, then you must enter the passphrase each time you use the private key. The passphrase is used locally, to decrypt your private key before use, unlike your password, which must be sent in clear text across the network for use.
      ```console
      [user@host ~]$ ssh-keygen -f .ssh/key-with-pass
      Generating public/private rsa key pair.
      Enter passphrase (empty for no passphrase): your_passphrase
      Enter same passphrase again: your_passphrase
      Your identification has been saved in .ssh/key-with-pass.
      Your public key has been saved in .ssh/key-with-pass.pub.
      The key fingerprint is:
      SHA256:w3GGB7EyHUry4aOcNPKmhNKS7dl1YsMVLvFZJ77VxAo user@host.lab.example.com
      The key's randomart image is:
      +---[RSA 2048]----+
      |    . + =.o ...  |
      |     = B XEo o.  |
      |  . o O X =....  |
      | = = = B = o.    |
      |= + * * S .      |
      |.+ = o + .       |
      |  + .            |
      |                 |
      |                 |
      +----[SHA256]-----+
      ```
      * The ```ssh-keygen``` command ```-f``` option specifies the files in which to save the keys. 
    
    * You can use the ```ssh-agent``` key manager locally, which caches your passphrase upon first use in a login session, and then provides the passphrase for all subsequent private key use in the same login session.
    
  2. Share the Public Key
    
    To configure your remote account for access, copy your public key to the remote system. The ```ssh-copy-id``` command copies the public key of the SSH key pair to the remote system. You can specify a specific public key with the ```ssh-copy-id``` command, or use the default ```~/.ssh/id_rsa.pub``` file.
    ```console
    [user@host ~]$ ssh-copy-id -i .ssh/key-with-pass.pub user@remotehost
    /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/user/.ssh/id_rsa.pub"
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    user@remotehost's password: redhat
    Number of key(s) added: 1

    Now try logging into the machine, with:   "ssh 'user@remotehost'"
    and check to make sure that only the key(s) you wanted were added.
    ```
    * Test the remote access, after placing the public key, with the corresponding private key. If the configuration is correct, you will gain access to your account on the remote system without being asked for your account password. If you do not specify a private key file, then the ```ssh``` command uses the default ```~/.ssh/id_rsa``` file if it exists.
    
    * If you configured a passphrase to protect your private key, the passphrase will be requested by SSH at first use. However, if the key authentication succeeds, you will not be asked for your account password.
    ```console
    [user@host ~]$ ssh -i .ssh/key-with-pass user@remotehost
    Enter passphrase for key '.ssh/key-with-pass': your_passphrase
    ...output omitted...
    [user@remotehost ~]$
    ```
    
<a name="ssh-agent"></a>
* If you encrypt your private key with a passphrase, then you must enter the passphrase each time you use the private key for authentication. However, you can configure the ```ssh-agent``` key manager to cache passphrases. Then, each time you use SSH, the ```ssh-agent key``` manager provides the passphrase for you. Using a key manager convenient and can improve security by providing fewer opportunities for other people to observe your passphrase.

  The ```ssh-agent``` key manager can be configured to start automatically when you log in. The GNOME graphical desktop environment can automatically start and configure the ```ssh-agent``` key manager. If you log in to a text environment, you must start the ```ssh-agent``` program manually for each session. Start the ```ssh-agent``` program with the following command.
  ```console
  [user@host ~]$ eval $(ssh-agent)
  Agent pid 10155
  ```

* ```ssh-add``` commands add the private keys from the default ```~/.ssh/id_rsa``` file and then from a ```~/.ssh/key-with-pass``` file.
  ```console
  [user@host ~]$ ssh-add
  Identity added: /home/user/.ssh/id_rsa (user@host.lab.example.com)
  [user@host ~]$ ssh-add .ssh/key-with-pass
  Enter passphrase for .ssh/key-with-pass: your_passphrase
  Identity added: .ssh/key-with-pass (user@host.lab.example.com)
  ```

* The following ```ssh``` command uses the ```~/.ssh/key-with-pass``` private key access your account on the remote server. The private key in this example was previously decrypted and added to the ```ssh-agent``` key manager, therefore the ```ssh``` command does not prompt you for the passphrase to decrypt the private key.
  ```console
  [user@host ~]$ ssh -i .ssh/key-with-pass user@remotehost
  Last login: Mon Mar 14 06:58:43 2022 from host.example.com
  [user@remotehost ~]$
  ```
  * When you log out of a session that used an ```ssh-agent``` key manager, all cached passphrases are cleared from memory.

<a name="troubleshooting"></a>
* SSH can appear complex when remote access using key pair authentication is not succeeding. The ```ssh``` command provides three verbosity levels with the ```-v```, ```-vv```, and ```-vvv``` options, that provide increasingly greater amounts of debugging information during ssh command use.
  ```console
  [user@host ~]$ ssh -v user@remotehost
  OpenSSH_8.7p1, OpenSSL 3.0.1 14 Dec 2021 i
  debug1: Reading configuration data /etc/ssh/ssh_config ii
  debug1: Reading configuration data /etc/ssh/ssh_config.d/01-training.conf
  debug1: /etc/ssh/ssh_config.d/01-training.conf line 1: Applying options for *
  debug1: Reading configuration data /etc/ssh/ssh_config.d/50-redhat.conf
  ...output omitted...
  debug1: Connecting to remotehost [192.168.1.10] port 22. iii
  debug1: Connection established.
  ...output omitted...
  debug1: Authenticating to remotehost:22 as 'user' iv
  ...output omitted...
  debug1: Authentications that can continue: publickey,gssapi-keyex,gssapi-with-mic,password v
  ...output omitted...
  debug1: Next authentication method: publickey vi
  debug1: Offering public key: /home/user/.ssh/id_rsa RSA SHA256:hDVJjD7xrUjXGZVRJQixxFV6NF/ssMjS6AuQ1+VqUc4 vii
  debug1: Server accepts key: /home/user/.ssh/id_rsa RSA SHA256:hDVJjD7xrUjXGZVRJQixxFV6NF/ssMjS6AuQ1+VqUc4 viii
  Authenticated to remotehost ([192.168.1.10]:22) using "publickey".
  ...output omitted...
  [user@remotehost ~]$
  ```
  1. OpenSSH and OpenSSL versions.
  2. OpenSSH configuration files.
  3. Connection to the remote host.
  4. Authentication methods that the remote host allows.
  5. Trying to authenticate the user on the remote host.
  6. Trying to authenticate the user by using the SSH key.
  7. Using the ```/home/user/.ssh/id_rsa``` key file to authenticate.
  8. The remote hosts accepts the SSH key.

<a name="client_config"></a>
* You can create the ```~/.ssh/config``` file to preconfigure SSH connections. Within the configuration file, you can specify connection parameters such as users, keys, and ports for specific hosts. This file eliminates the need to manually specify command parameters each time that you connect to a host.
  ```console
  [user@host ~]$ cat ~/.ssh/config
  host servera
       HostName                      servera.example.com
       User                          usera
       IdentityFile                  ~/.ssh/id_rsa_servera

  host serverb
       HostName                      serverb.example.com
       User                          userb
       IdentityFile                  ~/.ssh/id_rsa_serverb
  ```
* The ```~/.ssh/config``` file is also useful for configuring SSH jump hosts. An SSH jump host is a server that acts as a proxy for SSH connections to other, usually internal, hosts. Consider a scenario where a host called ```external``` is accessible via the internet, but a host called ```internal``` is only internally accessible. Use the ```ProxyHost``` parameter within the ```~/.ssh/config``` file to connect to the internal host via the ```external``` host:
  ```console
  [user@host ~]$ cat ~/.ssh/config
  host internal
       HostName                      internal.example.com
       ProxyHost                     external

  host external
       HostName                      external.example.com
  ```


<a name="10.5"></a>
## 10.5 Customize OpenSSH Service Configuration

| Content |
| --- |
| [Prohibit the Superuser from Logging In](#prohibit_superuser) |
| [Prohibit Password-based Authentication for SSH](#prohibit_password_authentication) |

<a name="prohibit_superuser"></a>
* The OpenSSH server uses the ```PermitRootLogin``` configuration setting in the ```/etc/ssh/sshd_config``` file to allow or prohibit users to log in to the system as root.
  ```bash
  PermitRootLogin yes
  ```
  ```console
  [root@host ~]# systemctl reload sshd
  ```

<a name="prohibit_password_authentication"></a>
* The OpenSSH server uses the ```PasswordAuthentication``` parameter in the ```/etc/ssh/sshd_config``` file to control whether users can use password-based authentication to log in to the system.
  ```bash
  PasswordAuthentication yes
  ```

