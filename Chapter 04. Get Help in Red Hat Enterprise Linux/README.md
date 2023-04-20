# Chapter 4. Get Help in Red Hat Enterprise Linux

| Content |
| --- |
| [man](#man) |

<a name="man"></a>
## man

### Navigate and Search man Pages
| Command	| Result |
| :---: | --- |
| Spacebar	| Scroll forward (down) one screen. |
| PageDown	| Scroll forward (down) one screen. |
| PageUp	| Scroll backward (up) one screen. |
| DownArrow	| Scroll forward (down) one line. |
| UpArrow	| Scroll backward (up) one line. |
| D	| Scroll forward (down) one half-screen. |
| U	| Scroll backward (up) one half-screen. |
| /string	| Search forward (down) for string in the man page. |
| N	| Repeat previous search forward (down) in the man page. |
| Shift+N	| Repeat previous search backward (up) in the man page. |
| G	| Go to the start of the man page. |
| Shift+G	| Go to the end of the man page. |
| Q	| Exit man and return to the command shell prompt. |

### Search for man Pages by Keyword

```-k``` option (equivalent to the ```apropos``` command) to search for a keyword in ```man``` pages' titles and descriptions.
```console
[user@host ~]$ man -k passwd
chgpasswd (8)        - update group passwords in batch mode
chpasswd (8)         - update passwords in batch mode
fgetpwent_r (3)      - get passwd file entry reentrantly
getpwent_r (3)       - get passwd file entry reentrantly
...
passwd (1)           - update user's authentication tokens
passwd (1ossl)       - OpenSSL application commands
passwd (5)           - password file
passwd2des (3)       - RFS password encryption
...
```
```-K``` (uppercase) option searches for the keyword in the full-text page, not only in the titles and descriptions.
```console
[user@host ~]# man -K passwd
--Man-- next: cut(1p) [ view (return) | skip (Ctrl-D) | quit (Ctrl-C) ]
Ctrl-D
--Man-- next: logname(1p) [ view (return) | skip (Ctrl-D) | quit (Ctrl-C) ]
Ctrl-D
--Man-- next: sort(1p) [ view (return) | skip (Ctrl-D) | quit (Ctrl-C) ]
Ctrl-D
--Man-- next: xargs(1) [ view (return) | skip (Ctrl-D) | quit (Ctrl-C) ]
Ctrl-D
--Man-- next: chage(1) [ view (return) | skip (Ctrl-D) | quit (Ctrl-C) ]
Ctrl-C
```


