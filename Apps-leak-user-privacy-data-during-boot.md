# Many Android apps leak user privacy data
First this bug is a design hole by Android OS itself and have nothing to do with AFWall+ or any other Firewall itself. 

### What's the issue?
Certain Android applications [#168](http://code.google.com/p/droidwall/issues/detail?id=168), [#235](http://code.google.com/p/droidwall/issues/detail?id=235), [#262](http://code.google.com/p/droidwall/issues/detail?id=262), including ones officially bundled with the OS such as Calendar, Contacts and Picasa, send certain data, including authentication tokens (a form of password used to identify a user), in a clear rather than encrypted format. This claim was made by German security researchers Bastian Könings, Jens Nickels, and Florian Schaub from the University of Ulm.

### What's the risk?
Think of it like accessing your online banking service via a website that isn't secure. Theoretically, anyone could intercept that unencrypted data as it travels between your computer and the bank's server, stealing your password details or initiating false transactions. 


### Does I have this problem?
You an check this with any [Onavo Count | Monitor Data](https://play.google.com/store/apps/developer?id=Onavo). Traffic at start shows you that you already have this data leak.

>* /sys/class/net/[interface]/statistics/tx_bytes
>* /sys/class/net/[interface]/statistics/rx_bytes 

These are the files that store traffic stats data. If you want to look manually through file manager.
For received UID traffic stats take a look at here:

>* /proc/uid_stat/[uid]/tcp_rcv
>* /proc/uid_stat/[uid]/tcp_snd

On older versions procfs is mounted at boot time, which means that every time your device is rebooted there are 0 traffic values for all UIDs (except lan0).
You can list _/proc/uid_stat/_ dir right now to see which UIDs have been spending traffic since last reboot.

### Temporally workarounds
- Open dialer
- Dial *#*#4636#*#* or *#*#6436#*#*
- Tap _'Phone Information'_
- Press Menu button
- Tap on _'More'_
- Tap _'Disable data connection'_, or _'disable data on boot'_. 
- Once the boot are complete you an re-enable the data connection.

--------------------
For the second workaround you need init.d support. Ask you ROM/Kernel Developer or see on you readme if you have init.d support or not. Do **NOT** ask on this Github-Repro!
- Open AFWall+ Settings
- Scroll down until you see _'Experimental Preferences'_
- Enable _'Fix Startup Data Leak'_
- On some ROM's you also have to enable _'Fix Device Start Rules'_ e.g. [CyanogenMod 10.1](http://www.cyanogenmod.org/).

### Default IPtables Chain Policy
The Default Android iptables chain policy is ACCEPT for all INPUT, FORWARD and OUTPUT policies. We can easily change this default policy to DROP with below listed commands.

>* iptables -P INPUT DROP
>* iptables -P FORWARD DROP
>* iptables -P OUTPUT DROP

After changing the INPUT, FORWARD, OUTPUT policies to DROP, all the incoming/outgoing/forwarding connections are dropped by AFWall+. So we need to open every INPUT, FORWARD and OUTPUT connections in firewall/iptables with rules. If you change the default OUTPUT policy to DROP you cannot communicate with other systems/networks.


## Some other useful links
* [TaintDroid: Realtime Privacy Monitoring on Smartphones | Appanalysis](http://appanalysis.org/)
* [Disabling Mobile Data at First Boot | XDA-Forum](http://forum.xda-developers.com/showthread.php?p=7196260)
* [How to Minimize Your Android Data Usage and Avoid Overage Charges | How-To Geek](http://www.howtogeek.com/140261/how-to-minimize-your-android-data-usage-and-avoid-overage-charges/)
* [The Android boot process from power on | Xdin Android Blog](http://www.androidenea.com/2009/06/android-boot-process-from-power-on.html)
* [Android Community | Android Source] (http://source.android.com/source/community/index.html)


---------------
### **Warning**: This article is not finished ... do not edit it until it's done. Thanks!