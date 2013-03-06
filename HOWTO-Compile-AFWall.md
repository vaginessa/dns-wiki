# Software we need:
* [7-Zip](http://7-zip.org/) to extract Eclipse/Android SDK
* [Eclipse Classic](http://www.eclipse.org/downloads/) (Google recommendation) 
* [AndroidSDK](http://developer.android.com/sdk/index.html) [here](http://developer.android.com/sdk/installing.html) is an self explanatory to install AndroidSDK and Emulator
* [Java JDK](http://java.sun.com/javase/downloads/index.jsp)
* [ADT for Eclipse](http://developer.android.com/sdk/installing/installing-adt.html) or get the Plugin via <code>https://dl-ssl.google.com/android/eclipse/</code> in Eclipse "Install New Software"
* If you know nothing about Compiler, Android you better read this [here](http://www.vogella.com/articles/Android/article.html) first


Advanced users can build the latest APK from the [Command Line](https://developer.android.com/tools/building/building-cmdline.html) or see the next steps.

### Via Command Line 
Choose the Android list targets - lists all your installed Adroid target e.g. API level 16 for the latest Android Version.

Select:
"id: 4 or "Google Inc.:Google APIs:16"
Name: Google APIs
Type: Add-On
Vendor: Google Inc. .... "

You also can use the "-t 4" argument

After this we set our JAVA_HOME path:

* export ANDROID_JAVA_HOME="/opt/sun-jdk-1.6.0.3__7"
* mkdir build_folder
* cd build_folder
* git clone git://github.com/<your github name>/<folder name>.git foo
* cp -r foo/library/ <your folder>
* cd <your folder>
* android update project -t 4 --path .
* cd ..
* git clone git://github.com/ukanth/afwall.git
* cd afwall
* android update project -t 4 --path .
* ant debug

This build the .apk and place it in ./bin/<your folder name>-debug.apk

#### Via GUI
* [Here](https://www.xda-developers.com/xda-tv-2/how-to-build-an-android-app-part-1-setting-up-eclipse-and-android-sdk-xda-tv/) is a Video Tutorial from a xda member for those users who want to use the GUI method. 
The tutorial is based on the needed software explained above. 
* [Here](https://developer.android.com/training/basics/firstapp/index.html) you get the official buld instruction

### For experts user only!

######  Create unsigned APK
use apkbuilder

apkbuilder  ${output.apk.file} -u -z  ${packagedresource.file} -f  ${dex.file}



######   Sign APK
use jarsigner

jarsigner  -keystore ${keystore} -storepass  ${keystore.password} -keypass ${keypass} -signedjar ${signed.apkfile} ${unsigned.apkfile} ${keyalias}



######   Publish the latest APK file
use adb
adb -d install -r ${signed.apk}



## Useful Links
* [List of Sample Apps | Android Developers](http://developer.android.com/intl/zh-CN/resources/samples/index.html)
* [Installing the SDK  |Android Developers](https://developer.android.com/sdk/installing/index.html)

## Thanks to welle12 & CHEF-KOCH 