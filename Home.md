## Welcome to the AFWall+ Wiki!

AFWall+ is an open source firewall user interface client focused on making it easier to work with [iptables](https://en.wikipedia.org/wiki/Iptables) on Android OS.

:warning: We are not responsible for any external content mentioned here in this Wiki! :warning:

Index
-----

* [Contact](#contact)
* [Requirements](#requirements)
* [Installation](#installation)
* [Important Information](#important-information)
* [Similar solutions](#similar-solutions)
* [A word about non-root Firewalls](#a-word-about-non-root-firewalls)
* [Making Donations](#making-donations)
* [License](#license)

Contact
-------

| Developer| Misc (Wiki/Patches/Icons) | Translator | Web/eMail |
| :--- | :--: | :---: | :---: |
| **Umakanthan Chandan** | | | cumakt [at] gmail.com |
| Kevin Cernekee |  | | cernekee [at] gmail.com |
| | CHEF-KOCH |  | Nvinside [at] gmail.com |
| | PdtS for Icons | | [Website](http://pdts.net/) |
| | Hush66 | | [Devianart](http://www.hush66.devianart.com/) |
| | | tianchaoren | Crowdin |
| | | wuwufei | Crowdin |
| | | yinfugang | Crowdin |
| | | shiuan | Crowdin |
| | | Syk3s | Crowdin |
| | | ondra | Crowdin |
| | | DutchWaG | Crowdin |
| | | GermainZ (German) | XDA |
| | | Looki75 | XDA |
| | | La_Globule | Crowdin |
| | | CHEF-KOCH (German) | XDA/Crowdin |
| | | user_99 | XDA |
| | | Gronkdalonka | XDA |
| | | mpqo | Crowdin |
| | | l3s | Crowdin |
| | | mirulumam | Crowdin |
| | | giammatheo | Crowdin |
| | | benzo | Crowdin |
| | | nnnn | Crowdin |
| | | tst | Crowdin |
| | | eskimos72 | Crowdin |
| | | Piotr Kowalski| Crowdin |
| | | lemor2008| Crowdin |
| | | mysterys3by-facebook | Crowdin |
| | | andycris | Crowdin |
| | | Kirhe | XDA |
| | | YaroslavKa78 | Crowdin |
| | | bungabunga | Crowdin |
| | | spezzino | Crowdin |
| | | CreepyLinguist | Crowdin |
| | | BahadirDemirel | Crowdin |
| | | myasa | Crowdin |
| | | ozgurahmet95 | Crowdin |
| | | andriykopanytsia | Crowdin |
| | | Osoitz (Basque) | Crowdin |
| | | alruwaile1234 (Persian/Arabic) | Crowdin |
| | | softiceup (Vietnamese) | Crowdin |
| | | slsimic (Serbian) | Crowdin |
| | | testxyz009 (Korean) | Crowdin |

Requirements
-------------

AFWall+ need this to run at his best:

- An Android OS device with at least the 4.0 (ICS/API level 14, NDK 10) or a higher version.
- Your device **must be rooted**.
- [[BusyBox]] and [[IPtables]] (also included as binary in AFWall+ for Kernel/ROM's without it!)
- The Kernel/ROM must have [[init.d support|init.d]] (for external custom script and data leakage fix)!
- The Kernel must have NETFILTER/CONFIG_NETFILTER enabled (<code>adb pull /proc/config.gz</code> - Once you unzip it, you can search for e.g. _NETFILTER_). AFWall+ normally check it automatically if your Kernel does have support for it or not.

Installation
-------------

A general discussion platform can be found [here on XDA](http://forum.xda-developers.com/showthread.php?t=1957231). Use the GitHub platform only for issue reports, [see here how-to do this](https://github.com/ukanth/afwall/wiki/HOWTO-Report-Bug). 

* Temporarily enable "Unknown sources" in your settings if it's disabled.
* Make a backup (if needed) - it will be saved under <code>/sdcard0/afwall/</code>
* Download the latest release from [Google Play Store](https://play.google.com/store/apps/details?id=dev.ukanth.ufirewall), [GitHub](https://github.com/ukanth/afwall/releases) or [F-Droid](https://f-droid.org/repository/browse/?fdid=dev.ukanth.ufirewall).
* Delete any previous version of the AFWall+ app, an upgrade from an existent installation is also possible.
* Delete/Restore any remnant application/setting(s) directory from <code>/sdcard0/afwall/</code> or similar.
* Install the newest version.
* Restore the backup.

Enjoy and profit. Contribute to the project with pull requests!

Important Information
---------------------

:exclamation: Before you ask anything please take a look first into our [[FAQ]]. :exclamation:

* [Changelog](https://github.com/ukanth/afwall/blob/master/Changelog.md) describes changes in each version of AFWall+.
* [[CustomScripts]] contains the beginnings of a user manual. Some questions about working with iptables might be answered here, it also contains some predefined scripts you can use (Copy into a .txt file, rename .txt extension to .sh).
* [[Apps leak user privacy data during boot]] contains background information about the 'Data leak' during boot.
* [[Android kernel traffic]] contains background information about the Android OS & Kernel generated traffic.

Got an error message?
* [[Error codes]] may help you to understand the error messages. 

Found a bug?
* [[Learn how to report it|HOWTO Report Bug]]

Want compile AFWall+ yourself?
* [[All you need to know about compiling AFWall+|HOWTO Compile AFWall]]
* [[All you need to know about compiling busybox|HOWTO Compiling busybox]]
* [[All you need to know about compiling iptables|HOWTO Compiling iptable]]

Some useful things:
* [[WhatsApp blocking|HOWTO blocking WhatsApp]]
* [[Orwall and AFWall+|HOWTO OrWall together with AFWall+]]

**(optional and not AFWall+ specific)**

TCP security under Linux:
* [[TCP security]] may help to protect you against some known attacks.

Kernel Security under Linux:
* [[Kernel security]] hardening the Kernel to maximum security (POC + WIP).

Phone "cheats":
* [[Phone codes secrets]] contain some useful codes for viewing e.g. MAC address.

Similar solutions
-----------------

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
* [Netspark Firewall](https://play.google.com/store/apps/details?id=com.netspark.firewall&hl=en)

_[Xposed](http://repo.xposed.info/module/de.robv.android.xposed.installer) based:_
* [LightningWall](http://repo.xposed.info/module/de.defim.apk.lightningwall)

_IP blocking (needs Xposed):_
* [PeerBlock](https://play.google.com/store/apps/details?id=com.peerblock) & [Source](https://apeerblock.codeplex.com/)

_Real-time iptables logging:_
* [Network Log](https://play.google.com/store/apps/details?id=com.googlecode.networklog&hl=en)

A word about non-root Firewalls
---------------------

All of these firewalls working with a Proxy/[VPN](https://developer.android.com/reference/android/net/VpnService.html) service, which means that they not working with iptables like AFWall+. They only work on a app-layer size which _fake_ a VPN connection, means the rules are applied on the VPN servers and not on the Android OS. The VPN package will be created to monitor incoming and outgoing traffic (which not need root access). The biggest problem is that such VPN services not work with WiFi tethering or WiFi hotspots. And another con is that you can't run other VPN services and VPN/Proxy's apps at the same time together. 

We not suggest to use any no-root firewall for above reasons and others, e.g. see [an German article](http://www.kuketz-blog.de/android-firewall-ohne-root-%E2%80%A2-noroot-firewall/):
* [NoRoot](https://play.google.com/store/apps/details?id=com.netspark.firewall&hl=en)

Making Donations
-------

Donations are optional and we don't want you to feel pressured to send money! But everything may helps to improve the product - and of course it's always a good motivation.
It can be done directly on [Google Play Store](https://play.google.com/store/apps/details?id=dev.ukanth.ufirewall.donate) or just look at [F-Droid repository](https://f-droid.org/repository/browse/?fdid=dev.ukanth.ufirewall) for more donations options.

License
-------

AFWall+ is open source software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.