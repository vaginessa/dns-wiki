Index
-----

* [Many Android apps leak user privacy data](#many-android-apps-leak-user-privacy-data)
* [What's the issue?](#what's-the-issue?)
* [What's the risk?](#what's-the-risk?)
* [Does I have such problem?](#does-i-have-such-problem?)
* [Temporally workarounds](#temporally-workarounds)
* [Default IPtables Chain Policy](#default-iptables-chain-policy)
* [Useful links](#useful-links)

Many Android apps leak user privacy data
----------------------------------------

First this bug is a design hole by Android OS itself and have nothing to do with AFWall+ or any other Firewall itself. 

What's the issue?
-----------------

Certain Android applications [#168](http://code.google.com/p/droidwall/issues/detail?id=168), [#235](http://code.google.com/p/droidwall/issues/detail?id=235), [#262](http://code.google.com/p/droidwall/issues/detail?id=262), including ones officially bundled with the OS such as Calendar, Google Play Service, Contacts and Picasa, send certain data, including authentication tokens (a form of password used to identify a user), in a clear rather than encrypted format. This claim was made by German security researchers [Bastian Könings, Jens Nickels, and Florian Schaub from the University of Ulm](http://www.uni-ulm.de/en/in/mi/staff/koenings/catching-authtokens.html).

What's the risk?
----------------

The main problem with mobile data connection is that most data plan from mobile carrier is not unlimited. When connection is established and data is transferred, the free bandwidth allocation is used and depleted. Once the free data transfer quota limit has been used up, users have to pay exorbitantly and ridiculously high and expensive price on extra data bandwidth been used.

Think of it like accessing your online banking service via a website that isn't secure. Theoretically, anyone could intercept that unencrypted data as it travels between your computer and the bank's server, stealing your password details or initiating false transactions.

**Warning**
Please do not post this <code>bugreport</code> anymore here on Github, we know about it and it was already mentioned many many times (#7, #91, #172, #235 and in the original Droidwall issue tracker).
It's pretty annoying and frustrating to see this again and again with every new Android OS version or new devices. Once there will be a final solution we will write it here not as workaround but as final solution.

Does I have such problem?
-------------------------

You an check this with any [Monitoring Data App](https://play.google.com/store/apps/developer?id=Onavo). Traffic at start shows you that you already have this data leak.

>* /sys/class/net/[interface]/statistics/tx_bytes
>* /sys/class/net/[interface]/statistics/rx_bytes 

These are the files that store traffic stats data. If you want to look manually through file manager.
For received UID traffic stats take a look at here:

>* /proc/uid_stat/[uid]/tcp_rcv
>* /proc/uid_stat/[uid]/tcp_snd

On older versions procfs is mounted at boot time, which means that every time your device is rebooted there are 0 traffic values for all UIDs (except lan0).
You can list _/proc/uid_stat/_ dir right now to see which UIDs have been spending traffic since last reboot.

Temporally workarounds
----------------------
**Maybe not working on all devices!**

Method 1: **Disable Data Connection on Android Core**
- Open dialer
- Dial *#*#4636#*#* or *#*#6436#*#* (also take a look at the [[Phone codes secrets]] article)
- Tap _'Phone Information'_
- Press Menu button
- Tap on _'More'_
- Tap _'Disable data connection'_, or _'disable data on boot'_. 
- Once the boot are complete you an re-enable the data connection.



Method 2: **Disable Data Connection through AFWall+**

For the second workaround you need [[init.d]] support. Ask you ROM/Kernel Developer or check the README if you have init.d support or not. Do **NOT** ask on this Github repository!
- Open AFWall+ Settings
- Scroll down until you see _'Experimental Preferences'_
- Enable _'Fix Startup Data Leak'_



Method 3: **APNDroid to Turn Off Data Connection**

[APNDroid](https://play.google.com/store/apps/developer?id=Apndroid+Inloop) prevents the phone from connecting to Internet through cellular data line by modifying the APN (Access Point Name) names and APN types by adding ‘apndroid’ suffix to them. There is also a script based Locale plug-in for free. 



Method 4: **XPrivacy**

There are a lot of difficult patches like Openpdroid, Pdroid 2.0 or PDroid which allow to prevent applications from leaking privacy sensitive data but this patches need to be integrate into the ROM you are using. It's maybe to complicated for some users and not work with all Apps without an FC (force close).
[XPrivacy](https://github.com/M66B/XPrivacy#installation) is the only solution that works an almost every device without patching everything after you upgrade your ROM like CM 10.1 and do almost the same.


Default IPtables Chain Policy
-----------------------------

The Default Android OS iptables chain policy is ACCEPT for all INPUT, FORWARD and OUTPUT policies. We can easily change this default policy to DROP with below listed commands.

>* iptables -P INPUT DROP
>* iptables -P FORWARD DROP
>* iptables -P OUTPUT DROP

After changing the INPUT, FORWARD, OUTPUT policies to DROP, all the incoming/outgoing/forwarding connections are dropped by AFWall+. So we need to open every INPUT, FORWARD and OUTPUT connections in firewall/iptables with rules. If you change the default OUTPUT policy to DROP you cannot communicate with other systems/networks.

Useful links
------------

* [TaintDroid: Realtime Privacy Monitoring on Smartphones | Appanalysis](http://appanalysis.org/)
* [Disabling Mobile Data at First Boot | XDA-Forum](http://forum.xda-developers.com/showthread.php?p=7196260)
* [How to Minimize Your Android Data Usage and Avoid Overage Charges | How-To Geek](http://www.howtogeek.com/140261/how-to-minimize-your-android-data-usage-and-avoid-overage-charges/)
* [The Android boot process from power on | Xdin Android Blog](http://www.androidenea.com/2009/06/android-boot-process-from-power-on.html)
* [Android Community | Android Source] (http://source.android.com/source/community/index.html)
* [iptables4n1 workaround for Android Froyo (Nexus One) | Google Code]  (http://code.google.com/p/iptables4n1/)
* [Possible userinit.sh & Orbot leak fix | https://github.com/ukanth/afwall/wiki/CustomScripts#some-examples]