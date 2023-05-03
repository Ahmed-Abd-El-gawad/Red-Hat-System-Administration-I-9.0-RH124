# Chapter 14. Install and Update Software

| content |
| --- |
| [14.1 Register Systems for Red Hat Support](#14.1) |
| [14.3 Explain and Investigate RPM Software Packages](#14.3) |
| [14.5 Install and Update Software Packages with DNF](#14.5) |
| [14.7 Enable DNF Software Repositories](#14.7) |


<a name="14.1"></a>
## 14.1 Register Systems for Red Hat Support

| Content |
| --- |
| [dnf](#dnf) |
| [Simple Content Access](#SCA) |
| [subscription-manager](#subscription-manager) |
| [Activation Keys](#key) |
| [Entitlement Certificates](#cert) |

<a name="dnf"></a>
* ```dnf``` command obtain software packages and updates through a content distribution network that the Red Hat Content Delivery Network provides.

<a name="SCA"></a>
* Simple Content Access (SCA) is a Red Hat subscription management capability. When you enable SCA for your organization, the entitlement process is simplified. SCA eliminates the requirement to attach subscriptions at a per-system level. You register your systems, enable the repositories that each system needs, and begin installing software packages.

<a name="subscription-manager"></a>
* Use the ```subscription-manager``` command to register a system without using a graphical environment. The ```subscription-manager``` command automatically attaches a system to the best-matched compatible subscriptions for the system.

  * Register a system by using the credentials of the Red Hat Customer Portal as the ```root``` user:
    ```console
    [root@host ~]# subscription-manager register --username <yourusername>
    Registering to: subscription.rhsm.redhat.com:443/subscription
    Password: yourpassword
    The system has been registered with ID: 1457f7e9-f37e-4e93-960a-c94fe08e1b4f
    The registered system name is: host.example.com
    ```
  * View available subscriptions for your Red Hat account:
    ```console
    [root@host ~]# subscription-manager list --available
    -------------------------------------------
        Available Subscriptions
    -------------------------------------------
    ...output omitted...
    ```
  * Auto-attach a subscription:
    ```console
    [root@host ~]# subscription-manager attach --auto
    ...output omitted...
    ```
  * Alternatively, attach a subscription from a specific pool from the list of available subscriptions:
    ```console
    [root@host ~]# subscription-manager attach --pool=poolID
    ...output omitted...
    ```
  * View consumed subscriptions:
    ```console
    [root@host ~]# subscription-manager list --consumed
    ...output omitted...
    ```
  * Unregister a system:
    ```console
    [root@host ~]# subscription-manager unregister
    Unregistering from: subscription.rhsm.redhat.com:443/subscription
    System has been unregistered.
    ```

<a name="key"></a>
* An activation key is a preconfigured subscription management file that available for use with both Red Hat Satellite Server and subscription management through the Red Hat Customer Portal. Use the ```subscription-manager``` command with activation keys to simplify the registration and assignment of predefined subscriptions. This method of registration is beneficial for automating installations and deployments. For organizations that enable Simple Content Access, activation keys can register systems and enable repositories without needing to attach subscriptions.

<a name="cert"></a>
* Digital certificates store current entitlement information on the local system. The registered system stores the entitlement certificates under the ```/etc/pki``` directory.
  * ```/etc/pki/product``` certificates indicates installed Red Hat products.
  * ```/etc/pki/consumer``` certificates identifies the Red Hat account for registration.
  * ```/etc/pki/entitlement``` certificates indicate which subscriptions are attached.
* The ```rct``` command inspects the certificates and the subscription-manager command examines the attached subscriptions on the system.


<a name="14.3"></a>
## 14.3 Explain and Investigate RPM Software Packages

| Content |
| --- |
| [Software Packages and RPM](#format) |
| [Examine RPM Packages](#examine) |
| [Install RPM Packages](#install) |

<a name="format"></a>
* ![rpm_name_structure](https://github.com/Ahmed-Abd-El-gawad/Red-Hat-System-Administration-I-9.0-RH124/blob/main/Chapter%2014.%20Install%20and%20Update%20Software/rpm_name_structure.svg)

  * ***NAME*** is one or more words describing the contents (```coreutils```).
  * ***VERSION*** is the version number of the original software (```8.32```).
  * ***RELEAS***E is the release number of the package based on that version, and is set by the packager, who might not be the original software developer (```31.el9```).
  * ***ARCH*** is the processor architecture the package is compiled to run on. The ```x86_64``` value indicates that this package is built for the 64-bit version of the x86 instruction set (as opposed to ```aarch64``` for 64-bit ARM, and so on).

<a name="examine"></a>
* Use the ```rpm``` command ```-p``` option to get information about a downloaded but uninstalled package file. 
* Retrieve general information about installed packages:
  * ```rpm -qa``` : List all installed packages.
  * ```rpm -qf FILENAME``` : Determine what package provides FILENAME.
    ```console
    [user@host ~]$ rpm -qf /etc/yum.repos.d
    redhat-release-9.1-1.0.el9.x86_64
    ```
* Get information about specific packages:
  * ```rpm -q``` : Lists the currently installed package version.
    ```console
    [user@host ~]$ rpm -q dnf
    dnf-4.10.0-4.el9.noarch
    ```
  * ```rpm -qi``` : Get detailed package information.
  * ```rpm -ql``` : List the files that the package installs.
    ```console
    [user@host ~]$ rpm -ql dnf
    /usr/bin/dnf
    /usr/lib/systemd/system/dnf-makecache.service
    /usr/lib/systemd/system/dnf-makecache.timer
    /usr/share/bash-completion
    /usr/share/bash-completion/completions
    /usr/share/bash-completion/completions/dnf
    ...output omitted...
    ```
  * ```rpm -qc``` : List only the configuration files that the package installs.
    ```console
    [user@host ~]$ rpm -qc openssh-clients
    /etc/ssh/ssh_config
    /etc/ssh/ssh_config.d/50-redhat.conf
    ```
  * ```rpm -qd``` : List only the documentation files that the package installs.
    ```console
    [user@host ~]$ rpm -qd openssh-clients
    /usr/share/man/man1/scp.1.gz
    /usr/share/man/man1/sftp.1.gz
    /usr/share/man/man1/ssh-add.1.gz
    /usr/share/man/man1/ssh-agent.1.gz
    ...output omitted...
    ```
  * ```rpm -q --scripts``` : List the shell scripts that run before or after you install or remove the package.
    ```console
    [user@host ~]$ rpm -q --scripts openssh-server
    preinstall scriptlet (using /bin/sh):
    getent group sshd >/dev/null || groupadd -g 74 -r sshd || :
    getent passwd sshd >/dev/null || \
      useradd -c "Privilege-separated SSH" -u 74 -g sshd \
      -s /sbin/nologin -r -d /usr/share/empty.sshd sshd 2> /dev/null || :
    postinstall scriptlet (using /bin/sh):

    if [ $1 -eq 1 ] && [ -x "/usr/lib/systemd/systemd-update-helper" ]; then
        # Initial installation
        /usr/lib/systemd/systemd-update-helper install-system-units sshd.service sshd.socket || :
    fi
    ...output omitted...
    ```
  * ```rpm -q --changelog``` : List the change log information for the package.
    ```console
    [user@host ~]$ rpm -q --changelog audit
    * Tue Feb 22 2022 Sergio Correia <scorreia@redhat.com> - 3.0.7-101
    - Adjust sample-rules dir permissions
      Resolves: rhbz#2054432 - /usr/share/audit/sample-rules is no longer readable by non-root users

    * Tue Jan 25 2022 Sergio Correia <scorreia@redhat.com> - 3.0.7-100
    - New upstream release, 3.0.7
      Resolves: rhbz#2019929 - capability=unknown-capability(39) in audit messages
    ...output omitted...
    ```
* Query local package files:
  * ```rpm -qlp``` : List the files that the local package installs.
    ```console
    [user@host ~]$ ls -l podman-4.0.0-6.el9.x86_64.rpm
    -rw-r--r--. 1 student student 13755101 Mar 22 11:35 podman-4.0.0-6.el9.x86_64.rpm2637-15.el9.x86_64.rpm
    [user@host ~]$ rpm -qlp podman-4.0.0-6.el9.x86_64.rpm
    /etc/cni/net.d
    /etc/cni/net.d/87-podman-bridge.conflist
    /usr/bin/podman
    ...output omitted...
    ```

<a name="install"></a>
* se the ```rpm``` command to install an RPM package that you downloaded to your local directory.
  ```console
  [root@host ~]# rpm -ivh podman-4.0.0-6.el9.x86_64.rpm
  Verifying...                          ################################# [100%]
  Preparing...                          ################################# [100%]
  Updating / installing...
          podman-2:4.0.0-6              ################################# [100%]
  ```


<a name="14.5"></a>
## 14.5 Install and Update Software Packages with DNF

| Content |
| --- |

<a name=""></a>
* 


<a name="14.7"></a>
## 14.7 Enable DNF Software Repositories

| Content |
| --- |

<a name=""></a>
* 


