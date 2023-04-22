# Chapter 11. Analyze and Store Logs

| Content |
| --- |
| [11.1 Describe System Log Architecture](#11.1) |
| [11.3 Review Syslog Files](#11.3) |
| [11.5 Review System Journal Entries](#11.5) |
| [11.7 Preserve the System Journal](#11.7) |
| [11.9 Maintain Accurate Time](#11.9) |


<a name="11.1"></a>
## 11.1 Describe System Log Architecture

| Content |
| --- |
| [systemd-journald](#systemd-journald) |
| [rsyslog](#rsyslog) |

<a name="systemd-journald"></a>
* The ```systemd-journald``` service is at the heart of the operating system event logging architecture. The ```systemd-journald``` service collects event messages from many sources:
  * System kernel
  * Output from the early stages of the boot process
  * Standard output and standard error from daemons
  * Syslog events

<a name="rsyslog"></a>
* The ```rsyslog``` service reads syslog messages that the ```systemd-journald``` service receives from the journal as they arrive. The ```rsyslog``` service then processes the syslog events, and records them to its log files or forwards them to other services according to its own configuration.
* The ```rsyslog``` service sorts and writes syslog messages to the log files that do persist across reboots in the ```/var/log``` directory.
  | Log file | Type of stored messages |
  | --- | --- |
  | ```/var/log/messages``` | Most syslog messages are logged here. Exceptions include messages about authentication and email processing, scheduled job execution, and purely debugging-related messages. |
  | ```/var/log/secure``` | Syslog messages about security and authentication events. |
  | ```/var/log/maillog``` | Syslog messages about the mail server. |
  | ```/var/log/cron``` | Syslog messages about scheduled job execution. |
  | ```/var/log/boot.log``` | Non-syslog console messages about system startup. |


<a name="11.3"></a>
## 11.3 Review Syslog Files

| Content |
| --- |
| [Syslog Facilities](#facilities) |
| [Syslog Priorities](#priorities) |
| [Log File Rotation ```logrotate```](#logrotate) |
| [Analyze a Syslog Entry](#analyze) |
| [Send Syslog Messages Manually](#manually) |

* Many programs use the syslog protocol to log events to the system. Each log message is categorized by ***facility*** (which subsystem produces the message) and ***priority*** (the message's severity).
  <a name="facilities"></a>
  * Overview of Syslog Facilities

     | Code | Facility | Facility description |
     | --- | --- | --- |
     | 0 | kern | Kernel messages |
     | 1 | user | User-level messages |
     | 2 | mail | Mail system messages |
     | 3 | daemon | System daemons messages |
     | 4 | auth | Authentication and security messages |
     | 5 | syslog | Internal syslog messages |
     | 6 | lpr | Printer messages |
     | 7 | news | Network news messages |
     | 8 | uucp | UUCP protocol messages |
     | 9 | cron | Clock daemon messages |
     | 10 | authpriv | Non-system authorization messages |
     | 11 | ftp | FTP protocol messages |
     | 16-23 | local0 | to local7	Custom local messages |

  <a name="priorities"></a>
  * Overview of Syslog Priorities

     | Code | Priority | Priority description |
     | --- | --- | --- |
     | 0 | emerg | System is unusable |
     | 1 | alert | Action must be taken immediately |
     | 2 | crit | Critical condition |
     | 3 | err | Non-critical error condition |
     | 4 | warning | Warning condition |
     | 5 | notice | Normal but significant event |
     | 6 | info | Informational event |
     | 7 | debug | Debugging-level message |

  * Sample Rules of the ```rsyslog``` Service
    ```bash
    #### RULES ####

    # Log all kernel messages to the console.
    # Logging much else clutters up the screen.
    #kern.*                                                 /dev/console

    # Log anything (except mail) of level info or higher.
    # Don't log private authentication messages!
    *.info;mail.none;authpriv.none;cron.none                /var/log/messages

    # The authpriv file has restricted access.
    authpriv.*                                              /var/log/secure

    # Log all the mail messages in one place.
    mail.*                                                  -/var/log/maillog


    # Log cron stuff
    cron.*                                                  /var/log/cron

    # Everybody gets emergency messages
    .emerg                                                 :omusrmsg:

    # Save news errors of level crit and higher in a special file.
    uucp,news.crit                                          /var/log/spooler

    # Save boot messages also to boot.log
    local7.*                                                /var/log/boot.log
    ```
   
<a name="logrotate"></a>
* The ```logrotate``` command rotates log files to prevent them from taking too much space in the ```/var/log``` directory. When a log file is rotated, it is renamed with an extension that indicates the rotation date. For example, the old ```/var/log/messages``` file is renamed to the ```/var/log/messages-20220320``` file when it is rotated on 2022-03-20. After the old log file rotates, it creates a log file and notifies the service that wrote the log file.
* After rotations during typically four weeks, the oldest log file is discarded to free disk space. A scheduled job runs the ```logrotate``` command daily to see the rotation requirement of any log files. Most log files rotate weekly; the ```logrotate``` command rotates some log files faster, or more slowly, or when they reach a specific size.

<a name="analyze"></a>
* Log messages start with the oldest message at the start and the newest message at the end of the log file. The ```rsyslog``` service uses a standard format for recording entries in log files. The following example explains the anatomy of a log message in the ```/var/log/secure``` log file.
  ```bash
  Mar 20 20:11:48 localhost sshd[1433]: Failed password for student from 172.25.0.10 port 59344 ssh2
  ```
  * ```Mar 20 20:11:48``` : Records the time stamp of the log entry.
  * ```localhost``` : The host that sends the log message.
  * ```sshd[1433```] : The program or process name and PID number that sent the log message.
  * ```Failed password for …```: The message that was sent.

<a name="manually"></a>
* The ```logger``` command sends messages to the ```rsyslog``` service. By default, it sends the message to the user type with the ```notice``` priority (```user.notice```) unless specified otherwise with the ```-p``` option. It is helpful to test any change to the ```rsyslog``` service configuration.

	To send a message to the rsyslog service to be recorded in the ```/var/log/boot.log``` log file, execute the following ```logger``` command:
  ```console
  [root@host ~]# logger -p local7.notice "Log entry created on host"
  ```


<a name="11.5"></a>
## 11.5 Review System Journal Entries

| Content |
| --- |
| [journalctl](#journalctl) |

* The ```systemd-journald``` service stores logging data in a structured, indexed binary file called ```journal```.
<a name="journalctl"></a>
* You can use the ```journalctl``` command to view all messages in the journal, or to search for specific events based on a wide range of options and criteria. If you run the command as ```root```, then you have full access to the journal. Regular users can also use the ```journalctl``` command, but the system restricts them from seeing certain messages.
	```console
	[root@host ~]# journalctl
	...output omitted...
	Mar 15 04:42:16 host.lab.example.com systemd[2127]: Listening on PipeWire Multimedia System Socket.
	Mar 15 04:42:16 host.lab.example.com systemd[2127]: Starting Create User's Volatile Files and Directories...
	Mar 15 04:42:16 host.lab.example.com systemd[2127]: Listening on D-Bus User Message Bus Socket.
	Mar 15 04:42:16 host.lab.example.com systemd[2127]: Reached target Sockets.
	Mar 15 04:42:16 host.lab.example.com systemd[2127]: Finished Create User's Volatile Files and Directories.
	Mar 15 04:42:16 host.lab.example.com systemd[2127]: Reached target Basic System.
	Mar 15 04:42:16 host.lab.example.com systemd[1]: Started User Manager for UID 0.
	Mar 15 04:42:16 host.lab.example.com systemd[2127]: Reached target Main User Target.
	Mar 15 04:42:16 host.lab.example.com systemd[2127]: Startup finished in 90ms.
	Mar 15 04:42:16 host.lab.example.com systemd[1]: Started Session 6 of User root.
	Mar 15 04:42:16 host.lab.example.com sshd[2110]: pam_unix(sshd:session): session opened for user root(uid=0) by (uid=0)
	Mar 15 04:42:17 host.lab.example.com systemd[1]: Starting Hostname Service...
	Mar 15 04:42:17 host.lab.example.com systemd[1]: Started Hostname Service.
	lines 1951-2000/2000 (END) q
	```



<a name="11.7"></a>
## 11.7 Preserve the System Journal

| Content |
| --- |

<a name=""></a>
*


<a name="11.9"></a>
## 11.9 Maintain Accurate Time

| Content |
| --- |

<a name=""></a>
*

