Index
-----

* [Description](#description)
* [Important](#important)
* [Useful links](#useful-links)

Description
-----------
Iptables is a powerful firewall built into the Linux kernel and is part of the netfilter project. It can be configured directly, or by using one of the many frontends and GUIs such AFWall+/Droidwall. With it we have control over IPv4  and IPv6 (ip6tables).

There are also some more benefits:
* Stateful packet inspection. This means that the firewall keeps track of each connection passing through it and in certain cases will view the contents of data flows in an attempt to anticipate the next action of certain protocols. This is an important feature in the support of active FTP and DNS, as well as many other network services.
* A rate limiting feature that helps iptables block some types of denial of service (DoS) attacks.
* A firewall is a critical part of any establishment that connects to an unprotected network such as the Internet, but a firewall is never sufficient.

[Nftables](http://www.netfilter.org/projects/nftables/index.html) will replace it some day, but is not merged with AFWall+ at the moment.

Important
---------

Please never ask basic questions how to handle or work with iptables if you not already have take a deeper look in the docs. AFWall+ is already a very easy to use GUI, it's not perfect, but we are working on it to improve it step-by-step. 
If you like to working with [[custom scripts|CustomScripts]] you definitely need some basic knowledge, but for most users the preset GUI options that is given is almost enough to control the important parts. So please take care about the fact we don't have time to explain anything to every new user that ask the same question multiple times, that's why we have the Wiki - to answer the most questions. 
  

Useful links
------------

* [iptables | Wikipedia](http://en.wikipedia.org/wiki/iptables)
* [netfilter/iptables project homepage | The netfilter.org project](http://netfilter.org/)
* [netfilter/iptables documentation | The netfilter.org project](http://www.netfilter.org/documentation/)
* [iptables developer page | netfilter.org](https://git.netfilter.org/iptables/)
* [iptables tutorial by Oskar Andreasson | Frozentux.net](http://www.frozentux.net/iptables-tutorial/iptables-tutorial.html)
* [Quick HOWTO : Ch14 : Linux Firewalls Using iptables | Linuxhomenetworking.com](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_:_Ch14_:_Linux_Firewalls_Using_iptables)