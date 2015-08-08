Index
-----------------

* [Similar Android Firewall solutions](#similar-android-firewall-solutions)
* [Non-root Firewalls](#non--root-firewalls)


Similar Android Firewall solutions
-----------------

Generally there are six categories of Android Firewalls:
* Firewalls which uses a _local VPN_ for traffic filtering (Dr. Web Anti-Virus,[...])
* Firewalls which use a _separate own VPN_ (Android doesn't allow to use two VPNs together the same time) like Opera's _Max/Turbo_ feature to compress/reduce the web traffic (if it's enabled the firewall may not work)
* Firewalls which using _iptables_, like AFWall+, Droidwall, Avast,[...]
* Firewalls which using a _local HTTP proxy_ (or integrate it in Android's VPN) like AdAway, AdGuard,...
* Browser or app related firewalls, like NoScript, Bluetooth Firewalls and such, which basically only blocking specific functions and not the traffic itself.
* Dynamic egress filtering: Monitors all outbound network traffic and issue dynamic prompts (on-demand) in order to determine egrees filter rules. The rules are defined per application. 

_IPTables based:_
* _Builtin_ [iptables](http://www.netfilter.org/projects/iptables/) (no GUI, but can be controlled via external scripts any Terminal Emulator app or ADB)
* [Android Firewall](https://play.google.com/store/apps/details?id=com.jtschohl.androidfirewall) & [Source](https://github.com/skullone/android_firewall) [_removed from Google Play Store!_]
* [FireWall Plus](https://play.google.com/store/apps/details?id=com.sethcottle.firewallplus) & [Source](https://github.com/Squario/Firewall-Plus) [_removed from Google Play Store!_]
* [Firewall Gold](https://play.google.com/store/apps/details?id=com.anstudios.androidfirewall)
* [3C Toolbox aka Android Tuner](https://play.google.com/store/apps/details?id=ccc71.at)
* ~~[DroidWall](https://play.google.com/store/apps/details?id=com.googlecode.droidwall.free)~~ _deprecated_ 
* [Avast Mobile Security & Antivirus](https://play.google.com/store/apps/details?id=com.avast.android.mobilesecurity) & [Forum](https://forum.avast.com/index.php?board=37.0)
* [LBE Privacy Master](https://play.google.com/store/apps/details?id=com.lbe.security.lite)
* [Advanced Firewall](https://play.google.com/store/apps/details?id=advancedfirewall.educational.ae)

_Anti IMSI-Catcher (protects against IMSI/StingRay-Catchers and Silent/Stealth SMS):_
* [Android IMSI-Catcher Detector (AIMSICD)](https://secupwn.github.io/Android-IMSI-Catcher-Detector/)
* [SnoopSnitch](https://play.google.com/store/apps/details?id=de.srlabs.snoopsnitch) + [F-Droid](https://f-droid.org/repository/browse/?fdid=de.srlabs.snoopsnitch) + [GSMMap Global Database](http://gsmmap.org/)

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
* [RouterCheck](https://play.google.com/store/apps/details?id=com.Sericon.RouterCheck.client.android&hl=en)

_Dynamic egress filtering_ (links not working - it's only for historical reasons):
* ~~[WhisperMonitor](http://www.whispersys.com/whispermonitor.html)~~ _deprecated_
* ~~[SnoopWall](https://play.google.com/store/apps/details?id=com.snoopwall.android)~~ _deprecated_


Non-root Android Firewalls
---------------------

All of these firewalls working with a local Proxy/[VPN](https://developer.android.com/reference/android/net/VpnService.html) service, which means that they not working with IPtables like AFWall+. They only work on a app-layer size which _fake_ a VPN connection, means the rules are applied on the VPN servers and not on the Android OS. The VPN package will be created to monitor incoming and outgoing traffic (which not need root access). The biggest problem is that such VPN services not work with WiFi tethering or hotspots. And another con is that you can't run other VPN services and VPN/Proxy's apps at the same time together. 

We not recommend to use any no-root firewall for above reasons, there are others too e.g. see [an German article](http://www.kuketz-blog.de/android-firewall-ohne-root-%E2%80%A2-noroot-firewall/):
* NetSpark/Mobiwol [NoRoot](https://play.google.com/store/apps/details?id=com.netspark.firewall&hl=en)

_Final version 08.08.2015_
