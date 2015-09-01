Index
-----

* [Description](#description)
* [Debugging](#debugging)
* [ADB](#adb)
* [Useful links](#useful-links)

Description
-----------

This section was designed only for advanced user, developers or users that having _special_ problems e.g. on connectivity related things or simply want to debug AFWall+ very quick. 

If you're asking for technical help, please be sure to include all your system info, including operating system, model number, and any other specifics related to the problem. Also please exercise your best judgment when posting in the forums - revealing personal information such as your e-mail address, telephone number, and address is not recommended.

Debugging
---------

To get all the logcat messages by AFWall+ run:

<code>adb logcat | grep `adb shell ps | grep dev.ukanth.ufirewall | cut -c10-15`</code>

AFWall+ itself indicates as AFWall (see TAG) in the logcat.

See also the [Android Gradle user guide](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Testing)
for more details, including how to use Android Studio to run tests (which provides more useful feedback than the command line).


ADB
---------

Of course the following commands must be changed per needs, the interfaces or paths could be named different. 

TCPDUMP Related
---------

```bash
# Example PCap a TCPDump wlan0 interface to external sdcard (e.g. DNS, 443 or ICMPv6)
adb shell tcpdump -ni wlan0 -U -w /sdcard/dump.pcap port 53 or port 443 or icmp6"

# Example PCap on data/local dir that captures a specific host
tcpdump -i br-lan -s0 -U -w /data/local/etherhost.pcap "ether host 34:xx:xx:xx:19:cb

# Example TCPDump that capture everything on ICMP6 
tcpdump -ni any -s0 -U -w /sdcard/icmp6.pcap icmp6
```

See if your Kernel does support e.g. the LOG/NFLOG target
---------

```bash
#su -c 'cat /proc/net/ip_tables_targets'
TRACE
NFQUEUE
NFQUEUE
NFQUEUE
NFLOG             <-- 
CLASSIFY
DNAT
SNAT
CONNMARK
MARK
REJECT
MASQUERADE
ERROR
TCPMSS
TPROXY
TPROXY
REDIRECT
NETMAP
LOG             <-- if the LOG-target is not supported by your kernel, it's not listed over here
DNAT
SNAT
...
..
.
```

DNS related (see also [DNS thread](https://github.com/ukanth/afwall/wiki/DNS)!)
---------

```bash
# NDC sets an dns resolver (100) to a specific host
ndc resolver setnetdns 100 "" 192.168.1.1

# Grep DNS addresses currently in use
adb shell dumpsys connectivity | grep DnsAddresses
```

Ping and Connectivity related
---------

```bash 
# Dumpsys connectivity tracker (shows a general overview of all connected devices and advanced network output)
adb shell dumpsys connectivity 
#or - does the same except the --diag tag which identify things for error resolving reasons
adb shell dumpsys connectivity --diag

# Greps on the wlan0 interface the current connection status 
ip a | grep "wlan0" -A10

# Check IPv6 (99,99% uptime)
https://ipv6.google.com/
# or IPv4
https://ipv4.google.com/

# Ping (two times -> -c2) the IPv4 Alphabet (alias Google)
adb shell ping6 -c2 ipv4.google.com

# nslookup on e.g. Google
nslookup google.com

```

Tethering
---------

```bash
# Shows TetherController output on dnsmasq (acts like a output filter)
adb logcat | egrep '(TetherController|dnsmasq)'


# Shows all devices the the device use
ls /sys/class/net/
```

Reverse tethering example
---------

```bash
Allow incoming LAN withing AFWAll+ and ensure the DHCP/DNS/tethering + root is checked
iptables -t nat -A POSTROUTING -j MASQUERADE
#iptables -t nat -A POSTROUTING -o ppp0 -j MASQUERADE
#sysctl -w net.ipv4.ip_forward=1

usb0: inet addr:169.254.0.1  Bcast:169.254.15.255  Mask:255.255.240.0
usb1: inet addr:169.254.16.1  Bcast:169.254.31.255  Mask:255.255.240.0
usb0 169.254.0.2/20 -> Default Gateway is 169.254.0.1 (host's usb0 iface)
Internal: usb0 169.254.16.2/20
Default Gateway 169.254.16.1 (host's usb1 iface)
```

Useful links
---------

* [Wiki Home](https://github.com/ukanth/afwall/wiki)
* [How a good bug report should looks like](https://github.com/ukanth/afwall/wiki/HOWTO-Report-Bug)
* [AFWAll+ Error codes](https://github.com/ukanth/afwall/wiki/Error-codes)
* [Compiling AFWall+](https://github.com/ukanth/afwall/wiki/HOWTO-Compile-AFWall)
* [Compiling BusyBox](https://github.com/ukanth/afwall/wiki/HOWTO-Compiling-busybox)
* [Compiling iptables](https://github.com/ukanth/afwall/wiki/HOWTO-Compiling-iptables-*under-construction!*)
* [Some phone codes](https://github.com/ukanth/afwall/wiki/Phone-codes-secrets)
