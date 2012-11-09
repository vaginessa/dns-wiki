######  You should NOT
* You should NOT contact the carrier or the store in which you purchase the phone. They can not help you. Any Application developers support their own applications.
* You should NOT use the comment section in the Android Market to report issues. The comment section is a peer-to-peer channel in which consumers can leave messages and ratings. This should be used to recommend or not recommend the application. Developers can not respond to these comments. If you are NOT a paid customer, developers can not access your email address to contact you about your concerns directly. Often developers get unusable comments in the Android Market (ie This app does not work!!). Without contacting the developer directly, there is nothing that can be done and your comment can be marked as SPAM.
* You should NOT report bugs in forums. The developers may never see the messages in forums. Report the bug to people who can fix them (ie on [Issues Tracker](https://github.com/ukanth/afwall/issues))
* DO NOT set a milestone unless you are a developer and expect to fix the bug yourself.


######  Before contacting support
* Ask yourself is your issue reproducible?
* Can you document the steps to reproduce it so that it happens every time?
* Search and browse current tickets to make sure someone hasn't already reported your issue.
* Make sure you have the latest version of the application/source. This can be done by checking the Android Market for updates or push the latest source.
* Make sure you have the latest Android OS updates. This can be done in Settings on the phone.
* Make sure you know the version of Android you are running on your phone. This can be found in the Setting under About Phone.


###### How do I capture my logcat?
* This is VERY important especially for any crashes. To do this I recommend an application like ['aLogCat'](https://play.google.com/store/apps/details?id=org.jtb.alogcat&hl=de) which is free in the Android Market. The most important part of the logcat are when plex first starts and logs around the time when your problem occurred. Generally it's a good idea to just capture the whole thing and attach that as discussed when reporting your issue. The logcat is only 64k big - so you have to act quickly after a problem occurs - this is a google Android not Plex limitation. 


######  Sending bug report
* Create a new ticket. ("New Issues")
* Make sure you include Phone Name and Model (ie Samsung Galaxy SII I-9100)
* Make sure you include version of Android (ie 2.2)
* Make sure you include your problem description. Your problem description should be a step-by-step way to reproduce your problem. Details are very important! Without any information it does not help the developers to find the problem and fix them.
* Upload any sample files if necessary (i.e. if Plex crashes when play a video, upload a sample that causes crash)
* Please also run <code>"adb logcat > logcat.txt"</code> or <code>logcat > /sdcard/logfile.txt</code>  via Terminal and archive the output.


######  After sending the report
* Be open to work with the developer to help them resolve the problem. [AFWall+](https://github.com/ukanth/afwall) is free!!

###### Useful Links
* [Report Bugs | Source Android](https://source.android.com/source/report-bugs.html)
* [Adding a Bug Reporter / Suggestion Box To Your Android App | dreamincode](http://www.dreamincode.net/forums/topic/244932-adding-a-bug-reporter-suggestion-box-to-your-android-app/)
* [androidlogcatviewer | Googlecode](https://code.google.com/p/androidlogcatviewer/downloads/list)
* [All about Log | Android Developers](https://developer.android.com/reference/android/util/Log.html)