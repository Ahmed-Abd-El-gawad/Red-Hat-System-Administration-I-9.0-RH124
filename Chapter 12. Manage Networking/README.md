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




<a name="12.7"></a>
## 12.7 Edit Network Configuration Files

| Content |
| --- |

<a name=""></a>
* 


<a name="12.9"></a>
## 12.9 Configure Hostnames and Name Resolution

| Content |
| --- |

<a name=""></a>
* 


