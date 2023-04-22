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

<a name=""></a>
* 


<a name="11.5"></a>
## 11.5 Review System Journal Entries

| Content |
| --- |

<a name=""></a>
*


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

