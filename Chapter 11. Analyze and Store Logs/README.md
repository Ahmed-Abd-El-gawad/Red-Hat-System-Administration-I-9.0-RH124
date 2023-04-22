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

