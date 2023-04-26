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



<a name="12.5"></a>
## 12.5 Configure Networking from the Command Line

| Content |
| --- |

<a name=""></a>
* 


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


