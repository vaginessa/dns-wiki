HOWTO compile AFWall+
Software we need:

7-Zip to extract Eclipse/Android SDK
Eclipse Classic (Google recommendation)
AndroidSDK here is an self explanatory to install AndroidSDK and Emulator
ADT for Eclipse or get the Plugin via https://dl-ssl.google.com/android/eclipse/ in Eclipse "Install New Software"
If you know nothing about Compiler, Android you better read this here first
Advanced users can build the latest APK from the Command Line or see the next steps.

Via Command Line

Choose the Android list targets - lists all your installed Adroid target e.g. API level 16 for the latest Android Version.

Select:
"id: 4 or "Google Inc.:Google APIs:16"
Name: Google APIs
Type: Add-On
Vendor: Google Inc. .... "

You also can use the "-t 4" argument

After this we set our JAVA_HOME path:
#path to your java jdk dir
export ANDROID_JAVA_HOME="/opt/sun-jdk-1.6.0.37"

mkdir build_folder
cd build_folder
git clone git://github.com//.git foo
cp -r foo/library/ 
cd 
android update project -t 4 --path .
cd ..
git clone git://github.com/ukanth/afwall.git
cd afwall
android update project -t 4 --path .
ant debug

This build the .apk and place it in ./bin/-debug.apk

Via GUI

Here is a Video Tutorial from a xda member for those users who want to use the GUI method. The tutorial is based on the needed software explained above.
Here you get the official buld instruction
For experts user only!
Create unsigned APK

use apkbuilder

apkbuilder ${output.apk.file} -u -z ${packagedresource.file} -f ${dex.file}

Sign APK

use jarsigner

jarsigner -keystore ${keystore} -storepass ${keystore.password} -keypass ${keypass} -signedjar ${signed.apkfile} ${unsigned.apkfile} ${keyalias}

Publish the latest APK file

use adb
adb -d install -r ${signed.apk}