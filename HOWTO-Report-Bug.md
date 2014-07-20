Index
-----

* [You should NOT](#you-should-not)
* [Before contacting support](#before-contacting-support)
* [How do I capture my logcat?](#how-do-i-capture-my-logcat?)
* [Logcat step-by-step](#logcat-step-by-step)
* [Sending bug report](#sending-bug-report)
* [Template](#template)
* [After sending the report](#after-sending-the-report)
* [Useful Links](#useful-links)

You should NOT
--------------

* You should NOT contact the carrier or the store in which you purchase the phone. They can not help you. Any Application developers support their own applications.
* You should NOT use the comment section in the Android Market to report issues. The comment section is a peer-to-peer channel in which consumers can leave messages and ratings. This should be used to recommend or not recommend the application. Developers can not respond to these comments. If you are NOT a paid customer, developers can not access your email address to contact you about your concerns directly. Often developers get unusable comments in the Android Market (ie This app does not work!!). Without contacting the developer directly, there is nothing that can be done and your comment can be marked as SPAM.
* You should NOT report bugs in forums. The developers may never see the messages in forums. Report the bug to people who can fix them (ie on [Issues Tracker](https://github.com/ukanth/afwall/issues))
* DO NOT set a milestone unless you are a developer and expect to fix the bug yourself.

Before contacting support
-------------------------

* Ask yourself is your issue reproducible?
* Can you document the steps to reproduce it so that it happens every time?
* Search and browse current tickets to make sure someone hasn't already reported your issue.
* Make sure you have the latest version of the application/source. This can be done by checking the Android Market for updates or push the latest source.
* Make sure you have the latest Android OS updates. This can be done in Settings on the phone.
* Make sure you know the version of Android you are running on your phone. This can be found in the Setting under About Phone.

How do I capture my logcat?
---------------------------
* This is **VERY important especially for any crashes**. To do this I recommend an application like [aLogCat](https://play.google.com/store/apps/details?id=org.jtb.alogcat&hl=de) or [Catlog](https://play.google.com/store/apps/details?id=com.nolanlawson.logcat&feature=search_result#?t=W251bGwsMSwxLDEsImNvbS5ub2xhbmxhd3Nvbi5sb2djYXQiXQ..) which is totally free available on Google Play Store. Both of these programs can dump their logs to a .txt file, which is very useful for debugging. Or, you can do it in terminal emulator. The most important part of the logcat are when AFWall+ first starts and logs around the time when your problem occurred. Generally it's a good idea to just capture the whole thing and attach that as discussed when reporting your issue. The logcat is only 64Kb big (adb logcat -g) - so you have to act quickly after a problem occurs - this is a Google Android OS limitation.

Logcat step-by-step
-------------------

Through Terminal Emulator:

Type _su_ and hit enter before you run the logcat command! 

The code for logcat to output to a file is:
> logcat > name of problem.txt

You can also do:
> logcat -f name of problem.txt

How I prefer to do it is this way:
> logcat -v long > name of problem.txt

Logcat will be saved under:
> /dev/log/[system]

If you want the log on your sdcard type this:
> logcat -d -f /sdcard/name of problem.txt *:V

With the -v flag & the long argument, it changes output to long style, which means every line of logcat will be on its own line. -f tell it where to save the log to and -d makes it dump the logcat.

**Important:** When outputting to a file, you will see a newline, but nothing printed, this is normal. To stop logcat from writing to a file, you need to press _ctrl+c_.


## Logcat step-by-step

Through SDK:

The simplest way is to use an application, like [Catlog](https://play.google.com/store/apps/details?id=com.nolanlawson.logcat),
but logcats captured this way are not always sufficient. The best way to capture a logcat is:

* Install the [Android SDK](http://developer.android.com/sdk/index.html) (Click *Download for other platforms* for a minimal download)
* Make sure you can connect to your device via USB (see [here](http://developer.android.com/sdk/win-usb.html) for drivers and instructions)
* **Enable AFWall+ logging in the main settings** (you can also export it to sdcard)
* Power off your device
* Start logging by entering this command on the command line: *adb logcat >log.txt*
* Power on your device
* Reproduce the problem

Upload the captured logcat somewhere, for example using Google Drive, Dropbox, Pastebin and link to it from the issue you (should) have created. Don't forget to mention the *uid* of the application to look into when relevant.

Sending bug report
------------------

* Create a new ticket. ("New Issues")
* Make sure you include Phone Name and Model (ie Samsung Galaxy SII I-9100)
* Make sure you include version of Android (ie 2.2)
* Make sure you include your problem description. Your problem description should be a step-by-step way to reproduce your problem. Details are very important! Without any information it does not help the developers to find the problem and fix them.
* Upload any sample files if necessary (i.e. if AFWall+ crashes when apply new rules, upload a sample that causes crash - Logcat or it never happened!)
* You can copy and paste the output to a site like [pastebin.com](http://www.pastebin.com/).

Template
--------

Please make sure AFWall+ running! (Green AFWall+ shield!)
>* **AFWall+ Mode (whitelist/blacklist)**
>* [your text]
>* **What steps will reproduce the problem?**
>* [your text]
>* **What is the expected output? What do you see instead?**
>* [your text]
>* **What version of the product are you using? On what operating system?**
>* [your text]
>* **Please provide any additional information below (e.g. logcat).**
>* [your text/file/link] 

After sending the report
------------------------

* Be open to work with the developer to help them resolve the problem. [AFWall+](https://github.com/ukanth/afwall) is free!!
* If you want to donating something to the developer there is also an [AFWall+ (Donate)](https://play.google.com/store/apps/details?id=dev.ukanth.ufirewall.donate#?t=W251bGwsMSwxLDIxMiwiZGV2LnVrYW50aC51ZmlyZXdhbGwuZG9uYXRlIl0.) version on Google Play Store. This have exact the same functionality except support to import Droidwall rules. 

Useful Links
------------

* [Logcat (read this first!)|  Android Developers] (http://developer.android.com/tools/help/logcat.html)
* [Working with ADB | Android Developer] (http://developer.android.com/tools/help/adb.html)
* [Report Bugs | Source Android](https://source.android.com/source/report-bugs.html)
* [Adding a Bug Reporter / Suggestion Box To Your Android App | dreamincode](http://www.dreamincode.net/forums/topic/244932-adding-a-bug-reporter-suggestion-box-to-your-android-app/)
* [androidlogcatviewer | Googlecode](https://code.google.com/p/androidlogcatviewer/downloads/list)
* [All about Log | Android Developers](https://developer.android.com/reference/android/util/Log.html)