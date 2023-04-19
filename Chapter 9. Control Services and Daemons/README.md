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

<a name="9.3"></a>
## 9.3 Control System Services

* 

