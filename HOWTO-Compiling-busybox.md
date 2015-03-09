Index
-----

* [Description](#description)
* [Requirements](#requirements)
* [Compile](#Compile)
* [Steps for SELinux-enabled busybox](#steps-for-selinux-enabled-busybox)
* [Useful links](#useful-links)

Description
-----------

Please take a look over [here](https://github.com/ukanth/afwall/wiki/BusyBox).


Requirements
-----------
 
* Android Terminal app (latest .apk is avaible [here](http://jackpal.github.com/Android-Terminal-Emulator/downloads/Term.apk))
* [Complete Linux Installer](http://sourceforge.net/projects/linuxonandroid/?source=typ_redirect) or Linux Deploy
* [Latest BusyBox source](http://www.busybox.net/downloads/busybox-snapshot.tar.bz2)
* OS, Kali, Debian or any other Live bootable OS (of course Windows and MAC OS would also work but this guide is written for Ubuntu which works as his best with the mentioned 'Complete Linux Installer' under Android)


Compile
-----------

Open Android Terminal app and set the environment tools via:
<code>apt-get install -y gcc build-essential libncurses5-dev libpam0g-dev libsepol1-dev libselinux1-dev</code>



Now we extract our downloaded BusyBox source to our Ubuntu home with something like:
```
cd
tar -xf /sdcard/Download/busybox*bz2
cd busybox*

```


So, the basic is been setted up. We need to make a statically BusyBox build, which can be done if we gonna generate a Makefile via _make_ command. The _defconfig_ (Default configuration file) will be created via <code>make defconfig</code>. 


If all goes well, we now have a Makefile and are ready to compile:
<code>make clean && make LDFLAGS=-static</code>

Let "make" crank out the binary for a couple minutes. The extra variable we set after make is to compile statically. When compiling is complete we'll have a few different busybox binaries at the root of the source directory. We use the one named "busybox" since we're not debugging.


Now let's copy it to /system/usr/bin to install for test usage.

```
cp ./busybox /android/data/media/0
(Open a new terminal tab to get into Android Environment)
mount -o remount,rw /system
mkdir -p /system/usr/bin
cp -f /sdcard/busybox /system/usr/bin
chmod 0555 /system/usr/bin/busybox
/system/usr/bin/busybox --install -s /system/usr/bin
mount -o remount,ro /system
PATH+=:/system/usr/bin
```

Steps for SELinux-enabled BusyBox
-----------

First we need to download the source for libselinux and libsepol and compile it. (This is for use with the standard glibc toolchain.)

```
cd
apt-get source libselinux libsepol
cd libselinux*
make
cd
cd libsepol*
make
```

Now if you have any error, you should make sure all is up-2-date, this can be done via:
```
apt-get build-dep busybox
apt-get install -y build-essential
apt-get -f update
dpkg --configure -a
```



Now the last step  we make a clean new compiled BusyBox via:
```
cd
cd busybox*
make clean && make LDFLAGS='-static -L ../libselinux*/src -L ../libsepol*/src' CFLAGS='-Os -I ../libselinux*/include -I ../libsepol*/include'
```


Useful links
-------------

* [Official BusyBox FAQ | BusyBox.net](http://www.busybox.net/FAQ.html)
* [ CWM pre-compiled binaries | forum.xda-developers.com](http://forum.xda-developers.com/android/software-hacking/guide-busybox-snapshot-building-android-t2857650)

