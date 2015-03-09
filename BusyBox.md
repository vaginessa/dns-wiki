Index
-----

* [Description](#description)
* [How do I configure Busybox?](#config-busybox)
* [Difference between SELinux and non-SELinux BusyBox](#difference-between-selinux-and-non-selinux-busybox)
* [Useful links](#useful-links)

Description
-----------

**BusyBox combines tiny versions of many common UNIX utilities into a single small executable.**  
It provides minimalist replacements for most of the utilities you usually find in bzip2, coreutils, dhcp, diffutils, e2fsprogs, file, findutils, gawk, grep, inetutils, less, modutils, net-tools, procps, sed, shadow, sysklogd, sysvinit, tar, util-linux, and vim.  The utilities in BusyBox often have fewer options than their full-featured cousins; however, the options that are included provide the expected functionality and behave very much like their larger counterparts.

BusyBox has been written with size-optimization and limited resources in mind, both to produce small binaries and to reduce run-time memory usage. Busybox is also extremely modular so you can easily include or exclude commands (or features) at compile time.  This makes it easy to customize embedded systems; to create a working system, just add /dev, /etc, and a Linux kernel. Busybox (usually together with uClibc) has also been used as a component of "thin client" desktop systems, live-CD distributions, rescue disks, installers, Android, and so on.

__Short description__
* Root required.
* Provides many standard Unix tools.
* Necessary in AFWall+ to execute some Linux commands.
* AFWall+ comes with a Busybox binary, so you don't have to install Busybox manually anymore.
* There are also some _non-root_ solutions on the Google Play Store, we not recommend to use one of this!

How do I configure Busybox?
---------------------------

The most important Busybox configurations are:

make <code>defconfig</code> - Create the maximum "sane" configuration, **AFWall+ use this as default**. This enables almost all features, minus things like debugging options and features that require changes to the rest of the system to work (such as selinux or devfs device names). Use this if you want to start from a full-featured Busybox and remove features until it's small enough.

make <code>allnoconfig</code> - Disable everything. This creates a tiny version of Busybox that doesn't do anything. Start here if you know exactly what you want and would like to select only those features.

make <code>menuconfig</code> - Interactively modify a .config file through a multi-level menu interface. Use this after one of the previous two.
Some other configuration options are:

make <code>oldconfig</code> - Update an old .config file for a newer version of Busybox.

make <code>allyesconfig</code> - Select absolutely everything. This creates a statically linked version of Busybox full of debug code, with dependencies on selinux, using devfs names... This makes sure everything compiles. Whether or not the result would do anything useful is an open question.

make <code>randconfig</code> - Create a random configuration for test purposes.


Difference between SELinux and non-SELinux BusyBox
-------------

The SELinux (NSA security enhanced Linux) binary comes with the following extra utilities: _chcon_, _getenforce_, _getsebool_, _load_policy_, _matchpathcon_, _restorecon_, _runcon_, _selinuxenabled_, _setenforce_, _setfiles_, _setsebool_, and _sestatus_. There are also some selinux flags enabled for applets such as "ps" and "ls", e.g. "ps -Z" and "ls -Z" to show the context for processes or files. If you are using Android 4.3 or higher, then you probably want to use the SELinux-enabled BusyBox since Android 4.3 is when SELinux was introduced to Android. Using the SELinux BusyBox on older version of Android without SELinux file structure should probably work besides the SELinux applets. The non-SELinux binary can be used on any version of Android. When it comes down to it, the system actually uses "/system/bin/toolbox" SELinux applets for SELinux operations, so unless you specifically want to use BusyBox's SELinux tools for personal use, the safest option is to go with the non-SELinux BusyBox. 

Useful links
-------------

* [Official BusyBox FAQ | BusyBox.net](http://www.busybox.net/FAQ.html)
* [BusyBox Download | BusyBox.net](http://busybox.net/downloads/)
* [BusyBox Download | F-Droid](https://f-droid.org/wiki/page/stericson.busybox)
* [Browse the source code |git.busybox.net](http://git.busybox.net/busybox/)
* [Anonymous GIT access | BusyBox.net](http://www.busybox.net/source.html)
* [Bug Tracker | Bugs.BusyBox.net](https://bugs.busybox.net)
* [BusyBox Installer | Google Play Store](https://play.google.com/store/apps/details?id=com.jrummy.busybox.installer)
* [busybox-free | Stericson - GitHub](https://github.com/Stericson/busybox-free)
