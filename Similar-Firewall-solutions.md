Index
-----------------

* [Similar Android Firewall solutions](#similar-android-firewall-solutions)
* [Non-root Firewalls](#non-root-firewalls)


Similar Android Firewall solutions
-----------------

Generally there are five categories of Android Firewalls:
* Firewalls which uses a _local VPN_ for traffic filtering (Dr. Web Anti-Virus,[...])
* Firewalls which use a _separate own VPN_ (Android doesn't allow to use two VPNs together the same time) like Opera Max for compressing the traffic (if it's enabled the firewall may not work)
* Firewalls which using _iptables_, like AFWall+, Droidwall, Avast, Comodo,[...]
* Firewalls which using a _local HTTP proxy_ (or integrate it in Android's VPN) like AdAway, AdGuard,...
* Browser or app related firewalls, like NoScript, Bluetooth Firewalls and such, which basically only blocking specific functions and not the traffic itself.

_IPTables based:_
* _Builtin_ Linux iptables (no GUI, but it can be controlled via scripts/Terminal/ADB)
* [Android Firewall](https://play.google.com/store/apps/details?id=com.jtschohl.androidfirewall) & [Source](https://github.com/skullone/android_firewall) [removed from Google Play Store!]
* [3C Toolbox aka Android Tuner](https://play.google.com/store/apps/details?id=ccc71.at)
* ~~[DroidWall](https://play.google.com/store/apps/details?id=com.googlecode.droidwall.free)~~ _deprecated_ 
* [Avast Mobile Security & Antivirus](https://play.google.com/store/apps/details?id=com.avast.android.mobilesecurity)
* [LBE Privacy Master](https://play.google.com/store/apps/details?id=com.lbe.security.lite)

_Anti IMSI-Catcher (protects against IMSI/StingRay-Catchers and Silent/Stealth SMS):_
* [Android IMSI-Catcher Detector (AIMSICD)](https://secupwn.github.io/Android-IMSI-Catcher-Detector/)

_Bluetooth Firewall:_
* [Bluetooth Firewall](https://play.google.com/store/apps/details?id=com.fruitmobile.android.bluetooth.firewall)

_Browser based Firewall (takes control over JavaScript,...):_
* [Firefox NSA NoScript Anywhere](https://noscript.net/nsa/#download)

_VPN/Proxy based:_
* [LostNet Firewall](https://play.google.com/store/apps/details?id=com.lostnet.fw.pro)
* [Grey Shirts NoRoot Firewall](https://play.google.com/store/apps/details?id=app.greyshirts.firewall)
* Dr. Web AntiVirus + Firewall
* Kaspersky Internet Security
* Comodo Internet Security
* ...

_[Xposed](http://repo.xposed.info/module/de.robv.android.xposed.installer) based:_
* [LightningWall](http://repo.xposed.info/module/de.defim.apk.lightningwall)
* [PeerBlock](https://play.google.com/store/apps/details?id=com.peerblock) & [Source](https://apeerblock.codeplex.com/) + [trusted blocklist's](http://list.iblocklist.com)

_Real-time iptables logging:_
* [Network Log](https://play.google.com/store/apps/details?id=com.googlecode.networklog&hl=en)

_Internet Diagnostic Tool_:
* [Network Monitor](http://fossdroid.com/a/network-monitor.html)


Non-root Android Firewalls
---------------------

All of these firewalls working with a local Proxy/[VPN](https://developer.android.com/reference/android/net/VpnService.html) service, which means that they not working with iptables like AFWall+. They only work on a app-layer size which _fake_ a VPN connection, means the rules are applied on the VPN servers and not on the Android OS. The VPN package will be created to monitor incoming and outgoing traffic (which not need root access). The biggest problem is that such VPN services not work with WiFi tethering or WiFi hotspots. And another con is that you can't run other VPN services and VPN/Proxy's apps at the same time together. 

We not recommend to use any no-root firewall for above reasons, there are others too e.g. see [an German article](http://www.kuketz-blog.de/android-firewall-ohne-root-%E2%80%A2-noroot-firewall/):
* NetSpark/Mobiwol [NoRoot](https://play.google.com/store/apps/details?id=com.netspark.firewall&hl=en)