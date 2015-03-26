Index
-----

* [Description](#description)
* [Error Codes](#error-codes)
* [Useful links](#useful-links)

Description
-----------

This page should explain some details about some error codes that AFWall+ may gave you.

Error Codes
-----------

<a name="Code1"></a>
**(1) Error applying iptables rules. Exit code: -1**
> This error usually means that the kernel does not support the iptables owner match kernel module. AFWall+ will unfortunately not work on your ROM.
But, there are some workarounds for this. First try to change the iptables binary from _preferences_ from "Built-in" to "System". In most cases this works on newer ROM's.

> Or manually via adb shell or Terminal Emulator:

    su
    mount -o remount,rw /system
    cp /data/data/android.tether/bin/iptables /system/bin
    mount -o remount,ro /system

<a name="Code2"></a>
**(2) Error 127 – “iptables: not found”**

> This error happens because iptables user-space software application that allows the system administrator to configure the tables provided by the Linux kernel firewall and the chains and rules for storage, is not an operating system Android OS. **Please try the same method as described above.**

<a name="Code3"></a>
**(3) Error applying iptables rules. Exit code: 4**

> IPTables can exit with status 4 if two or more processes tries to update the same table again and again. 

Useful links
------------
* [AFWall+ Issue Tracker | GitHub.com](https://github.com/ukanth/afwall/issues)