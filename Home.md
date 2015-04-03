## Welcome to the AFWall+ Wiki!

AFWall+ is an open source firewall user interface client focused on making it easy to work with [iptables](https://en.wikipedia.org/wiki/Iptables) on Android OS.

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
| | Hush66 | | [Devianart](http://www.hush66.devianart.com) |
| | | tianchaoren | Crowdin |
| | | wuwufei | Crowdin |
| | | yinfugang | Crowdin |
| | | shiuan | Crowdin |
| | | Syk3s | Crowdin |
| | | ondra | Crowdin |
| | | DutchWaG | Crowdin |
| | | GermainZ | XDA |
| | | Looki75 | XDA |
| | | La_Globule | Crowdin |
| | | CHEF-KOCH | XDA/Crowdin |
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

Requirements
-------------

AFWall+ need this to run at his best:

- An Android OS device with at least the 4.0 (ICS/API level 14, NDK 10) or a higher version.
- Your device **must be rooted**.
- [[BusyBox]] and [[IPtables]] (also included as binary in AFWall+ for Kernel/ROM's without it!)
- The Kernel/ROM must have [[init.d support|init.d]] (for external custom script and data leak fix)!
- The Kernel must have NETFILTER/CONFIG_NETFILTER enabled (<code>adb pull /proc/config.gz</code> - Once you unzip it, you can search for e.g. _NETFILTER_). AFWall+ normally check it automatically if your Kernel does have support for it or not.

Installation
-------------

A general discussion platform can be found [here on XDA](http://forum.xda-developers.com/showthread.php?t=1957231). Use the GitHub platform for issue reports.

* Temporarily enable "Unknown sources" in your settings.
* Download the latest release from [Google Play Store](https://play.google.com/store/apps/details?id=dev.ukanth.ufirewall), [GitHub](https://github.com/ukanth/afwall/releases) or [F-Droid](https://f-droid.org/repository/browse/?fdid=dev.ukanth.ufirewall).
* Delete any previous version of the AFWall+ app, an upgrade from an existent installation is also possible.
* Delete any remnant application directory from <code>/sdcard0/afwall/</code> or similar.
* Install the newest version.

Enjoy and profit. Contribute to the project with pull requests!

Important Information
---------------------

:exclamation: Before you ask anything please take a look first into our [[FAQ]].

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
* [[Phone codes secrets]] contain some useful codes for viewing e. g. MAC address.
* [[WhatsApp blocking|HOWTO blocking WhatsApp]]
* [[Orwall and AFWall+|HOWTO OrWall together with AFWall+]]

TCP security under Linux:
* [[TCP security]] may help to protect you against some known attacks. 

Similar solutions
-----------------

IPTables based:
* _Builtin_ linux iptables/toolbox (there is no GUI)
* [Android Firewall](https://github.com/skullone/android_firewall)
* [3C Toolbox aka Android Tuner](https://play.google.com/store/apps/details?id=ccc71.at)
* ~~[DroidWall](https://play.google.com/store/apps/details?id=com.googlecode.droidwall.free)~~ _deprecated_ 
* [Avast Mobile Security & Antivirus](https://play.google.com/store/apps/details?id=com.avast.android.mobilesecurity)
* [LBE Privacy Guard/Master](https://play.google.com/store/apps/details?id=com.lbe.security.lite)

Anti IMSI-Catcher (protection against IMSI-Catchers, StingRay and Silent/Stealth SMS):
* [Android IMSI-Catcher Detector (AIMSICD)](https://secupwn.github.io/Android-IMSI-Catcher-Detector/)

Bluetooth Firewall:
* [Bluetooth Firewall](https://play.google.com/store/apps/details?id=com.fruitmobile.android.bluetooth.firewall)

Browser based:
* [Firefox NSA NoScript Anywhere](https://noscript.net/nsa/#download)

VPN/Proxy based:
* [LostNet Firewall](https://play.google.com/store/apps/details?id=com.lostnet.fw.pro)
* [Grey Shirts NoRoot Firewall](https://play.google.com/store/apps/details?id=app.greyshirts.firewall)
* [Netspark Firewall](https://play.google.com/store/apps/details?id=com.netspark.firewall&hl=en)

[Xposed](http://repo.xposed.info/module/de.robv.android.xposed.installer) based:
* [LightningWall](http://repo.xposed.info/module/de.defim.apk.lightningwall)

A word about non-root Firewalls
---------------------

All of these firewalls working with a Proxy/[VPN](https://developer.android.com/reference/android/net/VpnService.html) service, which means that they not working with iptables like AFWall+. They only work on a app-layer size and _fake_ a VPN connection, means the rules are applied on the VPN servers and not on the Android OS. The VPN package will be created to monitor incoming and outgoing traffic (which not need root access). The biggest con is that such VPN services not works with WiFi tethering or WiFi hotspots. And another con is that you can't run other VPN service and VPN/Proxy apps at the same time. 

Making Donations
-------

Donations are optional and we don't want you to feel pressured to send money! But everything may helps to improve the product - and of course it's always a good motivation.
It can be done directly on F-Droid, Google Play Store or via PayPal (Donation email: <code>devipriyapu [at] gmail.com</code>.

<script src="paypal-button.min.js?merchant=YOUR_MERCHANT_ID"
    data-button="donate"
    data-name="AFWall+"
    data-amount="1.00"
    async
></script>

<script src="paypal-button.min.js?merchant=YOUR_MERCHANT_ID"
    data-button="qr"
    data-name="AFWall+ via QR code"
    data-amount="1.00"
    data-size="250"
    async
></script>

License
-------

AFWall+ is open source software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.
