Index
-----

* [You should NOT](#you-should-not)
* [Before contacting support](#before-contacting-support)
* [How do I capture my logcat?](#how-do-i-capture-my-logcat)
* [Logcat step-by-step via Terminal Emulator](#logcat-step-by-step-via-terminal-emulator)
* [Logcat step-by-step via SDK](#logcat-step-by-step-via-sdk)
* [Sending bug report](#sending-bug-report)
* [Template for GitHub](#template-for-github)
* [After sending the report](#after-sending-the-report)
* [Custom ROMs](#custom-roms)
* [Useful Links](#useful-links)

You should NOT
--------------

* You should NOT contact the carrier or the store in which you purchase the phone. They can't help you with software related problems. Developers support their own applications!
* You should NOT use the comment section on the official Google Play Store section to report issues. It cost a lot of time to track each new Google Play Store comment and answrring them there is often not helpful, it's simply the wrong platform. 
* You should NOT report bugs via XDA thread because we may never see the message, since we don't monitor all new comments on a daily basis.
* DO NOT set a milestone unless you are a developer and expect to fix the bug yourself. By default, this is disabled to any new contributor.

Before contacting support
-------------------------

* Watch the `Changelog.md` / `Todo.md` files if the problem is already fixed or on our radar.
* Can you document the steps to reproduce it so that it happens every time? - Or is it only once?
* [Search and browse current tickets](https://github.com/ukanth/afwall/issues) to make sure someone hasn't already reported your issue.
* Make sure you have the latest version of the application/source code. This can be done by checking the Google Play Store, G+ or F-Droid for updates or push the latest GitHub source and compile it yourself.
* Make sure you have the latest Android OS updates (stable! -> no nightly's!). This can be done under _Settings_ on the phone, via Over-the-Air (OTA updates) or manually on XDA by downloading another ROM which may fix your problem(s).
* Make sure you know the version of Android you are running on your phone. This can be found in the _Setting_ under _About Phone_.
* Make sure there is only AFWall+ installed and enabled, other tools like Avast could make some troubles if they are enabled since they are using iptables and other security features too.
* Make sure you enabled the in-build logging option under _Preferences_ - _Log_ _Preferences_ and enable the <code>Enable Firewall Logs</code> option. In the main menu you can now hit _Firewall_ _Logs_. Now you can see an _Export to SDCard_ option, which will export your log to <code>/sdcard/afwall/rules.log</code>.
* Make sure you disabled all your custom scripts before you test/reproduce the problem, we not supporting problems which may caused by external/optional rules if they are wrong or if you don't know how to deal with it.
* Ensure that AFWall+ was tested with several options that comes with it, sometimes it's easier to play with the settings to solve the problem as debugging all possibility's which may cause the problem. 

How do I capture my logcat?
---------------------------
* This is **VERY important especially for any crashes**. To do this I recommend an application like [aLogCat](https://play.google.com/store/apps/details?id=org.jtb.alogcat&hl=de) or [Catlog](https://play.google.com/store/apps/details?id=com.nolanlawson.logcat) which is totally free available on Google Play Store. Both of these programs can dump their logs to a .txt file, which is very useful for debugging. Or, you can do it in terminal emulator. The most important part of the logcat are when AFWall+ first starts and logs around the time when your problem occurred. Generally it's a good idea to just capture the whole thing and attach that as discussed when reporting your issue. The logcat is only 64Kb big (<code>adb logcat -g</code>) - so you have to act quickly after a problem occurs - this is a Google Android OS limitation to not flood the console.
* The following command will force Android to restart and only capture the AFWall+ related stuff, please use this only if it's only AFWall+ app specific and not iptables specific. <code>killall system_server; logcat | grep -i afwall</code>.

Logcat step-by-step via Terminal Emulator
-------------------

Through Terminal Emulator:

Type _su_ and hit enter before you run the logcat command! 

The code for logcat to output to a file is:
> logcat > name of problem.txt

You can also add the _-f_ argument:
> logcat -f name of problem.txt

How I prefer to do it is this way:
> logcat -v long > name of problem.txt

Logcat will be saved under:
> /dev/log/[system]

If you want the log on your sdcard type this:
> logcat -d -f /sdcard/name of problem.txt *:V

With the -v flag & the long argument, it changes output to long style, which means every line of logcat will be on its own line. -f tell it where to save the log to and -d makes it dump the logcat.

**Important:** When outputting to a file, you will see a newline, but nothing printed, this is normal. To stop logcat from writing to a file, you need to press _ctrl+c_.


Logcat step-by-step via SDK
-------------------

The simplest way is to use an external app, like [Catlog](https://play.google.com/store/apps/details?id=com.nolanlawson.logcat), but logcats captured this way are not always sufficient. The _best way_ to capture a logcat is:

* Install the [Android SDK](http://developer.android.com/sdk/index.html) (Click *Download for other platforms* for a minimal download) an light alternative to the huge SDK is the [ADB 15 seconds installer](http://www.xda-developers.com/15-second-adb-installer-gives-you-lightning-fast-adb-fastboot-and-driver-installation/).
* Make sure you can connect to your device via USB (see [here](http://developer.android.com/sdk/win-usb.html) for drivers and instructions)
* **Enable AFWall+ logging in the main settings** (you can also export it to _/sdcard/_)
* Power off your device
* Start logging by entering this command on the command line: *adb logcat >log.txt*
* Power on your device
* Reproduce the problem
* Send us the logcat 

Upload the captured logcat somewhere for example using your Google Drive, Dropbox, Pastebin and link to it from [the issue](https://github.com/ukanth/afwall/issues) you (should) have created or send it [directly via email](afwall-report@googlegroups.com). Don't forget to mention the *uid*/package name/process name of the application to look into when it's relevant.

Sending bug report
------------------

Directly within the AFWall+ app:
* Copy and send the integrated logcat via AFWall+. The otion is under _Firewall Rules_/_Send error report_, this will forward the bug via eMail to the developer. Make sure that AFWall+ isn't blocked via firewall or restricted by another app.

GitHub issue tickets:
* Create a new ticket (click on "New Issues") [here on GitHub](https://github.com/ukanth/afwall/issues).
* Make sure you include the phone name and model (ie Samsung Galaxy SII I-9100).
* Make sure you include version of Android (ie Android 5.1)
* Make sure you include your problem description. Your problem description should be a step-by-step way to reproduce your problem. Details are very important! Without any information it does not help the developers to find the problem and fix them!
* Make sure you **not use any CyanogenMod nightly**, CM does not allow nightly bug reports because it's unstable and some things may change with upcoming versions. 
* Upload any sample files if necessary (i.e. if AFWall+ crashes when apply your new rules, upload a sample that causes the crash - **Logcat or it never happened!**).
* You can copy and paste the output to a site like [defuse.ca](https://defuse.ca) or export them with the copy/export function.

Template for GitHub
------------------

This is a small template you can use to provide a useful bug report. Make sure you filled in all nessary information we need. AFWall+ must be running (Green AFWall+ shield activated!), if not we can't say if it's a AFWall+ related problem or something else.
 
>* **AFWall+ Mode (whitelist [default enabled]/blacklist)**
>* [your text/file or link]
>* **Android ROM + exact version number**
>* [your text/file or link]
>* **What steps will reproduce the problem?**
>* [your text/file or link]
>* **Additional security software installed (like XPrivacy/Avast)? Is it really deactivated?!**
>* [your text/file or link]
>* **What is the expected output? What do you see instead?**
>* [your text/file or link]
>* **Attach your exported rules.log (IPv4 + IPv6)** 
>* [your text/file or link]
>* **iptables -L -t mangle (only behind VPN)** 
>* [your text/file or link]
>* **Type 'adb bugreport' via ADB (only necessary if there is an app crash)** 
>* [your text/file or link]
>* **Please provide any additional information below (e.g. logcat).**
>* [your text/file or link]
>* **Which binaries are used for BusyBox/IPTables?**
>* [your text/file or link]
>* **Which DNS-proxy option is in usage?**
>* [your text/file or link]
>* **Are the experimental options enabled/disabled?!**
>* [your text/file or link]
>* **On DNS related problems please provide 'adb shell dumpsys connectivity | grep DnsAddresses' + 'nslookup google.com' + 'getprop | grep dns'**
>* [your text/file or link]
>* **For general connectivity problems please provide 'adb shell dumpsys connectivity'**
>* [your text/file or link]

:warning: More details always means more and faster/better help! :warning:

After sending the report
------------------------

* Be open to work with the developers to help us to resolve your problem. Remember: [AFWall+](https://github.com/ukanth/afwall) is free, do not expect 24 hours support per day!
* If you want donating something to the developer there is also an [AFWall+ (Donate)](https://play.google.com/store/apps/details?id=dev.ukanth.ufirewall.donate) version on Google Play Store. This have exact the same functionality except support to import Droidwall rules. 

Custom ROMs
-----------

Some custom ROMs come with their own shells and utilities. If you are using a custom ROM like LineageOS, check its documentation to find out what's available and what's not. For security reasons some binary's are not present e.g. on the Guardian ROM.

:warning: Again - We generally do not support nighty's/alpha's/rc's/beta's ROMs - due the mass of updates we not monitoring all changes that was possible included by them. First try a stable version and see if it works. :warning:

Useful Links
------------

* [Life of a Bug | Android Developers](https://source.android.com/source/life-of-a-bug.html)
* [ADB - Android Wiki](http://android-dls.com/wiki/index.php?title=ADB)
* [Logcat (read this first!) |  Android Developers](http://developer.android.com/tools/help/logcat.html)
* [Working with ADB | Android Developer](http://developer.android.com/tools/help/adb.html)
* [Report Bugs | Source Android](https://source.android.com/source/report-bugs.html)
* [Adding a Bug Reporter / Suggestion Box To Your Android App | dreamincode](http://www.dreamincode.net/forums/topic/244932-adding-a-bug-reporter-suggestion-box-to-your-android-app/)
* [androidlogcatviewer | Googlecode](https://code.google.com/p/androidlogcatviewer/downloads/list)
* [All about Log | Android Developers](https://developer.android.com/reference/android/util/Log.html)
* [Android Shell Command Reference](https://github.com/jackpal/Android-Terminal-Emulator/wiki/Android-Shell-Command-Reference)
* [ifconfig parameters| die.net](http://linux.die.net/man/8/ifconfig)
* [route parameters| die.net](http://linux.die.net/man/8/route)
* [How to use adb bugreport | adbshell ](http://adbshell.com/commands/adb-bugreport/how-to-use-adb-bugreport.html)