## Welcome to the AFWall+ Wiki!

AFWall+ is an open source firewall user interface client focused on making it easy to work with [iptables](https://en.wikipedia.org/wiki/Iptables) on Android OS.

Index
-----

* [Contact](#contact)
* [Requirements](#requirements)
* [Important Information](#important-information)
* [Similar solutions](#similar-solutions)
* [License](#license)

Contact
-------

| Developer| Misc (Wiki/Patches/Icons) | Translator | Web/eMail |
| :--- | :--: | :---: | :--- |
| Umakanthan Chandan | | | cumakt[at]gmail.com |
| Kevin Cernekee |  | | cernekee[at]gmail.com |
| | CHEF-KOCH |  | Nvinside [at] gmail.com |
| | Hush66 | | hush66.devianart.com |
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

- An Android OS device with at least the 2.3 (Gingerbread/API level 8, NDK 9) or a higher version. 
- Your device **must be rooted**.
- [[BusyBox]] and [[IPtables]] (also included as binary in AFWall+ for Kernel/ROM's without it!)
- The Kernel/ROM must have [[init.d support|init.d]] (for external custom script and data leak fix)!
- The Kernel must have NETFILTER/CONFIG_NETFILTER enabled (<code>adb pull /proc/config.gz</code> - Once you unzip it, you can search for e.g. _NETFILTER_). AFWall+ also can check it automatically if your Kernel does have support for it. 

Important Information
---------------------

* [Changelog](https://github.com/ukanth/afwall/blob/master/Changelog.md) describes changes in each version of AFWall+.
* [[CustomScripts]] contains the beginnings of a user manual. Some questions about working with iptables might be answered here, it also contains some predefined scripts you can use (Copy into a .txt file, rename .txt extension to .sh).
* [[Apps leak user privacy data during boot]] contains background information about the 'Data leak' during boot.
* Before you ask anything please take a look into our [[FAQ]].

Got an error message?
* [[Error codes]] may help you to understand the error messages. 

Found a bug?
* [[Learn how to report it|HOWTO Report Bug]]

Want to compile AFWall+ yourself?
* [[All you need to know about compiling AFWall+|HOWTO Compile AFWall]]
* [[All you need to know about compiling iptables|HOWTO Compiling iptable]]

Some useful things:
* [[Phone codes secrets]] contain some useful codes for viewing e. g. MAC address.

TCP security under Linux:
* [[TCP security]] may help to protect you against some known attacks. 

Similar solutions
-----------------

* _Builtin_ linux iptables (no GUI)
* [Android Firewall](https://github.com/skullone/android_firewall)
* [Android Tuner](https://play.google.com/store/apps/details?id=ccc71.at)
* [DroidWall](https://play.google.com/store/apps/details?id=com.googlecode.droidwall.free) _deprecated_ 
* [Avast Mobile Security & Antivirus](https://play.google.com/store/apps/details?id=com.avast.android.mobilesecurity)
* [LBE Privacy Guard](https://play.google.com/store/apps/details?id=com.lbe.security.lite)
* [LostNet Firewall](https://play.google.com/store/apps/details?id=com.lostnet.fw.pro)

[Xposed](http://repo.xposed.info/module/de.robv.android.xposed.installer) based:
* [LightningWall](http://repo.xposed.info/module/de.defim.apk.lightningwall)

License
-------

AFWall+ is open source software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.