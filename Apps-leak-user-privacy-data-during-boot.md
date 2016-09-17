First, this bug is a design hole by Android OS itself and have nothing to do with AFWall+ or any other external Firewall.

Index
-----

* [What's the issue?](#whats-the-issue-?)
* [What's the risk?](#whats-the-risk-?)
* [Do I have such a problem?](#do-i-have-such-a-problem-?)
* [Workarounds](#workarounds)
* [IPtables specific problem](#iptables-specific-problem)
* [Default IPtables Chain Policy](#default-iptables-chain-policy)
* [Useful links](#useful-links)


What's the issue?
-----------------

Certain Android applications [#168](http://code.google.com/p/droidwall/issues/detail?id=168), [#235](http://code.google.com/p/droidwall/issues/detail?id=235), [#262](http://code.google.com/p/droidwall/issues/detail?id=262), including ones officially bundled with the OS such as Calendar, Google Play Service, Contacts and Picasa, send certain data, including authentication tokens (a form of password used to identify a user), in a clear rather than encrypted format. This claim was made by German security researchers [Bastian Könings, Jens Nickels, and Florian Schaub from the University of Ulm](http://www.uni-ulm.de/en/in/mi/staff/koenings/catching-authtokens.html).

What's the risk?
----------------

The main problem with mobile data connection is that most data plan from mobile carrier is not unlimited. When connection is established and data is transferred, the free bandwidth allocation is used and depleted. Once the free data transfer quota limit has been used up, users have to pay exorbitantly and ridiculously high and expensive price on extra data bandwidth been used.

Think of it like accessing your online banking service via a website that isn't secure. Theoretically, anyone could intercept that unencrypted data as it travels between your computer and the bank's server, stealing your password details or initiating false transactions.

Do I have such a problem?
-------------------------

You can check this with any [Monitoring Data App](https://play.google.com/store/apps/developer?id=Onavo). Traffic at start shows you that you already have this data leak.

>* /sys/class/net/[interface]/statistics/tx_bytes
>* /sys/class/net/[interface]/statistics/rx_bytes 

These are the files that store traffic stats data. If you want to look manually through file manager.
For received UID traffic stats take a look at here:

>* /proc/uid_stat/[uid]/tcp_rcv
>* /proc/uid_stat/[uid]/tcp_snd

On older versions procfs is mounted at boot time, which means that every time your device is rebooted there are 0 traffic values for all UIDs (except lan0).
You can list <code>/proc/uid_stat/</code> dir right now to see which UIDs have been spending traffic since last reboot.

Workarounds
----------------------

**May not work on all devices/ROM's**

Method 1: **Disable Data Connection of Android Core**
- Open the 'Dialer'
- Dial _'*#*#4636#*#*'_ or _'*#*#6436#*#*'_ (also take a look at the [[Phone codes secrets]] article)
- Tap _'Phone Information'_
- Press Menu button
- Tap on _'More'_
- Tap _'Disable data connection'_, or _'disable data on boot'_. 
- Once the boot has completed you can re-enable the data connection.



Method 2: **Disable Data Connection through AFWall+**

For the second workaround, you need [[init.d]] support. Ask your ROM/Kernel Developer or check the README.md if you have init.d support or not. Please do **NOT** ask on this GitHub repository!
- Open AFWall+ Settings
- Scroll down until you see _'Experimental Preferences'_
- Enable _'Fix Startup Data Leak'_

**Warning**

Please do not post this <code>bugreport such 'Fix Startup Data Leak' doesn't work</code> on GitHub, as we know about it and it has already been mentioned many times ([#7](https://github.com/ukanth/afwall/issues/7), [#91](https://github.com/ukanth/afwall/issues/91), [#172](https://github.com/ukanth/afwall/issues/172), [#235] (https://github.com/ukanth/afwall/issues/235) - and in the original Droidwall issue tracker).
Once there is a final solution we will write it here, not as a workaround but as a solution.



Method 3: **APNDroid to Turn Off Data Connection**

[APNDroid](https://play.google.com/store/apps/developer?id=Apndroid+Inloop) prevents the phone from connecting to the Internet through cellular data line by modifying the APN (Access Point Name) names and APN types by adding `apndroid` suffix to them. There is also a script-based Locale plug-in for free. 



Method 4: **XPrivacy**

There are many different patches/apps like Openpdroid, Pdroid 2.0 or PDroid, and XPrivacy which prevent applications from leaking privacy sensitive data, but these patches need to be integrated into the ROM (except XPrivacy which needs the Xposed framework). Some of the patches/apps may not work with all apps without causing FC (Force Close) errors.
[XPrivacy](https://github.com/M66B/XPrivacy#installation) is the only solution that works on almost every device/OS without patching everything (needs only the Xposed framework to be installed).



Method 5: **Xposed based**

Similar to the XPrivacy Xposed app, the Xposed framework allows apps to manipulate at the OS level. This means Xposed will start directly right at the boot and before any app will be started; in this case no traffic will bypass the OS. An example app is LightningWall which use such a method.


IPtables specific problem
-----------------------------

* You should avoid complex scripts especially if you do not know what you're doing. Such complex scripts often result in security complications.
* IPtables can suffer from a possible _Race Conditions_ exception. On each IPtables request, the host must switch between the user application layer (the one apps running from) and the kernel layer, that usually coasts time and can end up with the mentioned problem.

An Race Condition example could be that the firewall rules aren't loaded completely/success, while the Kernel is already working on the network traffic (Output-Chain rules missing at this time) - This is an hole and answers the questions why there is still network traffic right after switch to the new rules. Closing this hole isn't possible, since iptables can't use the same ruleset for everything, only nftables not suffering from this. 


Default IPtables Chain Policy
-----------------------------

The Default Android OS IPTables chain policy is ACCEPT for all INPUT, FORWARD and OUTPUT policies. We can easily change this default policy to DROP with below listed commands.

>* iptables -P INPUT DROP
>* iptables -P FORWARD DROP
>* iptables -P OUTPUT DROP

After changing the INPUT, FORWARD, OUTPUT policies to DROP, all the incoming/outgoing/forwarding connections are dropped by AFWall+. So we need to open every INPUT, FORWARD and OUTPUT connections in firewall/IPTables with rules. If you change the default OUTPUT policy to DROP you cannot communicate with other systems/networks.

Useful links
------------

* [TaintDroid: Realtime Privacy Monitoring on Smartphones | Appanalysis](http://appanalysis.org/)
* [Disabling Mobile Data at First Boot | XDA-Forum](http://forum.xda-developers.com/showthread.php?p=7196260)
* [How to Minimize Your Android Data Usage and Avoid Overage Charges | How-To Geek](http://www.howtogeek.com/140261/how-to-minimize-your-android-data-usage-and-avoid-overage-charges/)
* [The Android boot process from power on | Xdin Android Blog](http://www.androidenea.com/2009/06/android-boot-process-from-power-on.html)
* [iptables4n1 workaround for Android Froyo (Nexus One) | Google Code](http://code.google.com/p/iptables4n1/)
* [Possible userinit.sh & Orbot leak fix](https://github.com/ukanth/afwall/wiki/CustomScripts#some-examples)

_ Final version 09.25.2015_