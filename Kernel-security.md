Index
-----

* [Introduction](#introduction)
* [Overview](#overview)
* [Permissions](#permissions)
* [Feature implementations](#feature-implementations)
* [SELinux](#selinux)
* [Android logging system](#android-logging-system)
* [GMS](#gms)
* [Disable binaries](#disable-binaries)
* [Protection against other attacks](#protection-against-other-attacks)
* [Android Marshmallow](#android-marshmallow)
* [Useful links](#useful-links)


An fantastic explanation and overview about Android's security mechanism can be found at the [TheNewCircle](https://thenewcircle.com/s/post/1473/Android_Security_Underpinnings.htm) [05.2013] or directly on [source.android.com](http://source.android.com/tech/security/index.html).

:warning: This is a POC (Proof-of-Concept) and WIP (Work-in-process) article, it's not designed (and never will be) to explain everything or show every possible configuration (it's simply impossible) and is more designed to quickly show the most important configurations, tricks or researches.

Due the lack of security of the consumer devices is not necessarily Android's problem. So far, most problems come from OEM's customization, and third party malicious apps pretending to be good citizen. Hardening the kernel won't fix all of these problems. :warning:


To prevent some of the latest networking attacks, you should try to keep your Kernel version current as soon as possible! This is the first and easiest step since a lot of modded/custom kernels are ready to download and flashable (.zip).

Introduction
------------

AFWall+ is a only a GUI for IPTables and people may want to know more about how they can generally hardening the lowest-security-level on there devices (Kernel/ROM). As the kernel controls your device networking, it is important that it be very secure, and could not be compromised. If the kernel is already compromised most of all security related features could be bypassed without any notification or log entry.

The config file of your currently running kernel is also always available in your file system at <code>/proc/config.gz</code>.

The main benefit and the main goal is to secure the kernel, so that no external tools are necessary. 

The Linux Kernel isn't exactly the Android Kernel, there are [several things you can't find in the original Linux kernel](http://www.elinux.org/Android_Kernel_Features), like OOM, Wakelocks, pmem, Binder, logger, ... - These features are [unique to Android](http://www.linuxfoundation.org/news-media/blogs/browse/2013/03/defining-android-vs-embedded-linux) only systems.


Kind of attacks:
* Offline attacks (physical attacks, side-loading, bypassing, off-device attacks ...[mostly needs root]).
* Online attacks (on the network stack against malware, dos, ...)
(optional - [OS specific](https://source.android.com/devices/tech/security/enhancements/enhancements50.html))
* On Android 5+ the user password is now protected against ordinary [brute-force](http://en.wikipedia.org/wiki/Brute_force) attacks using [scrypt](http://en.wikipedia.org/wiki/Scrypt) and, if available, the key is bound to the hardware keystore to prevent off-device attacks (i.e. brute-force). As always, the Android screen lock secret and the device encryption key are not sent off the device or exposed to any application. 
* TLSv1.2 are now enabled by default. Look, [here](https://developer.android.com/reference/javax/net/ssl/SSLSocket.html) that also means that Forward secrecy is enabled too which is used in AES-GCM (needs Play-services 7.x+ installed). The weak and vulnerably cipher suites like MD5, 3DES are by default disabled. Some pages are still using vulnerable RC4 ciphers, you should avoid visiting them especially if you care about security.

Overview
------------

* [Security Enhancements in Android 5.0](https://source.android.com/devices/tech/security/enhancements/enhancements50.html)
* [Security Enhancements in Android 4.4](https://source.android.com/devices/tech/security/enhancements/enhancements44.html)
* [Security Enhancements in Android 4.3](https://source.android.com/devices/tech/security/enhancements/enhancements43.html)
* [Security Enhancements in Android 4.2](https://source.android.com/devices/tech/security/enhancements/enhancements42.html)
* [Security Enhancements in Android 1.5 through Android 4.1](https://source.android.com/devices/tech/security/enhancements/enhancements41.html)
* [Android Security Enhancements](https://androidtamer.com/android-security-enhancements/)
* [Android Browsers Static Security Analysis](http://opensecurity.in/research/security-analysis-of-android-browsers.html)


Android use the standard process isolation to split application it therefore performs fork or exec when starting application. At the Unix level application are also, by default, run as different Unix users. This isolates application and protects applications from reading each-others data. By requesting permissions in the apk's AndroidManifest.xml it is possible to get those granted by the PackageManager. Such permissions can result in applications being run under the same user id as a other package or run with additional unix groups. Some group will give the application access to devices nodes or other files. Other Unix groups are mapped to Linux process capabilities being granted.

Binder:
As packages by default are running as separate user + process when they need to communicate the need to use some form of Inter process communication. In Android the preferred communication mechanism is called "binder". When processes communicate with each-other using binder Android will allow the service side code code to check if package manager was granted certain permissions by calling _checkCallingOrSelfPermission()_ of the context class. The implementation of this is done in the (native) service manager class. This class uses the service called “permission” that is implemented by the ActivityManagerService.java in the checkComponentPermission(permission, pid, uid, req-Uid) method and follow the following logic:

* The root and system user will always be granted all permissions
* If the permissions == NULL the permission will be granted
* In the other situation a request is sent to the package manager to check the permission based on the user id of the package.


Permissions
---------------

They are located at <code>frameworks/base/core/res/AndroidManifest.xml</code> and they're also can be declared in other manifest files (malware use this in order to bypass this mechanism). Pre-defined rules are depending with the OS version under <code>/etc/permissions/*.xml</code> or <code>frameworks/base/data/etc/platform.xml</code>, all other .xml files are read during the runtime. 

There are different Permission levels:
* Level 0 protection normal (A lower-risk permission that gives an application access to isolated application-level features)
* Level 1 protection dangerous (A higher-risk permission that would give a requesting application access to private user data or control over the device that can negatively impact the user)
* Level 2 protection signature (A permission that the system is to grant only if the requesting application is signed with the same certificate as the application that declared the permission.)
* Level 3 protection signature or system (A permission that the system is to grant only to packages in the Android system image or that are signed with the same certificates.)


Scheme:

* A permission definition is declared in a manifest file. The manifest frameworks/base/core/res/AndroidManifest.xml)
* A permission defines it’s own security level.
 contains many if not all of the platform permissions.
* A permission belongs to a group of permissions (this is mostly used by the UI to tag permissions).
* A permission belongs to a package and it has it’s certificate associated to it.

Feature implementations
---------------

The following examples show what can be integrated (of is already integrated) into the Kernel for security reasons:
* grsecurity (not available for Android yet)
* [SELinux](http://source.android.com/devices/tech/security/selinux/index.html) (introduced in Android 4.3.x -> Kernel 2.6+) [up-to-date info is always available here](http://seandroid.bitbucket.org/)
* ~SecDroid~ (optional - deprecated)
* CIS Benchmark (optional)
* Pentests (optional - Penetrations tests are only for experts and can be used to find known holes, like Metasploit)
* Bastille Linux (not available for Android yet)
* [IPTables](http://www.netfilter.org/projects/iptables/) (if not present AFWall+ brings them to your device!)
* [LIDS](http://sourceforge.net/projects/lids/) (not available for Android yet!)
* Linux Security Modules (LSMs)
* OpenSSL (integrated)
* Cryptography, AES, RSA, DSA, and SHA + API's for SSL/TLS and HTTPS (since Android 3.0 but weak) 
* [Encryption](https://source.android.com/devices/tech/security/encryption/index.html) (since 3.0 but weak)
* -> Known attacks can be found [here](https://source.android.com/devices/tech/security/overview/acknowledgements.html) [no direct links since they are mostly POC's and they of course will be fixed with next Android versions - so it's more for historical reasons]

Already implemented in every Android OS:
* Basic Password protection
* Process isolation like a Sandbox (Androids security model is based on application sandboxing)
* [Device Administration](https://developer.android.com/guide/topics/admin/device-admin.html) (Android 2.2+)
* A permission model for each user like on UNIX
* IPC security apps can explicitly share resources and data via Intents, ContentProviders, IPC and Binder and of course via the file system and local declared sockets)
* (optional) The ability to remove apps/system related kernel parts which may be insecure by design (depending on needs)
* Root ability (but restricted to the Kernel + a view core applications)
* (optional) [Security Tips | Android Developers](https://developer.android.com/training/articles/security-tips.html)
* Android supports many cryptographic and encryption tools and algorithms such as AES, RSA, DSA, SHA, CBC, and ESSIV:SHA256. See also [here](http://developer.android.com/training/articles/security-tips.html#Crypto), [here](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard), [here](http://en.wikipedia.org/wiki/RSA_%28cryptosystem%29) and [here](http://docs.oracle.com/javase/7/docs/api/java/security/MessageDigest.html).
* Another powerful security measure is that fact that the Android operating system (Linux kernel, libraries, app runtime + framework, etc.) is on its own partition that is read-only. Also, Android uses Unix-style file permissions (except for FAT32 filesystems). Due performance and security reasons it's recommend to use ext4 (because the integrated encryption works only with this and on Samsung devices it must be vfat).
* It is interesting to know that most of Android's security is meant to protect itself, not your personal files. While both are secured, there are more security measures that protect the operating system than the personal files (like pictures, music, contacts, etc.).


For app developer:
* [SIM Card Access](https://source.android.com/devices/tech/security/overview/app-security.html#sim-card-access) - Low level access to the SIM card is not available to third-party apps. The OS Handles all communication with the SIM Card.
* [Cost Sensitive APIs](https://source.android.com/devices/tech/security/overview/app-security.html#cost-sensitive-apis) - Cost sensitive means using them might generate cost, thus android provides a protection in OS level. Applications must grant explicit permission to use these APIs. Mostly: SMS/MMS, Telephony, Network/Data, In-App Billing or NFC Access.
* [Sensitive Data Input Devices](https://source.android.com/devices/tech/security/overview/app-security.html#sensitive-data-input-devices) - Applications must grant an explicit permission to use input devices such as the Camera, GPS and the Microphone.
* [Device Metadata](https://source.android.com/devices/tech/security/overview/app-security.html#sensitive-data-input-devices) - Device information might contains user information, thus applications must grant to access the OS system logs, Browser history, Hardware (such as network identification information) and the Phone number.
* [Application Signing](https://source.android.com/devices/tech/security/overview/app-security.html#application-signing) - ......
* [Storing Data - Internal Storage](http://developer.android.com/training/articles/security-tips.html#StoringData) - By default, files that you create on internal storage are accessible only to your app, make sure you not use _MODE_WORLD_READABLE_ & _MODE_WORLD_WRITEABLE_ (Google Play Store also scans for this) + Use content provider for share and prefer encryption for additional protection
* [Storing Data - External Storage](http://developer.android.com/training/articles/security-tips.html#StoringData) - Do not store executables or class files. + Always retrieve signed and verified files. See also [here](http://developer.android.com/training/articles/security-tips.html#StoringData).
* [Android Resources/Assets](http://stackoverflow.com/questions/3406581/security-of-android-assets-folder) - Please never share/store any sensitive information and if possible meta-data such as the keystore, keys,[...] because they can read and accessed by anyone.
* [Permission Model](http://developer.android.com/training/articles/security-tips.html#Permissions) - If possible: Minimize the permission request and never request unless it's really needed or it would break your app. Protect the IPC via permissions to avoid leak permission-protected data.
* [Performing Input Validation](http://developer.android.com/training/articles/security-tips.html#InputValidation) - If possible beware of JavaScript injection and SQL injection and verify the format. + Make sure if native code is used that there are non potential secure issues.
* [Handling User Data](http://developer.android.com/training/articles/security-tips.html#UserData) - If a GUID is required, use UUID. Do not use phone identifiers.
* [WebView](http://developer.android.com/training/articles/security-tips.html#WebView) - ....
* [Handling Credentials](http://developer.android.com/reference/android/accounts/AccountManager.html) - ...
* [Be aware of reverse engineering](https://code.google.com/p/android-apktool/) - The application apk can be exported easily. Apk itself is a zip folder and it is very easy to extract it (via apktool and others).
* [Be aware of app licensing](http://developer.android.com/google/play/licensing/index.html) - Google Play offers a licensing service that lets you enforce licensing policies for applications that you publish on Google Play. With Google Play Licensing, your application can query Google Play at run time to obtain the licensing status for the current user, then allow or disallow further use as appropriate.


[Networking](http://developer.android.com/training/articles/security-tips.html#Networking):
* CONFIG_FIREWALL
* CONFIG_IP_FORWARD
* CONFIG_SYN_COOKIES
* CONFIG_IP_FIREWALL
* CONFIG_IP_FIREWALL_VERBOSE
* CONFIG_IP_NOSR
* CONFIG_IP_MASQUERADE
* CONFIG_IP_MASQUERADE_ICMP
* CONFIG_IP_TRANSPARENT_PROXY
* CONFIG_IP_ALWAYS_DEFRAG
* CONFIG_NCPFS_PACKET_SIGNING
* CONFIG_IP_FIREWALL_NETLINK

* AID_NET_BT
* AID_NET_BT_ADMIN
* AID_INET 
* AID_NET_RAW 
* AID_NET_ADMIN

The full list can be found [here](https://github.com/ukanth/afwall/wiki/Android-kernel-traffic#protocol-statistics


Kernel Compiling:
* CONFIG_FILTER


SELinux
------------

Read the following pages to better understand and possible solve SELinux related problems (scripts, binaries,..):
* [Customize SELinux](https://source.android.com/devices/tech/security/selinux/customize.html)
* [Deeper look into the source code](https://android.googlesource.com/platform/external/sepolicy/+/master)
* [Commands](https://github.com/xmikos/setools-android/tree/master/jni/sepolicy-inject)


The IPTables/scripts should run at <code>u:r:init:s0</code> to avoid possible security problems, since this is the only authorization we need.


Android Logging System
------------

The Android system has a logging facility that allows system wide logging of information, from applications and system components. This is separate from the Linux kernel's own logging system, which is accessed using 'dmesg' or '/proc/kmsg'. However, the logging system does store messages in kernel buffers. 

- The Kernel driver is called logger
- Application Log is used by [android.util.Log class](http://developer.android.com/reference/android/util/Log.html)
- System log is used to keep their messages separate from the application log messages system
- Event log are created by using [android.util.EventLog class](http://developer.android.com/reference/android/util/EventLog.html)
- ADB (Android Debug Bridge) reference page is available over [here](http://developer.android.com/guide/developing/tools/adb.html) (if ADB is not global wide disabled it's the easiest way to _logcat_ the necessary stuff you want to see)

We have four log buffers:
- events : System event related information <code>/dev/log/main</code>
- main : The main application log
- radio : Phone/radio related information
- system : Low-level system messages (for e.g. debugging reasons) 

GSM security
------------

Android use the following Global System for Mobile Communications specifications:
* A5/0 was disabled in Europe by most providers 
* Encryption is based on [A5/1](http://en.wikipedia.org/wiki/A5/1) ([known as vulnerable](http://cryptome.org/a51-bsw.htm) + [Security](http://en.wikipedia.org/wiki/A5/1#Security)) The key length is 128 bit, it's widely used in North America.
* [A5/2](https://en.wikipedia.org/wiki/A5/2) seems also deprecated
* [A5/3](https://en.wikipedia.org/wiki/A5/3) is mostly used 
* UMTS-Communication use [Signalling System 7](http://en.wikipedia.org/wiki/Signalling_System_No._7) (SS7) which can be bypassed via side-channel attacks
* NSA and others are able to [crack it](http://yro.slashdot.org/story/13/12/14/0148251/nsa-able-to-crack-a51-cellphone-crypto) + [Full Story](http://www.washingtonpost.com/business/technology/by-cracking-cellphone-code-nsa-has-capacity-for-decoding-private-conversations/2013/12/13/e119b598-612f-11e3-bf45-61f69f54fc5f_story.html)
* Handy-Jammer are _often_ used to block UTMS signals, so that the lower bandwidth e.g. over GPRS will be used instead - because the basic station force the connectivity. After that it's possible to enable the _bull-encryption_ option. 
* Some provider claim to use a more secure A5/3 instead of [A5/1](http://www.scard.org/gsm/a51.html) or A5/2, needs confirmation (country specific).
* The full specifications for all networks can be found over [here](http://www.gsma.com/technicalprojects/fraud-security/security-algorithms)


LTE Vs. [GSM](http://www.gsmworld.com/)

In fact the new LTE is less secure since there is no common used standard which makes it possible to get the data via e.g. Ethernet-Uplink. Some providers saying that they use a strong encryption via digital signatures, the problem is that there is no proof if they really use such security gateway (because the LTE standard does not require such gateway).


How the NSA/GCHQ crack it?
* [How the NSA Hacks Cellphone Networks Worldwide](https://firstlook.org/theintercept/2014/12/04/nsa-auroragold-hack-cellphones/)
* Attacks on A5/3 are "[OPULENT PUP](https://firstlook.org/theintercept/document/2014/12/04/opulent-pup-encryption-attack/)" and "[WOLFRAMITE](https://firstlook.org/theintercept/document/2014/12/04/wolframite-encryption-attack)"
* An cracking example is available as .pdf over [here](http://cryptome.org/gsm-crack-bbk.pdf).
* [How Spies Stole the Keys to the Encryption Castle](https://firstlook.org/theintercept/2015/02/19/great-sim-heist/)


Research:
* An [Wiki is available by SRLabs](https://opensource.srlabs.de/projects/a51-decrypt) which shows A5/1 weaknesses
* [Ciphering Indicator - Android Issue #5353](https://code.google.com/p/android/issues/detail?id=5353)
* [GSM 02.07 specification (Version 7.1.0 Released in 1998)](http://www.3gpp.org/ftp/Specs/archive/02_series/02.07/0207-710.zip)
* A basic whitepaper and overview of GSM, UMTS and LTE security can be found over [here](https://eprint.iacr.org/2013/227.pdf)
* [Network bugs](https://bugs.launchpad.net/telephony-service/+bug/1276208/+attachment/3968775/+files/ts_122101v110900p.pdf)


Detectors:
* [Android IMSI-Catcher Detector](https://secupwn.github.io/Android-IMSI-Catcher-Detector/)
* [darkshark](https://play.google.com/store/apps/details?id=com.darshak&hl=en) - [Source](https://github.com/darshakframework/darshak)
* [SnoopSnitch](https://opensource.srlabs.de/projects/snoopsnitch)
* [Am I Vulnerable](http://delhi.securitycompass.com/) - Scans for known security holes (no 0day only public).


:warning: Actually there is no tool/app/API which protects against this known weaknesses, all apps only warn about possible problems but they're not able to fix them! :warning:


Disable binaries
------------

Each application is given its own Linux User ID (UID) and group ID, besides this indication some apps need the following binaries to work, so removing them blocks possible attacks direct on the device (Offline). 

* irsii
* bash (optional, default removed)
* SQL 
* nano
* nc (net cat)
* netserver
* netperf
* opcontrol
* rsync
* scp
* sdptest
* ssh
* sshd
* strace
* tcpdump
* vim
* ping (needs root)
* pm (needs root)
* adbd  (needs root -> adb deamon)
* ...


Security Tricks
--------------

Some applications require more permissions (e.g. Flashlight) than they really need. You can alter the set of permissions granted to an application by editing /data/system/pacakges.xml.
After editing you need to reboot the device, because it gets overwritten by the system again. All changes will be read at system boot.

On some external ROMs, there is also an GUI for this, it's called [Privacy Guard](https://www.julianevansblog.com/2014/06/how-to-use-android-privacy-guard-on-cyanogenmod-11.html) which first was introduced in CyanogenMod 11. This makes it easier for users to disable critical permissions on an app-by-app basis. An alternative on newer system seems XPrivacy (includes a basic _firewall_) or AppOps.

Protection against spoofing attacks
```
#Global via sysctl
net.ipv4.conf.all.rp_filter=1
#net.ipv6.conf.all.rp_filter=1

#Via iptables (for e.g. logging) [this solution is better]
iptables -t raw -I PREROUTING -m rpfilter --invert -j DROP
````

SMS:
With Android 4.2, the user gets a notification when the SMS feature is used, then user decides to whether send the message or not.


Application Signing:
Never ever forget your keystore. Send it via email to yourself to keep it. If you lose your keystore, you won't be able to get it back and you cannot update your application anymore.

General Security tips:
* Can be found over [here](http://developer.android.com/training/articles/security-tips.html)
* [Projects/OWASP Mobile Security Project - Top Ten Mobile Controls](https://www.owasp.org/index.php/Projects/OWASP_Mobile_Security_Project_-_Top_Ten_Mobile_Controls)
* [Secure coding under Android](https://www.securecoding.cert.org/confluence/pages/viewpage.action?pageId=111509535)


Protection against other attacks
--------------

Bruteforce
A lot of wrong statements are on the internet to _stop_ e.g. bruteforce by simply using a blacklist. In fact if you use iptables as a blacklist you will suffering from the following problems:
*  /var may become full because everything will be logged in here (e.g. in the attacker is pounding our server)
* The attack may already got your IP address which allows to send packets with a spoofed source header, overall that would mean you get locked out of the server
* Higher memory usage 


To solve this you can work with SSH keys, which are not affected by the above two mentioned problems. To do so we can use:
```
iptables -N SSH_IN
iptables -A INPUT -p tcp --dport ssh -m conntrack --ctstate NEW -j SSH_IN
iptables -A SSH_IN -m recent --name ssh --rttl --rcheck --hitcount 5 --seconds 15 -j DROP
iptables -A SSH_IN -m recent --name ssh --rttl --rcheck --hitcount 6 --seconds 1200 -j DROP 
iptables -A SSH_IN -m recent --name ssh --set -j ACCEPT
```

This was original taken from [here](http://compilefailure.blogspot.com/2011/04/better-ssh-brute-force-prevention-with.html).


Android Marshmallow
--------------

With Android Marshmallow (Kernel 4.1+) (API level 23) a lot of _under the hood_ changes are coming to the user including a new [permission controlling system](http://www.androidpolice.com/2015/05/28/io-2015-android-m-will-include-a-new-enhanced-permission-management-scheme-with-fine-control-over-apps/) that works similar compared to AppOps and integrates an user interface that allow to control most of all [permissions](https://developer.android.com/preview/features/runtime-permissions.html). Compared to Android 4/Lollipop that means you don't have only two options (take it and accept it or simply do not install it) - now you now can just install everything and change e.g. the camera permission to avoid that your new installed application (or existent) [can take control over that](https://en.wikipedia.org/wiki/Android_M). See, also the [https://developer.android.com/preview/overview.html).

In fact that makes more or less most of _security apps_ obsolete since the user can control what permission which app can really use. Except the internet stuff (since Google simply never wants it, because ads reasons,[...]) which overall means firewalls have still a solid standing. 

The same principle as a firewall can be used to control the permission (whitelist/revoke access), which possible fc some apps first that are not updated. For malware reasons that means using AFWall+ & the new system it would be a lot harder to bypass all of these protections (of course the existent apps need a small update to support that feature).

Firewall specific:
Under Android M, app data will be automatically [backed-up](http://developer.android.com/preview/backup/index.html) onto Google Drive on a daily basis, when the phone is connected to Wi-Fi and power (opt-out option), that may lead in more traffic if the user forgets to disable it, so it's highly recommend to check this setting. 
See also AOSP Network Security Policy, over [here](https://android.googlesource.com/platform/frameworks/base/+/refs/heads/master/core/java/android/security/NetworkSecurityPolicy.java?ref=driverlayer.com%2Fweb%2F%2F%2F%2F%2F%2F%2F).

HTTPS:
<code>application android:usesCleartextTraffic = "false” /</code> will prevent any component in the app from performing any network I/O over unencrypted socket connections (supports HTTP, FTP, WebSockets, IMAP, SMTP & XMPP). This was designed for third party libraries that use insecure channels of communication. In fact that could be bypassed in the future.

Useful links
------------

* [Android (Operating System) overview | Wikipedia.org](https://en.wikipedia.org/wiki/Android_(operating_system))
* [OpenBTS open source tool | Wikipedia.org](http://en.wikipedia.org/wiki/OpenBTS)
* [Building Kernels | Android Developers](https://source.android.com/source/building-kernels.html) and a tutorial on [XDA](http://www.xda-developers.com/compile-kernel-tutorial/)
* [System and kernel security | Android Developers](https://source.android.com/devices/tech/security/overview/kernel-security.html)
* [Kernel Documentation/android.txt](http://android.git.kernel.org/?p=kernel/common.git;a=blob_plain;f=Documentation/android.txt;hb=HEAD)
* [Kernel analyzed (2010) | The Register](http://www.theregister.co.uk/2010/11/02/android_security/)
* [Android Kernel Features | eLinux.org](http://elinux.org/Android_Kernel_Features)
* [Linux Linux Kernel:List of security vulnerabilities | CVEDetails.com](http://www.cvedetails.com/vulnerability-list/vendor_id-33/product_id-47/cvssscoremin-7/cvssscoremax-7.99/Linux-Linux-Kernel.html) 
* [How does Android's security model differ from UNIX's | Security.StackExchange.com](http://security.stackexchange.com/questions/72733/how-does-androids-security-model-differ-from-unixs)
* [Tutorial: Building Secure Android Applications by William Enck and Patrick McDaniel | SIIScse.psu.edu](http://siis.cse.psu.edu/android-tutorial.html)
* [AUSA secure android kernel technology | GCN.com](http://gcn.com/Articles/2011/10/11/AUSA-secure-andriod-kernel-technology.aspx?Page=1)
* [Security Enhancements for Android quick overview | selinuxproject.org](http://selinuxproject.org/page/NB_SEforAndroid_1)
* [Validate SElinux | Source.Android.com](https://source.android.com/devices/tech/security/selinux/validate.html)
* [HowTo: Linux Hard Disk Encryption With LUKS | cyberciti.biz](http://www.cyberciti.biz/hardware/howto-linux-hard-disk-encryption-with-luks-cryptsetup-command/)
* [Effective Android Security to build a secure Android app| GitHub.com](https://github.com/orhanobut/effective-android-security)
* [Cellular encryption article | blog.cryptographyengineering.com](http://blog.cryptographyengineering.com/2013/05/a-few-thoughts-on-cellular-encryption.html)
* [eLinux Android category | eLinux.org Wiki](http://elinux.org/Category:Android)
* [Linux Security Summit 2015/Abstracts/Smalley | kernsec.org](http://kernsec.org/wiki/index.php/Linux_Security_Summit_2015/Abstracts/Smalley)
* [Kernel Planet | Planet.Kernel.org](http://planet.kernel.org/)


Feed:
* [Black Hat media feed for audio and video stuff](http://www.blackhat.com/BlackHatRSS.xml)



Security focused forums and discussions:
* [Mobile Security Wiki | mobilesecuritywiki.com](https://mobilesecuritywiki.com/) - [Source](https://github.com/exploitprotocol/mobile-security-wiki)
* [/r/Netsec | reddit.com](https://www.reddit.com/r/netsec) - [/r/AndSec](http://www.reddit.com/r/andsec)


PDF:
* [PCI Mobile Payment Acceptance Security Guidelines for Developers](https://www.pcisecuritystandards.org/documents/Mobile%20Payment%20Security%20Guidelines%20v1%200.pdf)
* [Android Security Hide Android Applications in Images | BlackHat.com](https://www.blackhat.com/docs/eu-14/materials/eu-14-Apvrille-Hide-Android-Applications-In-Images-wp.pdf)
* [Android Security overview at Black Hat 2011 | BlackHat.com](https://www.blackhat.com/docs/webcast/bhwebcast30_anderson.pdf)
* [Android Fake ID hole | BlackHat.com](https://www.blackhat.com/docs/us-14/materials/us-14-Forristal-Android-FakeID-Vulnerability-Walkthrough.pdf)
* [Android: One Root to Own Them All | media.blackhat.com](https://media.blackhat.com/us-13/US-13-Forristal-Android-One-Root-to-Own-Them-All-Slides.pdf)
* [A Look Inside the Android Kernel with Automated Tools by Coverity | Coverity.com](http://www.coverity.com/library/pdf/a-look-inside-the-android-kernel.pdf)
* [Android security maximized by Samsung KNOX | SamsungKnox.com](https://www.samsungknox.com/ru/system/files/whitepaper/files/Android%20security%20maximized%20by%20Samsung%20KNOX_2.pdf)
* [Android Hacker protection | DefCon.org](https://www.defcon.org/images/defcon-22/dc-22-presentations/Strazzere-Sawyer/DEFCON-22-Strazzere-and-Sawyer-Android-Hacker-Protection-Level-UPDATED.pdf)
* [DefCon 23 Material | media.defcon.org](https://media.defcon.org/DEF%20CON%2023/DEF%20CON%2023%20presentations/Speaker%20&%20Workshop%20Materials/)
* [One Class to Rule Them All: Deserialization Vulnerabilities in Android (CVE-2015-3825) and SDKs | usenix.org](https://www.usenix.org/system/files/conference/woot15/woot15-paper-peles.pdf)
* [ATTACKING THE LINUX PRNG ON ANDROID](https://www.usenix.org/system/files/conference/woot14/woot14-kaplan.pdf)
* [Attacks on Android Clipboard](http://www.cis.syr.edu/~wedu/Research/paper/clipboard_attack_dimva2014.pdf)
* [A Study of Android Application Security](http://www.cs.rice.edu/~sc40/pubs/enck-sec11.pdf)
* [Why Eve and Mallory Love Android: An Analysis of Android SSL (In)Security](http://www2.dcsec.uni-hannover.de/files/android/p50-fahl.pdf)
* [PowerSpy: Location Tracking using Mobile Device Power Analysis](http://arxiv.org/pdf/1502.03182v2)
* [Leaving our ZIP undone: how to abuse ZIP to deliver malware apps](https://www.virusbtn.com/pdf/conference/vb2014/VB2014-Panakkal.pdf)
* [Hey, You, Get off of My Market: Detecting Malicious Apps in Official and Alternative Android Markets](http://www.csc.ncsu.edu/faculty/jiang/pubs/NDSS12_DROIDRANGER.pdf)
* [Evaluations of Security Solutions for Android Systems](http://arxiv.org/ftp/arxiv/papers/1502/1502.04870.pdf)


Android Vulnerability List:
* [Android CVE Details | cvedetails.com](http://www.cvedetails.com/vulnerability-list/vendor_id-1224/product_id-19997/Google-Android.html)
* [Android Vulnerability/Exploit List | Google Docs](https://docs.google.com/spreadsheet/pub?key=0Am5hHW4ATym7dGhFU1A4X2lqbUJtRm1QSWNRc3E0UlE&single=true&gid=0&output=html)


Online Analyzers:
* [Android Observatory](https://androidobservatory.org/)
* [Android APK Decompiler](http://www.decompileandroid.com/)
* [Virus Total](https://www.virustotal.com/)
* [Mobile Sandbox](http://mobilesandbox.org/)


Android Testing:
* [Appie](https://manifestsecurity.com/appie)
* [AppUse](https://appsec-labs.com/AppUse/)
* [Android Tamer](https://androidtamer.com/)
* [Mobisec](http://sourceforge.net/projects/mobisec/)
* [Vezir Project Vm](https://github.com/oguzhantopgul/Vezir-Project)


Hooks (may unstable, all of them requires root):
* [ADBI](https://github.com/crmulliner/ddi)
* [Xposed](http://forum.xda-developers.com/xposed/xposed-installer-versions-changelog-t2714053)
* [Frida](http://www.frida.re/)
* [Cydia](http://www.cydiasubstrate.com/)


Forensics tools:
* [Android Forensics](https://github.com/viaforensics/android-forensics)
* [Open Source Android Forensics](http://www.osaf-community.org/)
* [Android Data Extractor Lite](https://github.com/mspreitz/ADEL)
* [BitPim](http://www.bitpim.org/)
* [pySimReader](https://www.isecpartners.com/tools/mobile-security/pysimreader.aspx)
* [P2P-ADB](https://github.com/kosborn/p2p-adb/)


Todo:
* add example malware app on android to show possible holes (low-prio), most known and popular vulnerable app seems [InsecureBank V2](https://github.com/dineshshetty/Android-InsecureBankv2)
* Sort vuln. via own categories 