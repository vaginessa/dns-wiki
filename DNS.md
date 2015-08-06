Index
-----

* [Description](#description)
* [DNS under Android](#dns-under-android)
* [Resolver commands](#resolver-commands)
* [Browser](#browser)
* [Notice](#notice)
* [Useful links](#useful links)

Description
-----------

<NOTHING> .. really?


DNS under Android
-----------

more nothing .... 


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

## Master (latest versions [08.07.2015](https://android.googlesource.com/platform/system/netd/+/master/server/CommandListener.cpp#805) checked)
* `ndc resolver setnetdns <netId> <domains> <dns1> <dns2> ...` 
* `ndc resolver clearnetdns <netId>`
* `ndc resolver flushnet <netId>`

Browser
---------

List/explain all top browsers that working out-of-the-box (without flushing the cache/changing the about:config), Dolphin browser should work out-of-the-box.

Notice
---------

Currently the DNS servers gets overridden after each reboot, IP change or reconnection (connectivity changes -> RIL via e.g. ndc resolver setifdns rmnet0 x.x.x.x y.y.y.y). 

Useful links
-----------
* [Latest AOSP netd version | Android.Googlesource.com](https://android.googlesource.com/platform/system/netd/+/master/)
* 



Todo:
* Complete the missing parts 
* Link all dns related stuff in this thread (e.g. from the faq)
* Add several workarounds since newer systems ignoring the etc/resolver.conf or dhcpcd/dhcpcd-hooks/20-dns.conf files
* Add AFWall+ workarounds via custom scripts or separate tips
* possible explain why Android 4.4.+ wants to call the RIL with RIL_REQUEST_SETUP_DATA_CALL <netcfg dhcp iface>
* .... 