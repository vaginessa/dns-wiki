## Welcome to the AFWall+ Wiki!

AFWall+ is an Android open source firewall client, focused on making it easier to work with [iptables](https://en.wikipedia.org/wiki/Iptables). It keeps track of your mobile broadband usage and can block internet access to selected apps to avoid exceeding your data usage.

:exclamation: Before you ask anything please take a look first into our official [[FAQ]]. A general discussion platform can be found [here on XDA-Developers](http://forum.xda-developers.com/showthread.php?t=1957231). Use our GitHub platform only for [issue reports](https://github.com/ukanth/afwall/issues), [see here how-to do this](https://github.com/ukanth/afwall/wiki/HOWTO-Report-Bug) - or [pull requests](https://github.com/ukanth/afwall/pulls). :exclamation: 

:warning: We are not responsible for any external content mentioned here on this Wiki! 
[Due the mass of Chrome/Chromium security complications](https://trac.torproject.org/projects/tor/wiki/doc/ImportantGoogleChromeBugs) read also [this](https://blog.torproject.org/blog/isec-partners-conducts-tor-browser-hardening-study) and [this](https://blog.torproject.org/blog/google-chrome-incognito-mode-tor-and-fingerprinting), our Wiki is focused on browsing with Firefox (mobile), please do not ask anything how to secure/harden Chrome/Chromium or any mod of this Browser which use the same engine. Thanks! :warning:

Index
-----

* [Contribution](#contribution)
* [Requirements](#requirements)
* [Not working on](#not-working-on)
* [Installation](#installation)
* [Important Information](#important-information)
* [Similar firewall solutions](#similar-firewall-solutions)
* [Making Donations](#making-donations)
* [Wiki cloning](#wiki-cloning)
* [Feeds](#feeds)
* [License](#license)

Contribution
-------

Your contributions and suggestions are heartily♥ welcome. (✿◕‿◕)


Additional thanks goes out to our top contributors & developers, listed over [[here|Contributors]].


Enjoy and profit. Contribute to the project with pull requests!

Requirements
-------------

AFWall+ needs the following to run at his best:
 
- The Android 4.x OS (ICS/API level 15, NDK 10) or a higher version.
- Your device **must be rooted**.
- [[BusyBox]] and [[IPtables]] (also included as binary in AFWall+ for Kernel/ROM's without it!)
- The Kernel/ROM _must have_ [[init.d support|init.d]] (for external custom script and data leakage fix)!
- The Kernel _must have_ NETFILTER/CONFIG_NETFILTER enabled (<code>adb pull /proc/config.gz</code> - Once you unzip it, you can search for e.g. _NETFILTER_). AFWall+ normally check's it automatically if your Kernel does have support for it or not.

Not working on
-------------

Android overall supports over 15k devices, which means it's impossible for AFWall+ to support each every single one of them. The community (_YOU_) can help in this case with a quality bug report to possible add the missing thing to your version/device to get it working, besides this we are not officially supporting _special_ Android version like:

* [Android Wear](https://developer.android.com/wear/index.html)
* [Android TV](https://developer.android.com/tv/index.html)
* [Android Auto](https://developer.android.com/auto/index.html)
* [Windows Mobile](https://github.com/ukanth/afwall/wiki/Windows-10-Mobile)

Installation
-------------

* If you've not installed AFWall+ from the official stores like F-Droid or Google Play Store you may need to temporarily enable "Unknown sources" in your settings if that's disabled.
* Make a current settings backup (optional) - it will be saved under <code>/sdcard0/afwall/</code>.
* Download the latest release from [Google Play Store](https://play.google.com/store/apps/details?id=dev.ukanth.ufirewall), [GitHub](https://github.com/ukanth/afwall/releases) or [F-Droid](https://f-droid.org/repository/browse/?fdid=dev.ukanth.ufirewall).
* Delete any previous version of the AFWall+ app, an upgrade from an existent installation is also possible.
* Delete/Restore any remnant application/setting(s) directory from <code>/sdcard0/afwall/</code> or similar.
* Install the latest available version.
* Restore your settings backup.

Important Information
---------------------

Before you submit any issue report via the integrated AFWall+ option - or here on GitHub, you maybe first read the following stuff which possible solve your problems questions.

* Our official [[FAQ]] helps to solve the most known problems and questions.
* Our [official issue tracker](https://github.com/ukanth/afwall/issues) here on GitHub, please always make a search before you open another ticket.
* [Changelog](https://github.com/ukanth/afwall/blob/stable/Changelog.md) describes changes in each new version of AFWall+.
* [Custom-Scripts](https://github.com/ukanth/afwall/wiki/CustomScripts) for advanced users only. Some questions about working with IPTables script(s) might be answered over there, it also contains some predefined scripts templates and examples you can use.
* [Apps leak user privacy data during boot](Apps-leak-private-user-data-during-boot) contains background information about how to solve the 'Data leakage' during boot problem.
* [[Android kernel traffic]] contains background information about the Android OS & Kernel generated traffic.
* [Which system apps can I block?](https://github.com/ukanth/afwall/wiki/System-Applications-to-block-or-allow) contains background information about which (system-)apps are blockable and which should better not been blocked.  

Got an error message?
* [[Error codes]] may help you to understand the error messages. 

Found a bug?
* [[Learn how to report it|HOWTO Report Bug]]

Want compile AFWall+ yourself?
* [[All you need to know about compiling AFWall+|HOWTO Compile AFWall]]
* [[All you need to know about compiling busybox|HOWTO Compiling busybox]]
* [[All you need to know about compiling iptables|HOWTO-Compiling-iptables]]
* [[All you need to know about debugging|HOWTO debugging]]

Some useful tricks:
* [[WhatsApp blocking|HOWTO blocking WhatsApp]]
* [[Orwall and AFWall+|HOWTO-OrWall-together-with-AFWall]]
* [[Advertisements blocking]]


**(Optional and not only AFWall+ specific)**

TCP security under Linux:
* [[TCP security]] may help to protect you against some known attacks.

Kernel Security under Linux:
* [[Kernel security]] hardening the Kernel to maximum security (POC + WIP).

Phone code "cheats":
* [[Phone codes secrets]] contain some useful codes for viewing e.g. MAC address.

Similar firewall solutions
-----------------

A quick overview of other Android firewalls and a short explanation how they're working is explained over our [[Similar Firewall solutions]] page. 


Making Donations
-------

Donations are optional and we don't want you to feel pressured to send money! But everything may help to improve the our product — and of course it's always a good motivation!

It can be done directly on [Donate Version](https://play.google.com/store/apps/details?id=dev.ukanth.ufirewall.donate), [Unlocker](https://play.google.com/store/apps/details?id=dev.ukanth.ufirewall.donatekey) or via below options.

> Paypal - [![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=6E4VZTULRB8GU) (official PayPal account) - https://www.paypal.me/afwallplus

> Bitcoin Address : bc1qehf4td7zg7t4lhmyxra0xf78aeuk44808ls52k



> Amazon Gift vouchers : cumakt+amazon at gmail.com

Please drop me an mail to (contact @ portgenix.com ) after your donation to get the unlocker APK (closed source for now) with details. I usually respond back within an day ( In worst case please allow 3 working days )

Wiki cloning
-------

Feel free to fork/clone our Wiki, but remember to give proper credits where it belongs. 

Via: <code>git clone https://github.com/ukanth/afwall.wiki.git</code>.

To stay on sync, here is an [example](https://gist.github.com/larrybotha/10650410) how to do this. An API reference can be found over [here](https://github.com/mbostock/d3/wiki/API-Reference) or on the [official Docs](https://help.github.com/).

Feeds
-------

It's now very easy to stay up-to-date by subscribing the latest changes via Browser live [feeds](https://atom.io/docs/latest/):

```bash
# The available feeds are:
https://github.com/ukanth/afwall/commits/beta.atom (latest)
https://github.com/ukanth/afwall/commits/HEAD.atom
https://github.com/ukanth/afwall/commits/donate.atom
https://github.com/ukanth/afwall/commits/stable.atom

or 
https://github.com/ukanth/afwall/commits/<TAG or BRANCHES>.atom

```

License
-------

Copyright (c) 2012-2019 [ukanth](http://forum.xda-developers.com/member.php?u=3249429).
All rights reserved.

[![Creative Commons License](https://licensebuttons.net/l/by/3.0/88x31.png)](https://www.gnu.org/licenses/gpl.txt).

AFWall+ is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your discretion) any later version.

AFWall+ is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with AFWall+. If not, see [https://www.gnu.org/licenses/](http://www.gnu.org/licenses/).