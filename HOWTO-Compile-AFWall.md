### Software we need:
* [7-Zip](http://7-zip.org/) to extract Eclipse/Android SDK
* [Eclipse Classic](http://www.eclipse.org/downloads/) (Google recommendation) 
* [AndroidSDK](http://developer.android.com/sdk/index.html) [here](http://developer.android.com/sdk/installing.html) is an self explanatory to install AndroidSDK and Emulator
* [Java Develop Kit (JDK)](http://java.sun.com/javase/downloads/index.jsp)
* [ADT for Eclipse](http://developer.android.com/sdk/installing/installing-adt.html) or get the Plugin via <code>https://dl-ssl.google.com/android/eclipse/</code> in Eclipse "Install New Software"
* If you know nothing about compiling apps, you better read this [article here](http://www.vogella.com/articles/Android/article.html) first.
* Basic Cmd/terminal knowledge (for experts).


Advanced users can build the latest .APK from the [Command Line](https://developer.android.com/tools/building/building-cmdline.html) or see the next steps.

### Build via Command Line 
Choose the Android list targets - lists all your installed Android target e.g. API level 17 for the latest Android Version.

Select:
"id: 4 or "Google Inc.:Google APIs:17"
Name: Google APIs
Type: Add-On
Vendor: Google Inc. .... "

You also can use the "-t 4" argument.

After this we set our JAVA_HOME path:

> * export ANDROID_JAVA_HOME="/opt/sun-jdk-1.7.0__21" 
> * mkdir afwall_build
> * cd afwall_build
> * git clone git://github.com/ukanth/afwall.git 
> * cd afwall
> * git submodule init
> * git submodule update
> * android update project -p . -s
> * ant debug

This build the .apk and place it in bin/afwall-debug.apk

#### Compiling native binaries

On the host side you'll need to install:

* NDK r9, nominally under /opt/android-ndk-r9
* Host-side gcc, make, etc. (Red Hat "Development Tools" group or Debian build-essential)
* autoconf, automake, and libtool

This command will build the Android binaries and copy them into res/raw/:

> make -C external NDK=/opt/android-ndk-r9

#### Via graphical user interface (GUI)
* [Here](https://www.xda-developers.com/xda-tv-2/how-to-build-an-android-app-part-1-setting-up-eclipse-and-android-sdk-xda-tv/) is a Video Tutorial from a xda member for those users who want to use the GUI method. 
The tutorial is based on the needed software explained above. 
* [Here](https://developer.android.com/training/basics/firstapp/index.html) you get the official buld instruction

### For experts user only!

######  Create an unsigned .APK file with apkbuilder
use _apkbuilder_ (included in the Android Develop Kit)

apkbuilder  ${output.apk.file} -u -z  ${packagedresource.file} -f  ${dex.file}


######  Sign an .APK file via jarsigner
use _[jarsigner](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/jarsigner.html)_

jarsigner  -keystore ${keystore} -storepass  ${keystore.password} -keypass ${keypass} -signedjar ${signed.apkfile} ${unsigned.apkfile} ${keyalias}



######   Publish the latest APK file
use adb
adb -d install -r ${signed.apk}



## Some other useful links
* [List of Sample Apps | Android Developers](http://developer.android.com/intl/zh-CN/resources/samples/index.html)
* [Installing the Android SDK | Android Developers](https://developer.android.com/sdk/installing/index.html)
* [Signing Your Applications | Android Developers](http://developer.android.com/tools/publishing/app-signing.html#signapp)
* [How To: Decompile / Compile .APK | XDA-Froum](http://forum.xda-developers.com/showthread.php?t=707189)
* [How to build .apk file? | Stack Overflow](http://stackoverflow.com/questions/4600891/how-to-build-apk-file)
* [How-To: Decompile/Recompile apk's with ApkTool | AndroidForums](http://androidforums.com/esteem-all-things-root/520917-guide-how-properly-decompile-recompile-apks-apktool.html)
* [How to Create Android Apps - Eclipse Export .apk Market Ready Files Step-by-Step Guide | YouTube](http://www.youtube.com/watch?v=DvBI16jv7xs)