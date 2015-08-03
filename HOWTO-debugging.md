Debugging
---------

To get all the logcat messages by AFWall+ you could run:

<code>adb logcat | grep `adb shell ps | grep dev.ukanth.ufirewall | cut -c10-15`</code>

See the [Android Gradle user guide](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Testing)
for more details, including how to use Android Studio to run tests (which provides more useful feedback than the command line).