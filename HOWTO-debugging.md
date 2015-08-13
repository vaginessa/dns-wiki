Index
-----

* [Description](#description)
* [Debugging](#debugging)
* [ADB](#adb)
* [Useful links](#useful-links)

Description
-----------

This section was designed to advanced user, developers or users that having _Special_ problems e.g. on connectivity related things or simply want to debug AFWall+ quick. 

Debugging
---------

To get all the logcat messages by AFWall+ you could run:

<code>adb logcat | grep `adb shell ps | grep dev.ukanth.ufirewall | cut -c10-15`</code>

AFWall+ itself indicates as AFWall (see TAG) in the logcat.

See also the [Android Gradle user guide](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Testing)
for more details, including how to use Android Studio to run tests (which provides more useful feedback than the command line).


ADB
---------

Of course the following commands can be changed per needs, the interfaces or paths could be different. 

```bash
# Example PCap a TCPDump wlan0 interface to external sdcard (e.g. DNS, 443 or ICMPv6)
adb shell tcpdump -ni wlan0 -U -w /sdcard/dump.pcap port 53 or port 443 or icmp6"

# Example PCap on data/local dir that captures a specific host
tcpdump -i br-lan -s0 -U -w /data/local/etherhost.pcap "ether host 34:xx:xx:xx:19:cb

# Example TCPDump that capture everything on ICMP6 
tcpdump -ni any -s0 -U -w /sdcard/icmp6.pcap icmp6

# Dumpsys connectivity tracker (shows a general overview of all connected devices and advanced network output)
adb shell dumpsys connectivity 
#or - does the same except the --diag tag which identify things for error resolving reasons
adb shell dumpsys connectivity --diag

# NDC sets an dns resolver (100) to a specific host
ndc resolver setnetdns 100 "" 192.168.1.1

# Greps on the wlan0 interface the current connection status 
ip a | grep "wlan0" -A10

# Check IPv6 (just only for test reasons you should against this sites because 99,99% uptime)
https://ipv6.google.com/
# or IPv4
https://ipv4.google.com/

# Ping (two times) the IPv4 Alphabet/Google 
adb shell ping6 -c2 ipv4.google.com
```

Useful links
---------

* [AFWAll+ Error codes](https://github.com/ukanth/afwall/wiki/Error-codes)


ToDO: 
* Explain all commands, really? 
* Sort the commands - meh! Maybe own sections for TCPDUmp, dumpsys, ping, whatever ...