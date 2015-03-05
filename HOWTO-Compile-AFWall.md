If you don't know anything about compiling Android apps (apk's), you should better read this [article here](http://www.vogella.com/articles/Android/article.html) first.

Index
-----

* [Software](#software)
* [IDE Requirements](#ide-requirements)
* [General procedure using Android Studio](#general-procedure-using-android-studio)
* [After repository refactoring and cleanup](after-repository-refactoring-and-cleanup)
* [Build via Command Line](#build-via-command-line)
* [Compiling native binaries](#compiling-native-binaries)
* [Compiling via graphical user interface](#compiling-via-graphical-user-interface)
* [For experts user only!](#for-experts-user-only!)
* [Useful links](#useful-links)

Software
--------

Android Studio:
* [AndroidSDK](http://developer.android.com/sdk/index.html)
* [Java Develop Kit (JDK) 1.7](http://java.sun.com/javase/downloads/index.jsp)
* [7-Zip](http://7-zip.org/) to extract Eclipse/Android SDK packages [**optional**]
* Basic cmd/terminal knowledge [**optional**]

Eclipse (old):
* [Eclipse Classic](http://www.eclipse.org/downloads/)
* [ADT for Eclipse](http://developer.android.com/sdk/installing/installing-adt.html) or get the Plugin via <code>https://dl-ssl.google.com/android/eclipse/</code> in Eclipse "Install New Software"


Advanced users can build the latest AFWall+.APK from the [Command Line](https://developer.android.com/tools/building/building-cmdline.html) or see the next steps.

IDE Requirements
----------------------

AFWall+ can be built using any IDE that supports the [Gradle](https://www.gradle.org/) build tool, used by projects such as [Android Studio](http://developer.android.com/sdk/installing/studio.html) or [Intellij IDEA](http://www.jetbrains.com/idea/) plus many others. (Gradle is the new equivalent of the old _"Ant"_ build tool.)


General procedure using Android Studio
----------------------

* Download and install some GitHub sync-tool like [SmartGit](http://www.syntevo.com/smartgit/) or [GitHub for Win/Mac](https://github.com/).
* Download and install the JAVA SE Development Kit 7 (JDK7) from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html).
* Install the latest Android Studio from [here](http://developer.android.com/sdk/installing/studio.html).
* When you start Android Studio, you'll see "Welcome to Android Studio" with several options. Here you choose to Open Project and then navigate to your GitHub synchronization directory where you have added AFWAll+, from your GitHub sync-tool.

The project will open immediately, but now you want to close it, so that you can return to the _Welcome screen_, this time you choose Configure >> SDK Manager. Here you select all API's related to API 14 (or higher), which is our lowest supported API.
For more details follow the setup instructions [here](http://developer.android.com/sdk/installing/index.html?pkg=studio).
Now click on the sideways Gradle tab, on the right side of the IDE. This will open a new window, once in it, double click on _Release_ (_assembleRelease_). Check for build errors in the lower screen log window.
If it compiles without errors, you can now build your own APK by clicking on the menu items: Build >> Generate Signed APK. This will ask you for 3 new passwords to be used in your "KeyStore" (1 master, 1 file, 1 APK alias). You may also add your credentials in signing.properties to enable automatic signing.

Example:

> WARNING: NEVER UPLOAD THIS FILE TO GITHUB!
> .gitignore will reject accidental uploads.
 
> * KEYSTORE_FILE=path\\to\\mykeystore.keystore
> * KEYSTORE_PASSWORD=*************************
> * KEY_ALIAS=*********************************
> * KEY_PASSWORD=******************************

If all of the above succeeded, push the AFWall+ .apk to your device and run.

---

:exclamation: **JAVA 8 is not yet supported.** Use a supported JavaVersion (`class -> dex` conversion can't handle 1.8 right now). VERSION is unrelated to your JDK. The JDK has to be >= VERSION.

---

After repository refactoring and cleanup
----------------------

Apparently, the first time after you have added our repo using the (Windows) GitHub app, or deleted a previous version from Android Studio, you need to import it into Android Studio by:

* Selecting **"Import Non-Android Studio Project"**

Selecting "Open an existing Android Studio Project" doesn't work, as it doesn't seem to setup the .gradle and .idea files etc.

Build via Command Line 
----------------------

Use the Android SDK Manager to install API 19 (or higher) and select it.
To list all of available targets, type

```
$ android list targets
```

Then, you will see like

```
"id: 4 or "Google Inc.:Google APIs:19"
Name: Google APIs
Type: Add-On
Vendor: Google Inc. .... "
```

To select this, use the "-t" argument in the following build-process like

```
$ android update project -p . -s -t 4
```

After this we set our JAVA_HOME path and go into building:

```
$ export ANDROID_JAVA_HOME="/opt/sun-jdk-1.9.0__22" 
$ mkdir afwall_build
$ cd afwall_build
$ git clone git://github.com/ukanth/afwall.git 
$ cd afwall
$ git submodule init
$ git submodule update
$ android update project -p . -s
$ ant debug
```

This build the afwall-debug.apk and place it into <code>/bin</code> folder.

Compiling native binaries
-------------------------

On the host side you'll need to install:

* NDK r9, nominally under /opt/android-ndk-r9
* Host-side gcc, make, etc. (Red Hat "Development Tools" group or Debian build-essential)
* autoconf, automake, and libtool

This command will build the Android binaries and copy them into <code>res/raw/</code>

> make -C external NDK=/opt/android-ndk-r9

Compiling via graphical user interface
--------------------------------------

* [Here](https://www.xda-developers.com/xda-tv-2/how-to-build-an-android-app-part-1-setting-up-eclipse-and-android-sdk-xda-tv/) is a Video Tutorial from a xda member for those users who want to use the graphical user interface (GUI) method. 
The tutorial is based on the needed software explained above. 
* [Here](https://developer.android.com/training/basics/firstapp/index.html) you get the official build instructions.

For experts user only!
----------------------

######  Create an unsigned .APK file with apkbuilder
Use <code>apkbuilder</code> (included in the Android Develop Kit)

> apkbuilder  ${output.apk.file} -u -z  ${packagedresource.file} -f  ${dex.file}


######  Sign an .APK file via jarsigner
Use [jarsigner](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/jarsigner.html)

> jarsigner  -keystore ${keystore} -storepass  ${keystore.password} -keypass ${keypass} -signedjar ${signed.apkfile} ${unsigned.apkfile} ${keyalias}


######  Publish the latest .APK file
> adb -d install -r ${signed.apk}

Useful links
------------

* [List of Sample Apps | Android Developers](http://developer.android.com/intl/zh-CN/resources/samples/index.html)
* [Installing the Android SDK | Android Developers](https://developer.android.com/sdk/installing/index.html)
* [Signing Your Applications | Android Developers](http://developer.android.com/tools/publishing/app-signing.html#signapp)
* [How To: Decompile / Compile .APK | XDA-Froum](http://forum.xda-developers.com/showthread.php?t=707189)
* [How to build .APK file? | Stack Overflow](http://stackoverflow.com/questions/4600891/how-to-build-apk-file)
* [How-To: Decompile/Recompile APK's with ApkTool | AndroidForums](http://androidforums.com/esteem-all-things-root/520917-guide-how-properly-decompile-recompile-apks-apktool.html)
* [How to Create Android Apps - Eclipse Export .APK Market Ready Files Step-by-Step Guide | YouTube](http://www.youtube.com/watch?v=DvBI16jv7xs)