## Welcome to the AFWall+ wiki!

AFWall+ is an open source firewall client focused on making it easy to work with iptables on your Android OS.


Requirements
-------------

- An Android OS device with at least the 2.3 (Gingerbread/API level 8, NDK 4) or higher version. 
- The device **must be rooted**.
- The Kernel/ROM must have init.d support (for external custom script and data leak fix!).
- The Kernel must have NETFILTER/CONFIG_NETFILTER enabled (adb pull /proc/config.gz - Once you unzip it, you can search for e.g. "NETFILTER").


##Information for AFWall+ users

* [Changelog](https://github.com/ukanth/afwall/blob/master/Changelog.md) describes changes in each version of AFWall+.
* [[CustomScripts]] contains the beginnings of a user manual. Some questions about working with iptables might be answered here.
* [[Apps leak user privacy data during boot]] contains background information about the 'Data leak' during boot.


Need help using or setting up AFWall+?
* Before you ask everything take a look into our [[FAQ]].

Got an error code?
* [[Error codes]] may help you.

Found a bug?
* [[Learn how to report it|HOWTO Report Bug]]

Want to compile AFWall+ yourself?
* [[All you need to know about compiling AFWall+|HOWTO Compile AFWall]]

Debugging?
* [[Phone codes secrets]] contain some useful codes for viewing e. g. MAC address.


License
-------------

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.