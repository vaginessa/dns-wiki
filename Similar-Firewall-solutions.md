Index
-----------------

* [Similar Android Firewall solutions](#similar-android-firewall-solutions)
* [Non-root Firewalls](#non--root-firewalls)

Similar Android Firewall solutions
-----------------

Generally these are the types of Android firewalls available:

* Firewalls which uses a _local VPN_ for traffic filtering (Netguard[Recommended])
* Firewalls which use a _separate own VPN_ (Adguard etc.,)
* Firewalls which using _iptables_, like AFWall+, Droidwall, Avast,[...]. Iptables (netfilter) will be called to execute e.g. the NAT table. 
* Browser or app related blockers, like NoScript, Bluetooth Firewalls and such, which basically only blocking specific functions and not the traffic itself.
* Application-Layer Firewalls (LineageOS/DataSaver), they using the Android Framework to block app requests.
* XPosed hacks to intercept directly on the OS layer (e.g. LightningWall). This doesn't need any iptables or additional scripts since the xposed framework provides the _hacking_ ability's. 


Non-root Android Firewalls
---------------------

All of these firewalls working with a local Proxy/[VPN](https://developer.android.com/reference/android/net/VpnService.html) service, which means that they not working with IPtables like AFWall+. The VPN package will be created to monitor incoming and outgoing traffic (which not need root access). The biggest problem is that such VPN services can't run other VPN services and VPN/Proxy's apps at the same time together. 

See [an German article](http://www.kuketz-blog.de/android-firewall-ohne-root-%E2%80%A2-noroot-firewall/) for review the non root firewall.

