Index
-----

* [Description](#description)
* [Important](#important)
* [Useful links](#useful-links)

Description
-----------

> Originally, the most popular firewall/[NAT](http://en.wikipedia.org/wiki/Network_address_translation) package running on Linux/Android was ipchains, but it had a number of shortcomings. To rectify this, the [Netfilter](http://en.wikipedia.org/wiki/Netfilter) organization decided to create a new product called iptables, giving it such improvements as:

* Better integration with the Linux kernel with the capability of loading iptables-specific kernel modules designed for improved speed and reliability.
* [Stateful packet inspection](http://en.wikipedia.org/wiki/Stateful_packet_inspection). This means that the firewall keeps track of each connection passing through it and in certain cases will view the contents of data flows in an attempt to anticipate the next action of certain protocols. This is an important feature in the support of active [FTP](http://en.wikipedia.org/wiki/FTP) and [DNS](http://en.wikipedia.org/wiki/Domain_Name_System), as well as many other network services.
* Filtering packets based on a [MAC address](http://en.wikipedia.org/wiki/MAC_address) and the values of the flags in the TCP header. This is helpful in preventing attacks using malformed packets and in restricting access from locally attached servers to other networks in spite of their IP addresses.
* System logging that provides the option of adjusting the level of detail of the reporting.
* Better network address translation.
* A rate limiting feature that helps iptables block some types of denial of service ([DoS](http://en.wikipedia.org/wiki/Denial-of-service_attack)) attacks. 

Considered a faster and more secure alternative to [ipchains](http://en.wikipedia.org/wiki/Ipchains), iptables has become the default firewall package installed under Linux and Android OS.

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