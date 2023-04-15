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
  


<a name="8.3"></a>
## 8.3 Control Jobs

* 


<a name="8.5"></a>
## 8.5 Kill Processes

* 


<a name="8.7"></a>
## 8.7 Monitor Process Activity

* 


