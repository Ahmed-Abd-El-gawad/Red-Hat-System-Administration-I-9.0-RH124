# Chapter 9. Control Services and Daemons

| content |
| --- |
| [9.1 Identify Automatically Started System Processes](#9.1) |
| [9.3 Control System Services](#9.3) |


<a name="9.1"></a>
## 9.1 Identify Automatically Started System Processes

| systemctl |
| --- |
| [systemctl](#systemctl) |
| [Service States](#states) |

<a name="systemctl"></a>
* lists and paginates all currently loaded service units.
  ```console
  [root@host ~]# systemctl list-units --type=service
  UNIT                  LOAD    ACTIVE  SUB      DESCRIPTION
  atd.service           loaded  active  running  Job spooling tools
  auditd.service        loaded  active  running  Security Auditing Service
  chronyd.service       loaded  active  running  NTP client/server
  crond.service         loaded  active  running  Command Scheduler
  dbus.service          loaded  active  running  D-Bus System Message Bus
  ...output omitted...
  ```
  * ```--type=service``` option limits the type of ```systemd``` units to service units.
  * **UNIT**
    
    The service unit name.
  * **LOAD**
    
    Whether the systemd daemon properly parsed the unit's configuration and loaded the unit into memory.
  * **ACTIVE**
    
    The high-level activation state of the unit. This information indicates whether the unit started successfully.
  * **SUB**
    
    The low-level activation state of the unit. This information indicates more detailed information about the unit. The information varies based on unit type, state, and how the unit is executed.
  * **DESCRIPTION**
    
    The short description of the unit.
* ```systemctl list-units --all``` option lists all service units regardless of the activation states. Use the ```--state=``` option to filter by the values in the ```LOAD```, ```ACTIVE```, or ```SUB``` fields.
  ```console
  [root@host ~]# systemctl list-units --type=service --all
  UNIT                          LOAD      ACTIVE   SUB     DESCRIPTION
    atd.service                 loaded    active   running Job spooling tools
    auditd.service              loaded    active   running Security Auditing ...
    auth-rpcgss-module.service  loaded    inactive dead    Kernel Module ...
    chronyd.service             loaded    active   running NTP client/server
    cpupower.service            loaded    inactive dead    Configure CPU power ...
    crond.service               loaded    active   running Command Scheduler
    dbus.service                loaded    active   running D-Bus System Message Bus
  ● display-manager.service     not-found inactive dead    display-manager.service
  ...output omitted...
  ```
* The ```systemctl``` command without any arguments lists units that are both loaded and active.
  ```console
  [root@host ~]# systemctl
  UNIT                                 LOAD   ACTIVE SUB       DESCRIPTION
  proc-sys-fs-binfmt_misc.automount    loaded active waiting   Arbitrary...
  sys-devices-....device               loaded active plugged   Virtio network...
  sys-subsystem-net-devices-ens3.deviceloaded active plugged   Virtio network...
  ...output omitted...
  -.mount                              loaded active mounted   Root Mount
  boot.mount                           loaded active mounted   /boot
  ...output omitted...
  systemd-ask-password-plymouth.path   loaded active waiting   Forward Password...
  systemd-ask-password-wall.path       loaded active waiting   Forward Password...
  init.scope                           loaded active running   System and Servi...
  session-1.scope                      loaded active running   Session 1 of...
  atd.service                          loaded active running   Job spooling tools
  auditd.service                       loaded active running   Security Auditing...
  chronyd.service                      loaded active running   NTP client/server
  crond.service                        loaded active running   Command Scheduler
  ...output omitted...
  ```
* The ```systemctl``` command ```list-units``` option displays units that the ```systemd``` service attempts to parse and load into memory. This option does not display services that are installed but not enabled. You can use the ```systemctl``` command ```list-unit-files``` option to see the state of all the installed unit files:
  ```console
  [root@host ~]# systemctl list-unit-files --type=service
  UNIT FILE                         STATE       VENDOR PRESET
  arp-ethers.service                disabled    disabled
  atd.service                       enabled     enabled
  auditd.service                    enabled     enabled
  auth-rpcgss-module.service        static      -
  autovt@.service                   alias       -
  blk-availability.service          disabled    disabled
  ...output omitted...
  ```
  
<a name="states"></a>
* View a unit's status with the ```systemctl status name.type``` command.
  ```console
  [root@host ~]# systemctl status sshd.service
  ● sshd.service - OpenSSH server daemon
       Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
       Active: active (running) since Mon 2022-03-14 05:38:12 EDT; 25min ago
         Docs: man:sshd(8)
               man:sshd_config(5)
     Main PID: 1114 (sshd)
        Tasks: 1 (limit: 35578)
       Memory: 5.2M
          CPU: 64ms
       CGroup: /system.slice/sshd.service
               └─1114 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

  Mar 14 05:38:12 workstation systemd[1]: Starting OpenSSH server daemon...
  Mar 14 05:38:12 workstation sshd[1114]: Server listening on 0.0.0.0 port 22.
  Mar 14 05:38:12 workstation sshd[1114]: Server listening on :: port 22.
  Mar 14 05:38:12 workstation systemd[1]: Started OpenSSH server daemon.
  ...output omitted...
  ```
  * Service Unit Information
    | Field | Description |
    | --- | --- |
    | ```Loaded``` | Whether the service unit is loaded into memory. |
    | ```Active``` | Whether the service unit is running and if so, for how long. |
    | ```Docs``` | Where to find more information about the service. |
    | ```Main PID``` | The main process ID of the service, including the command name. |
    | ```Status``` | More information about the service. |
    | ```Process``` | More information about related processes. |
    | ```CGroup``` | More information about related control groups. |
    
  * Service States in the Output of systemctl
    | Keyword | Description |
    | --- | --- |
    | ```loaded``` | The unit configuration file is processed. |
    | ```active (running)``` | The service is running with continuing processes. |
    | ```active (exited)``` | The service successfully completed a one-time configuration. |
    | ```active (waiting)``` | The service is running but waiting for an event. |
    | ```inactive``` | The service is not running. |
    | ```enable``` | 	The service starts at boot time. |
    | ```disabled``` | The service is not set to start at boot time. |
    | ```static``` | The service cannot be enabled, but an enabled unit might start it automatically. |
* ```console
  [root@host ~]# systemctl is-active sshd.service
  active
  ```
  The command returns the service unit state, which is usually active or inactive.
* ```console
  [root@host ~]# systemctl is-enabled sshd.service
  enabled
  ```
  The command returns whether the service unit is enabled to start at boot time, and is usually enabled or disabled.
* ```console
  [root@host ~]# systemctl is-failed sshd.service
  active
  ```
  The command returns active if the service is properly running, or failed if an error occurred during startup. If the unit was stopped, it returns unknown or inactive.
* To list all the failed units, run the ```systemctl --failed --type=service``` command.

<a name="9.3"></a>
## 9.3 Control System Services

| content |
| --- |
| [status](#status) |
| [start](#start) |
| [stop](#stop) |
| [restart](#restart) |
| [reload](#reload) |
| [reload-or-restart](#reload-or-restart) |
| [list-dependencies](#list-dependencies) |
| [mask](#mask) |
| [unmask](#unmask) |
| [enable and disable](#enable_disable) |
| [Summary](#summary) |

<a name="status"></a>
* Use the ```systemctl status``` command to verify the status of a service, if the service is running or stopped.
  ```console
  [root@host ~]# systemctl status sshd.service
  ● sshd.service - OpenSSH server daemon
       Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
       Active: active (running) since Wed 2022-03-23 11:58:18 EDT; 2min 56s ago
  ...output omitted...
  ```

<a name="start"></a>
* Use the ```systemctl start``` command as the root user (with the sudo command if necessary) to start a sevice.
  ```console
  [root@host ~]# systemctl start sshd
  ```
  ```console
  [root@host ~]# systemctl start sshd.service
  ```
<a name="stop"></a>
* To stop a running service, use the ```systemctl``` command ```stop``` option.
  ```console
  [root@host ~]# systemctl stop sshd.service
  ```
  
<a name="restart"></a>
* When you restart a running service, the service first stops and then starts again. On the service restart, the new process gets a new ID during the startup and thus the process ID changes.
  ```console
  [root@host ~]# systemctl restart sshd.service
  ```
  
<a name="reload"></a>
* Some services can reload their configuration files without requiring a restart, which is called a service reload. Reloading a service does not change the process ID that is associated with various service processes.
  ```console
  [root@host ~]# systemctl reload sshd.service
  ```

<a name="reload-or-restart"></a>
* If you are unsure whether the service has the function to reload the configuration file changes, use the systemctl command ```reload-or-restart``` option. The command reloads the configuration changes if the reloading function is available. Otherwise, the command restarts the service to implement the new configuration changes.
  ```console
  [root@host ~]# systemctl reload-or-restart sshd.service
  ```

<a name="list-dependencies"></a>
* The ```systemctl list-dependencies UNIT``` command displays a hierarchy mapping of dependencies to start the service unit. To list reverse dependencies (units that depend on the specified unit), use the ```--reverse``` option with the command.
  ```console
  [root@host ~]# systemctl list-dependencies sshd.service
  sshd.service
  ● ├─system.slice
  ● ├─sshd-keygen.target
  ● │ ├─sshd-keygen@ecdsa.service
  ● │ ├─sshd-keygen@ed25519.service
  ● │ └─sshd-keygen@rsa.service
  ● └─sysinit.target
  ...output omitted...
  ```

<a name="mask"></a>
* Masking a service prevents an administrator from accidentally starting a service that conflicts with others. Masking creates a link in the configuration directories to the ```/dev/null``` file which prevents the service from starting. To mask a service, use the ```systemctl``` command ```mask``` option.
  ```console
  [root@host ~]# systemctl mask sendmail.service
  Created symlink /etc/systemd/system/sendmail.service → /dev/null.
  ```
  * Check the state of the service by using the ```systemctl list-unit-files``` command.
    ```console
    [root@host ~]# systemctl list-unit-files --type=service
    UNIT FILE                                   STATE
    ...output omitted...
    sendmail.service                            masked
    ...output omitted...
    ```
  * Attempting to start a masked service unit fails with the following output:
    ```console
    [root@host ~]# systemctl start sendmail.service
    Failed to start sendmail.service: Unit sendmail.service is masked.
    ```
    
<a name="unmask"></a>
* Use the ```systemctl unmask``` command to unmask the service unit.
  ```console
  [root@host ~]# systemctl unmask sendmail
  Removed /etc/systemd/system/sendmail.service.
  ```

<a name="enable_disable"></a>
* Creating links in the ```systemd``` configuration directories enables the service to start at boot. You can create or remove these links by using the ```systemctl``` command with the ```enable``` or ```disable``` option.
  ```console
  [root@root ~]# systemctl enable sshd.service
  Created symlink /etc/systemd/system/multi-user.target.wants/sshd.service → /usr/lib/systemd/system/sshd.service.
  ```

* To start the service and enable it to start automatically during boot, you can execute both the ```systemctl start``` and ```systemctl enable``` commands, or use the equivalent ```systemctl enable --now``` command.
  ```conso;e
  [root@root ~]# systemctl enable --now sshd.service
  Created symlink /etc/systemd/system/multi-user.target.wants/sshd.service → /usr/lib/systemd/system/sshd.service.
  ```
  ```console
  [root@host ~]# systemctl disable --now sshd.service
  Removed /etc/systemd/system/multi-user.target.wants/sshd.service.
  ```

* To verify whether the service is enabled or disabled, use the ```systemctl is-enabled``` command.
  ```console
  [root@host ~]# systemctl is-enabled sshd.service
  enabled
  ```
  
<a name="summary"></a>
* Summary
  | Command | Task |
  | --- | --- |
  | ```systemctl status UNIT``` | View detailed information about a unit's state. |
  | ```systemctl stop UNIT``` | Stop a service on a running system. |
  | ```systemctl start UNIT``` | Start a service on a running system. |
  | ```systemctl restart UNIT``` | Restart a service on a running system. |
  | ```systemctl reload UNIT``` | Reload the configuration file of a running service. |
  | ```systemctl mask UNIT``` | Disable a service from being started, both manually and at boot. |
  | ```systemctl unmask UNIT``` | Make a masked service available. |
  | ```systemctl enable UNIT``` | Configure a service to start at boot time. Use the ```--now``` option to also start the service. |
  | ```systemctl disable UNIT``` | Disable a service from starting at boot time. Use the ```--now``` option to also stop the service. |

