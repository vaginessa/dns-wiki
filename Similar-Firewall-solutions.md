Index
-----------------

* [Similar Android Firewall solutions](#similar-android-firewall-solutions)
* [Non-root Firewalls](#non--root-firewalls)

Similar Android Firewall solutions
-----------------

Generally there are seven categories of Android firewalls available:
* Firewalls which uses a _local VPN_ for traffic filtering (Dr. Web Anti-Virus,[...])
* Firewalls which use a _separate own VPN_ (Android doesn't allow to use two VPNs together the same time) like Opera's _Max/Turbo_ feature to compress/reduce the web traffic (if it's enabled the firewall may not work)
* Firewalls which using _iptables_, like AFWall+, Droidwall, Avast,[...]. Iptables (netfilter) will be called to execute e.g. the NAT table. 
* Firewalls which using a _local HTTP proxy_ (or integrate it in Android's VPN) like AdAway, AdGuard (vpn is the first method but you can switch to http proxy method),...
* Browser or app related firewalls, like NoScript, Bluetooth Firewalls and such, which basically only blocking specific functions and not the traffic itself.
* Dynamic egress filtering: Monitors all outbound network traffic and issue dynamic prompts (on-demand) in order to determine egress filter rules. The rules are defined per application.
* Application-Layer Firewalls (all outdated), they using the Android Framework to block app requests.
* XPosed hacks to intercept directly on the OS layer (e.g. LightningWall). This doesn't need any iptables or additional scripts since the xposed framework provides the _hacking_ ability's. 


Non-root Android Firewalls
---------------------

All of these firewalls working with a local Proxy/[VPN](https://developer.android.com/reference/android/net/VpnService.html) service, which means that they not working with IPtables like AFWall+. They only work on a app-layer size which _fake_ a VPN connection, means the rules are applied on the VPN servers and not on the Android OS. The VPN package will be created to monitor incoming and outgoing traffic (which not need root access). The biggest problem is that such VPN services not work with WiFi tethering or hotspot's. And another con is that you can't run other VPN services and VPN/Proxy's apps at the same time together. 

We not recommend to use any no-root firewall for above reasons, there are others too e.g. see [an German article](http://www.kuketz-blog.de/android-firewall-ohne-root-%E2%80%A2-noroot-firewall/):

_Final version 01.23.2016_
