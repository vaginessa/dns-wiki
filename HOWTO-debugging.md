Debugging
---------

To get all the logcat messages by F-Droid, you can run:

<code>adb logcat | grep `adb shell ps | grep dev.ukanth.ufirewall | cut -c10-15`</code>