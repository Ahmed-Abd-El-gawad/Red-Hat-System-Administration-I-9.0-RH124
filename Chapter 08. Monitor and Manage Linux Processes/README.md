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
* Use the ```bg``` command with the job ID to start running the suspended process.
  ```console
  [user@host ~]$ bg %1
  [1]+ sleep 10000 &
  ```
  

<a name="8.5"></a>
## 8.5 Kill Processes

| content |
| --- |
| [Fundamental process management signals](#8.5.1) |
| [kill](#kill) |
| [killall](#killall) |
| [pkill](#pkill) |
| [w](#w) |
| [pgrep](#pgrep) |
| [pstree](#pstree) |


<a name="8.5.1"></a>
* Fundamental process management signals
  
  | Signal | Name | Definition |
  | --- | --- | --- |
  | 1 | **HUP** | ```Hangup``` : Reports termination of the controlling process of a terminal. Also requests process re-initialization (configuration reload) without termination. |
  | 2 | **INT** | ```Keyboard interrupt``` : Causes program termination. It can be blocked or handled. Sent by pressing the INTR (Interrupt) key sequence (**Ctrl+c**). |
  | 3 | **QUIT** | ```Keyboard quit``` : Similar to SIGINT; adds a process dump at termination. Sent by pressing the QUIT key sequence (kbd:[Ctrl+\]). |
  | 9 | **KILL** | ```Kill```, ```unblockable``` : Causes abrupt program termination. It cannot be blocked, ignored, or handled; consistently fatal. |
  | 15 default | **TERM** | ```Terminate``` : Causes program termination. Unlike SIGKILL, it can be blocked, ignored, or handled. The "clean" way to ask a program to terminate; it allows the program to complete essential operations and self-cleanup before termination. |
  | 18 | **CONT** | ```Continue``` : Sent to a process to resume if stopped. It cannot be blocked. Even if handled, it always resumes the process. |
  | 19 | **STOP** | ```Stop```, ```unblockable``` : Suspends the process. It cannot be blocked or handled. |
  | 20 | **TSTP** | ```Keyboard stop``` : Unlike SIGSTOP, it can be blocked, ignored, or handled. Sent by pressing the suspend key sequence (**Ctrl+z**). |

<a name="kill"></a>
* The ```kill``` command uses a PID number to send a signal to a process. Despite its name, you can use the ```kill``` command to send any signal, not just those signals for terminating programs. You can use the ```kill``` command ```-l``` option to list the names and numbers of all available signals.
  ```console
  [user@host ~]$ kill -l
   1) SIGHUP      2) SIGINT      3) SIGQUIT     4) SIGILL      5) SIGTRAP
   6) SIGABRT     7) SIGBUS      8) SIGFPE      9) SIGKILL    10) SIGUSR1
  11) SIGSEGV    12) SIGUSR2    13) SIGPIPE    14) SIGALRM    15) SIGTERM
  16) SIGSTKFLT  17) SIGCHLD    18) SIGCONT    19) SIGSTOP    20) SIGTSTP
  ...output omitted...
  ```
  ```console
  [user@host ~]$ ps aux | grep job
  5194 0.0 0.1 222448 2980 pts/1 S  16:39 0:00 /bin/bash /home/user/bin/control job1
  5199 0.0 0.1 222448 3132 pts/1 S  16:39 0:00 /bin/bash /home/user/bin/control job2
  5205 0.0 0.1 222448 3124 pts/1 S  16:39 0:00 /bin/bash /home/user/bin/control job3
  5430 0.0 0.0 221860 1096 pts/1 S+ 16:41 0:00 grep --color=auto job
  [user@host ~]$ kill 5194
  [user@host ~]$ ps aux | grep job
  user   5199  0.0  0.1 222448  3132 pts/1    S    16:39   0:00 /bin/bash /home/user/bin/control job2
  user   5205  0.0  0.1 222448  3124 pts/1    S    16:39   0:00 /bin/bash /home/user/bin/control job3
  user   5783  0.0  0.0 221860   964 pts/1    S+   16:43   0:00 grep --color=auto job
  [1]   Terminated              control job1
  [user@host ~]$ kill -9 5199
  [user@host ~]$ ps aux | grep job
  user   5205  0.0  0.1 222448  3124 pts/1    S    16:39   0:00 /bin/bash /home/user/bin/control job3
  user   5930  0.0  0.0 221860  1048 pts/1    S+   16:44   0:00 grep --color=auto job
  [2]-  Killed                  control job2
  [user@host ~]$ kill -SIGTERM 5205
  user   5986  0.0  0.0 221860  1048 pts/1  S+  16:45  0:00 grep --color=auto job
  [3]+  Terminated              control job3
  ```
  
<a name="killall"></a>
* The ```killall``` command can signal multiple processes, based on their command name.
  ```console
  [user@host ~]$ ps aux | grep job
  5194  0.0  0.1 222448  2980 pts/1    S    16:39   0:00 /bin/bash /home/user/bin/control job1
  5199  0.0  0.1 222448  3132 pts/1    S    16:39   0:00 /bin/bash /home/user/bin/control job2
  5205  0.0  0.1 222448  3124 pts/1    S    16:39   0:00 /bin/bash /home/user/bin/control job3
  5430  0.0  0.0 221860  1096 pts/1    S+   16:41   0:00 grep --color=auto job
  [user@host ~]$ killall control
  [1]   Terminated              control job1
  [2]-  Terminated              control job2
  [3]+  Terminated              control job3
  [user@host ~]$
  ```
  
<a name="pkill"></a>
* Use the ```pkill``` command to signal one or more processes that match selection criteria. Selection criteria can be a command name, a process owned by a specific user, or all system-wide processes. The ```pkill``` command includes advanced selection criteria:
  * **Command** : Processes with a pattern-matched command name.
  * **UID** : Processes owned by a Linux user account, effective or real.
  * **GID** : Processes owned by a Linux group account, effective or real.
  * **Parent** : Child processes of a specific parent process.
  * **Terminal** : Processes that run on a specific controlling terminal.
  ```console
  [user@host ~]$ ps aux | grep pkill
  user   5992  0.0  0.1 222448  3040 pts/1    S    16:59   0:00 /bin/bash /home/user/bin/control pkill1
  user   5996  0.0  0.1 222448  3048 pts/1    S    16:59   0:00 /bin/bash /home/user/bin/control pkill2
  user   6004  0.0  0.1 222448  3048 pts/1    S    16:59   0:00 /bin/bash /home/user/bin/control pkill3
  [user@host ~]$ pkill control
  [1]   Terminated              control pkill1
  [2]-  Terminated              control pkill2
  [user@host ~]$ ps aux | grep pkill
  user   6219  0.0  0.0 221860  1052 pts/1    S+   17:00   0:00 grep --color=auto pkill
  [3]+  Terminated              control pkill3
  [user@host ~]$ ps aux | grep test
  user   6281  0.0  0.1 222448  3012 pts/1    S    17:04   0:00 /bin/bash /home/user/bin/control test1
  user   6285  0.0  0.1 222448  3128 pts/1    S    17:04   0:00 /bin/bash /home/user/bin/control test2
  user   6292  0.0  0.1 222448  3064 pts/1    S    17:04   0:00 /bin/bash /home/user/bin/control test3
  user   6318  0.0  0.0 221860  1080 pts/1    S+   17:04   0:00 grep --color=auto test
  [user@host ~]$ pkill -U user
  [user@host ~]$ ps aux | grep test
  user   6870  0.0  0.0 221860  1048 pts/0    S+   17:07   0:00 grep --color=auto test
  ```
  
<a name="w"></a>
* Use the ```w``` command to list user logins and currently running processes. Note the ```TTY``` and ```FROM``` columns to determine the closing sessions.
  ```console
  [user@host ~]$ w
   12:43:06 up 27 min,  5 users,  load average: 0.03, 0.17, 0.66
  USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
  root     tty2                      12:26   14:58   0.04s  0.04s -bash
  bob      tty3                      12:28   14:42   0.02s  0.02s -bash
  user     pts/1    desktop.example.com 12:41    2.00s  0.03s  0.03s w
  [user@host ~]$
  ```
  
<a name="pgrep"></a>
* use the ```pgrep``` command to identify the PID numbers to kill. This command operates similarly to the ```pkill``` command, including the same options, except that the ```pgrep``` command lists processes rather than killing them.
  ```console
  [root@host ~]# pgrep -l -u bob
  6964 bash
  6998 sleep
  6999 sleep
  7000 sleep
  [root@host ~]# pkill -SIGKILL -u bob
  [root@host ~]# pgrep -l -u bob
  ```
  
<a name="pstree"></a>
* Use the ```pstree``` command to view a process tree for the system or a single user. Use the parent process's PID to kill all children that they created. The parent ```Bas```h login shell survives this time because the signal is directed only at its child processes.
  ```console
  [root@host ~]# pstree -p bob
  bash(8391)─┬─sleep(8425)
             ├─sleep(8426)
             └─sleep(8427)
  [root@host ~]# pkill -P 8391
  [root@host ~]# pgrep -l -u bob
  bash(8391)
  [root@host ~]# pkill -SIGKILL -P 8391
  [root@host ~]# pgrep -l -u bob
  bash(8391)
  ```

<a name="8.7"></a>
## 8.7 Monitor Process Activity

| content |
| --- |
| [uptime](#uptime) |
| [lscpu](#lscpu) |
| [top](#top) |


<a name="uptime"></a>
* ```uptime``` command is one way to display the current load average. It prints the current time, how long the machine has been up, how many user sessions are running, and the current load average.
  ```console
  [user@host ~]$ uptime
   15:29:03 up 14 min,  2 users,  load average: 2.92, 4.48, 5.20
   ```
  * The three values for the load average represent the load over the last ***1***, ***5***, and ***15*** minutes. It indicates whether the system load appears to be increasing or decreasing.

<a name="lscpu"></a>
* Use the ```lscpu``` command to determine the number of CPUs that are present on a system.
  ```console
  [user@host ~]$ lscpu
  Architecture:        x86_64
  CPU op-mode(s):      32-bit, 64-bit
  Byte Order:          Little Endian
  CPU(s):              4
  On-line CPU(s) list: 0-3
  Thread(s) per core:  2
  Core(s) per socket:  2
  Socket(s):           1
  NUMA node(s):        1
  ...output omitted...
  ```
  
  ```
  # From lscpu, the system has four logical CPUs, so divide by 4:
  #                               load average: 2.92, 4.48, 5.20
  #           divide by number of logical CPUs:    4     4     4
  #                                             ----  ----  ----
  #                       per-CPU load average: 0.73  1.12  1.30
  #
  # This system's load average appears to be decreasing.
  # With a load average of 2.92 on four CPUs, all CPUs were in use ~73% of the time.
  # During the last 5 minutes, the system was overloaded by ~12%.
  # During the last 15 minutes, the system was overloaded by ~30%.
  ```
  
<a name="top"></a>
* The default top output columns are as follows:
  * The process ID (PID).
  * The process owner user name (USER).
  * Virtual memory (<sub>VIRT</sub>) is all the memory that the process uses, including the resident set, shared libraries, and any mapped or swapped memory pages. (labeled ```VSZ``` in the ```ps``` command.)
  * Resident memory <sub>RES</sub> is the physical memory that the process uses, including any resident, shared objects. (labeled ```RSS``` in the ```ps``` command.)
  * Process state (S) can be one of the following states:
    * D = Uninterruptible Sleeping
    * R = Running or Runnable
    * S = Sleeping
    * T = Stopped or Traced
    * Z = Zombie
  * CPU time (<sub>TIME</sub>) is the total processing time since the process started. It can be toggled to include a cumulative time of all previous children.
  * The process command name (COMMAND).

* Fundamental Keystrokes in top Command
  
  | Key | Purpose |
  | --- | --- |
  | ? or **h** | Help for interactive keystrokes. |
  | **l**, **t**, **m** | Toggles for load, threads, and memory header lines. |
  | **1** | Toggle for individual CPUs or a summary for all CPUs in the header. |
  | **s** | Change the refresh (screen) rate, in decimal seconds (such as 0.5, 1, 5). |
  | **b** | Toggle reverse highlighting for <sub>Running</sub> processes; default is bold only. |
  | **Shift+b** | Enables bold use in display, in the header, and for Running processes. |
  | **Shift+h** | Toggle threads; show process summary or individual threads. |
  | **u**, **Shift+u** | Filter for any user name (effective, real). |
  | Shift+m** | Sort process listing by memory usage, in descending order. |
  | **Shift+p** | Sort process listing by processor utilization, in descending order. |
  | **k** | Kill a process. When prompted, enter PID, and then <sub>signal</sub>. |
  | **r** | Renice a process. When prompted, enter PID, and then <sub>nice_value.</sub> |
  | **Shift+w** | Write (save) the current display configuration for use at the next top restart. |
  | **q** | Quit. |
  | **f** | Manage the columns by enabling or disabling fields. You can also set the sort field for top. |

  * The s, k, and r keystrokes are not available when the top command is started in a secure mode.
```console
[student@servera ~]$ top
top - 12:13:03 up 11 days, 58 min,  3 users,  load average: 0.00, 0.00, 0.00
Tasks: 113 total,   2 running, 111 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.2 us,  0.0 sy,  0.0 ni, 99.8 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   1829.4 total,   1377.3 free,    193.9 used,    258.2 buff/cache
MiB Swap:   1024.0 total,   1024.0 free,      0.0 used.   1476.1 avail Mem

PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
5861 root      20   0       0      0      0 I   0.3   0.0   0:00.71 kworker/1:3-events
6068 student   20   0  273564   4300   3688 R   0.3   0.2   0:00.01 top
   1 root      20   0  178680  13424   8924 S   0.0   0.7   0:04.03 systemd
   2 root      20   0       0      0      0 S   0.0   0.0   0:00.03 kthreadd
   3 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp
...output omitted...
```
