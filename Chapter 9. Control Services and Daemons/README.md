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


<a name="9.3"></a>
## 9.3 Control System Services

* 

