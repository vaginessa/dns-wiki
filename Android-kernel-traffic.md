:exclamation: There is absolute no malware/trojan/spyware/bloatware/whatever within UID 1000/0, please stop mention this or open bug reports with such questions. Android does not 'spy' in any way on you or your private data. Aftermarket firmware like CyanogenMod/MUI only have an opt-in/opt-out to send statistics to improve the product (but no personally information are been collected if it's enabled)! If you found any unknown or strange behavior, feel free to contact me directly (Nvinside [at] gmail.com) and I will look what it is (requires Wireshark/Burp logfile or tcpdump file). :exclamation: 

:warning: On Windows I highly recommend to use Burp/HTTP Scoop/Fiddler for the [deep packet inspection](http://en.wikipedia.org/wiki/Deep_packet_inspection) instead of Wireshark for several reasons, one of them is that it's low-level and overpowered for quickly looking through HTTP(s) traces [but you're the boss].

![This is a typical Kernel request](http://i.imgur.com/yYjfyuLl.png)

As you might know, Android's Kernel is based on the Linux Kernel so the output could be similar. This article will clear some questions about the Android Kernel and his data usage and also may answer some addition questions like why is there traffic under _Android OS_ (UID = 1000) or other UID's.

Most Internet applications are using TCP as their protocol of choice, and TCP maintains a connection bound to an IP address. Whenever you change your Internet access, you switch IP addresses all existing TCP connections will be vanish. Your downloads are aborted, your SSH connections are closed,[...].

**Important**:
* [WifiManager](http://developer.android.com/reference/android/net/wifi/WifiManager.html)
* [Network Interface](http://developer.android.com/reference/java/net/NetworkInterface.html)
* [Java SE NetworkInterface](http://docs.oracle.com/javase/6/docs/api/java/net/NetworkInterface.html)
* To get list of available interfaces use NetworkInterface#getNetworkInterfaces: <code>NetworkInterface.getNetworkInterfaces()</code>
* [NetworkConnectivityListener](http://android.git.kernel.org/?p=platform/frameworks/base.git;a=blob;f=core/java/android/net/NetworkConnectivityListener.java)
* [ConnectivityService.java](http://android.git.kernel.org/?p=platform/frameworks/base.git;a=blob;f=services/java/com/android/server/ConnectivityService.java)
* [android_filesystem_config.h](https://android.googlesource.com/platform/system/core.git/+/master/include/private/android_filesystem_config.h)
* [Legit traffic](#legit-traffic)


Index
-----

* [Battery](#battery)
* [Known ports](#known-ports)
* [Interface statistics](#interface-statistics)
* [Routing and ARP tables](#routing-and-arp-tables)
* [Protocol statistics](#protocol-statistics)
* [How do I use this information?](#how-do-i-use-this-information-?)
* [Capture all network traffic](#capture-all-network-traffic)
* [Reduce traffic](#reduce-traffic)
* [Tethering data](#tethering-data)
* [Modem traffic](#modem-traffic)
* [Closing words](#closing-words)
* [Useful links](#useful-links)


Battery
-------

The battery draining aspect is a widely known myth. The kernel doesn't need much energy since this is very well development and optimized for Android - **the main question is why VPN/Firewall/OS apps _drain_ your battery without much running background processes if internet is on**. Well, I'm not explaining everything but the most important reason is the _keepalive_ problem (this is only one thing!). This is a server <-> client problem. The server sends every x seconds a package to the client, mostly every 10 or 15 seconds. The packages are very small and consuming not much traffic BUT that always means that there is traffic which coasts overall in a month a lot of bandwidth (even with that small packages due the mass of it!).

To reduce the client side keepalive time is a bad idea, this will course troubles on e.g. UDP (because the packages are bigger and they also need more time). Because of the same reason you should also not reduce this on the server, using 60 seconds as interval (default) works without bigger problems and not break something.

See also:
* [Optimizing Downloads for Efficient Network Access | Android Developers](http://developer.android.com/training/efficient-downloads/efficient-network-access.html) & [this](http://developer.android.com/training/monitoring-device-state/)
* [Understanding the Android "Radio State Machine" for better life | Stackoverflow](http://stackoverflow.com/questions/19141050/understanding-the-android-radio-state-machine-for-better-battery-life)
* [Why TCP Over TCP Is A Bad Idea | sites.inka](http://sites.inka.de/bigred/devel/tcp-tcp.html)
* [networking - Under what circumstances is TCP-over-TCP performing significantly worse than tcp | Serverfault](http://serverfault.com/questions/630837/under-what-circumstances-is-tcp-over-tcp-performing-significantly-worse-than-tcp)


Known ports
-------

This is a small list which ports are open on every Linux/Android system, this is normally not critical as long there is no high traffic on it but you may need to understand what they do and how they should work. Remember: Closing all port's isn't a good idea (and in fact you not close the port you only disabling the application listening on it) since this will break most of internet stuff, but as long there are no suspect activity's all is good. For example one of a hacker goal is to send large volumes of network traffic at a host in order to cause legitimate traffic to be dropped, normally this behavior exists if the attacker doesn't control enough bandwidth himself to exceed the target's bandwidth. As a result you'll see that your sever or address is unreasonable over the network. 

* <code>Port 53</code> (tcp/udp) - Port 53 is used by well known DNS (Domain Name System). DNS takes care of resolving human readable 'host names' into numeric IP addresses. A commonly used DNS server called BIND has had a rich history of security problems. As a result, [BIND](http://bindguard.activezone.de/) and port 53 are frequent targets and a couple worms used BIND exploits to propagate. There are [several attacks](https://www.dns-oarc.net/wiki/mitigating-dns-denial-of-service-attacks), like DNS resolver/recursive, poisoning, DOS, worms, ...
* <code>Port 80</code> (tcp/udp) - Hypertext Transfer Protocol (HTTP) - BOOTP (for Android OS + MEID detection) 
* <code>Port 123</code> (udp) - Network Time Protocol (NTP)
* <code>Port 443</code> - Hypertext Transfer Protocol over SSL/TLS (HTTPS)
* <code>Port 5552</code> - Often used by messenger like WhatsApp, GTalk (Jabber)
* Anything what cause <code>SYNC_SENT/BIND</code> (which shows traffic on some roms/apps in the notification bar) in fact that does not cause any real traffic and your provider does not log such traffic (no traffic lost).
* These are all known ports which may cause or show traffic even if nothing is started or have any active internet connection (also keepalive/SYN/FINISH).
* You'll need tools like <code>tcpdump/wireshark/burf</code> to really see what's going on behind the scenes since you mostly see symptoms on your device.
* <code>Standard Ports</code> ([list](http://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers)), Android mostly use only the mentioned ports for security reasons.

**Remember**: Some Messenger, services and apps need root (0) enabled in AFWall+. _The question is why?_ - _Answer_: E.g. Threema use GCM (Google Cloud Messaging) or ( if no Google Play Services is installed as alternative polling), which working both via external services like Google Play Services (or there own implemented polling) so if root is blocked this means GCM/polling can connect via netd (you'll see a red icon in Threema). WhatsApp fix such behavior with external ports and services (so in this case root 0 isn't necessary) - and this is the reasons why root 0 (Android OS/Play Services) sometimes shows huge traffic, in fact it does not generate any traffic itself, it's called by external apps like Threema.

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
 


Protocol statistics
-------

`/proc/net/snmp` can exports protocol statistics (SNMP - Simple Network Management Protocol) and provides a summary stats for each of the IP, ICMP, TCP, and UDP protocol.



UIDs stats
providing traffic usage per application.



```
AID_ROOT             0  /* traditional unix root user */

AID_SYSTEM        1000  /* system server */

AID_RADIO         1001  /* telephony subsystem, RIL */
AID_BLUETOOTH     1002  /* bluetooth subsystem */
AID_GRAPHICS      1003  /* graphics devices */
AID_INPUT         1004  /* input devices */
AID_AUDIO         1005  /* audio devices */
AID_CAMERA        1006  /* camera devices */
AID_LOG           1007  /* log devices */
AID_COMPASS       1008  /* compass device */
AID_MOUNT         1009  /* mountd socket */
AID_WIFI          1010  /* wifi subsystem */
AID_ADB           1011  /* android debug bridge (adbd) */
AID_INSTALL       1012  /* group for installing packages */
AID_MEDIA         1013  /* mediaserver process */
AID_DHCP          1014  /* dhcp client */
AID_SDCARD_RW     1015  /* external storage write access */
AID_VPN           1016  /* vpn system */
AID_KEYSTORE      1017  /* keystore subsystem */
AID_USB           1018  /* USB devices */
AID_DRM           1019  /* DRM server */
AID_MDNSR         1020  /* MulticastDNSResponder (service discovery) */
AID_GPS           1021  /* GPS daemon */
AID_UNUSED1       1022  /* deprecated, DO NOT USE */
AID_MEDIA_RW      1023  /* internal media storage write access */
AID_MTP           1024  /* MTP USB driver access */
AID_UNUSED2       1025  /* deprecated, DO NOT USE */
AID_DRMRPC        1026  /* group for drm rpc */
AID_NFC           1027  /* nfc subsystem */
AID_SDCARD_R      1028  /* external storage read access */
AID_CLAT          1029  /* clat part of nat464 */
AID_LOOP_RADIO    1030  /* loop radio devices */
AID_MEDIA_DRM     1031  /* MediaDrm plugins */
AID_PACKAGE_INFO  1032  /* access to installed package details */
AID_SDCARD_PICS   1033  /* external storage photos access */
AID_SDCARD_AV     1034  /* external storage audio/video access */
AID_SDCARD_ALL    1035  /* access all users external storage */
AID_LOGD          1036  /* log daemon */
AID_SHARED_RELRO  1037  /* creator of shared GNU RELRO files */
AID_DBUS          1038  /* dbus-daemon IPC broker process */

AID_SHELL         2000  /* adb and debug shell user (below >4.3 .txt,.png and such may "create" traffic */
AID_CACHE         2001  /* cache access */
AID_DIAG          2002  /* access to diagnostic resources */

//The range 2900-2999 is reserved for OEM, and must never be used here
AID_OEM_RESERVED_START 2900
AID_OEM_RESERVED_END   2999

The 3000 series are intended for use as supplemental group id's only.
They indicate special Android capabilities that the kernel is aware of.
AID_NET_BT_ADMIN  3001  /* bluetooth: create any socket */
AID_NET_BT        3002  /* bluetooth: create sco, rfcomm or l2cap sockets */
AID_INET          3003  /* can create AF_INET and AF_INET6 sockets */
AID_NET_RAW       3004  /* can create raw INET sockets */
AID_NET_ADMIN     3005  /* can configure interfaces and routing tables. */
AID_NET_BW_STATS  3006  /* read bandwidth statistics */
AID_NET_BW_ACCT   3007  /* change bandwidth statistics accounting */
AID_NET_BT_STACK  3008  /* bluetooth: access config files */

// The range 5000-5999 is also reserved for OEM, and must never be used here
AID_OEM_RESERVED_2_START 5000
AID_OEM_RESERVED_2_END   5999

AID_EVERYBODY     9997  /* shared between all apps in the same profile */
AID_MISC          9998  /* access to misc storage */
AID_NOBODY        9999  /* Nobody */

AID_APP          10000  /* first app user */

AID_ISOLATED_START 99000 /* start of uids for fully isolated sandboxed processes */
AID_ISOLATED_END   99999 /* end of uids for fully isolated sandboxed processes */

AID_USER        100000  /* offset for uid ranges for each user */

AID_SHARED_GID_START 50000 /* start of gids for apps in each user to share */
AID_SHARED_GID_END   59999 /* start of gids for apps in each user to share */

// https://android.googlesource.com/platform/system/core/+/master/include/private/android_filesystem_config.h
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


About DNS traffic:

It's an UDP based protocol and each packet is around ~62 bytes per request, which comes in only a few packets. Means if you have an app that does generate frequent DNS lookups, these can add up to a fair amount of data usage (dependency how many requests it uses).

This is how DNS requests looks like in a firewall (iptables based) like Avast, AFWall+ or Avast:

```
Chain afwall (1 references)
pkts bytes target     prot opt in     out     source               destination
    0     0 RETURN     udp  --  *      *       0.0.0.0/0            0.0.0.0/0
         udp dpt:53
```

They're quite a few types of packets that are reported by the "data usage" app as part of the network traffic of "Android OS". Among others DNS requests belong to these kind of traffic. 

But it's wrong:

UID=1000 belongs to an Android component that is usually called "Android OS", but DNS requests come from a process running as root (UID=0 [root]) so if you see "Android OS" in an app list, it can mean a root process or an UID=1000 process as well.


Reduce traffic
-------

There are some common 'tricks' to reduce the entire OS traffic.

* Do not use sync or any account and no Google apps (in fact that possible lowers the fun since mostly every app depending on it, for push and and and).
* Not install 200+ apps you never need (or only once). Less apps -> possible less data leakages due ads, infections and more.
* In fact 'secure' protocols need more traffic due handshakes (we are talking about some byte). So to say that you better use old or compromised protocols instead is horrible wrong.
* Use AdBlock/AdAway to block Domains you not need.
* Capture the traffic and use Tools like tcpdump (included in every AOSP or e.g. AdAway) to see which Domains your OS wants to connect.
* Do not block everything, because to block mtalk and such things could lead in much troubles (no signal and and and) it's not worth to waste the time to debug the problems because of some kB/mb's).
* Use fly-mode/block mobile data as much as possible and use wlan instead.
* Uninstall/Disable/Freeze Cell Broadcast which informs you about local warnings


So for Google users (gapps) there are some 'special tricks':
* Open up the 'Google settings' in your launcher, you see several Options like security and many more (depending on which Google Play Services Version you use) and uncheck every security mechanism which sends every sha1 to Google which checks your apps, similar to safe-browsing. There is also an option to let Google check your apps (means new installed ones) if present also disable it.
* Google Play Store sends/check your apps because license verification (every 24/48) hours, These are only some kB (which can't or shouldn't be blocked). Even if you disabled the above mentioned security option's if will check/send the sha1 of your new installed apps anyway to Google (no option to disable it).
* Uncheck all Google Play Store notification options.
* Check your telephone.apk/dialer and surf the advanced option, some ROM's use a backtrace option to reveal more Information about your contact which calls you, disable it.
* Use as less as possible 'push' apps or if possible disable the push/notification option. E.g. on Google+ app. because every push notification related app constantly send/recieve a small amount of data each xyz minutes.
* Under develop options (must be manually enaled first) or advance option some roms have a switch to cut of OS statistics. If present also cut off the bugreport function and enable it only if you need it.
* Try e.g. with privacy guard or external tools to cut off some functions e.g. the ads reciever (but be careful it could crash the app).
* Only enable the sync and Google backup option if you are on wifi.

Mostly you are done, now you see only 2-6 MB (depending on your usage) traffic, this is normal because some 'security' mechanism can't be disabled via the visible interface. I will explain (for experts) to also cut this off.


How do I use this information?
-------

~~- It's not done, so atm it only shows the important parts about kernel generated traffic. -~~ 


Legit traffic
-------

The OS itself _collects_ the following data (aka Android OS):
* Google Safe-browsing
* APK checksums (no opt-out) even if you disabled it within Google Play services it's been collected by Google Play Store and/or Google Play Services 
* (*optional*) if _push_ apps installed Google Play services uses it to get notification e.g. WhatsApp, same with other services like Anti-Theft 
* Location
* TLS certificate checks
* Time synchronization (NTP) on UDP 123, also needed for TLS certificates to stay in sync
* Cell Broadcasts (can be uninstalled/deactivated)
* Statistics about the device (no opt-out)
* Google Play Services checks for license verification (once every 24 hours)
* Android OS update check 
* Android WebView update check (also for Google Play Store and Google Play Services)
* Android debug infos (opt-out, default disabled)
* Ads-services, depending if any app requires/ask for it (also depending what aps did you have installed, mostly free apps using 'ads' or asking for this. It's not always visible within the app itself or Android traffic overview [you see mostly Android OS or Google Play Services traffic].
* (*optional*) CM Updater (if CyanogenMod is installed -> background data can be restricted).
 

Closing words
-------

Internet security is hard. Let's go bake some cookies!


Useful links
-------

* [ifconfig |Wikipedia](http://en.wikipedia.org/wiki/Ifconfig) (or if not present use <code>netcfg</code> - but's limited!)
* [Domain Name System (DNS) | Wikipedia](http://en.wikipedia.org/wiki/Domain_Name_System)
* [Android Data Usage | Source Android](https://source.android.com/devices/tech/datausage/index.html)
* [Android Git repositories | Android Google Source](https://android.googlesource.com/) (iptables, kernel,[...])
* [Android Traffic Stats | Android Developer] (http://developer.android.com/reference/android/net/TrafficStats.html)
* [sysctl | kernel.org](https://www.kernel.org/doc/Documentation/networking/ip-sysctl.txt)
* [[MOD]Any phone, any ROM: Wi-Fi only mode (disable cell radio) | RootWiki.com](http://rootzwiki.com/topic/25016-modany-phone-any-rom-wi-fi-only-mode-disable-cell-radio/)
* [Connectivitymanager in Android SDK | MSDN Blogs](http://blogs.msdn.com/zhengpei/archive/2009/09/22/connectivitymanager-in-android-sdk.aspx)
* [Robtex Swiss Army Knife Internet Tool | Robtex.com](https://www.robtex.com/)
* [MobileApp-Pentest-Cheatsheet by tanprathan | GitHub.com](https://github.com/tanprathan/MobileApp-Pentest-Cheatsheet)

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
* Tools which make life easier to inspect Android OS traffic directly on the device (most tools are way outdated/deprecated, like OS Monitor, Shark, ... or they simply not work on Android 5 because it needs to be recompiled with the Position Indipendent Executables (-pie or on libs -fPIC) flag
```
