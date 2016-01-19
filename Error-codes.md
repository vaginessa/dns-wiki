Index
-----

* [Description](#description)
* [Error Codes](#error-codes)

Description
-----------

This page explains details about some error codes that AFWall+ may gave you during the normal or custom script usage. 

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

> IPTables can exit with status 4 if two or more processes tries to update the same table again and again. See [here](https://github.com/ukanth/afwall/commit/909934d9b69c99120cfb495e998d9795468bbfed) for detailed information.

<a name="Code4"></a>
**(4) Error applying iptables rules. Exit code: 6**

> Index of insertion too big - This mostly comes from custom ROM's (non CM). A workaround seems to play with the SuperSU settings (also try to update to latest first) and contact your ROM developer. In some situations it might help to play with AFWall+ internal binary options or to re-install busybox.

_ Final version 01.19.2016_