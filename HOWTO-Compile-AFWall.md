:warning: If you don't know anything about compiling Android apps (apk's), you should better read this [article here](http://www.vogella.com/articles/Android/article.html) first. :warning:

Index
-----

* [Software](#software)
* [IDE Requirements](#ide-requirements)
* [General procedure using Android Studio](#general-procedure-using-android-studio)
* [After repository refactoring and cleanup](#after-repository-refactoring-and-cleanup)
* [Build via Command Line](#build-via-command-line)
* [Compiling native binaries](#compiling-native-binaries)
* [After Repository Refactoring](#after-repository-refactoring)
* [Useful links](#useful-links)

Software
--------

Android Studio:
* [AndroidSDK](http://developer.android.com/sdk/index.html)
* [Java Develop Kit (JDK) 1.8](http://java.sun.com/javase/downloads/index.jsp)
* [7-Zip](http://7-zip.org/) to extract Eclipse/Android SDK packages [**optional**]
* Basic cmd/terminal knowledge [**optional**]

Eclipse (old):
* [Eclipse Classic](http://www.eclipse.org/downloads/)
* [ADT for Eclipse](http://developer.android.com/sdk/installing/installing-adt.html) or get the plugin via <code>https://dl-ssl.google.com/android/eclipse/</code> in Eclipse "Install New Software"


Advanced users can build the latest AFWall+.APK from the [Command Line](https://developer.android.com/tools/building/building-cmdline.html) or see the next steps.

IDE Requirements
----------------------

AFWall+ can be built using any IDE that supports the [Gradle](https://www.gradle.org/) build tool, used by projects such as [Android Studio](http://developer.android.com/sdk/installing/studio.html) or [Intellij IDEA](http://www.jetbrains.com/idea/) plus many others. (Gradle is the new equivalent of the old _"Ant"_ build tool.)


General procedure using Android Studio
----------------------

* Download and install your favorite GitHub sync-tool like [SmartGit](http://www.syntevo.com/smartgit/) or [GitHub for Win/Mac](https://github.com/).
* Download and install the JAVA SE Development Kit 8 (JDK8) from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html).
* Install the latest Android Studio from [here](http://developer.android.com/sdk/installing/studio.html).
* When you start Android Studio, you'll see "Welcome to Android Studio" with several options. Here you choose to Open Project and then navigate to your GitHub synchronization directory where you have added AFWall+, from your GitHub sync-tool.

The project will open immediately, but now you want to close it, so that you can return to the _Welcome screen_, this time you choose Configure >> SDK Manager. Here you select all APIs related to API 14 (or higher), which is our lowest supported API.
For more details follow the setup instructions [here](http://developer.android.com/sdk/installing/index.html?pkg=studio).
Now click on the sideways Gradle tab, on the right side of the IDE. This will open a new window, once in it, double click on _Debug_ (_assembleDebug_). Check for build errors in the lower screen log window.
If it compiles without errors, you can now build your own APK by clicking on the menu items: Build >> Generate Signed APK. This will ask you for 3 new passwords to be used in your "KeyStore" (1 master, 1 file, 1 APK alias). You may also add your credentials in signing.properties to enable automatic signing.

Example:

> WARNING: NEVER UPLOAD THIS FILE TO GITHUB!
> .gitignore will reject accidental uploads.
 
> * KEYSTORE_FILE=path\\to\\mykeystore.keystore
> * KEYSTORE_PASSWORD=*************************
> * KEY_ALIAS=*********************************
> * KEY_PASSWORD=******************************

If all of the above succeeded, push the AFWall+ .apk to your device and run.


After repository refactoring and cleanup
----------------------

Apparently, the first time after you have added our repo using the (Windows) GitHub app, or deleted a previous version from Android Studio, you need to import it into Android Studio by:

* Selecting **"Import Non-Android Studio Project"**

Selecting "Open an existing Android Studio Project" doesn't work, as it doesn't seem to setup the .gradle and .idea files etc.

Build via Command Line 
----------------------

Install the latest API and related packages in Android SDK Manager:

```
$ android
```

Clone the git repository and build by using gradlew:

```
$ git clone git://github.com/ukanth/afwall.git 
$ cd afwall
$ ./gradlew clean assembleDebug
```

After successful build, afwall-debug.apk will be found in <code>aFWall/build/outputs/apk/</code> folder.

Compiling native binaries
-------------------------

On the host side you'll need to install (for compiling e.g. iptables):

* Android Studio 1.3.1 or higher (If you used the above steps you're mostly done).
* Ensure _Makefile_ are on the correct place.
* Ensure you use _allDebug_ or _allRelease_ (since Android Studio may have problems with other flags).
* Edit and compile the binaries as per needs, the debugger shows the important stuff within the output.txt file.

After Repository Refactoring
-------------------------

After the first time you have added our AFWall+ repo using the GitHub app, or deleted a previous version from Android Studio, you may need to import it into Android Studio again/by:

    Selecting "Import Non-Android Studio Project"

Selecting "Open an existing Android Studio Project" doesn't work, as it doesn't seem to setup the .gradle and .idea files etc. This is due a bug within Android Studio. 

Useful links
------------

* [List of Sample Apps | Android Developers](http://developer.android.com/intl/zh-CN/resources/samples/index.html)
* [Installing the Android SDK | Android Developers](https://developer.android.com/sdk/installing/index.html)
* [Signing Your Applications | Android Developers](http://developer.android.com/tools/publishing/app-signing.html#signapp)
* [How To: Decompile / Compile .APK | XDA-Froum](http://forum.xda-developers.com/showthread.php?t=707189)
* [How to build .APK file? | Stack Overflow](http://stackoverflow.com/questions/4600891/how-to-build-apk-file)
* [How-To: Decompile/Recompile APK's with ApkTool | AndroidForums](http://androidforums.com/esteem-all-things-root/520917-guide-how-properly-decompile-recompile-apks-apktool.html)
* [How to Create Android Apps - Eclipse Export .APK Market Ready Files Step-by-Step Guide | YouTube](http://www.youtube.com/watch?v=DvBI16jv7xs)

_ Final version 08.24.2015_