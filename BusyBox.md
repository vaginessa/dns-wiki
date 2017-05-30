Index
-----

* [Description](#description)
* [Installation instructions](#installation-instructions)
* [How do I configure BusyBox?](#how-do-i-configure-busybox-?)
* [Difference between SELinux and non-SELinux BusyBox](#difference-between-selinux-and-non-selinux-busybox)
* [Useful links](#useful-links)

Description
-----------

**BusyBox combines tiny versions of many common UNIX utilities into a single small executable.**  
It provides minimalist replacements for most of the utilities you usually find in _bzip2_, _coreutils_, _dhcp_, _dhcpv6_, _diffutils_, _e2fsprogs_, _file_, _findutils_, _gawk_, _grep_, _inetutils_, _less_, _modutils_, _net-tools_, _procps_, _sed_, _shadow_, _sysklogd_, _sysvinit_, _tar_, _util-linux_ and _vim_.  The utilities in BusyBox often have fewer options than their full-featured cousins; however, the options that are included provide the expected functionality and behave very much like their larger counterparts.

BusyBox has been written with size-optimization and limited resources in mind, both to produce small binaries and to reduce run-time memory usage. Busybox is also extremely modular so you can easily include or exclude commands (or features) at compile time.  This makes it easy to customize embedded systems; to create a working system, just add /dev, /etc, and a Linux kernel. Busybox (usually together with uClibc) has also been used as a component of "thin client" desktop systems, live-CD distributions, rescue disks, installers, Android, and so on.

__Short description__
* Root required
* Provides many standard Unix tools
* Necessary in AFWall+ to execute some Linux commands
* AFWall+ comes with an integrated BusyBox binary, so you don't have to install BusyBox manually anymore
* There are also some _non-root_ solutions on the Google Play Store, we do not recommend use of these!

Installation instructions
---------------------------

Normally BusyBox is included in your ROM and AFWall+ also comes with his own, but on some Stock (OEM Firmwares) and AOSP (Android Open Source Project) based ROMs it is possible that this isn't integrated (Androids own toolbox will be used instead). 

In this case:
1) Open AFWall+ 
2) In the main menu tap on 'Preferences'
3) Now tap on 'Binaries' 
4) Tap on 'BusyBox Binary' and select '_Build-in BusyBox_'

If you like to manually update your system own binary, because it getting an update or for other reasons, you can use the external BusyBox app (the name is a bit 'wrong/confusing' since it isn't a BusyBox app - it's more an app which integrates a binary (called _busybox_) that will be installed/integrated in your system) or you can just use your own (or pre) compiled binary explained over [here](https://github.com/ukanth/afwall/wiki/HOWTO-Compiling-busybox).

With the BusyBox app make sure all symlinks are proper set to /xbin (system/xbin) this will normally automatically set by this app (all shows green and you see small check marks).

But if you use any external or own compiled binary, you can simple use the following commands via adb:
```bash
mount -o remount,rw /system
cp /sdcard/busybox /system/xbin/busybox
chmod 755 /system/xbin/busybox
```

All BusyBox binaries also provide a simply install setup if you binary was already copied into _xbin_ you can simply use <code>busybox --install /system/xbin</code>. On older ROMs like 2.3 or 3.x this needs to be changed to _bin_ instead _xbin_ directory.

On the recovery TWRP you can do this via (CWM does not include any file manager!):
```
Go to 'Mount'-> tick 'system'
Go back, then enter the 'File Manager' under 'Advanced' -> 'File Manager'
Go to '_/sdcard/_' (internal sometimes also called sdcard0), tap on the 'busybox' file 
Tap 'copy'
Navigate to '_/system/xbin_'
Tap 'Select'
Navigate again to '_/system/xbin_'
Tap on your 'busybox' binary
Tap "chmod 755" (this will change the permissions to the correct one)
Done
```

Some binaries are also flashable (means you can select your .zip under TWRP and flash that, sometimes that can be problematically if the permissions are not proper set or the system shows the wrong symlinks (means you will see strange and cryptographic chars in the SuperSU log. 

How do I configure BusyBox?
---------------------------

**This is not necessary, but important for developers and advanced users which want to compile there own BusyBox from the source**

The most important BusyBox configurations are:

make <code>defconfig</code> - Create the maximum "sane" configuration, **AFWall+ use this as default**. This enables almost all features, minus things like debugging options and features that require changes to the rest of the system to work (such as selinux or devfs device names). Use this if you want to start from a full-featured Busybox and remove features until it's small enough.

make <code>allnoconfig</code> - Disable everything. This creates a tiny version of Busybox that doesn't do anything. Start here if you know exactly what you want and would like to select only those features.

make <code>menuconfig</code> - Interactively modify a .config file through a multi-level menu interface. Use this after one of the previous two.
Some other configuration options are:

make <code>oldconfig</code> - Update an old .config file for a newer version of BusyBox.

make <code>allyesconfig</code> - Select absolutely everything. This creates a statically linked version of Busybox full of debug code, with dependencies on selinux, using devfs names... This makes sure everything compiles. Whether or not the result would do anything useful is an open question.

make <code>randconfig</code> - Create a random configuration for test purposes.


Difference between SELinux and non-SELinux BusyBox
-------------

The SELinux (NSA security enhanced Linux) binary comes with the following extra utilities: _chcon_, _getenforce_, _getsebool_, _load_policy_, _matchpathcon_, _restorecon_, _runcon_, _selinuxenabled_, _setenforce_, _setfiles_, _setsebool_, and _sestatus_. There are also some selinux flags enabled for applets such as "ps" and "ls", e.g. "ps -Z" and "ls -Z" to show the context for processes or files. If you are using Android 4.3 or higher, then you probably want to use the SELinux-enabled BusyBox since Android 4.3 is when SELinux was introduced to Android. Using the SELinux BusyBox on older version of Android without SELinux file structure should probably work besides the SELinux applets. The non-SELinux binary can be used on any version of Android. When it comes down to it, the system actually uses "/system/bin/toolbox" SELinux applets for SELinux operations, so unless you specifically want to use BusyBox's SELinux tools for personal use, the safest option is to go with the non-SELinux BusyBox. 

Useful links
-------------

* [Official BusyBox Download Documentation Page | BusyBox.net](http://busybox.net/downloads/)
* [Official BusyBox FAQ | BusyBox.net](http://www.busybox.net/FAQ.html)
* [Official Free Busybox | Stericson on GitHub](https://github.com/Stericson/busybox-free)
* [Unofficial Busybox On Rails](https://play.google.com/store/apps/details?id=me.timos.busyboxonrails&hl=en)
* [BusyBox (Free-Version) Download via F-Droid | F-Droid](https://f-droid.org/wiki/page/stericson.busybox)
* [BusyBox NDK | GitHub.com](https://github.com/tias/android-busybox-ndk)
* [Browse the git source code | git.busybox.net](http://git.busybox.net/busybox/)
* [Anonymous GIT access | BusyBox.net](http://www.busybox.net/source.html)
* [Bug Tracker | Bugs.BusyBox.net](https://bugs.busybox.net)
* [Official BusyBox Installer | Google Play Store](https://play.google.com/store/apps/details?id=com.jrummy.busybox.installer)

_ Final version 08.24.2015_