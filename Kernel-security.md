Index
-----

* [Introduction](#introduction)
* [Implementations](#implementations)
* [Disable binaries](#disable-binaries)
* [Useful links](#useful-links)


An fantastic explanation and overview about Android's security can be found at the [TheNewCircle](https://thenewcircle.com/s/post/1473/Android_Security_Underpinnings.htm) [05.2013] or directly on [source.android.com](https://source.android.com/devices/tech/security/).

:warning: This is a POC (Proof-of-Concept) and WIP (Work-in-process) article, it's not designed (and never will be) to explain everything or show every possible configuration (it's simply impossible) and is more designed to quickly show the most important configurations! 

Due the lack of security of the consumer devices is not necessarily Android's problem. So far, most problems come from OEM's customization, and third party malicious apps pretending to be good citizen. Hardening the kernel won't fix all of these problems.


To prevent some of the latest networking attacks, you should try to keep your kernel version current as soon as possible.

The reason why Android (Kernel is based on Linux) and open source will _always_ be more secure compared than Windows is because you can build advanced security right in.

Introduction
------------

Since AFWall+ is a GUI for iptables, people may started to want to know more about how they can generally more secure the lowest-level (like iptables) - the Kernel itself. As the kernel controls your computer's networking, it is important that it be very secure, and not be compromised. 

The config file of your currently running kernel is also always available in your file system at <code>/proc/config.gz</code>.

The main benefit and the main goal is to secure the kernel, so that no external tools are necessary. 

The Linux Kernel isn't exactly the Android Kernel, there are [several things you can't find in the original Linux kernel](http://www.elinux.org/Android_Kernel_Features), like OOM, Wakelocks, pmem, Binder, logger, ... - These features are [unique to Android](http://www.linuxfoundation.org/news-media/blogs/browse/2013/03/defining-android-vs-embedded-linux) only systems.


Kind of attacks:
* Offline attacks (physical attacks, side-loading, bypassing, off-device attacks ...[mostly needs root]).
* Online attacks (on the network stack against malware, dos, ...)
(optional - [OS specific](https://source.android.com/devices/tech/security/enhancements/enhancements50.html))
* On Android 5+ the user password is now protected against ordinary [brute-force](http://en.wikipedia.org/wiki/Brute_force) attacks using [scrypt](http://en.wikipedia.org/wiki/Scrypt) and, if available, the key is bound to the hardware keystore to prevent off-device attacks (i.e. brute-force). As always, the Android screen lock secret and the device encryption key are not sent off the device or exposed to any application. 
* TLSv1.1 + 1.2 are now enabled by default. Look, [here](https://developer.android.com/reference/javax/net/ssl/SSLSocket.html) that also means that Forward secrecy is enabled too which is used in AES-GCM (needs Services 7.x+ installed). The weak and vulnerably cipher suites like MD5, 3DES are by default disabled. 

Implementations
---------------

The following examples show what can be integrated (of is already integrated) into the Kernel for security reasons:
* grsecurity (not available for Android yet)
* [SELinux](http://source.android.com/devices/tech/security/selinux/index.html) (introduced in Android 4.3.x -> Kernel 2.6+) [up-to-date info is always available here](http://seandroid.bitbucket.org/)
* ~SecDroid~ (optional - deprecated)
* CIS Benchmark (optional)
* Pentests (optional - Penetrations tests are only for experts and can be used to find known holes, like Metasploit)
* Bastille Linux (not available for Android yet)
* IPTables (if not present AFWall+ brings them to your device)
* LIDS (not available for Android yet)
* Linux Security Modules (LSMs)
* Cryptography, AES, RSA, DSA, and SHA + API's for SSL/TLS and HTTPS (since 3.0 but weak) 
* [Encryption](https://source.android.com/devices/tech/security/encryption/index.html) (since 3.0 but weak)
* -> Known attacks can be found [here](https://source.android.com/devices/tech/security/overview/acknowledgements.html) [no direct links since they are mostly POCs and they of course will be fixed with next Android versions - so it's more for historical reasons]

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

Kernel-level security
------------

An overview which permission an application use can be done via terminal <code>find /data/data -type f | xargs ls -l | sort -k3 -n</code>. To get the an sample process list we just can use <code>ps</code>.


Application-level security
------------

Android also uses a user-space level security system to regulate communication and interaction among applications and system components.


Disable binaries
------------

Each application is given its own Linux user ID (UID) and group ID, besides this indication some apps need the following binaries to work, so removing them blocks possible attacks direct on the device (Offline). 

* irsii
* bash
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


Security Tricks
--------------

Some applications request more permissions (e.g. Flashlight) than they really need. You can alter the set of permissions granted to an application by editing /data/system/pacakges.xml.
After editing you need to reboot the device, because it gets overwritten by the system again. All changes will be read at system boot.

On some external ROMs, there is also an GUI for this, it's called [Privacy Guard](https://www.julianevansblog.com/2014/06/how-to-use-android-privacy-guard-on-cyanogenmod-11.html) which first was introduced in CyanogenMod 11. This makes it easier for users to disable critical permissions on an app-by-app basis. 


SMS:
With Android 4.2, the user gets a notification when the SMS feature is used, then user decides to whether send the message or not.


Application Signing:
Never ever forget your keystore. Send it via email to yourself to keep it. If you lose your keystore, you won't be able to get it back and you cannot update your application anymore.



Useful links
------------

* [Android (operating system) | Wikipedia.org](https://en.wikipedia.org/wiki/Android_(operating_system))
* [Building Kernels | Android Developers](https://source.android.com/source/building-kernels.html) an on [XDA](http://www.xda-developers.com/compile-kernel-tutorial/)
* [System and kernel security | Android Developers](https://source.android.com/devices/tech/security/overview/kernel-security.html)
* [Documentation/android.txt](http://android.git.kernel.org/?p=kernel/common.git;a=blob_plain;f=Documentation/android.txt;hb=HEAD)
* [Kernel analyzed | The Register](http://www.theregister.co.uk/2010/11/02/android_security/)
* [Android Kernel Features | eLinux.org](http://elinux.org/Android_Kernel_Features)
* [Linux Linux Kernel:List of security vulnerabilities | CVE Details](http://www.cvedetails.com/vulnerability-list/vendor_id-33/product_id-47/cvssscoremin-7/cvssscoremax-7.99/Linux-Linux-Kernel.html) 
* [How does Android's security model differ from UNIX's | Security.StackExchange](http://security.stackexchange.com/questions/72733/how-does-androids-security-model-differ-from-unixs)
* [Tutorial: Building Secure Android Applications by William Enck and Patrick McDaniel | SIIS](http://siis.cse.psu.edu/android-tutorial.html)
* [Android Security - Black Hat 2011 (.pdf)](https://www.blackhat.com/docs/webcast/bhwebcast30_anderson.pdf)
* [AUSA secure android kernel technology | GCN](http://gcn.com/Articles/2011/10/11/AUSA-secure-andriod-kernel-technology.aspx?Page=1)
* [Security Enhancements for Android™ quick overview | selinuxproject.org](http://selinuxproject.org/page/NB_SEforAndroid_1)
* [HowTo: Linux Hard Disk Encryption With LUKS | cyberciti.biz](http://www.cyberciti.biz/hardware/howto-linux-hard-disk-encryption-with-luks-cryptsetup-command/)
* [Android security maximized by Samsung KNOX (.pdf) | SamsungKnox.com](https://www.samsungknox.com/ru/system/files/whitepaper/files/Android%20security%20maximized%20by%20Samsung%20KNOX_2.pdf)
* [Effective Android Security to build a secure Android app| GitHub](https://github.com/orhanobut/effective-android-security)