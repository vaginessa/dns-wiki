Index
-----

* [Description](#description)
* [Important](#important)
* [Basics](#basics)
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

Basics
------

Working with the basics can be useful if you having troubles getting something to work. So please compare your problems with the [stock source](https://android.googlesource.com/platform/system/netd/+/master/SecondaryTableController.cpp) (VpnService.java, ConnectivityService.java, NetworkManagementService.java, NativeDaemonConnector.java, [CommandListener.cpp](https://android.googlesource.com/platform/system/netd/+/refs/heads/kitkat-release/CommandListener.cpp), [NetdConstants.cpp](https://android.googlesource.com/platform/system/netd/+/refs/heads/kitkat-release/NetdConstants.cpp), etc.) and look if your configuration is different from it. You also need to know that the result from the output that was given may can differ, decency which os version, kernel (iptables) and config you are using.

<code>ip rule list</code> or <code>iptables -t nat --list-rules</code>

root@Android: / | # iptables -t nat --list-rules
------------ | -------------
-P | PREROUTING ACCEPT
-P | INPUT ACCEPT
-P | OUTPUT ACCEPT
-P | POSTROUTING ACCEPT
-N | natctrl_nat_POSTROUTING
-N | oem_nat_pre
-N | st_nat_POSTROUTING
-A | PREROUTING -j oem_nat_pre
-A | POSTROUTING -j natctrl_nat_POSTROUTING
-A | POSTROUTING -j st_nat_POSTROUTING




<code>ip6tables -L st_filter_OUTPUT</code>

<code>Chain st_filter_OUTPUT (1 references)</code>

target | prot opt | source | destination | .....
------------ | ------------- | ------------- | ------------- | -------------
REJECT       | all           | anywhere      | anywhere      | mark match 0x3d reject-with icmp6-port-unreachable




Option | Description | Valid states are
------------ | ------------- | -------------
-A | Append this rule to a rule chain. | INPUT, FORWARD and OUTPUT
-L | List the current filter rules. | 
-mconntrack | Allow filter rules to match based on connection state. Permits the use of the --ctstate option. | 
--ctstate | Define the list of states for the rule to match on. | NEW, RELATED, ESTABLISHED or INVALID
-m limit | Require the rule to match only a limited number of times. Allows the use of the --limit option. Useful for limiting logging rules. | 
--limit | The maximum matching rate, given as a number followed by "/second", "/minute", "/hour", or "/day" depending on how often you want the rule to match. | If this option is not used and -m limit is used, the default is "3/hour". | 
-p | The connection protocol used. | etho, tun0, ppp0, etc. As a special case, an interface name ending with a `+`' will match all interfaces. The interface name can be preceded by a `!`' with spaces around it, to match a packet which does not match the specified interface(s), eg -i ! ppp+.
--dport | The destination port(s) required for this rule. A single port may be given, or a range may be given as start:end, which will match all ports from start to end, inclusive. | 
-j | Jump to the specified target. By default, iptables allows four targets | ACCEPT, REJECT, DROP or LOG
--log-prefix | When logging, put this text before the log message. Use double quotes around the text to use. | 
--log-level | Log using the specified syslog level. 7 is a good choice unless you specifically need something else. | 
-i | Only match if the packet is coming in on the specified interface. | 
-I | Inserts a rule. Takes two options, the chain to insert the rule into, and the rule number it should be. | -I INPUT 5 would insert the rule into the INPUT chain and make it the 5th rule in the list.
-v | Display more information in the output. Useful for if you have rules that look similar without using -v. |  
-s --source | address[/mask] source specification | 
-i --destination | address[/mask] destination specification | 
-o --out-interface | output name[+] network interface name  | [+] for wildcard




The State Match | Description
------------ | -------------
NEW | A packet which creates a new connection.
ESTABLISHED | A packet which belongs to an existing connection.
RELATED | A packet which is related to, but not part of, an existing connection, such as an ICMP error, or (with the FTP module inserted), a packet establishing an ftp data connection.
INVALID | A packet which could not be identified for some reason: this includes running out of memory and ICMP errors which don't correspond to any known connection. Generally these packets should be dropped.

Useful links
------------

* [iptables | Wikipedia](http://en.wikipedia.org/wiki/iptables)
* [netfilter/iptables project homepage | The netfilter.org project](http://netfilter.org/)
* [netfilter/iptables documentation | The netfilter.org project](http://www.netfilter.org/documentation/)
* [iptables developer page | netfilter.org](https://git.netfilter.org/iptables/)
* [iptables tutorial by Oskar Andreasson | Frozentux.net](http://www.frozentux.net/iptables-tutorial/iptables-tutorial.html)
* [Quick HOWTO : Ch14 : Linux Firewalls Using iptables | Linuxhomenetworking.com](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_:_Ch14_:_Linux_Firewalls_Using_iptables)
* [IptablesHowTo | Help.Ubuntu.com](https://help.ubuntu.com/community/IptablesHowTo)