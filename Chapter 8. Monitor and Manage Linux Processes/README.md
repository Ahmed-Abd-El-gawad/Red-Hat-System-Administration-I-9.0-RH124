# Chapter 8. Monitor and Manage Linux Processes

| content |
| --- |
| [8.1 Process States and Lifecycle](#8.1) |
| [8.3 Control Jobs](#8.3) |
| [8.5 Kill Processes](#8.5) |
| [8.7 Monitor Process Activity](#8.7) |


<a name="8.1"></a>
## 8.1 Process States and Lifecycle

* 
  ![process-states](https://github.com/Ahmed-Abd-El-gawad/Red-Hat-System-Administration-I-9.0-RH124/blob/main/Chapter%208.%20Monitor%20and%20Manage%20Linux%20Processes/process-states.svg)
  | Name| Flag | Kernel-defined state name and description |
  | --- | --- | --- |
  | Running | R | **TASK_RUNNING**: The process is either executing on a CPU or waiting to run. The process can be executing user routines or kernel routines (system calls), or be queued and ready when in the *Running* (or *Runnable*) state. |
  | Sleeping | S | **TASK_INTERRUPTIBLE**: The process is waiting for some condition: a hardware request, system resource access, or signal. When an event or signal satisfies the condition, the process returns to Running. |
  |  | D | **TASK_UNINTERRUPTIBLE**: This process is also sleeping, but unlike ```S``` state, does not respond to signals. Used only when process interruption might cause an unpredictable device state. |
  |  | K | **TASK_KILLABLE**: Identical to the uninterruptible ```D``` state, but modified to allow a waiting task to respond to the signal that it should be killed (exit completely). Utilities frequently display *Killable* processes as ```D``` state. |
  |  | I | **TASK_REPORT_IDLE**: A subset of state ```D```. The kernel does not count these processes when calculating load average. Used for kernel threads. Flags **TASK_UNINTERRUPTIBLE** and **TASK_NOLOAD** are set. Similar to **TASK_KILLABLE**, also a subset of state ```D```. It accepts fatal signals. |
  | Stopped | T | **TASK_STOPPED**: The process is stopped (suspended), usually by being signaled by a user or another process. The process can be continued (resumed) by another signal to return to running. |
  |  | T | **TASK_TRACED**: A process that is being debugged is also temporarily stopped and shares the same ```T``` state flag. |
  | Zombie | Z | **EXIT_ZOMBIE**: A child process signals to its parent as it exits. All resources except for the process identity (PID) are released.
  |  | X | **EXIT_DEAD**: When the parent cleans up (reaps) the remaining child process structure, the process is now released completely. This state cannot be observed in process-listing utilities. |
  
* When troubleshooting a system, it is important to understand how the kernel communicates with processes and how processes communicate with each other. The system assigns a state to every new process. The ```S``` column of the ```top``` command or the ```STAT``` column of the ```ps``` command shows the state of each process. On a single CPU system, only one process can run at a time. It is possible to see several processes with an R state. However, not all processes are running consecutively; some of them are in waiting status.
  ```console
  [user@host ~]$ top
  PID USER  PR  NI    VIRT    RES    SHR S  %CPU  %MEM   TIME+    COMMAND
  2259 root 20   0  270856  40316   8332 S   0.3   0.7   0:00.25  sssd_kcm
     1 root 20   0  171820  16140  10356 S   0.0   0.3   0:01.62  systemd
     2 root 20   0       0      0      0 S   0.0   0.0   0:00.00  kthreadd
  ...output omitted...
  ```
  ```console
  [user@host ~]$ ps aux
  USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
  ...output omitted...
  root         2  0.0  0.0      0     0 ?        S    11:57   0:00 [kthreadd]
  student   3448  0.0  0.2 266904  3836 pts/0    R+   18:07   0:00 ps aux
  ...output omitted...
  ```
  
* ```ps -aux``` command is not the same as the ```ps aux``` command.
* ps command aux option displays all processes including processes without a controlling terminal. 
  ```console
  [user@host ~]$ ps aux
  USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
  root           1  0.1  0.2 171820 16140 ?        Ss   16:47   0:01 /usr/lib/systemd/systemd ...
  root           2  0.0  0.0      0     0 ?        S    16:47   0:00 [kthreadd]
  root           3  0.0  0.0      0     0 ?        I<   16:47   0:00 [rcu_gp]
  root           4  0.0  0.0      0     0 ?        I<   16:47   0:00 [rcu_par_gp]
  root           6  0.0  0.0      0     0 ?        I<   16:47   0:00 [kworker/0:0H-events_highpri]
  ...output omitted...
  ```
* A long listing (```lax``` options) provides more detail, and displays faster by avoiding username lookups.
  ```console
  [user@host ~]$ ps lax
  F   UID     PID    PPID PRI  NI    VSZ   RSS WCHAN  STAT TTY  TIME COMMAND
  4     0       1       0  20   0 171820 16140 -      Ss   ?    0:01 /usr/lib/systemd/systemd ...
  1     0       2       0  20   0      0     0 -      S    ?    0:00 [kthreadd]
  1     0       3       2   0 -20      0     0 -      I<   ?    0:00 [rcu_gp]
  1     0       4       2   0 -20      0     0 -      I<   ?    0:00 [rcu_par_gp]
  1     0       6       2   0 -20      0     0 -      I<   ?    0:00 [kworker/0:0H-events_highpri]
  ...output omitted...
  ```
* ```-ef``` options to display all processes.
  ```console
  [user@host ~]$ ps -ef
  UID        PID  PPID  C STIME TTY          TIME CMD
  root           1       0  0 16:47 ?        00:00:01 /usr/lib/systemd/systemd ...
  root           2       0  0 16:47 ?        00:00:00 [kthreadd]
  root           3       2  0 16:47 ?        00:00:00 [rcu_gp]
  root           4       2  0 16:47 ?        00:00:00 [rcu_par_gp]
  root           6       2  0 16:47 ?        00:00:00 [kworker/0:0H-events_highpri]
  ...output omitted...
  ```
* You can use the ```ps``` command ```--forest``` option to display the processes in a tree format so you can view relationships between parent and child processes.
  

<a name="8.3"></a>
## 8.3 Control Jobs

* 


<a name="8.5"></a>
## 8.5 Kill Processes

* Run Jobs in the Background
  ```console
  [user@host ~]$ sleep 10000 &
  [1] 5947
  [user@host ~]$
  ```
* Use the ```jobs``` command to display the list of jobs for the shell's session.
  ```console
  [user@host ~]$ jobs
  [1]+ Running    sleep 10000 &
  [user@host ~]$
  ```
* Use the ```fg``` command to bring a background job to the foreground. Use the the (```%jobNumber```) format to specify the process to foreground.
  ```console
  [user@host ~]$ fg %1
  sleep 10000
  ```
* To send a foreground process to the background, press the keyboard-generated suspend request (**Ctrl+z**) in the terminal. The job is placed in the background and suspended.
  ```console
  [user@host ~]$ sleep 10000
  ^Z
  [1]+  Stopped                 sleep 10000
  [user@host ~]$
  ```
* The ```ps j``` command displays information about jobs. Use the ```ps j``` command to find process and session information.
  * The **PID** is the unique process **ID** of the process.
  * The **PPID** is the PID of the *parent process* of this process, the process that started (forked) it.
  * The **PGID** is the PID of the *process group leader*, normally the first process in the job's pipeline.
  * The **SID** is the PID of the *session leader*, which (for a job) is normally the interactive shell that is running on its controlling terminal.
  In the next example, the ```sleep``` command is currently suspended and the process state is ```T```.
  ```console
  [user@host ~]$ ps j
   PPID   PID  PGID   SID TTY      TPGID STAT   UID   TIME COMMAND
   2764  2768  2768  2768 pts/0     6377 Ss    1000   0:00 /bin/bash
   2768  5947  5947  2768 pts/0     6377 T     1000   0:00 sleep 10000
   2768  6377  6377  2768 pts/0     6377 R+    1000   0:00 ps j
   ```


<a name="8.7"></a>
## 8.7 Monitor Process Activity

* 


