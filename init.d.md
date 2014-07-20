Index
-----

* [Description](#description)
* [Do I have init.d support?](#do-I-have-init.d-support?)
* [What should I know?](#what-should-i-know?)
* [Check if BusyBox run-parts are present](#check-if-busybox-run-parts-are-present)
* [How to remove init.d?](#how-to-remove-init.d?)
* [Useful links](#useful-links)

Description
-----------

> **Originally Posted by the_scotsman (Moderator Liaison Admin / Moderator Committee / XDA News Writer)**

> Init.d plays an important role in the world of Android development and customization It allows users to install scripts and mods to be run at boot—everything from battery tweaks to performance tweaks. It essentially opens the door to a world of mods only possible through the Init.d process, which in turn is usually only available on custom kernels.

Only "init.d" is responsible for loading up stuff at startup. Sometimes kernels don't make "init.d" folder and you have to create them manually.

Do I have init.d support?
-------------------------
Please check if you have such folder:
<pre>/system/etc/init.d  (permission -> 755/rwxr-xr-x)</pre> 

If it is there you are good to go!

A other method is:
1. Download the file from here: [test_initd.zip | XDA](http://forum.xda-developers.com/attachment.php?attachmentid=1612958&d=1357201287) - it's from the XDA Member [Inside 4ndroid](http://forum.xda-developers.com/member.php?u=5006081).
2. Extract the file, you will get a file named 00test. **DO NOT flash!**
3. Paste it into /etc/init.d. If there is no init.d folder, most probably you DO NOT have init.d support. However, if you still wanna try, just create the folder named "init.d"
4. Change the permissions of the init.d folder and 00test into rwxrwxrwx.
5. Reboot.
6. If you see a file named Test.log in /data, you have init.d support. If not, you will have to run [Uni-init](http://forum.xda-developers.com/showpost.php?p=32716399&postcount=1), [Term-init](http://forum.xda-developers.com/showpost.php?p=32716412&postcount=2) or [Zip-init](http://forum.xda-developers.com/showpost.php?p=32716432&postcount=3).

What should I know?
-------------------

Quick and dirty:
* You need a **rooted device**!
* To enable init.d scripts (BusyBox is required!) Some kernel use an inbuild BusyBox version and some not, but if you don't have BusyBox don't worry, install a kernel with it (example [Kernel](https://github.com/asasoft/Kernel-B5510), or simply install the [BusyBox from Google Play Store](https://play.google.com/store/apps/details?id=stericson.busybox).
* Install-recovery is placed in init.rc, so in some ROM's it's maybe on a different place.

Check if BusyBox run-parts are present
--------------------------------------

Type "busybox run-parts" (without quotes) in a terminal emulator and press enter. If you get an output similar to what's shown below you're all set. It means that BusyBoxs installed and the command is present.

<pre>BusyBox v1.20.2-jb static (2014-07-21 00:00 +0100) multi-call binary.

Usage: run-parts [-t] [-l] [-a ARG] [-u MASK] DIRECTORY

Run a bunch of scripts in DIRECTORY

        -t      Print what would be run, but don't actually run anything
        -a ARG  Pass ARG as argument for every program
        -u MASK Set the umask to MASK before running every program
        -l      Print names of all matching files even if they are not executable</pre>

How to remove init.d?
---------------------

Install your ROM again or simply delete all files and of course the init.d folder. 

Useful links
------------

* [[MOD][APK+SCRIPT+ZIP] Enable Init.d for Any Phones w/o Need of Custom Kernels!!! | XDA](http://forum.xda-developers.com/showthread.php?t=1933849)
* [How to modify initialization scripts or startup routines in Android | androidquestions.org](http://www.androidquestions.org/threads/34-How-to-modify-initialization-scripts-or-startup-routines-in-android)
* [Init.d Toggler | Android Apps on Google Play](https://play.google.com/store/apps/details?id=com.broodplank.initdtoggler)
* [Script Manager - SManager | Android Apps on Google Play](https://play.google.com/store/apps/details?id=os.tools.scriptmanager&hl=en)
* [Universal Init.d | Android Apps on Google Play](http://tabletrepublic.com/forum/novo-7-basic/init-d-support-startup-script-support-479.html) - (init.d at app level)