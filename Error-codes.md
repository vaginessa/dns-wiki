### Error applying iptables rules. Exit code: -1
This error usually means that the kernel does not support the iptables owner match kernel module. AFWall+ will unfortunately not work on your ROM.

But! there are some workarounds for this. First try to change the iptables binary from preferences from "Built-in" to "System". In most cases this works on newer ROM's.



Or manually via Terminal Emulator

//Switch to root

su

//Remount the system as rw (read / write enabled)

mount -o remount,rw /system

//Add syslink IPtable to the android.tether directory

cp /data/data/android.tether/bin/iptables /system/bin

//Remount the system as ro! (read only)

mount -o remount,ro -t yaffs2 /dev/block/mtdblock3 /system



### Error 127 – “iptables: not found”
This error happens because iptables user-space software application that allows the system administrator to configure the tables provided by the Linux kernel firewall and the chains and rules for storage, is not an operating system Android OS.

> Try the same method as described above.