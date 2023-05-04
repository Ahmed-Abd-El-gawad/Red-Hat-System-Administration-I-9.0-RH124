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
| [Manage Software Packages with DNF](#manage) |
| [Find Software with DNF](#find) |
| [Install and Remove Software with DNF](#remove) |
| [View Transaction History](#hist) |
| [Summary of DNF Commands](#summary) |

<a name="manage"></a>
* DNF (Dandified YUM) replaced YUM as the package manager in Red Hat Enterprise Linux 9. DNF commands are functionally the same as YUM commands. For compatibility, YUM commands still exist as symbolic links to DNF:
  ```console
  [user@host ~]$ ls -l /bin/ | grep yum | awk '{print $9 " " $10 " " $11}'
  yum -> dnf-3
  yum-builddep -> /usr/libexec/dnf-utils
  yum-config-manager -> /usr/libexec/dnf-utils
  yum-debug-dump -> /usr/libexec/dnf-utils
  yum-debug-restore -> /usr/libexec/dnf-utils
  yumdownloader -> /usr/libexec/dnf-utils
  yum-groups-manager -> /usr/libexec/dnf-utils
  ```
* The low-level ```rpm``` command can be used to install packages, but it is not designed to work with package repositories or to resolve dependencies from multiple sources automatically.
* DNF improves RPM-based software installation and updates. With the ```dnf``` command, you can install, update, remove, and get information about software packages and their dependencies. You can get a history of transactions and work with multiple Red Hat and third-party software repositories.

<a name="find"></a>
* The ```dnf help``` command displays usage information. The ```dnf list``` command displays installed and available packages.
  ```console
  [user@host ~]$ dnf list 'http*'
  Available Packages
  http-parser.i686               2.9.4-6.el9    rhel-9.0-for-x86_64-appstream-rpms
  http-parser.x86_64             2.9.4-6.el9    rhel-9.0-for-x86_64-appstream-rpms
  httpcomponents-client.noarch   4.5.13-2.el9   rhel-9.0-for-x86_64-appstream-rpms
  httpcomponents-core.noarch     4.4.13-6.el9   rhel-9.0-for-x86_64-appstream-rpms
  httpd.x86_64                   2.4.51-5.el9   rhel-9.0-for-x86_64-appstream-rpms
  httpd-devel.x86_64             2.4.51-5.el9   rhel-9.0-for-x86_64-appstream-rpms
  httpd-filesystem.noarch        2.4.51-5.el9   rhel-9.0-for-x86_64-appstream-rpms
  httpd-manual.noarch            2.4.51-5.el9   rhel-9.0-for-x86_64-appstream-rpms
  httpd-tools.x86_64             2.4.51-5.el9   rhel-9.0-for-x86_64-appstream-rpms
  ```
* The ```dnf search KEYWORD``` command lists packages by keywords found in the name and summary fields only. To search for packages with "web server" in their name, summary, and description fields, use ```search all```:
  ```console
  [user@host ~]$ dnf search all 'web server'
  ================== Summary & Description Matched: web server ===================
  nginx.x86_64 : A high performance web server and reverse proxy server
  pcp-pmda-weblog.x86_64 : Performance Co-Pilot (PCP) metrics from web server logs
  ========================= Summary Matched: web server ==========================
  libcurl.x86_64 : A library for getting files from web servers
  libcurl.i686 : A library for getting files from web servers
  ======================= Description Matched: web server ========================
  freeradius.x86_64 : High-performance and highly configurable free RADIUS server
  git-instaweb.noarch : Repository browser in gitweb
  http-parser.i686 : HTTP request/response parser for C
  http-parser.x86_64 : HTTP request/response parser for C
  httpd.x86_64 : Apache HTTP Server
  mod_auth_openidc.x86_64 : OpenID Connect auth module for Apache HTTP Server
  mod_jk.x86_64 : Tomcat mod_jk connector for Apache
  mod_security.x86_64 : Security module for the Apache HTTP Server
  varnish.i686 : High-performance HTTP accelerator
  varnish.x86_64 : High-performance HTTP accelerator
  ...output omitted...
  ```
* The ```dnf info PACKAGENAME``` command returns detailed information about a package, including the needed disk space for installation. For example, the following command retrieves information about the ```httpd``` package:
  ```console
  [user@host ~]$ dnf info httpd
  Available Packages
  Name         : httpd
  Version      : 2.4.51
  Release      : 5.el9
  Architecture : x86_64
  Size         : 1.5 M
  Source       : httpd-2.4.51-5.el9.src.rpm
  Repository   : rhel-9.0-for-x86_64-appstream-rpms
  Summary      : Apache HTTP Server
  URL          : https://httpd.apache.org/
  License      : ASL 2.0
  Description  : The Apache HTTP Server is a powerful, efficient, and extensible
               : web server.
  ```
* The ```dnf provides PATHNAME``` command displays packages that match the specified path name (the path names often include wildcard characters). For example, the following command finds packages that provide the ```/var/www/html``` directory:
  ```console
  [user@host ~]$ dnf provides /var/www/html
  httpd-filesystem-2.4.51-5.el9.noarch : The basic directory layout for the Apache HTTP Server
  Repo        : rhel-9.0-for-x86_64-appstream-rpms
  Matched from:
  Filename    : /var/www/html
  ```

<a name="remove"></a>
* In Red Hat Enterprise Linux 9, the ```dnf``` command can install two kinds of package groups. 
  * Regular groups are collections of packages. 
  * Environment groups are collections of regular groups. 
  * The packages or groups that these collections provide might be listed as mandatory (they must be installed if the group is installed), default (normally installed if the group is installed), or optional (not installed when the group is installed, unless specifically requested).
  * Similar to the ```dnf list``` command, the ```dnf group list``` command shows the names of installed and available groups.
    ```
    [user@host ~]$ dnf group list
    Available Environment Groups:
       Server with GUI
       Server
       Minimal Install
    ...output omitted...
    Available Groups:
       Legacy UNIX Compatibility
       Console Internet Tools
       Container Management
    ...output omitted...
    ```
  * Some groups are normally installed through environment groups and are hidden by default. List these hidden groups with the ```dnf group list hidden``` command.
* The ```dnf group info``` command displays information about a group. It includes a list of mandatory, default, and optional package names.
  ```console
  [user@host ~]$ dnf group info "RPM Development Tools"
  Group: RPM Development Tools
   Description: Tools used for building RPMs, such as rpmbuild.
   Mandatory Packages:
     redhat-rpm-config
     rpm-build
   Default Packages:
     rpmdevtools
   Optional Packages:
     rpmlint
  ```
* The ```dnf group install``` command installs a group that installs its mandatory and default packages and their dependent packages.
  ```console
  [root@host ~]# dnf group install "RPM Development Tools"
  ...output omitted...
  Installing Groups:
   RPM Development Tools

  Transaction Summary
  ================================================================================
  Install  19 Packages

  Total download size: 4.7 M
  Installed size: 15 M
  Is this ok [y/N]: y
  ...output omitted...
  ```

<a name="hist"></a>
* All installation and removal transactions are logged in the ```/var/log/dnf.rpm.log``` file.
  ```console
  [user@host ~]$ tail -5 /var/log/dnf.rpm.log
  2022-03-23T16:46:43-0400 SUBDEBUG Installed: python-srpm-macros-3.9-52.el9.noarch
  2022-03-23T16:46:43-0400 SUBDEBUG Installed: redhat-rpm-config-194-1.el9.noarch
  2022-03-23T16:46:44-0400 SUBDEBUG Installed: elfutils-0.186-1.el9.x86_64
  2022-03-23T16:46:44-0400 SUBDEBUG Installed: rpm-build-4.16.1.3-11.el9.x86_64
  2022-03-23T16:46:44-0400 SUBDEBUG Installed: rpmdevtools-9.5-1.el9.noarch
  ```
  * The ```dnf history``` command displays a summary of installation and removal transactions.
  ```console
  [root@host ~]# dnf history
  ID     | Command line              | Date and time    | Action(s)      | Altered
  --------------------------------------------------------------------------------
       7 | group install RPM Develop | 2022-03-23 16:46 | Install        |   20
       6 | install httpd             | 2022-03-23 16:21 | Install        |   10 EE
       5 | history undo 4            | 2022-03-23 15:04 | Removed        |   20
       4 | group install RPM Develop | 2022-03-23 15:03 | Install        |   20
       3 |                           | 2022-03-04 03:36 | Install        |    5
       2 |                           | 2022-03-04 03:33 | Install        |  767 EE
       1 | -y install patch ansible- | 2022-03-04 03:31 | Install        |   80
  ```
* The ```dnf history undo``` command reverses a transaction.
  ```console
  [root@host ~]# dnf history undo 6
  ...output omitted...
  Removing:
   apr-util-openssl x86_64 1.6.1-20.el9 @rhel-9.0-for-x86_64-appstream-rpms  24 k
   httpd            x86_64 2.4.51-5.el9 @rhel-9.0-for-x86_64-appstream-rpms 4.7 M
  ...output omitted...
  ```

<a name="summary"></a>
* Summary
 
    | Task: | Command: |
    | --- | --- |
    | List installed and available packages by name | ```dnf list [NAME-PATTERN]``` |
    | List installed and available groups | ```dnf group list``` |
    | Search for a package by keyword | ```dnf search KEYWORD``` |
    | Show details of a package | ```dnf info PACKAGENAME``` |
    | Install a package | ```dnf install PACKAGENAME``` |
    | Install a package group | ```dnf group install GROUPNAME``` |
    | Update all packages | ```dnf update``` |
    | Remove a package | ```dnf remove PACKAGENAME``` |
    | Display transaction history | ```dnf history``` |



<a name="14.7"></a>
## 14.7 Enable DNF Software Repositories

| Content |
| --- |

<a name=""></a>
* 


