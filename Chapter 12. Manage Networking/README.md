# Chapter 12. Manage Networking

| Content |
| --- |
| [12.1 Describe Networking Concepts](#12.1) |
| [12.3 Validate Network Configuration](#12.3) |
| [12.5 Configure Networking from the Command Line](#12.5) |
| [12.7 Edit Network Configuration Files](#12.7) |
| [12.9 Configure Hostnames and Name Resolution](#12.9) |


<a name="12.1"></a>
## 12.1 Describe Networking Concepts

Content you should know:
* TCP/IP Network Model
* Network Interface Names
* IPv4 Networks
  * IPv4 Addresses
  * IPv4 Subnets and Netmasks
  * IPv4 Routes
  * IPv4 Address and Route Configuration
* IPv6 Networks
  * IPv6 Addresses
  * IPv6 Subnets
  * IPv6 Address Configuration


<a name="12.3"></a>
## 12.3 Validate Network Configuration

| Content |
| --- |
| [ip link](#ip_link) |
| [ip addr](#ip_addr) |
| [ip -s](#-s) |
| [ping](#ping) |
| [ping6](#ping6) |
| [ping6](#ping6) |
| [route](#route) |
| [tracepath](#tracepath) |
| [ss](#ss) |

<a name="ip_link"></a>
* The ```ip link``` command lists all available network interfaces on your system.
  ```console
  [user@host ~]$ ip link show
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
  2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
      link/ether 52:54:00:00:00:0a brd ff:ff:ff:ff:ff:ff
  3: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
      link/ether 52:54:00:00:00:1e brd ff:ff:ff:ff:ff:ff
  ```

<a name="ip_addr"></a>
* Use the ```ip``` command to view device and address information.
  ```console
  [user@host ~]$ ip addr show ens3
  2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 10001
      link/ether 52:54:00:00:00:0b brd ff:ff:ff:ff:ff:ff2
      inet 192.0.2.2/24 brd 192.0.2.255 scope global ens33
         valid_lft forever preferred_lft forever
      inet6 2001:db8:0:1:5054:ff:fe00:b/64 scope global4
         valid_lft forever preferred_lft forever
      inet6 fe80::5054:ff:fe00:b/64 scope link5
         valid_lft forever preferred_lft forever
  ```

<a name="-s"></a>
* The ```ip``` command can also show statistics about network performance.
  ```console
  [user@host ~]$ ip -s link show ens3
  2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
  link/ether 52:54:00:00:00:0a brd ff:ff:ff:ff:ff:ff
      RX: bytes  packets  errors  dropped overrun mcast
      269850     2931     0       0       0       0
      TX: bytes  packets  errors  dropped carrier collsns
      300556     3250     0       0       0       0
  ```

<a name="ping"></a>
* The ```ping``` command tests connectivity.
  ```console
  [user@host ~]$ ping -c3 192.0.2.254
  PING 192.0.2.1 (192.0.2.254) 56(84) bytes of data.
  64 bytes from 192.0.2.254: icmp_seq=1 ttl=64 time=4.33 ms
  64 bytes from 192.0.2.254: icmp_seq=2 ttl=64 time=3.48 ms
  64 bytes from 192.0.2.254: icmp_seq=3 ttl=64 time=6.83 ms

  --- 192.0.2.254 ping statistics ---
  3 packets transmitted, 3 received, 0% packet loss, time 2003ms
  rtt min/avg/max/mdev = 3.485/4.885/6.837/1.424 ms
  ```

<a name="ping6"></a>
* The ```ping6``` command is the IPv6 version of the ```ping``` command.
  ```console
  [user@host ~]$ ping6 2001:db8:0:1::1
  PING 2001:db8:0:1::1(2001:db8:0:1::1) 56 data bytes
  64 bytes from 2001:db8:0:1::1: icmp_seq=1 ttl=64 time=18.4 ms
  64 bytes from 2001:db8:0:1::1: icmp_seq=2 ttl=64 time=0.178 ms
  64 bytes from 2001:db8:0:1::1: icmp_seq=3 ttl=64 time=0.180 ms
  ^C
  --- 2001:db8:0:1::1 ping statistics ---
  3 packets transmitted, 3 received, 0% packet loss, time 2001ms
  rtt min/avg/max/mdev = 0.178/6.272/18.458/8.616 ms
  [user@host ~]$
  ```

<a name="route"></a>
* Use the ```ip``` command ```route``` option to show routing information.
  ```console
  [user@host ~]$ ip route
  default via 192.0.2.254 dev ens3 proto static metric 1024
  192.0.2.0/24 dev ens3 proto kernel scope link src 192.0.2.2
  10.0.0.0/8 dev ens4 proto kernel scope link src 10.0.0.11
  ```
* Use the ```ip``` command ```-6``` option to show the IPv6 routing table.
  ```console
  [user@host ~]$ ip -6 route
  unreachable ::/96 dev lo  metric 1024  error -101
  unreachable ::ffff:0.0.0.0/96 dev lo  metric 1024  error -101
  2001:db8:0:1::/64 dev ens3  proto kernel  metric 256
  unreachable 2002:a00::/24 dev lo  metric 1024  error -101
  unreachable 2002:7f00::/24 dev lo  metric 1024  error -101
  unreachable 2002:a9fe::/32 dev lo  metric 1024  error -101
  unreachable 2002:ac10::/28 dev lo  metric 1024  error -101
  unreachable 2002:c0a8::/32 dev lo  metric 1024  error -101
  unreachable 2002:e000::/19 dev lo  metric 1024  error -101
  unreachable 3ffe:ffff::/32 dev lo  metric 1024  error -101
  fe80::/64 dev ens3  proto kernel  metric 256
  default via 2001:db8:0:1::ffff dev ens3  proto static  metric 1024
  ```

<a name="tracepath"></a>
* To trace the network traffic path to reach a remote host through multiple routers, use either the ```traceroute``` or ```tracepath``` command. These commands can identify issues with one of your routers or an intermediate router. 
* Both commands use ***UDP*** packets to trace a path by default; however, many networks block UDP and ICMP traffic. The traceroute command has options to trace the path with ***UDP*** (default), ***ICMP*** (```-I```), or ***TCP*** (```-T```) packets.
  ```console
  [user@host ~]$ tracepath access.redhat.com
  ...output omitted...
   4:  71-32-28-145.rcmt.qwest.net                          48.853ms asymm  5
   5:  dcp-brdr-04.inet.qwest.net                          100.732ms asymm  7
   6:  206.111.0.153.ptr.us.xo.net                          96.245ms asymm  7
   7:  207.88.14.162.ptr.us.xo.net                          85.270ms asymm  8
   8:  ae1d0.cir1.atlanta6-ga.us.xo.net                     64.160ms asymm  7
   9:  216.156.108.98.ptr.us.xo.net                        108.652ms
  10:  bu-ether13.atlngamq46w-bcr00.tbone.rr.com           107.286ms asymm 12
  ...output omitted...
  ```
* The ```tracepath6``` and ```traceroute``` ```-6``` commands are the equivalent IPv6 commands to the tracepath and traceroute commands.
  ```console
  [user@host ~]$ tracepath6 2001:db8:0:2::451
   1?: [LOCALHOST]                        0.091ms pmtu 1500
   1:  2001:db8:0:1::ba                   0.214ms
   2:  2001:db8:0:1::1                    0.512ms
   3:  2001:db8:0:2::451                  0.559ms reached
       Resume: pmtu 1500 hops 3 back 3
  ```

<a name="ss"></a>
* The ```ss``` command is used to display socket statistics. The ```ss``` command replaces the older ```netstat``` tool, from the ```net-tools``` package, which might be more familiar to some system administrators but is not always installed.
  ```console
  [user@host ~]$ ss -ta
  State      Recv-Q Send-Q      Local Address:Port          Peer Address:Port
  LISTEN     0      128                     *:sunrpc                   *:*
  LISTEN     0      128                     *:ssh                      *:*
  LISTEN     0      100             127.0.0.1:smtp                      :
  LISTEN     0      128                     *:36889                    *:*
  ESTAB      0      0           172.25.250.10:ssh         172.25.254.254:59392
  LISTEN     0      128                    :::sunrpc                  :::*
  LISTEN     0      128                    :::ssh                     :::*
  LISTEN     0      100                   ::1:smtp                    :::*
  LISTEN     0      128                    :::34946                   :::*
  ```
  | Option | Description |
  | --- | --- |
  | ```-n``` | Show numbers instead of names for interfaces and ports. |
  | ```-t``` | Show TCP sockets. |
  | ```-u``` | Show UDP sockets. |
  | ```-l``` | Show only listening sockets. |
  | ```-a``` | Show all (listening and established) sockets. |
  | ```-p``` | Show the process that uses the sockets. |
  | ```-A``` | inet	Display active connections (but not listening sockets) for the ```inet``` address family. That is, ignore local UNIX domain sockets. For the ```ss``` command, both IPv4 and IPv6 connections are displayed. For the ```netstat``` command, only IPv4 connections are displayed. (The ```netstat -A inet6``` command displays IPv6 connections, and the ```netstat -46``` command displays IPv4 and IPv6 at the same time.) |


<a name="12.5"></a>
## 12.5 Configure Networking from the Command Line

| Content |
| --- |
| [View Network Information](#view) |
| [Add a Network Connection](#add) |
| [Manage Network Connections](#manage) |
| [Update Network Connection Settings](#update) |
| [Delete a Network Connection](#delete) |
| [Permissions to Modify NetworkManager Settings](#permissions) |
| [Summary](#summary) |

<a name="view"></a>
* The ```nmcli device status``` command displays the status of all network devices:
  ```console
  [user@host ~]$ nmcli dev status
  DEVICE  TYPE      STATE         CONNECTION
  eno1    ethernet  connected     eno1
  ens3    ethernet  connected     static-ens3
  eno2    ethernet  disconnected  --
  lo      loopback  unmanaged     --
  ```
* The ```nmcli connection show``` command displays a list of all connections. Use the ```--active``` option to list only active connections.
  ```console
  [user@host ~]$ nmcli con show
  NAME         UUID                                  TYPE            DEVICE
  eno2         ff9f7d69-db83-4fed-9f32-939f8b5f81cd  802-3-ethernet  --
  static-ens3  72ca57a2-f780-40da-b146-99f71c431e2b  802-3-ethernet  ens3
  eno1         87b53c56-1f5d-4a29-a869-8a7bdaf56dfa  802-3-ethernet  eno1
  [user@host ~]$ nmcli con show --active
  NAME         UUID                                  TYPE            DEVICE
  static-ens3  72ca57a2-f780-40da-b146-99f71c431e2b  802-3-ethernet  ens3
  eno1         87b53c56-1f5d-4a29-a869-8a7bdaf56dfa  802-3-ethernet  eno1
  ```

<a name="add"></a>
* Use the ```nmcli connection add``` command to add network connections.
  ```console
  [root@host ~]# nmcli con add con-name eno2 \
  type ethernet ifname eno2
  Connection 'eno2' (8159b66b-3c36-402f-aa4c-2ea933c7a5ce) successfully added
  ```
  ```console
  [root@host ~]# nmcli con add con-name eno3 type ethernet ifname eno3 \
  ipv4.addresses 192.168.0.5/24 ipv4.gateway 192.168.0.254
  ```
  ```console
  [root@host ~]# nmcli con add con-name eno4 type ethernet ifname eno4 \
  ipv6.addresses 2001:db8:0:1::c000:207/64 ipv6.gateway 2001:db8:0:1::1 \
  ipv4.addresses 192.0.2.7/24 ipv4.gateway 192.0.2.1
  ```

<a name="manage"></a>
* The ```nmcli connection up``` command activates a network connection on the device that it is bound to. Activating a network connection requires the connection name, not the device name.
  ```console
  [user@host ~]$ nmcli con show
  NAME         UUID                                  TYPE            DEVICE
  static-ens3  72ca57a2-f780-40da-b146-99f71c431e2b  802-3-ethernet  --
  static-ens5  87b53c56-1f5d-4a29-a869-8a7bdaf56dfa  802-3-ethernet  --
  [root@host ~]# nmcli con up static-ens3
  Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/2)
  ```
* The ```nmcli device disconnect``` command disconnects the network device and brings down the connection.
  ```console
  [root@host ~]# nmcli dev disconnect ens3
  ```
* Use ```nmcli device disconnect``` to stop traffic on a network interface and deactivate the connection.

  Because most connections enable the <sub>autoconnect</sub> parameter, the ```nmcli connection down``` command is ineffective for stopping traffic. Although the connection deactivates, autoconnect immediately reactivates the connection if the device is up and available. Autoconnect is a desired behavior because it maintains connections through temporary network outages.

  By disconnecting the device under the connection, the connection is forced to be down until the device is connected again.

<a name="update"></a>
* ***NetworkManager*** service connections have two kinds of settings. Static connection properties are configured by the administrator and stored in the ```/etc/NetworkManager/system-connections/*.nmconnection``` configuration files. Dynamic connection properties are requested from a DHCP server and are not stored persistently.
* To list the current settings for a connection, use the ```nmcli connection show``` command. Settings in lowercase are static properties that the administrator can change. Settings in uppercase are active settings in temporary use for this connection instance.
  ```console
  [root@host ~]# nmcli con show static-ens3
  connection.id:                          static-ens3
  connection.uuid:                        87b53c56-1f5d-4a29-a869-8a7bdaf56dfa
  connection.interface-name:              --
  connection.type:                        802-3-ethernet
  connection.autoconnect:                 yes
  connection.timestamp:                   1401803453
  connection.read-only:                   no
  connection.permissions:
  connection.zone:                        --
  connection.master:                      --
  connection.slave-type:                  --
  connection.secondaries:
  connection.gateway-ping-timeout:        0
  802-3-ethernet.port:                    --
  802-3-ethernet.speed:                   0
  802-3-ethernet.duplex:                  --
  802-3-ethernet.auto-negotiate:          yes
  802-3-ethernet.mac-address:             CA:9D:E9:2A:CE:F0
  802-3-ethernet.cloned-mac-address:      --
  802-3-ethernet.mac-address-blacklist:
  802-3-ethernet.mtu:                     auto
  802-3-ethernet.s390-subchannels:
  802-3-ethernet.s390-nettype:            --
  802-3-ethernet.s390-options:
  ipv4.method:                            manual
  ipv4.dns:                               192.168.0.254
  ipv4.dns-search:                        example.com
  ipv4.addresses:                         { ip = 192.168.0.2/24,
                                            gw = 192.168.0.254 }
  ipv4.routes:
  ipv4.ignore-auto-routes:                no
  ipv4.ignore-auto-dns:                   no
  ipv4.dhcp-client-id:                    --
  ipv4.dhcp-send-hostname:                yes
  ipv4.dhcp-hostname:                     --
  ipv4.never-default:                     no
  ipv4.may-fail:                          yes
  ipv6.method:                            manual
  ipv6.dns:                               2001:4860:4860::8888
  ipv6.dns-search:                        example.com
  ipv6.addresses:                         { ip = 2001:db8:0:1::7/64,
                                            gw = 2001:db8:0:1::1 }
  ipv6.routes:
  ipv6.ignore-auto-routes:                no
  ipv6.ignore-auto-dns:                   no
  ipv6.never-default:                     no
  ipv6.may-fail:                          yes
  ipv6.ip6-privacy:                       -1 (unknown)
  ipv6.dhcp-hostname:                     --
  ...output omitted...
  ```
* Use the ```nmcli connection modify``` command to update connection settings. These changes are saved in the ```/etc/NetworkManager/system-connections/name.nmconnection``` file.
  ```console
  [root@host ~]# nmcli con mod static-ens3 ipv4.addresses 192.0.2.2/24 \
  ipv4.gateway 192.0.2.254 connection.autoconnect yes
  ```
  ```console
  [root@host ~]# nmcli con mod static-ens3 ipv6.addresses 2001:db8:0:1::a00:1/64 \
  ipv6.gateway 2001:db8:0:1::1
  ```
* Some settings can have multiple values. A specific value can be added to the list or deleted from the connection settings by adding a plus (```+```) or minus (```-```) symbol to the start of the setting name. If a plus or minus is not included, then the specified value replaces the setting's current list.
  ```console
  [root@host ~]# nmcli con mod static-ens3 +ipv4.dns 2.2.2.2
  ```
* You can also modify network profiles by editing the connection's configuration file in ```/etc/NetworkManager/system-connections/```. While ```nmcli``` commands communicate directly with ***NetworkManager*** to implement modifications immediately, connection file edits are not implemented until NetworkManager is asked to reload the configuration file. With manual editing, you can create complex configurations in steps, and then load the final configuration when ready. 
  
  The following example loads all connection profiles.
  ```console
  [root@host ~]# nmcli con reload
  ```
  The next example loads only the ```eno2``` connection profile at ```/etc/NetworkManager/system-connections/eno2.nmconnection```.
  ```console
  [root@host ~]# nmcli con reload eno2
  ```

<a name="delete"></a>
* The ```nmcli connection delete``` command deletes a connection from the system. This command disconnects the device and removes the connection configuration file.
  ```console
  [root@host ~]# nmcli con del static-ens3
  ```

<a name="permissions"></a>
* The root user can use the nmcli command to change the network configuration.
* Non-privileged users that are logged in on the physical or virtual console can also make most network configuration changes. If a person is on the system's console, then the system is likely being used as a workstation or laptop where the user needs to configure, activate, and deactivate connections. Non-privileged users that log in with ```ssh``` must switch to the root user to change network settings.
* Use the ```nmcli general permissions``` command to view your current permissions.
  ```console
  [root@host ~]# nmcli gen permissions
  PERMISSION                                                        VALUE
  org.freedesktop.NetworkManager.checkpoint-rollback                yes
  org.freedesktop.NetworkManager.enable-disable-connectivity-check  yes
  org.freedesktop.NetworkManager.enable-disable-network             yes
  org.freedesktop.NetworkManager.enable-disable-statistics          yes
  org.freedesktop.NetworkManager.enable-disable-wifi                yes
  org.freedesktop.NetworkManager.enable-disable-wimax               yes
  org.freedesktop.NetworkManager.enable-disable-wwan                yes
  org.freedesktop.NetworkManager.network-control                    yes
  org.freedesktop.NetworkManager.reload                             yes
  org.freedesktop.NetworkManager.settings.modify.global-dns         yes
  org.freedesktop.NetworkManager.settings.modify.hostname           yes
  org.freedesktop.NetworkManager.settings.modify.own                yes
  org.freedesktop.NetworkManager.settings.modify.system             yes
  org.freedesktop.NetworkManager.sleep-wake                         yes
  org.freedesktop.NetworkManager.wifi.scan                          yes
  org.freedesktop.NetworkManager.wifi.share.open                    yes
  org.freedesktop.NetworkManager.wifi.share.protected               yes
  ```
  ```console
  [user@host ~]$ nmcli gen permissions
  PERMISSION                                                        VALUE
  org.freedesktop.NetworkManager.checkpoint-rollback                auth
  org.freedesktop.NetworkManager.enable-disable-connectivity-check  no
  org.freedesktop.NetworkManager.enable-disable-network             no
  org.freedesktop.NetworkManager.enable-disable-statistics          no
  org.freedesktop.NetworkManager.enable-disable-wifi                no
  org.freedesktop.NetworkManager.enable-disable-wimax               no
  org.freedesktop.NetworkManager.enable-disable-wwan                no
  org.freedesktop.NetworkManager.network-control                    auth
  org.freedesktop.NetworkManager.reload                             auth
  org.freedesktop.NetworkManager.settings.modify.global-dns         auth
  org.freedesktop.NetworkManager.settings.modify.hostname           auth
  org.freedesktop.NetworkManager.settings.modify.own                auth
  org.freedesktop.NetworkManager.settings.modify.system             auth
  org.freedesktop.NetworkManager.sleep-wake                         no
  org.freedesktop.NetworkManager.wifi.scan                          auth
  org.freedesktop.NetworkManager.wifi.share.open                    no
  org.freedesktop.NetworkManager.wifi.share.protected               no
  ```

<a name="summary"></a>
* Useful NetworkManager Commands

| Command | Purpose |
| --- | --- |
| ```nmcli dev status``` | Show the NetworkManager status of all network interfaces. |
| ```nmcli con show``` | List all connections. |
| ```nmcli con show name``` | List the current settings for the connection name. |
| ```nmcli con add con-name name``` | Add and name a new connection profile. |
| ```nmcli con mod name``` | Modify the connection name. |
| ```nmcli con reload``` | Reload the configuration files, after manual file editing. |
| ```nmcli con up name``` | Activate the connection name. |
| ```nmcli dev dis dev``` | Disconnect the interface, which also deactivates the current connection. |
| ```nmcli con del name``` | Delete the specified connection and its configuration file. |


<a name="12.7"></a>
## 12.7 Edit Network Configuration Files

| Content |
| --- |
| [Connection Configuration Files](#files) |
| [Key File Format](#key) |
| [Modify Network Configuration](#modify) |

<a name="files"></a>
* Starting with Red Hat Enterprise Linux 8, network configurations are stored in the ```/etc/NetworkManager/system-connections/``` directory.
* This new configuration location uses the ***key*** file format instead of the ```ifcfg``` format. 
* The previously stored configurations at ```/etc/sysconfig/network-scripts/``` continue to work. The ```/etc/NetworkManager/system-connections/``` directory stores any changes with the nmcli con mod name command.

<a name="key"></a>
* The NetworkManager uses the INI-style key format for storing network connection profiles. The key-value pairs store configurations as sections (groups). Each configuration key/value pair in the section is one of the listed properties in the settings specification. This configuration file stores most of the settings in the same format as the INI-style format. For example, writing IP addresses as 192.168.0.1/24 is easier to read than as integer arrays.

  Although the recommended way to manage profiles is with the nmcli command, users might still manually create or modify the configuration files. After editing the configuration file, run the nmcli con reload command to inform NetworkManager about these changes.


| ```nmcli con mod``` | ```*.nmconnection``` file | Effect |
| --- | --- | --- |
| ipv4.method manual | [ipv4] method=manual | Configure IPv4 addresses statically. |
| ipv4.method auto | [ipv4] method=auto | Look for configuration settings from a DHCPv4 server. It does not bring up any static addresses until it has information from DHCPv4. |
| ipv4.addresses 192.0.2.1/24 | [ipv4] address1=192.0.2.1/24 | Set a static IPv4 address and network prefix. For more than one connection address, the ```address2``` key defines the second address, and the ```address3``` key defines the third address. |
| ipv4.gateway 192.0.2.254 | [ipv4] gateway=192.0.2.254 | Set the default gateway. |
| ipv4.dns 8.8.8.8 | [ipv4] dns=8.8.8.8 | Modify ```/etc/resolv.conf``` to use this name server. |
| ipv4.dns-search example.com | [ipv4] dns-search=example.com | Modify ```/etc/resolv.conf``` to use this domain in the search directive. |
| ipv4.ignore-auto-dns true | [ipv4] ignore-auto-dns=true | Ignore DNS server information from the DHCP server. |
| ipv6.method manual | [ipv6] method=manual | Configure IPv6 addresses statically. |
| ipv6.method auto | [ivp6] method=auto | Configure network settings with SLAAC from router advertisements. |
| ipv6.method dhcp | [ipv6] method=dhcp | Configure network settings by using DHCPv6, but not SLAAC. |
| ipv6.addresses 2001:db8::a/64 | [ipv6] address1=2001:db8::a/64 | Set static IPv6 address and network prefix. When using more than one address for a connection, the ```address2``` key defines the second address, and the ```address3``` key defines the third address. |
| ipv6.gateway 2001:db8::1 | [ipv6] gateway=2001:db8::1 | Set the default gateway. |
| ipv6.dns fde2:6494:1e09:2::d | [ipv6] dns=fde2:6494:1e09:2::d | Modify ```/etc/resolv.conf``` to use this name server. The same as IPv4. |
| ipv6.dns-search example.com | [ipv6] dns-search=example.com | Modify ```/etc/resolv.conf``` to use this domain in the search directive. |
| ipv6.ignore-auto-dns true | [ipv6] ignore-auto-dns=true | Ignore DNS server information from the DHCP server. |
| connection.autoconnect yes | [connection] autoconnect=true | Automatically activate this connection at boot. |
| connection.id ens3 | [connection] id=Main eth0 | The name of this connection. |
| connection.interface-name ens3 | [connection] interface-name=ens3 | The connection is bound to the network interface with this name. |
| 802-3-ethernet.mac-address … | [802-3-ethernet] mac-address= | The connection is bound to the network interface with this MAC address. |

<a name="modify"></a>
* You can also configure the network by directly editing the connection configuration files. Connection configuration files control the software interfaces for individual network devices. These files are usually called ```/etc/sysconfig/network-scripts/name.nmconnection```, where name refers to the device's name or connection that the configuration file controls.
* The ```/etc/NetworkManager/system-connections/``` directory stores persistent profiles that the user created and edited. NetworkManager copies them automatically to the ```/etc/NetworkManager/system-connections/``` directory.
* The ```/run/NetworkManager/system-connections/``` directory stores temporary profiles, which are automatically removed when you reboot the system.
* The ```/usr/lib/NetworkManager/system-connections/``` directory stores predeployed immutable profiles. When you edit such a profile with the NetworkManager API, NetworkManager copies this profile to either the persistent or the temporary storage.
* After modifying the configuration files, set permissions on the configuration file for the root user to read and modify the configuration file.
  ```console
  [root@host ~]# chown root:root /etc/NetworkManager/system-connections/"Main eth0.nmconnection"
  [root@host ~]# chmod 600 /etc/NetworkManager/system-connections/"Main eth0.nmconnection"
  ```
  Run the nmcli con reload command for NetworkManager to read the configuration changes. When the autoconnect variable in the profile uses the false value, then activate the connection.
  ```console
  [root@host ~]# nmcli con reload
  [root@host ~]# nmcli con up "static-ens3"
  ```


<a name="12.9"></a>
## 12.9 Configure Hostnames and Name Resolution

| Content |
| --- |

<a name=""></a>
* 


