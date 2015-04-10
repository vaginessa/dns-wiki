:warning: This article is in a early beta stage and could contain _false information_ until all is done. This article is based on the Stock Android 4.0 OS (Kernel 2.6.x), this means newer Kernel or/and network changes could be differ a little bit.

:warning: On Windows I highly recommend to use Burp/HTTP Scoop/Fiddler for the [deep packet inspection](http://en.wikipedia.org/wiki/Deep_packet_inspection) instead of Whireshark for several reasons, one of them is that it's low-level and overpowered for quickly looking through HTTP(s) traces [but you're the boss].

As you might know, Android's kernel is based on the Linux kernel so the output could be similar. This article will clear some questions about the Android Kernel and his data usage and also may answer some addition questions like why is there traffic under _Android OS_ (UID=1000).

Most Internet applications are using TCP as their protocol of choice, and TCP maintains a connection bound to an IP address. Whenever you change your Internet access, you switch IP addresses and all existing TCP connections vanish. Your downloads are aborted, your SSH connections are closed,[...]

**Important**:
* [NetworkConnectivityListener](http://android.git.kernel.org/?p=platform/frameworks/base.git;a=blob;f=core/java/android/net/NetworkConnectivityListener.java)
* [ConnectivityService.java](http://android.git.kernel.org/?p=platform/frameworks/base.git;a=blob;f=services/java/com/android/server/ConnectivityService.java)


Index
-----

* [Known ports](#known-ports)
* [Interface statistics](#interface-statistics)
* [Routing and ARP tables](#routing-and-arp-tables)
* [Protocol statistics](#protocol-statistics)
* [How do I use this information?](#how-do-i-use-this-information?)
* [Capture all network traffic](#capture-all-network-traffic)
* [Tethering data](#tethering-data)
* [Modem traffic](#modem-traffic)
* [Closing words](#closing-words)
* [Useful links](#useful-links)

Known ports
-------

This is a small list which ports are open on every Linux/Android system, this is normally not critical as long there is no high traffic on it but you may need to understand what they do and how they should work. Remember: Closing all port's isn't a good idea (and in fact you not close the port you only disabling the application listening on it) since this will break most of internet stuff, but as long there are no suspect activity's all is good. For example one of a hacker goal is to send large volumes of network traffic at a host in order to cause legitimate traffic to be dropped, normally this behavior exists if the attacker doesn't control enouth bandwidth himself to exceed the target's bandwidth. As a result you'll see that your sever or address is unreasonable over the network. 

* Port 53 (tcp/udp) - Port 53 is used by well known DNS (Domain Name System). DNS takes care of resolving human readable 'host names' into numeric IP addresses. A commonly used DNS server called BIND has had a rich history of security problems. As a result, [BIND](http://bindguard.activezone.de/) and port 53 are frequent targets and a couple worms used BIND exploits to propagate. There are [several attacks](https://www.dns-oarc.net/wiki/mitigating-dns-denial-of-service-attacks), like DNS resolver/recursive, poisoning, DOS, worms, ...
* Port 80 -
* Anything what cause SYNC_SENT/BIND (which shows traffic on some roms/apps on the notification bar) in fact that does not cause any real traffic and your provider does not log such traffic (no traffic lost).
* These are known ports which "cause/show" traffic even if nothing is started or have any active internet connection.
Port 443 - .....
* You will need tools like tcpdump/wireshark/burf to really see what's going on behind the scenes since you only see symptoms on your device.

Interface statistics
-------

Each network interface config is in a `/sys/class/net/` dir.

TrafficStats data are stored under:
* `tx_bytes` (/sys/class/net/[interface]/statistics/tx_packets)
* `rx_bytes` (/sys/class/net/[interface]/statistics/rx_packets)

`/proc/net/dev` normally contains the configured network interfaces. We have:

* bytes - Data transmitted or received by the interface
* carrier - Carrier losses detected by the device driver
* colls - collisions detected on the interface
* compressed - Compressed packets transmitted or received by the device driver
* drop - Packets dropped by the device driver
* errs - Transmit or receive errors detected by the device driver
* fifo - FIFO buffer errors
* frame - Framing errors (via packet)
* packets - Packets of data transmitted or received by the interface

`/proc/net/` (for IPv4) exports information about open network sockets such as _tcp_, _udp_ & _raw_. The syntax is always identical.
`/proc/net/ipv6` (for IPv6 - can't control forwarding per device)
`/proc/net/tcp6` (for IPv6-based connections only)


* `getMobileRxBytes` - Returns data received over 3G/4G/GPRS
* `getTotalRxBytes` -  Includes all data received including over WiFi

TrafficStats Bugs (Android 2/3/4.3):
* [Android TrafficStats.getTotalRxBytes() is less than expected](http://stackoverflow.com/questions/8478696/android-trafficstats-gettotalrxbytes-is-less-than-expected)
* [TrafficStats.getMobileRxBytes() and TrafficStats.getMobileTxBytes()](https://code.google.com/p/android/issues/detail?id=19938)
* [TrafficStats APIs do not report UDP traffic even though API level 12 onwards they should have been supported (never fixed)](https://code.google.com/p/android/issues/detail?id=32410)
* [TrafficStats.getUidRxBytes and getUidTxBytes always return 0 in 4.3](https://code.google.com/p/android/issues/detail?id=58210)


Get basic info about process info via `busybox netstat -tp`.


Log in/out iptables:
```
$ adb shell su -c "fgrep '[IPtables IN ' /proc/kmsg"
$ adb shell su -c "fgrep '[IPtables OUT ' /proc/kmsg"
```


Drop rule into the output chain
``` 
iptables -F OUTPUT
iptables -A OUTPUT -j DROP
```



Routing and ARP tables
-------

* Destination - In combination with the Mark field, this specifies which data grams will match this route
* Iface - The network interface that data grams matching this route will leave
* 


Protocol statistics
-------

`/proc/net/snmp` can exports protocol statistics (SNMP - Simple Network Management Protocol) and provides a summary stats for each of the IP, ICMP, TCP, and UDP protocol.



UIDs stats
providing traffic usage per application


UID stats (this ones never change):
```
0 - Root
1000 - System
1001 - Radio
1002 - Bluetooth
1003 - Graphics
1004 - Input
1005 - Audio
1006 - Camera
1007 - Log
1008 - Compass
1009 - Mount
1010 - WiFi
1011 - ADB
1012 - Install
1013 - Media
1014 - DHCP
1015 - External Storage
1016 - VPN
1017 - Keystore
1018 - USB Devices
1019 - DRM
1020 - Available
1021 - GPS
1022 - deprecated
1023 - Internal Media Storage
1024 - MTP USB
1025 - NFC
1026 - DRM RPC
2000 - Shell (User) for example ddms (generates traffic for txt/png and such)
<10000 = system reserved 
10000(and higher) = applications UIDs
```


Tethering data
-------

We can't monitor our WiFi network using tethering even if Android supports promiscuous mode for the WiFi chipset.


Because:

Tethering does NAT internally and assigns you an IP in a 192.168.* private range via a DHCP daemon running on our phone. There's no way we can see pure WiFi traffic only this way.
The only workaround is a custom firmware we can install on our device and do a tcpdump (see above) on the phone itself, but only if it supports both types, promisc mode and tcpdump [raw 802.11 frames -> "Monitor mode"]).

Modem traffic
-------

:warning: Also, cell phones have [RTOS code](http://www.androidauthority.com/smartphones-have-a-second-os-317800/) running on a second processor in the baseband (modem) unit which is independent of the primary OS. - On IOS there is [hardware tracking](https://en.wikipedia.org/wiki/IPhone#Secret_tracking) :warning:

The problem is that like all software there are bugs in the operating systems used by the baseband modem makers. If there are bugs then there are security vulnerabilities. If there are security vulnerabilities then there is a doorway for hackers to get in.

Iptables or any other low-level binary/tool can't block modem traffic! IPtables wasn't designed for Computers and not for mobile devices + the fact that this sector can't be that easily touched without destroying his function. AFWall+ blocks only the UID's per-app-basis which means everything what hits the traffic behind it is not visible.

Capture all network traffic
-------

:warning: You will need to make sure you supply the right interface name for the capture and this varies from one device to another, eg -i eth0 or -i tiwlan0 - or use -i any to log all interfaces!

* tcpdump
* tcptrace
* ~~[Fuse](http://de.wikipedia.org/wiki/Filesystem_in_Userspace) (driver) / [YAFFS](http://en.wikipedia.org/wiki/YAFFS) (on newer models YAFFS2)~~ (only for forensic reasons)
* `tcpdump -i br0 -s 0 -w [SSID].cap`
* [Wireshark](https://www.wireshark.org/download.html) / [Burp](http://portswigger.net/) / [HTTP Scoop](http://www.tuffcode.com/) (for our .cap files)

Host and Guest connections:
* lsof -i -n -P

Capture HTTP traffic over port 80 (change iface name if it's different):
* tcpdump -i eth0 -s 0 -w [name].cap port 80
* tcptrace -n /tmp/[name].cap | grep :80 (only on a emulator for some tests)
* tcptrace -n /tmp/[name].cap | awk '{print $4}' | sort -g | uniq | awk -F":" '{print $1}' (same but it sorts the ip's)
* 



About DNS traffic:

It's an UDP based protocol and each packet is around ~62 bytes per request, which comes in only a few packets. Means if you have an app that does generate frequent DNS lookups, these can add up to a fair amount of data usage (dependency how many requests it uses).

This is how DNS requests looks like in a firewall (iptables based) like Avast, AFWall+ or Avast:

```
Chain afwall (1 references)
pkts bytes target     prot opt in     out     source               destination
    0     0 RETURN     udp  --  *      *       0.0.0.0/0            0.0.0.0/0
         udp dpt:53
```

There're quite a few types of packets that are reported by the "Data usage" app as part of the network traffic of "Android OS". Among others DNS requests belong to these kind of traffic. 

But it's wrong:

UID=1000 belongs to an Android component that is usually called "Android OS", but DNS requests come from a process running as root (UID=0 [root]) so if you see "Android OS" in an app list, it can mean a root process or an UID=1000 process as well.


How do I use this information?
-------

~~- It's not done, so atm it only shows the important parts about kernel generated traffic. -~~ 


Closing words
-------

Internet security is hard. Let's go bake some cookies!


Useful links
-------

* [Domain Name System (DNS) | Wikipedia](http://en.wikipedia.org/wiki/Domain_Name_System)
* [Android Data Usage | Source Android](https://source.android.com/devices/tech/datausage/index.html)
* [Android Git repositories | Android GoogleSource](https://android.googlesource.com/) (iptables, kernel,[...])
* [Android Traffic Stats | Android Developer] (http://developer.android.com/reference/android/net/TrafficStats.html)
* [sysctl | kernel.org](https://www.kernel.org/doc/Documentation/networking/ip-sysctl.txt)
* [[MOD]Any phone, any ROM: Wi-Fi only mode (disable cell radio) | RootWiki.com](http://rootzwiki.com/topic/25016-modany-phone-any-rom-wi-fi-only-mode-disable-cell-radio/)
* [Connectivitymanager in Android SDK | MSDN Blogs](http://blogs.msdn.com/zhengpei/archive/2009/09/22/connectivitymanager-in-android-sdk.aspx)
* [Robtex Swiss Army Knife Internet Tool | Robtex.com](https://www.robtex.com/#faq)

Tutorials:
* [Sniffing Network Traffic on Android | Infosec](http://resources.infosecinstitute.com/sniffing-network-traffic-android/)
* [Set up your PC as a wireless access point | LifeHacker](http://lifehacker.com/5369381/turn-your-windows-7-pc-into-a-wireless-hotspot)
* [Ettercap Tutorial | openmaniak](http://openmaniak.com/ettercap_arp.php)
* [Passing Android Traffic through Burp | Systemoverlord](https://systemoverlord.com/blog/2014/07/13/passing-android-traffic-through-burp/)
* [Whireshark WLAN capturing on 802.11 | Wiki Whireskark](http://wiki.wireshark.org/CaptureSetup/WLAN)
* [Bluetooth packet capture on Android 4.4 | Nowsecure](https://www.nowsecure.com/blog/2014/02/07/bluetooth-packet-capture-android/)
* [How to sniffing Android Application with Wireshark (VirtualBox) | talat](http://talat.uyarer.com/post/68706747099/how-to-sniffing-android-application-with-wireshark)
* [Ralf-Philipp Weinmann presented some finding at a security conference | Readwrite](http://readwrite.com/2011/01/18/baseband_hacking_a_new_frontier_for_smartphone_break_ins#awesm=~onynAS4WgTRdXI)

```bash
 _________       ______            
|  _   _  |     |_   _ `.          
|_/ | | \_|.--.   | | `. \  .--.   
    | |  / .'`\ \ | |  | |/ .'`\ \ 
   _| |_ | \__. |_| |_.' /| \__. | 
  |_____| '.__.'|______.'  '.__.'  .....
                                                                                                      
* Add more basic info to prevent and analyze such traffic
* Explain why is there a difference between the amount of data reported from the device, and from what your carrier counts - reconnects, bad connections/signals/re-transmitted data/location data/push/backup/device manager/scroogle
* Making a tunnel to existent article "tcp security"
* Add a good method for block DNS queries, WhatsApp and other apps (and the os) constantly want to connect to it without any reasons ... (without iptables/root)
* Seems [TrafficStats](http://developer.android.com/reference/android/net/TrafficStats.html).getUidRxBytes(uid) & TrafficStats.getUidTxBytes(uid) isn't fixed at all (need some more tests to really see if the data traffic is still incorrect for e.g. Facebook app) 
* Maybe a quick guide to show howto debug traffic on the emulator (via DDMS) or with fiddler (on ssl)
* Stuff/tools which make life easier to inspect Android OS traffic directly on the device (most tools are way outdated/deprecated, like OS Monitor, Shark, ... or they simply not work on Android 5 because it needs to be recompiled with the Position Indipendent Executables (-pie or on libs -fPIC) flag
```

-> Since I don't have any real motivation I'm always open minded what I should show/explain over here...
