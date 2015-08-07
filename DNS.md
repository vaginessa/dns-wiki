Index
-----

* [Description](#description)
* [DNS under Android](#dns-under-android)
* [Already reported DNS problems](#already-reported-DNS-problems)
* [Resolver commands](#resolver-commands)
* [Commands to check if DNS is working](#commands-to-check-if-dns-is-working)
* [Browser](#browser)
* [Apps to change the current DNS](apps-to.change-the-current-dns)
* [Useful links](#useful links)

Description
-----------

Nothing yet, will be imported soon,... 


DNS under Android
-----------

By default the Google DNS server is set (8.8.8.8/8.8.4.4), currently the DNS servers gets overridden after each reboot, IP change or reconnection (connectivity changes -> RIL via e.g. ndc resolver setifdns rmnet0 x.x.x.x y.y.y.y). 


Already reported DNS problems
-----------

An easy method to look at opened or closed threads is to search via <code>is:issue is:open dns</code> / <code>is:issue is:closed dns</code> which shows the important threads (if the topic/thread content was correct labaled), alternative just click on the follow links (or copy/paste the issue number in the search e.g. <code>https://github.com/ukanth/afwall/issues/<insert-number-here>).


Already reported DNS related topics:
* #377 [DNS (port 53) is blocked for Wifi tethering](https://github.com/ukanth/afwall/issues/377)
* #344 [Wi-Fi-Hotspot not working while AFWall+ is enabled](https://github.com/ukanth/afwall/issues/344)
* #326 [Add mDNS support for local networks](https://github.com/ukanth/afwall/issues/326)
* #318 [Show host names in log view](https://github.com/ukanth/afwall/issues/318)
* #257 [USB/Bluetooth Tethering fails on CM11 mako](https://github.com/ukanth/afwall/issues/257)
* #209 [Bluetooth tethering borked as well](https://github.com/ukanth/afwall/issues/209)
* #206 [DNS Requests fail](https://github.com/ukanth/afwall/issues/206)
* #178 [Tethering](https://github.com/ukanth/afwall/issues/178)
* #18  [UDP 53 bypass because logging & whitelisting are enabled](https://github.com/ukanth/afwall/issues/18)


**Important**: Please always use the search function here on AFWall's/AOSP issue tracker (second link), to search already known existent problems to avoid duplicate threads. 


Resolver commands
-----------

## Android 4.0.4
http://androidxref.com/4.0.4/xref/system/netd/CommandListener.cpp#778

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`

## Android 4.1.1
http://androidxref.com/4.1.1/xref/system/netd/CommandListener.cpp#803

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`

## Android 4.2_r1
http://androidxref.com/4.2_r1/xref/system/netd/CommandListener.cpp#873

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`

## Android 4.3_r2.1
http://androidxref.com/4.3_r2.1/xref/system/netd/CommandListener.cpp#770

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <domains> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`
* `ndc resolver setifaceforpid <iface> <pid>`
* `ndc resolver clearifaceforpid <pid>`

## Android 4.4_r1
http://androidxref.com/4.4_r1/xref/system/netd/CommandListener.cpp#941

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <domains> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`
* `ndc resolver setifaceforpid <iface> <pid>`
* `ndc resolver clearifaceforpid <pid>`
* `ndc resolver setifaceforuid <iface> <l> <h>`
* `ndc resolver clearifaceforuid <l> <h>`
* `ndc resolver clearifacemapping`

## Android 4.4.2_r1
http://androidxref.com/4.4.2_r1/xref/system/netd/CommandListener.cpp#942

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <domains> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`
* `ndc resolver setifaceforpid <iface> <pid>`
* `ndc resolver clearifaceforpid <pid>`
* `ndc resolver setifaceforuidrange <iface> <l> <h>`
* `ndc resolver clearifaceforuidrange <l> <h>`
* `ndc resolver clearifacemapping`

## Android 4.4.3_r1.1
http://androidxref.com/4.4.3_r1.1/xref/system/netd/CommandListener.cpp#940

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <domains> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`
* `ndc resolver setifaceforpid <iface> <pid>`
* `ndc resolver clearifaceforpid <pid>`
* `ndc resolver setifaceforuidrange <iface> <l> <h>`
* `ndc resolver clearifaceforuidrange <if> <l> <h>`
* `ndc resolver clearifacemapping`

## Android 4.4.4_r1
http://androidxref.com/4.4.4_r1/xref/system/netd/CommandListener.cpp#940

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <domains> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`
* `ndc resolver setifaceforpid <iface> <pid>`
* `ndc resolver clearifaceforpid <pid>`
* `ndc resolver setifaceforuid <iface> <l> <h>`
* `ndc resolver clearifaceforuid <if> <l> <h>`
* `ndc resolver clearifacemapping`

## Android 5.0.0_r2
http://androidxref.com/5.0.0_r2/xref/system/netd/server/CommandListener.cpp#776

* `ndc resolver setnetdns <netId> <domains> <dns1> <dns2> ...`
* `ndc resolver flushnet <netId>`

## Android 5.1.0_r1
http://androidxref.com/5.1.0_r1/xref/system/netd/server/CommandListener.cpp#791

* `ndc resolver setnetdns <netId> <domains> <dns1> <dns2> ...`
* `ndc resolver clearnetdns <netId>`
* `ndc resolver flushnet <netId>`

## Master tree (latest versions [08.07.2015](https://android.googlesource.com/platform/system/netd/+/master/server/CommandListener.cpp#805) checked)
* `ndc resolver setnetdns <netId> <domains> <dns1> <dns2> ...` 
* `ndc resolver clearnetdns <netId>`
* `ndc resolver flushnet <netId>`

Commands to check if DNS is working
---------


The following may are nessary to indicate if all is working (dhcp/nameserver/dnsmasq,...), may needs to be changed for your interfaces you want to check: cellular, tethered, ...

* Grep the current DNS resolver/settings, reads them via: <code>adb shell getprop | grep dns</code>
* Initial check via <code>adb shell dumpsys connectivity</code> or <code>adb shell dumpsys connectivity | grep DnsAddresses</code>
* Via nslookup <code>nslookup google.com</code>
* See the current dhcp info <code>cat /system/etc/dhcpcd/dhcpcd.conf</code>
* List the tethered dns configuration <code>adb logcat | egrep '(TetherController|dnsmasq)'</code>
* Check routing via <code>ip ru</code> + <code>ip route ls</code>
* On Bluetooth tethering <code>ip addr show bt-pan<code> (optional)
* 

Browser
---------

Working without any manual adjustments:
* Dolphin 
* Naked Browser
* Firefox
* ....

Needs changes in the settings:
* Chrome -> Clean DNS cache ....
* Firefox -> for Tor/Orbot ....
* ...

Apps to change the current DNS
-----------

The following apps are success tested on all systems to work:
* OverrideDNS (paid)
* ... add more, remember they must work on all systems (even 5.1.1/M)


Useful links
-----------
* [Latest AOSP netd version (source code) | Android.GoogleSource.com](https://android.googlesource.com/platform/system/netd/+/master/)
* [List common DNS issues (read-only) | Google Issue Tracker](https://code.google.com/p/android/issues/list?can=2&q=DNS&colspec=ID+Type+Status+Owner+Summary+Stars&cells=tiles)
* [DNS (local) resolution on Android Lollipop #79504 | Android Open Source Project - Issue Tracker](https://code.google.com/p/android/issues/detail?id=79504)
* [Android 5 broke tethering (DNS REFUSED) #82545 | Android Open Source Project - Issue Tracker](https://code.google.com/p/android/issues/detail?id=82545)
* [NsdManager | Android Developers.com](http://developer.android.com/reference/android/net/nsd/NsdManager.html)
* [Microsoft KB99686 – Enabling IP Routing | Support.Microsoft.com](https://support.microsoft.com/en-us/kb/99686)



Todo:
* <s>Complete the missing parts </s>
* Link all dns related stuff in this thread (e.g. from the FAQ)
* <s>Add several workarounds since newer systems ignoring the etc/resolver.conf or dhcpcd/dhcpcd-hooks/20-dns.conf files</s>, explained with given links
* Add AFWall+ workarounds via custom scripts or separate tips
* Possible explain why Android 4.4.+/5+ wants to call the RIL with RIL_REQUEST_SETUP_DATA_CALL <netcfg dhcp iface>
* Add example output how it should looks like, really?
* Explain the broken MTU (saw this on so many roms) and list this bug (see ConnectivityService), since Android 5.1.x always seems to use 1500 regardless of what value has dhcp daemon set in option 26 (call interface_mtu).