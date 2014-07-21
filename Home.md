## Welcome to the AFWall+ Wiki!

AFWall+ is an open source firewall client focused on making it easy to work with iptables on your Android OS.

Index
-----

* [Requirements](#requirements)
* [Important Information](#Important Information)
* [License](#license)

Requirements
-------------

- An Android OS device with at least the 2.3 (Gingerbread/API level 8, NDK 4) or a higher version. 
- The device **must be rooted**.
- The Kernel/ROM must have [[init.d support|init.d]] (for external custom script and data leak fix)!
- The Kernel must have NETFILTER/CONFIG_NETFILTER enabled (adb pull /proc/config.gz - Once you unzip it, you can search for e.g. "NETFILTER").

Important Information
---------------------

* [Changelog](https://github.com/ukanth/afwall/blob/master/Changelog.md) describes changes in each version of AFWall+.
* [[CustomScripts]] contains the beginnings of a user manual. Some questions about working with iptables might be answered here, it also contains some predefined scripts you can use (Copy into a .txt file, rename .txt extension to .sh).
* [[Apps leak user privacy data during boot]] contains background information about the 'Data leak' during boot.
* Before you ask everything please take a look into our [[FAQ]].

Got an error message?
* [[Error codes]] may help you.

Found a bug?
* [[Learn how to report it|HOWTO Report Bug]]

Want to compile AFWall+ yourself?
* [[All you need to know about compiling AFWall+|HOWTO Compile AFWall]]

Debugging?
* [[Phone codes secrets]] contain some useful codes for viewing e. g. MAC address.

License
-------

This program is open source software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.