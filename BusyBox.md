Index
-----

* [Description](#description)
* [How do I configure Busybox?](#config-busybox)
* [Useful links](#useful-links)

Description
-----------
BusyBox combines tiny versions of many common UNIX utilities into a single small executable.  
It provides minimalist replacements for most of the utilities you usually find in bzip2, coreutils, dhcp, diffutils, e2fsprogs, file, findutils, gawk, grep, inetutils, less, modutils, net-tools, procps, sed, shadow, sysklogd, sysvinit, tar, util-linux, and vim.  The utilities in BusyBox often have fewer options than their full-featured cousins; however, the options that are included provide the expected functionality and behave very much like their larger counterparts.

BusyBox has been written with size-optimization and limited resources in mind, both to produce small binaries and to reduce run-time memory usage. Busybox is also extremely modular so you can easily include or exclude commands (or features) at compile time.  This makes it easy to customize embedded systems; to create a working system, just add /dev, /etc, and a Linux kernel. Busybox (usually together with uClibc) has also been used as a component of "thin client" desktop systems, live-CD distributions, rescue disks, installers, and so on.

How do I configure Busybox?
---------------------------

The most important Busybox configurators are:
make <code>defconfig</code> - Create the maximum "sane" configuration, AFWall+ use this as default. This enables almost all features, minus things like debugging options and features that require changes to the rest of the system to work (such as selinux or devfs device names). Use this if you want to start from a full-featured Busybox and remove features until it's small enough.

make <code>allnoconfig</code> - Disable everything. This creates a tiny version of Busybox that doesn't do anything. Start here if you know exactly what you want and would like to select only those features.

make <code>menuconfig</code> - Interactively modify a .config file through a multi-level menu interface. Use this after one of the previous two.
Some other configuration options are:

make <code>oldconfig</code> - Update an old .config file for a newer version of Busybox.

make <code>allyesconfig</code> - Select absolutely everything. This creates a statically linked version of Busybox full of debug code, with dependencies on selinux, using devfs names... This makes sure everything compiles. Whether or not the result would do anything useful is an open question.

make <code>randconfig</code> - Create a random configuration for test purposes.


Useful links
-------------

* [Official BusyBox FAQ | BusyBox.net](http://www.busybox.net/FAQ.html)
* [BusyBox Download | BusyBox.net](http://busybox.net/downloads/)
* [Browse the source code |git.busybox.net](http://git.busybox.net/busybox/)
* [Anonymous GIT access | BusyBox.net](http://www.busybox.net/source.html)
* [Bug Tracker | Bugs.BusyBox.net](https://bugs.busybox.net)