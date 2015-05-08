## Welcome to the AFWall+ Wiki!

AFWall+ is an open source firewall user interface client focused on making it easier to work with [iptables](https://en.wikipedia.org/wiki/Iptables) on Android OS. Keep track of your mobile broadband usage and block internet access to selected apps to avoid exceeding your data usage.

:warning: We are not responsible for any external content mentioned here in this Wiki! 
[Due the mass of Chrome/Chromium security complications](https://trac.torproject.org/projects/tor/wiki/doc/ImportantGoogleChromeBugs), this Wiki is focused on browsing with Firefox/STOCK (+ mobile), please do not ask anything how to secure or harden Chrome/Chromium or any Mod of this Browser which use the same engine! :warning:

Index
-----

* [Contact and contributors](#contact-and-contributors)
* [Requirements](#requirements)
* [Installation](#installation)
* [Important Information](#important-information)
* [Similar solutions](#similar-solutions)
* [Blocking advertisements](#blocking-advertisements)
* [A word about non-root Firewalls](#a-word-about-non-root-firewalls)
* [Making Donations](#making-donations)
* [Clone the Wiki](#clone-the-wiki)
* [License](#license)

Contact and contributors
-------

Thanks to all who have contributed to the AFWall+ project!

 
Additional thanks and contact options goes out to our top contributors & developers, listed over [[here|Contributors]]. 

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

**(Optional and not 100% AFWall+ specific)**

TCP security under Linux:
* [[TCP security]] may help to protect you against some known attacks.

Kernel Security under Linux:
* [[Kernel security]] hardening the Kernel to maximum security (POC + WIP).

Phone "cheats":
* [[Phone codes secrets]] contain some useful codes for viewing e.g. MAC address.

Similar solutions
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
* ...

_[Xposed](http://repo.xposed.info/module/de.robv.android.xposed.installer) based:_
* [LightningWall](http://repo.xposed.info/module/de.defim.apk.lightningwall)
* [PeerBlock](https://play.google.com/store/apps/details?id=com.peerblock) & [Source](https://apeerblock.codeplex.com/) + [trusted blocklist's](http://list.iblocklist.com)

_Real-time iptables logging:_
* [Network Log](https://play.google.com/store/apps/details?id=com.googlecode.networklog&hl=en)

_Internet Diagnostic Tool_:
* [Network Monitor](http://fossdroid.com/a/network-monitor.html)


Blocking advertisements
---------------------

**Please do not send any email/ask here on GitHub how to block the Android advertisements!** It's mentioned in the official FAQ that AFWall+ or iptables itself _isn't designed to block ads_, yes iptables can block ads - but there is no fine control/GUI/filter for it to really monitor and take control for each new single request. Google decided to remove all Adblockers from the Google Play Store for [several reasons](https://adblockplus.org/blog/adblock-plus-for-android-removed-from-google-play-store) - which means official there is no support!

There are several ad-blocking techniques:
- via iptables (which truly works on everything VPN/Proxy,...)
- via [HOSTS](http://en.wikipedia.org/wiki/Hosts_%28file%29) (most solutions) - also [can be edited manually without any apps](http://www.techrepublic.com/article/edit-your-rooted-android-hosts-file-to-block-ad-servers/)
- a mix: Hosts + iptables (which often creates a local proxy/vpn tunnel)
- Xposed based (nothing special, HOSTS file too)
- Browser based (works only in the browser and not blocks app/in-app ads - they mostly combine a cached hosts + a filter engine e.g. pattern-based to fine control ads per site basis)


Iptables:
* This is only for expert users.


Browser based Firefox (mobile) addons:
* [uBlock](https://github.com/gorhill/uBlock/releases) also called [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/) which use a browser HOSTS + a pattern-based filtering engine (similar to NoScript / RequestsPolicy/Policeman ones)
* [uMatrix](https://github.com/gorhill/uMatrix/releases) - [the difference](https://github.com/gorhill/uMatrix/issues/32) here is that it really can control everything and should be used by advanced users which really want to control every request.
* AdBlock Edge (a fork of AdBlock Plus) are [discontinued](http://www.ghacks.net/2015/04/13/popular-adblock-plus-fork-adblock-edge-to-be-discontinued/)


Mixed ones (which are designed to work on Android + Browser):
* [AdGuard](http://adguard.com/en/adguard-android/overview.html) - free version available but the important ones need a _Premium license_


Hosts only:
* [AdBlock Plus](https://adblockplus.org/de/android-install)
* [AdAway](https://f-droid.org/repository/browse/?fdid=org.adaway) - should be preferred by root users, since AdBlock Plus have some limitations on Android (proxy/vpn ones)


XPosed based:
* [UnbelovedHosts](http://unbelovedhosts.apk.defim.de/) more or less the same as AdBlock Plus and others, but is an Xposed module which updates/manage the internal HOSTS file


_To answer the question which is the best one?_ It's depending on user needs, if you want to support app developers but hate web ads, feel free to start with uBlock. If you want to block everything, just use an HOSTS based solution. If you really want to block everything AND fine control per-site ads just use AdAway and NoScript + uBlock/uMatrix. 


A word about non-root Firewalls
---------------------

All of these firewalls working with a local Proxy/[VPN](https://developer.android.com/reference/android/net/VpnService.html) service, which means that they not working with iptables like AFWall+. They only work on a app-layer size which _fake_ a VPN connection, means the rules are applied on the VPN servers and not on the Android OS. The VPN package will be created to monitor incoming and outgoing traffic (which not need root access). The biggest problem is that such VPN services not work with WiFi tethering or WiFi hotspots. And another con is that you can't run other VPN services and VPN/Proxy's apps at the same time together. 

We not suggest to use any no-root firewall for above reasons and others, e.g. see [an German article](http://www.kuketz-blog.de/android-firewall-ohne-root-%E2%80%A2-noroot-firewall/):
* NetSpark/Mobiwol [NoRoot](https://play.google.com/store/apps/details?id=com.netspark.firewall&hl=en)

Making Donations
-------

Donations are optional and we don't want you to feel pressured to send money! But everything may helps to improve the product - and of course it's always a good motivation!
It can be done directly on [Google Play Store](https://play.google.com/store/apps/details?id=dev.ukanth.ufirewall.donate) or just look at [F-Droid repository](https://f-droid.org/repository/browse/?fdid=dev.ukanth.ufirewall) for more donations options.

Clone the Wiki
-------

Feel free to fork/clone our Wiki, but remember to give proper credits where it belongs. 


Via: <code>git clone https://github.com/ukanth/afwall.wiki.git</code>.


To stay on sync, here is an [example](https://gist.github.com/larrybotha/10650410) how to do this. An API reference can be found over [here](https://github.com/mbostock/d3/wiki/API-Reference) or on the [official Docs](https://help.github.com/).

License
-------

AFWall+ is open source software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.