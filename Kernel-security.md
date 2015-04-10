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

The Linux Kernel isn't exactly the Android Kernel, there are several things you can't find in the original Linux kernel, like OOM, Wakelocks, pmem, Binder, logger, ... - These features are [unique to Android](http://www.linuxfoundation.org/news-media/blogs/browse/2013/03/defining-android-vs-embedded-linux) only systems.


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
* SELinux (introduced in Android 4.1.x -> Kernel 2.6+)
* ~SecDroid~ (optional - deprecated)
* CIS Benchmark
* Bastille Linux (not available for Android yet)
* IPTables (if not present AFWall+ brings them to your device)
* LIDS (not available for Android yet)
* Linux Security Modules (LSMs)
* Cryptography, AES, RSA, DSA, and SHA + API's for SSL/TLS and HTTPS (since 3.0 but weak) 
* [Encryption](https://source.android.com/devices/tech/security/encryption/index.html) (since 3.0 but weak)

Already implemented in every Android OS:
* Password protection
* Process isolation like Sandbox
* [Device Administration](https://developer.android.com/guide/topics/admin/device-admin.html) (Android 2.2+)
* A permission model for each user like on UNIX
* IPC security apps can explicitly share resources and data via Intents, ContentProviders, IPC and Binder and of course via the file system and local declared sockets)
* (optional) The ability to remove apps/system related kernel parts which may be insecure by design (depending on needs)
* Root ability (but restricted to the Kernel + a view core applications)



Network:
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

On some external Roms, there is also an GUI for this, it's called [Privacy Guard](https://www.julianevansblog.com/2014/06/how-to-use-android-privacy-guard-on-cyanogenmod-11.html) which first was introduced in CyanogenMod 11. This makes it easier for users to disable critical permissions on an app-by-app basis. 



Useful links
------------

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