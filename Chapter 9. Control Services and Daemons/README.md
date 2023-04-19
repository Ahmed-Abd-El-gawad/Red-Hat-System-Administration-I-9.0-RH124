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

<a name="9.3"></a>
## 9.3 Control System Services

* 

