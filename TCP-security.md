Index
-----

* [Introduction](#introduction)
* [Ports](#ports)
* [Known attacks](known-attacks)
* [Requirements](#requirements)
* [sysctl](#sysctl)
* [IP Rules](#ip-rules)
* [Security testing tools](#security-testing-tools)
* [Protocols](#protocols)
* [IPSec](#ipset)
* [Tcpcrypt](#tcpcrypt)
* [Fingerprinting](#fingerprinting)
* [IPv6 hardening](#ipv6-hardening)
* [External Links](#external-links)

Introduction
-----------

This guide has nothing much todo with AFWall+ itself or it's own configuration, but it can help to protect against known _problems_ and _attacks_ like DNS/DOS/UDP flooding - and they're well tested with Linux Kernels 2.6 up to 4.0, so that's the reason why it's written down here (security everywhere!). These _tweaks_ are based on the articles (designed for a faster broadband) that you can find on the bottom under the ['External Links'](https://github.com/ukanth/afwall/wiki/TCP-security#useful-links) category. 

Please make a **backup first**, and of course there is **no support** or **guarantee that it works on your system**. If you unsure, simply don't use it, ask your ROM/Kernel developer if it's useful to integrate/use it.

You can use it directly via ADB/Terminal Emulator (busybox sysctl -e -w [_your_param_] script or edit the _/proc/sys/net/ipv4/[param_script]_ directly and native under _system/etc/sysctl.conf_ (same like e.g. FreeBSD) with e.g. EsExplorer.


Ports
------------

On Unix based systems all [well-known ports](http://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers#Well-known_ports) below 1024 (0 - 1023) are protected by the root user account. A full list is (sometimes) available under <code>/etc/services</code> or <code>/system/etc/.*</code>. 

Browsers like Firefox introducing an integrated [blocking](https://www.mozilla.org/projects/netlib/PortBanning.html) mechanism that already blocks all (most) suspect ports by default. Due security reasons it isn't suggest to adjust the <code>network.security.ports.banned</code> function, which allow us to disable a banned port (for whatever reasons e.g. port 87 that unlock the security warning). 


#### Interfaces
Android use the following important interfaces:

<code>Default Route</code> - The special address ::/0 denotes default route in IPv6.

<code>Prefix Documentation</code> - This starts with 2001:0DB8::. It's a /32 range that has been reserved for use in documentation and illustrative examples.

<code>Global Multicast</code> - This starts with FF0E::. It's a /8 range reserved for global multicast's. The _E_ (scope) in FF0E distinguishes global multicast from other types of multicast addresses.

<code>Global Unicast</code> - This starts with 2000::. It's a /3 range. ISPs normally get /32 assigned to them out of these ranges and they in turn out /48, /56 and /64 sub-ranges to their customers. These addresses are globally routable.

<code>IPv4 Compatible</code> - These are special addresses assigned to Ipv6-capable devices, such as 'dual stack' devices that speak both IPv4 and IPv6. They have all zeroes for the middle 16bits - thus, they start off with a string of 96 zeroes, followed by the IPv4 addresses. An example of such an address would be 0:0:0:0:0:0:8.8.4.4 in mixed notation, or in short form, ::A.B.C.D where A.B.C.D is an IPv4 address. The IPv4 portion may be represented in HEX notation e.g. STUV:WXYZ instead of the quad octet notation (A.B.C.D). These are now deprecated in favor of IPv4 mapped addresses.

<code>IPv4 Mapped</code> - These are IPv6 addresses that contain embedded IPv4 addresses (basically IPv4 addresses mapped into the IPv6 address space) and are used for devices that are only IPv4-capable. They have a set of 16 ones, after the initial string of 80 zeroes, and then the IPv4 address. So, if an IPv4 device has the address A.B.C.D, it will be represented as 0:0:0:0:FFFF:A.B.C.D, or in short form as ::FFFF:A.B.C.D. The IPv4 portion may be represented in HEX notation, e.g. STUV:WXYZ instead of the quad octet notation (A.B.C.D). This one if the SIIT (stateless IP / ICMP translation) techniques to enable IPv4 and IPv6 hosts to communicate with each other.

<code>IPv4 Translated</code> - These are IPv6 addresses that contain embedded IPv4 addresses which have been temporarily assigned to IPv6-only hosts to allow them to communicate with IPv4 hosts. This is another SIIT (Stateless IP/ICMP translation) technique which allows IPv6 and IPv4 hosts to talk each other without having to assign both type of addresses to all hosts. These addresses can be represented  by the form ::FFFF:A.B.C.D. The IPv4 portion may be represented in HEX notation, e.g. STUV:WXYZ instead of the quad octet notation (A.B.C.D). This one if the SIIT (stateless IP / ICMP translation) techniques to enable IPv4 and IPv6 hosts to communicate with each other.

<code>ISATAP</code> - Intra-Site Automatic Tunnel Addressing Protocol is an IPv6 transition mechanism meant to transmit IPv6 packets between 'dual-stack' devices on top of an IPv4 network. The addresses have the form ::0:5EFE:A.B.C.D. where A.B.C.D. is the embedded IPv4 address. The IPv4 portion may be represented in HEX notation like STUV:WXYZ instead of the quad octet A.B.C.D. Depending on the prefix, the ISATAP address can be a link-local, site-local, global or global-6to4 address.

<code>Link Local Multicast</code> - These are a type of multicast IPv6 addresses matching the format FFx2::/16. Here 'FF' denotes a multicast address, '2' gives it Link-Local scope (i.e. nodes on the sane subnet so packets may not be routed) and the 'x' can be zero (denotation that the address is permanently  assigned by IANA and well known) or non-zero denoting a transient multicast address.

<code>Link Local</code> - These addresses match the format FE80:://10 with the tenth bit (scope bit) set to 0 which denotes local scope i.e. they will not be routed outside the local subnet. The possible prefixes are FE8x::, FE9x::, FEAx:: and FEBx::. They are analogous to the IPv4 automatic private IP address or auto-IP range of 169.254.1.ß-169.254.254.255. They are assigned using stateless addresses auto-configuration procedure and are required for internal functioning of various protocol components before any other mechanism like DHCP is available. These are intended only for communications within the local sub-network and are not routed.

<code>Loopback</code> - The special address ::1/128 in IPv6 is the loopback address which is analogous to 127.0.0.1 in IPv4. The loopback address is an unicast localhost addresses. If an application in a host sends packets to this addresses, the IPv6 stack will loop these packets back on the same virtual interface. 

<code>Multicast</code> - There is no broadcast address in IPv6 but there are various types of multicast address matches the format FFxx::/8. The 'FF' prefix denotes a multicast address (i.e. all bits in bit position 1-8 are ones). There are various sub-types of multicast addresses denotes by their flag bits (bits 9-12) and scope bits (bits 13-16). Flag bits (9-12) denote the nature of the multicast address. If a bit position 12 (Transient  Bit) is 0, then its a well-known address, otherwise it's a transient assignment. Bit positions 9, 10, 11 are unused for now.

<code>NAT64</code> - Is a mechanism to allow IPv6 hosts to communicate with IPv4 server. The NAT64 server is the endpoint for at least one IPv4 address and an IPv6 network segment of 32-bits (64:FF9B::/96). The IPv6 client embeds the IPv4 address it wishes to communicate with using these bits, and sends it's packets to the resulting address. The NAT64 server then creates a NAT-mapping between the IPv6 and the IPv4 address, allowing them to communicate.

<code>Node Local Multicast</code> - Starts with FF01::. It's a /8 range reserved for Node Local Multicast. The '1' (scope) in FF01 distinguishes node local multicast from other types of multicast addresses.

<code>ORCHID</code> - Addresses identified by a prefix of 2001:10::/28 are Overlay Routable Cryptographic Hash IDentifies addresses. These addresses are used as identifies and are not routable at the IP layer. Addresses within this block should not appear on the public internet. RFC 4843 provides more information about their purpose and generation.

<code>OSI NSAP mapped</code> - is another IPv6 transition mechanism which has been deprecated. The address block 0200::/7 was defined as an OSI NSAP-mapped prefix set in 1996, but was deprecated in Dec. 2004.

<code>Organization Local Multicast</code> - Starts with FF08::. It's a /8 range reserved for Organization Local multicast. The '8' (scope) in FF08 distinguishes organization local multicast from other types of multicast addresses.

<code>Reserved</code> - These addresses generally start with 00xx::. These belong to a portion of the address space set aside as reserved for various uses by the IEFT, both present and future. Unlike IPv4, which has many small reserved blocks in various locations in the address space, in IPv6 the reserved block is at the top of the address space first 8 bits are zeroes. This represents 1/256 of the total address space. 

<code>Site Local Multicast</code> - ?????

<code>Site Local</code> - These addresses start with FECx::, FEDx::, FEEEx:: or FEFx::. They have their tenth bit set to 1 which denotes scope over the entire site, i.e. they can be routed thought the entire site but not outside. The scope distinguishes them from Link Local addresses. 

<code>Terredo</code> - This is an transition technology that gives full IPv4 connectivity for IPv6-capable hosts which are on the IPv4 Internet but which have no direct native connection to an Ipv6 network. Compared to other similar protocols it's distinguishing feature is that it is able to perform it's function even from behind network address translation (NAT) devices such as home routers. Teredo client IPv6 addressees start with the Teredo prefix 2001:0::/32. The Teredo IPv6 address contains IPv4 server address, obfuscated UDP port number occupying different bit position. This helps Teredo traverse NAT routers.

<code>Transport Relay Translator</code> - This is another IPv4-IPv6 translation mechanism working on the transport layer. A TRT system is installed between IPv6-only hosts and IPv4 hosts and it translates TCP, UDP/IPv6 to TCP, UDP/IPv4 and vice versa. A dummy address prefix of C6::/64 is proposed in RFC 3142 for use in the TRT system. The addresses contain embedded IPv4 address in the last 32 bits.

<code>Unique Local Unicast</code> - These are analogues to private IPv4 address ranges described in RFC 1918. These address prefix is FC00::/7 and the addresses starts with FD00 since the 8th bit or the 'L' bit is always 1 in these addresses (RFC 4193). The algorithmically-derived 48-bit 'Global ID' in bits 9 through 48 is what gives 'uniqueness' to the addresses for a particular site.

<code>Unique Wildcard</code> The special address 0:0:0:0:0:0:0:0 which can also be written in short form as ::. This is generally used as source address in UDP packets during dynamic address configuration. It generally refers to the hosts itself and used when the host does not know it's own address.

<code>6bone</code> - 6bone was once an experimental project used as testbed for IPv6 deployment and testing. It has been decommissioned since 2006 and addresses have been returned to the IANA. 6bone addresses had a orefix of 3FFE::/16.

<code>6in4</code> - This is another transition mechanism for migrating from Internet Protocol version 4 (IPv4) to IPv6. 6in4 uses tunneling to encapsulate IPv6  traffic over explicitly-configured IPv4 links as defined in RFC 4213. The 6in4 traffic is sent over the IPv4 Internet inside IPv4  packets whose IP headers have the IP protocol number set to 41. This protocol number is specifically designed for IPv6 encapsulation. There are no special IPv6 addresses associated with 6in4. The RFC specifies discarding of invalid 6in4 packets, i.e. those containing IPv4-mapped., IPv4-compatible, loopback and multicast addresses as their source address. 

<code>6rd</code> - This is a mechanism to facilitate Ipv6 rapid deployment across IPv4 infrastructures of Internet service providers (ISPs). This is improved form of the 6to4 transition mechanism which aims to encourage ISPs to deploy IPv6 to their customers without placing substantial network upgrade requirement on the ISP. RFC 5569 describes 6rd in detail and does not specific a specific address prefix.

<code>6to4</code> - This is another transition mechanism to facilitate migration from IPv4 to IPv6. It allows IPv6 packets to be transmitted over an IPv4 network (generally the IPv4 Internet) without the need to configure explicit tunnels. Special relay servers are also in place that allow 6to4 network to communicate with native IPv6 networks. Most IPv6 networks use autoconfiguration, which requires the last 64 bits for the host.The first 64 bits are the IPv6 prefix. The first 16 bits of the prefix are always 2002:; the next 32 bits are the IPv4 address, and the last 16 bits of the prefix are arbitrarily chosen by the router. Any IPv6 address that begins with the 2002::/16 prefix is known as a 6to4 address. 6to4 does not facilitate interoperation between IPv4-only hosts and IPv6-only hosts. 6to4 is simply a transparent mechanism used as a transport layer between IPv6 nodes. 


Known attacks
------------

These are all (or mostly all) of known possible attacks which is TCP affected by. Most off all kernels and configuration are already hardened by this, but you need to check it manually in the source, via ADB or simply ask your developer if it's included. 

* Desynchronization during connection establishment
* Desynchronization in the middle of a connection
* Fingerprinting
* ICMP attacks
* Nonblind Spoofing
* Routing (RIP) attacks
* DNS attacks such DNS Poisoning and BIND attacks
* Hijacking/Injection of the connection 
* Sequence Guessing
* Replay attacks + Dsniff attacks
* LAN Sniffing (or general packets sniffing)
* Unique Identifiers attacks
* TCP SYN attacks (flooding)
* Denial of Service (DoS)
* IP spoofing
* Smurf attacks
* Man-in-the-Middle Attacks via faked SSL/TLS certificates ([example apps](https://docs.google.com/spreadsheets/d/1t5GXwjw82SyunALVJb2w0zi3FoLRIkfGPc7AMjRF0r4/edit#gid=1053404143) that are still affected)
* Backdoors and side channel attacks on protocol layer or software layer
* 0day
* Authentication and encryption problems
* ....

A very good explanation of all the attacks listed above are on [Peters Smith fantastic article/book](http://linuxbox.co.uk/linux-network-security/) about common Linux security. It's highly recommend to read it, since it's very detailed and good to understand. 

Requirements
------------

As described over [here](http://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml) the following stuff should be _blacklisted_ by default for any vulnerability scanner (and only for them!). These is necessary if you scan your network security against known port scanners like _Nmap_ to get an better result, since it may can display stuff which more confuse you as this shows useful stuff. 

| Address | RFC | Description |
| :--- | :--: | ---: |
| 0.0.0.0/8 | [RFC1122](https://tools.ietf.org/html/rfc1122) | Host Network |
| 100.64.0.0/10 | [RFC6598](https://tools.ietf.org/html/rfc6598)| Shared Address Space |
| 127.0.0.0/8 | [RFC1122](https://tools.ietf.org/html/rfc1122) | Loopback|
| 169.254.0.0/16 | [RFC3927](https://tools.ietf.org/html/rfc3927) | Link Local |
| 172.16.0.0/12 + 10.0.0.0/8 | [RFC1918](https://tools.ietf.org/html/rfc1918) | Private-Use |
| 192.0.0.0/24 | [RFC6890](https://tools.ietf.org/html/rfc6890) | IETF Protocol Assignments |
| 192.0.2.0/24 | [RFC5737](https://tools.ietf.org/html/rfc5737) | Documentation (TEST-NET-1) |
| 192.88.99.0/24 | [RFC3068](https://tools.ietf.org/html/rfc3068) | 6to4 Relay Anycast |
| 192.168.0.0/16 | [RFC1918](https://tools.ietf.org/html/rfc1918) | Private-Use |
| 198.18.0.0/15 | [RFC2544](https://tools.ietf.org/html/rfc2544) | Benchmarking (Nmap) |
| 192.0.2.0/24 | [RFC5737](https://tools.ietf.org/html/rfc5737) | TEST-NET-1 (non puplic) |
| 198.51.100.0/24 | [RFC5737](https://tools.ietf.org/html/rfc5737) | TEST-NET-2 (non puplic) |
| 203.0.113.0/24 | [RFC5737](https://tools.ietf.org/html/rfc5737) | TEST-NET-3 (non puplic) |
| 240.0.0.0/4 | [RFC1112](https://tools.ietf.org/html/rfc1112) | Reserved |
| 255.255.255.255/32 | [RFC0919](https://tools.ietf.org/html/rfc0919) | Limited Broadcast |

For [Multicast](http://www.iana.org/assignments/multicast-addresses/multicast-addresses.xhtml):

| Address | RFC | Description |
| :--- | :--: | ---: |
| 224.0.0.0/4 | [RFC5771](https://tools.ietf.org/html/rfc5771) | Multicast/Reserved |

* All of the hardening stuff mentioned on this page should be used from advanced users only!
* Time to read this little article + to backup (display current configuration via _sysctl -a_) your existent configuration (just in case something goes wrong) 
* ADB/Terminal Emulator/BusyBox & a **rooted device**
* Kernel/ROM that support these kind of tweaks (if not it doesn't work or is useless)
* init.d support if you like to store these tweaks into a .sh script and apply them at boot (once)
* sysctl support
* A compiled Kernel based on 2.6 up to 4.0+

sysctl
------------

Sysctl is an interface that allows you to make changes to a running Linux kernel. With _/etc/sysctl.conf_ you can configure various Linux networking and system settings.

To load existent settings run:
<code># sysctl -p</code> or better <code>#adb shell su sysctl -p</code> (to avoid some trouble like access denied or xyz is an unknown key [except if your kernel does not support it]). 

To apply all security settings, you need a Kernel which handles sysctl and applies them to <code>/proc</code>. Normally all kernels use that ability by default. If there is no _/etc/sysct.conf_ you can just create them and use a init.d script to apply all included parameters. Or just ask your kernel developer to enable it.

Since the list is huge, here is an [default sysctl on my gist](https://gist.github.com/CHEF-KOCH/0001e66a8c10b1177abe#file-sysctl-conf). 

And a tweaked one with preferred security over speed is also available [here](https://gist.github.com/CHEF-KOCH/0001e66a8c10b1177abe#file-tweaked-sysctl-conf) (needs to be renamed to _sysctl.conf_).


**It's not recommend to apply any debug/memory or other 'performance' tweaks into the sysctl file (use init.d for it) since this could be end-up in a boot-loop. So here are only entries which never cause any problems that can't be fixed very easy by editing the lines as per needs.**

:warning: Do not add any non listed entries in your sysctl.conf - there are a lot of false/myth entries explained on the wild which not work anymore with newer kernels above 2.6+! So, all is already very well explained and which settings are deprecated too - just read the comments! :warning:

IP Rules
------------

Most of the mentioned port's are already supported by AFWall+ and can be controlled directly with the interface (to allow/reject them) but these are normally open (and should be open to avoiding some troubles):

| Rule Name | Status | Range | Protocol | Remote Port | Local Port |
| :--- | :--: | :---: | :--: | :---: | :--: |
| Allow DNS | Allow | All IP packets | UDP | 53 | Any Port |
| Allow Hypertext Transfer | Allow | All IP packets | TCP | 80 | 80 |
| Allow dynamic IP| Allow* | All IP packets | UDP| 67-68 | 67-68 |
| Allow SNMP | Allow* | All IP packets | UDP | Any Port | 162, + ?* |
| Allow Secure Shell/File Transfer Protocl (SSH)| Block* (default disabled) | All IP packets | UDP | 22 | 22 |
| Allow VPN (GRE) | Allow | All IP packets | GRE |  |  |
| Allow VPN (AH) | Allow | All IP packets | AH | | |
| Allow IKE VPN | Allow | All IP packets | UDP | 500, + ?* | 500, + ?* |
| Allow PPTP VPN | Allow | All IP packets | TCP | 1024-65535 | 1723 |
| Allow L2TP VPN | Allow | All IP packets | TCP | 1024-65535 | 1701 |
| Allow FTP Data | Allow | All IP packets | TCP | 20 | Any Port |
| Allow BT Download| Allow | All IP packets | TCP | Any Port | 6881-6890 |
| Allow outbound ping (ICMP Echo Reply) | Allow | Got IP packets | ICMP | depending if it's a trusted signature 0 |  |
| Allow outbound ping (ICMP Echo Requests) | Allow | Sent IP packets | ICMP | depending if it's a trusted signature 0 |  |
| Deny inbound ping | Block | Got IP packets | ICMP | depending if it's a trusted signature 0 |  |
| Allow Destination Unreachable | Allow | All IP packets | ICMP | depending if it's a trusted signature 0 |  |
| Allow all ICMP | Allow | All IP packets | ICMP | Any Port|  |
| Deny 135 and 445 (TCP)| Block* | All IP packets | TCP | Any Port | 135, 445 |
| Allow IGMP | Allow | All IP packets | IGMP | System only | |
| Allow VPN ESP | Allow | All IP packets | ESP | | |
| | | | | | |
| A complete | port list| is available  | over | [here](http://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers) on  | Wikipedia |

* Depending on which configuration you're on

-> AFWall+ doesn't currently supports ESP, IKE, L2TP, AH and GRE protocols (or any exotic port such QUIC/QOTD) via a gui switch but you can control this via custom scripts/iptables itself.

-> Depending which kernel you use and which configuration it was compiled with, you will see or not see with external tools and commands traffic like IGMP, but you don't need to worry about such stuff. On some configuration it's really necessary, like on [IPv6 systems](https://github.com/ukanth/afwall/wiki/CustomScripts#important-notes-about-ipv4-and-ipv6-differences).

-> It's not worth to touch any of the mentioned ports, because it doesn't have any notable effect (also no security plus). There are rare situations, mostly only on servers which are compromised by brute force and other attacks (Port 22) but a normal Android client is mostly not affected by this and remember that the port will be automatically closed after the connection is lost/dropped, [it's also a bad idea to put such port to another place](https://www.adayinthelifeof.nl/2012/03/12/why-putting-ssh-on-another-port-than-22-is-bad-idea/). In AFWall+ the whole inbound traffic is by default disabled, means you need to enable the "Allow inbound" option first.

Security testing tools
------------

The term _security tools_ are a bit misleading, the tools doesn't secure anything by itself, but they can display open ports, possible vulnerability's and other threads. Often there are called 'scanners'/'network intrusion detection'/'preventation' or penetration/auditing testing tools which scanning the OS against known security holes or reveal other potential weaknesses. 

There are mostly all **for advances users**, because it need a lot of time, knowledge and tests to really understand them - of course reading is always necessary - but since most of them are well documented it shouldn't be a problem to crow from a beginner to an expert. 

The following list is unsorted and for Windows/Linux/Mac OS/Android systems and may list tools that coasts (_a lot of_) money! Your Av may detect the sites or the tools as insecure/malware in fact they can be used for good/evil!

### Unsorted
* [Ncat](http://nmap.org/ncat/)
* [tcpdump](http://www.tcpdump.org/) - sometimes already shipped with Android as standalone binary
* [Kismet](http://www.kismetwireless.net/)
* [Nikto](http://www.cirt.net/nikto2)
* [Hping](http://www.hping.org/)
* [Ettercap](http://ettercap.sourceforge.net/)
* [w3af](http://w3af.sourceforge.net/)
* [OpenVAS](http://www.openvas.org/)
* [Scapy](http://www.secdev.org/projects/scapy/)
* Ping/telnet/dig/traceroute/whois/netstat (sometimes already shipped with Android/Windows/Unix/Linux/Mac OS as standalone binaries) - On some systems the names are different but the function should be the same!
* [Paros Proxy](http://www.parosproxy.org/)
* [Maltego](http://www.paterva.com/web4/index.php/maltego)
* [The Sleuth Kit](http://www.sleuthkit.org/sleuthkit/)
* [EnCase](http://www.guidancesoftware.com/)
* [Unicornscan](http://www.unicornscan.org/)
* [SAINT](http://www.saintcorporation.com/saint/)
* [Core Impact](http://www.coresecurity.com/content/core-impact-overview)
* [PWDDump](http://www.tarasco.org/security/pwdump_7/)
* [Lucy](http://gtta.net/PS/lucy.html#blog)
* [Stunnel](https://www.stunnel.org/index.html)
* [Fwknop](http://www.cipherdyne.org/fwknop/)
* [aircrack-ng](http://www.aircrack-ng.org/)
* [ettercap](http://ettercap.github.io/ettercap/)
* [p0f](http://lcamtuf.coredump.cx/p0f.shtml)
* [Nessus](http://www.nessus.org/)
* [cowpatty](http://www.willhackforsushi.com/?page_id=50)
* [Zed Attack Proxy (ZAPP)](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project)
* [Snort](http://www.snort.org/)
* [Yersinia](http://www.yersinia.net/download.htm)
* [Skipfish](http://code.google.com/p/skipfish/)
* [RFIDiot](http://rfidiot.org/)
* [John the Ripper](http://www.openwall.com/john/)
* [ophcrack](http://ophcrack.sourceforge.net/)
* [hrPING](http://www.cfos.de/en/ping/ping.htm)
* [SQLiX](https://www.owasp.org/index.php/Category:OWASP_SQLiX_Project)
* [btscanner](http://www.pentest.co.uk/downloads.html)
* [raw2vmdk](https://github.com/Zapotek/raw2vmdk)
* [Paros Proxy](http://sourceforge.net/projects/paros/)
* [sqlmap](http://sqlmap.org/)
* [UCSniff](http://ucsniff.sourceforge.net/)
* [sipproxy](http://siproxd.sourceforge.net/)
* [SWFIntruder](http://code.google.com/p/swfintruder/)
* [Lorcon](http://code.google.com/p/lorcon/)
* [ICMP tools](http://gont.com.ar/tools/icmp-attacks/)
* [bt_audit](http://www.betaversion.net/btdsd/)
* [dnsmap](http://code.google.com/p/dnsmap/)
* [chaosreader](http://www.brendangregg.com/chaosreader.html)
* [Firekeeper](http://firekeeper.mozdev.org/) (outdated)
* [Malzilla](http://malzilla.sourceforge.net/)
* [Packetyzer](http://sourceforge.net/projects/packetyzer/)
* [Airsnort](http://sourceforge.net/projects/airsnort/)
* [ATK - Attack Tool Kit](http://www.computec.ch/projekte/atk/)
* [ike-scan](https://github.com/royhills/ike-scan)
* [WebScarab](http://sourceforge.net/projects/owasp/files/WebScarab/)
* [DHCPing](http://c3rb3r.openwall.net/dhcping/)
* [UDPFlood](http://www.mcafee.com/us/downloads/free-tools/udpflood.aspx)
* [IKEProbe](http://www.ernw.de/)
* [fragroute](http://www.monkey.org/~dugsong/fragroute/)
* [TCP Wrapper](ftp://ftp.porcupine.org/pub/security/index.html)
* [SARA](http://www-arc.com/sara/)

#### Windows Tools
* [Sysinternals Suite](http://technet.microsoft.com/en-us/sysinternals/bb842062) - The Sysinternals Troubleshooting Utilities
* [Windows Credentials Editor](http://www.ampliasecurity.com/research/windows-credentials-editor/) - security tool to list logon sessions and add, change, list and delete associated credentials
* [mimikatz](http://blog.gentilkiwi.com/mimikatz) - Credentials extraction tool for Windows OS

###  Online/Web-server test tools/pages:
* [Check current Webserver version | urlm.de](http://urlm.de/)
* [SSL quality check by Qualys SSL Labs | ssllabs.com](https://www.ssllabs.com/ssltest/)
* [SSL Mailserver check by CheckTLS | Checktls.com](http://checktls.com/perl/TestReceiver.pl)
* [PFS Mailserver check by SSL-Tools | De.SSL-Tools.net](https://de.ssl-tools.net/mailservers)
* [SSL analysis via Burp Proxy under Linux | portswigger.net](http://portswigger.net/burp/help/proxy.html)
* [SSL analysis via Fiddler under Windows | Fiddler.com](http://fiddler2.com/)
* [Heartbleed attack test page | possible.lv](http://possible.lv/tools/hb/)
* [Another Heartbleed attack by filippo.io | filippo.io](http://filippo.io/Heartbleed/)
* [Decrypt browser SSL (Firefox/Chrome) via SSLKEYLOGFILE | Developer.Mozilla.org](https://developer.mozilla.org/en-US/docs/NSS_Key_Log_Format)


#### DDoS Tools
* [LOIC](https://github.com/NewEraCracker/LOIC/) - An open source network stress tool for Windows
* [JS LOIC](http://metacortexsecurity.com/tools/anon/LOIC/LOICv1.html) - JavaScript based in-browser version of LOIC


### Information Security Conferences
* [DEF CON](https://www.defcon.org/) - An annual hacker convention in Las Vegas
* [Black Hat](http://www.blackhat.com/) - An annual security conference in Las Vegas
* [BSides](http://www.securitybsides.com/) - A framework for organising and holding security conferences
* [CCC](http://events.ccc.de/congress/) - An annual meeting of the international hacker scene in Germany
* [DerbyCon](https://www.derbycon.com/) - An annual hacker conference based in Louisville
* [PhreakNIC](http://phreaknic.info/) - A technology conference held annually in middle Tennessee
* [ShmooCon](http://www.shmoocon.org/) - An annual US east coast hacker convention
* [CarolinaCon](http://www.carolinacon.org/) - An infosec conference, held annually in North Carolina
* [HOPE](http://hope.net/) - A conference series sponsored by the hacker magazine 2600
* [SummerCon](http://www.summercon.org/) - One of the oldest hacker conventions, held during Summer
* [Hack.lu](http://hack.lu/) - An annual conference held in Luxembourg
* [HITB](http://conference.hitb.org/) - Deep-knowledge security conference held in Malaysia and The Netherlands
* [Troopers](https://www.troopers.de) - Annual international IT Security event with workshops held in Heidelberg, Germany
* [Hack3rCon](http://hack3rcon.org/) - An annual US hacker conference
* [ThotCon](http://thotcon.org/) - An annual US hacker conference held in Chicago
* [LayerOne](http://www.layerone.org/) - An annual US security conference held every spring in Los Angeles
* [DeepSec](https://deepsec.net/) - Security Conference in Vienna, Austria
* [SkyDogCon](http://www.skydogcon.com/) - A technology conference in Nashville
* [SECUINSIDE](http://secuinside.com) - Security Conference in Seoul
* [DefCamp](http://defcamp.ro) - Largest Security Conference in Eastern Europe, (Bucharest, Romania)


### Vulnerability Databases
* [NVD](http://nvd.nist.gov/) - US National Vulnerability Database
* [CERT](http://www.us-cert.gov/) - US Computer Emergency Readiness Team
* [OSVDB](http://osvdb.org/) - Open Sourced Vulnerability Database
* [Bugtraq](http://www.securityfocus.com/) - Symantec SecurityFocus
* [Exploit-DB](http://www.exploit-db.com/) - Offensive Security Exploit Database
* [Fulldisclosure](http://seclists.org/fulldisclosure/) - Full Disclosure Mailing List
* [MS Bulletin](https://technet.microsoft.com/security/bulletin/) - Microsoft Security Bulletin
* [MS Advisory](https://technet.microsoft.com/security/advisory/) - Microsoft Security Advisories
* [Inj3ct0r](http://1337day.com/) - Inj3ct0r Exploit Database
* [Packet Storm](http://packetstormsecurity.com/) - Packet Storm Global Security Resource
* [SecuriTeam](http://www.securiteam.com/) - Securiteam Vulnerability Information
* [CXSecurity](http://cxsecurity.com/) - CSSecurity Bugtraq List
* [Vulnerability Laboratory](http://www.vulnerability-lab.com/) - Vulnerability Research Laboratory
* [ZDI](http://www.zerodayinitiative.com/) - Zero Day Initiative
* [FREAK attack test page | freakattack.com](https://freakattack.com/clienttest.html) - FREAK Vulnerability Test


#### SSL Analysis Tools
* [SSLyze](https://github.com/nabla-c0d3/sslyze) - SSL configuration scanner
* [sslstrip](http://www.thoughtcrime.org/software/sslstrip/) - a demonstration of the HTTPS stripping attacks
* [SSLsniff](http://www.thoughtcrime.org/software/sslsniff/) - a demonstration of the HTTPS sniffing attacks


#### Network Tools
* [nmap](http://nmap.org/) - Free Security Scanner For Network Exploration & Security Audits
* [tcpdump/libpcap](http://www.tcpdump.org/) - A common packet analyzer that runs under the command line
* [Burp Suite](http://portswigger.net/burp/) - A network protocol analyzer for Unix and Windows
* [Wireshark](http://www.wireshark.org/) - A network protocol analyzer for Unix and Windows
* [Network Tools](http://network-tools.com/) - Different network tools: ping, lookup, whois, etc
* [netsniff-ng](https://github.com/netsniff-ng/netsniff-ng) - A Swiss army knife for for network sniffing
* [Intercepter-NG](http://intercepter.nerf.ru/) - A multifunctional network toolkit
* [SPARTA](http://sparta.secforce.com/) - Network Infrastructure Penetration Testing Tool


Some other free boot able LIVE CD's like Kali, Tails, Helix,[...] are Linux distributions that claiming to be more secure and hardened against the known attacks compared to other systems like on Windows (general there is no proof except there words since nobody really can compare Windows <-> Linux on a seriously way). They often already included a huge collection of scanning tools. It's always worth to keep on eye on this, since you don't even need to installing them. 

Protocols
---------

Here is a quick list showing the important protocols and their status.

| Protocol | Status | Link |
| :--:| :--: | :--: | 
| HTTP/1.1 | insecure| [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt) / [RFC 2068](http://tools.ietf.org/html/rfc2068) |
| HTTP/2| secure? | [NO! RFC yet](https://tools.ietf.org/html/draft-ietf-httpbis-http2-17) + [this](https://http2.github.io/http2-spec/) |
| SSL 3.0 | insecure | [RFC 6101](https://tools.ietf.org/html/rfc6101) |
| TLS 1.0 | insecure| [RFC 2246](http://www.ietf.org/rfc/rfc2246.txt) |
| TLS 1.1 | insecure | [RFC 4346](http://www.ietf.org/rfc/rfc4346.txt) |
| TLS 1.2 | secure | [RFC 5246](http://www.ietf.org/rfc/rfc5246.txt) |
| SPDY | secure? (alternative for HTTP/1.1 | [Framing layer + RFC 2616](https://tools.ietf.org/html/draft-mbelshe-httpbis-spdy-00) |
| SSH | _possible insecure_ | [RFC 4253](https://tools.ietf.org/html/rfc4253) |
| IPSec | _possible insecure_ (Bullrun) | [RFC 4301](https://tools.ietf.org/html/rfc4301) and the old [RFC 2401](https://www.ietf.org/rfc/rfc2401.txt) |


TLS Handshake example
```bash
Client                                             Server

ClientHello                -------->
                                              ServerHello
                                              Certificate
                                        ServerKeyExchange
                           <--------      ServerHelloDone
ClientKeyExchange
ChangeCipherSpec
Finished                   -------->
                                         ChangeCipherSpec
                           <--------             Finished
Application Data           <------->     Application Data
```

TLS Handshake with Proxy example
```bash
Client                   Proxy                    Server

ClientHello        -->  ------>   -->
                   <--  <------   <--        ServerHello
                          /--     <--        Certificate
                          |
                   verify(certificate)
                          |
                   <--  --/
                   <--  <-----    <--   ServerKeyExchange
                   <--  <-----    <--     ServerHelloDone
ClientKeyExchange
ChangeCipherSpec
Finished           -->  ----->    -->
                                         ChangeCipherSpec
                   <--  <-----    <--            Finished
Application Data   <->  <---->    <->    Application Data
```

TLS CAs Theories:
* Root Certification Authority (CA) are based on trust, they promising to e.g. protect against MITM attacks and offering authentications 
* All Browsers and OS by default trust over 100+ organizations, see [here](https://www.eff.org/observatory) and a detailed map is available as .pdf over [here](https://www.eff.org/files/colour_map_of_CAs.pdf)
* If one CA is compromised you get a huge problem since it have access to everything
* 



Addons for Firefox which claim to fix the situation:
* [Perspective](http://perspectives-project.org/) - [Firefox addon](http://perspectives-project.org/firefox/) - Please read the _Known Issues_ on the page before installing it!
* [Convergence](http://convergence.io/) - [Firefox addon](http://convergence.io/releases/firefox/convergence-current.xpi) or as forked _improved_ version over [here](https://github.com/mk-fg/convergence)
* [Certificate Patrol](http://patrol.psyced.org/) - [Firefox addon](https://addons.mozilla.org/de/firefox/addon/certificate-patrol/) or as alternative [Dane Patrol](https://github.com/hiviah/DanePatrol)


Perspectives:
* Use/search for alternatives auths via PKI
* It's based on _trust on first use_ principle (TOFU)
* Use an SSH trust model
* Unknown keys must be accepted manually by the user
* Keys/Host pairs are cached which doesn't need any interaction (like other addons)
* With every new key the user needs to apply/agree it again 
* Possible MITM - because first use (if it's not from the beginning installed you may already trust a MITM)
* A lot of manual interaction and it's not easy for beginners to see what's secure and what's not
* Compromised Notary-Server is not as bad as a compromised CA
* You can set-up your own Notary-Server, see [here](http://perspectives-project.org/get-involved/)
* [Video](https://www.youtube.com/watch?v=Z7Wl2FW2TcA) _BlackHat USA 2011: SSL And The Future Of Authenticity_ 


Convergence:
* Claims to replace completely usage of any external CA
* Personally I would not use this - because it communicate with an external source 
* Based on the _Perspectives_ idea
* Notary are not only limit to certificates, which makes it possible to allow DNSSEC, Content,...
* Notary can interact like a proxy
* User can choose what he trust and what not - _Trust Agility_
* It installs a local CA 
* Unknown (Port, Host names, certificates) are verified by Notary
* Local CA sig. + Cache 
* Browser only see certificates that are marked as trusted by your own local CA 
* [Notary-Protocol explained](https://github.com/moxie0/Convergence/wiki/Notary-Protocol)
* API problem which means it only works on Firefox - a port to other systems is not _easily_ possible


Certificate Patrol / Dane Patrol (or RequestPolicy):
* Certificate Patrol only monitors certificates that have already been accepted by the browser
* Tools like CertPatrol constantly watching for certificate changes by the browser, depending on what was changed you get different warnings
* MITM is still possible because _Trust on first usage_ 
* Possible annoying due the huge effort which is needed to take control over the CAs since e.g. Google and other big ones have different certificates on different servers on their own network - In fact you often gets routed to a different one 
* Tor Browser Bundle have problems with it and [it's not needed](https://trac.torproject.org/projects/tor/ticket/4064) since it use HTTPS-everywhere observatory 
* Reject the certificate during the handshake of SSL/TLS through POST is a problem especially on banking sites, e.g. if a bank main site is under test.com and the login is posted to anotherpage.com.
* IDN domains are not supported?!
* Possible no autoupdate function (but RequestPolicy does offer such function)
* HTTPS-Everywhere works with Cert Patrol / Dane Patrol / RequestPolicy (but it's not necessary)


On Firefox:
```
security.OCSP.enabled;0 
or under:
Options-> Advanced -> Certificates -> uncheck -> Query OSCP responder servers to confirm the current validity of certificates
```

Tor Browser Bundle / Orbot:
* Just read the note about Tor and addons, [here](https://www.torproject.org/docs/faq.html.en#TBBOtherExtensions) - or just [visit the Tor Forum](https://tor.stackexchange.com/) to talk about the necessary.



An fantastic article about the Certificates and Public Key pinning is available at [OWASP](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning) - for certificate transparency information take a ride over [here](http://www.certificate-transparency.org/). Mozilla itself also have special programs and a huge [overview](https://wiki.mozilla.org/CA:Overview) how they handle the policy and included CAs. 

An article which explains a _Life without a CA_ is available over the official [tor project blog](https://blog.torproject.org/blog/life-without-ca) - [but this doesn't solve all the problems and possible attacks](http://newschoolsecurity.com/2010/03/life-without-certificate-authorities/).

Other _solutions_:
* DNS Certification Authority Authorization ([CAA](http://tools.ietf.org/html/draft-hallambaker-donotissue-03))
* [DANE](https://en.wikipedia.org/wiki/DNS-based_Authentication_of_Named_Entities)
* [HASTLS](http://tools.ietf.org/html/draft-hoffman-server-has-tls-04)
* [The Monkeysphere Project](http://web.monkeysphere.info/)
* Use alternatives to [Public Key Pinning Exstention](https://tools.ietf.org/html/draft-ietf-websec-key-pinning-21) or just alternatives like [Track](http://tack.io/)


IPSec
------------

Internet Security (IPsec) was build as a result of the current IP weaknesses - basically it's a own internet layer which claims to be more secure compared to e.g. IPv4 - in fact it was developed around IPv6 (from Internet Engineering Task Force [IETF]), but was engineered to provide security for both IPv4/IPv6. There are some differences in the datagram formats used for AH and ESP depending on whether IPSec is used in IPv4 and IPv6, since the two versions have different datagram formats and addressing. I highlight these differences where appropriate.

Without going deeper these are the main-cores - you often hear:
* Security Parameter Index (SPI)
* Authentication Header (AH)
* Encapsulating Security Payload (ESP)
* Internet Key Exchange (IKE)
* Security Policy Database (SPD)
* Security Association Database (SAD)
* Security Associations (SA)
* Key Management Protocol (ISAKMP)
* ... and [others](http://tcpipguide.com/free/t_IPSecurityIPSecProtocols.htm)
* A full documentation which is quite "easy" to understand for beginners can be found [here](http://www.tech-faq.com/ipsec.html)

Since this is more secure as the good old IPv4 and you maybe have the ability to use it, this should be preferred. AFWall+ does not have any interface to control it (yet), so the only chance is to work with a custom script. See also [#255](https://github.com/ukanth/afwall/issues/255) and the official [issue tracker](https://code.google.com/p/android/issues/list?can=2&q=IPSEC&colspec=ID+Type+Status+Owner+Summary+Stars&cells=tiles) before you report anything which is already known.

Remember that there is no guarantee since even [IPSec have some weaknesses](http://etutorials.org/Networking/network+security+assessment/Chapter+11.+Assessing+IP+VPN+Services/11.2+Attacking+IPsec+VPNs/).

[OpenSwan](http://www.openswan.org/) is an example implementation of IPSec under Linux. [StrongSwan](http://www.strongswan.org/) does the same and includes support of X.509.

Here is a quick guide:

```
1. Open the Main-Menu
2. Go to "Settings".
3. Open "Wireless & Networks" or "Wireless Controls" depending on your version of Android
4. Select "VPN Settings"
5. Select "Add VPN"
6. Select "Add L2TP/IPSEC PSK VPN" Make sure to select the correct one out of the 4 listed. (NOT L2TP VPN and NOT L2TP/IPSEC CRT VPN)
7. Select "VPN Name" and enter "_<yourownstuff>_" (without quotes) or anything else you want.
8. Select "Set VPN Server" and enter the given server IP Address from your provider.
9. Select Set IPSec pre-shared key and enter the pre-shared key provided to you in the welcome email.
10. L2TP secret & IPSEC identifier remain blank if it's given fill in.
11. Use the 'Menu' button and save the connection.
12. You may be asked to confirm operation with storage password. This is your Android device password!!! Not the VPN password!

13. How to Connect using L2TP IPSEC PSK VPN once configured on Android
- Open the menu and select Settings.
- Select Wireless and Network or Wireless Controls
(depends on your version of Android.)
- Select the VPN configuration you created from the list.
- Enter your username and password.
- Select Remember username and Select Connect

14. How to disconnect using L2TP IPSEC PSK VPN once configured on Android
- Open the menu and choose Settings.
- Select Wireless and Network or Wireless Controls
(depends on your version of Android.)
- Select the VPN configuration from the list.
- Select Disconnect
```

An example IPsec configuration for Windows, Android and Linux can be found over [here](https://help.ubuntu.com/community/L2TPServer).

The main goal from using IPsec:
* Prevent man-in-the-middle attacks from possible attackers attempting your network
* Encryption, to obfuscate your data packets against [most packet sniffers](https://wiki.wireshark.org/IPsec)
* Protection against data corruption
* Theft of user credentials 
* Denial-of-service (Dos) attacks
* ...

Of course since nothing is perfect there is currently no protection against:
* [Decrypting ESP packets](https://ask.wireshark.org/questions/13022/decrypting-esp-packets-verifying-ipsec-settings) (only with ESP tunnel mode)
* IPsec Replay Attack
* Bypassing Firewall Defenses (well, this affects any OS and any configuration)
* Trusted Man-in-the-Middle Attack
* Stealth DOS Attack (Today usually stronger algorithms are used like TripleDES, AES and HMAC-SHA1)
* ....

Overall there are some weaknesses with the IPsec extensions too but it's still preferred over standard  "non" protected connections even if [NSA's Bullrun](https://en.wikipedia.org/wiki/Bullrun_(code_name)) may break it.

Tcpcrypt
------------

The benefit of Tcpcrypt compared to IPsec and others is that this doesn't need any special configuration and the remote end does not need to support any Tcpcrypt extension (ends up with clear text TCP). Like IPsec it supports most common used OS like Windows, Linux, Mac OSX, ... and another benefit [it's overall a bit faster (.pdf)](http://tcpcrypt.org/tcpcrypt-slides.pdf).

Please [see here for an example configuration](https://github.com/scslab/tcpcrypt/blob/master/INSTALL-Linux.markdown) and requirements.

IPv6 hardening
------------

If you're not on any nativ IPv6 environment you can simply skip this step and disable IPv6 global. To avoid slow Wifi connections and other problems it could also be useful to use IPv4, since this seems to be problematic on some ROMs.

Known IPv6 specific attacks:
• Router Advertisement related attacks (MiTM, router redirection, DoS, etc)
• MITM or DoS attacks during the Neighbor Discovery process
• DoS during the DAD/IN process
• IPv6 Extension Headers related attacks
• Smurf-like attacks at the local link
• Reconnaissance by exploiting various ICMPv6 messages
• Packet Too Big Attacks
• ....


The following needs to be changed manually:
• MTU from 1500 to ≥1280 bytes, this should be used by default, broken in most Android ROM's
• The IPv6 host address
• Gateway
• Default DNS server
• Neighbor Cache (obsolete in Android, use this only on servers)
• Hop Limit (current)
• ...


The following needs to be changed disabled in the name of security (see RFC3849):
• Router Advertisements (see resolv.conf/dnsmasq)
• DAD and MLD process
• Generally IPv6 Extension Headers needs to be blocked
• Unwanted ICMPv6 messages
• ...


Outgoing allowed (all the rest needs to be blocked):
• Packet Too Big
• Echo Replies
• Echo Requests


Incoming (allowed):
• Destination Unreachable
• Packet Too Big
• Echo Replies
• Time Exceeded (Type 3 Code 0, Error msg)
• Parameter Problem (Type 4 Codes 1 and 2) (obsolete?)
• For network troubleshooting purposes, you can allow Echo Requests messages from very specific(s) hosts.


Unfortunately AFWall+ does not support this via UI, an alternative could be to use custom scripts.


Fingerprinting
------------

AFWall+ or any other Firewall does not protect against the following fingerprint detection mechanism:
* JavaScript link rewriting  / JavaScript Performance Fingerprinting 
* Window Name detection (DCOM identifier storage)
* Headers (referrer, HTTP, agent,... + Timestamps + Timezone leaks)
* LXC-specific leaks
* Operating System Type Fingerprinting
* Keystroke Fingerprinting
* Supercookies (Master/evercookies/HSTS supercookies/Flash) [this is a result because of third-party cookies blocking]
* WebGL / WebRTC
* Display Media information
* Monitor, Widget, and OS Desktop Resolution + Fonts 
* USB Device ID Enumeration
* Invasive Authentication Mechanisms
* Open TCP Port Fingerprinting 
* HTML5 Canvas and Image Extraction
* Flash (Since Android 4.x removed official in Android -> HTML5)
* Plugins intended to be good, but leaking sensitive data (meta,big/small,...) 
* SSL/TLS session resumption, HTTP Keep-Alive and SPDY
* DOM Storage and auth.
* Several bypass xyz settings (like Proxy/VPN/physically access/records,...)

A full overview can be found over [here](https://www.torproject.org/projects/torbrowser/design/#fingerprinting-linkability). A fingerprint test can be found over [here](https://panopticlick.eff.org/about.php).

It's highly recommend to use Tor/Orbot + NoScript (NSA NoScript) + Certificate Control and (if no special about:config is in use) [Random Agent Spoofer](https://github.com/dillbyrne/random-agent-spoofer/).
[HTTPS-Everywhere isn't necessary with the correct Firefox and Tor settings](https://gist.github.com/CHEF-KOCH/b730d9511761e999c9ba) (since HTTPS-Everywhere may break some sites and is already implemented in NoScript [Desktop Version only] or can be managed by the Browser itself).

External Links
------------

* [Internet Engineering Task Force (IETF) | IETF.org](https://www.ietf.org/)
* [IPsec | Wikipedia.org](https://en.wikipedia.org/wiki/IPsec)
* [/proc file system | Wikipedia.org](http://en.wikipedia.org/wiki/Procfs)
* [TCP Vs. UDP by Erik Rodriguez | Skullbox.net](http://www.skullbox.net/tcpudp.php)
* [TCP/IP Security by CHRIS CHAMBERS | LinuxSecurity.com](http://www.linuxsecurity.com/resource_files/documentation/tcpip-security.html)
* [Linux TCP Tune | PSC.edu ](http://www.psc.edu/networking/projects/tcptune/#Linux)
* [Linux TCP Tuning | Cyberciti.biz](http://www.cyberciti.biz/faq/linux-tcp-tuning)
* [IP sysctl documentation (.txt file) | Cyberciti.biz](http://www.cyberciti.biz/files/linux-kernel/Documentation/networking/ip-sysctl.txt)
* [Linux Kernel etcsysctl conf security hardening | Cyberciti.biz](http://www.cyberciti.biz/faq/linux-kernel-etcsysctl-conf-security-hardening)
* [sysctl | ArchWiki](https://wiki.archlinux.org/index.php/Sysctl)
* [Hardening TCP/IP Stack Syn Attacks | Symantec.com](http://www.symantec.com/connect/articles/hardening-tcpip-stack-syn-attacks)
* [Whitepaper - Android Security Hardening (33799.pdf file) | Sans.org](http://www.sans.org/reading-room/whitepapers/sysadmin/securely-deploying-android-devices-33799)
* [Ipsysctl-tutorial | Frozentux.net](https://www.frozentux.net/documents/ipsysctl-tutorial/)
* [Clock Skew Fingerprinting (fingerprinting-by-tcp-timestamps.pdf) | Anonymous-Proxy-Servers.net](https://anonymous-proxy-servers.net/paper/fingerprinting-by-tcp-timestamps.pdf)
* [Nmap | nmap.org](http://nmap.org/) and for Android exclusive there is an [anmap app](http://code.google.com/p/anmap/) and the [Nmap binary](https://github.com/kost/nmap-android)
* [Nmap/Android quick guide | Secwiki.org](https://secwiki.org/w/Nmap/Android)
* [DNScrypt build instructions + binaries | forum-xda.developers.com](http://forum.xda-developers.com/showpost.php?p=56068030&postcount=20)
* [Tcpcrypt | Tcpcrypt.org](http://www.tcpcrypt.org)
* [privacytools.io | privacytools.io](https://www.privacytools.io/)
* [Firefox list of TCP ports open on 127.0.0.1 (HTML5 based) | AndLabs.orp Port-Scanner](http://www.andlabs.org/tools/jsrecon.html)
* [Sam Bowne and Students about SSL MITM holes on Android | SamClass.info](https://samsclass.info/128/proj/popular-ssl.htm)
* [Google Analytics Opt-out Browser Add-on | Tools.google.com](http://tools.google.com/dlpage/gaoptout)
* ["Google Analytics Opt Out? Not really." | Unrest.ca](http://www.unrest.ca/Net-Neutrality-and-The-Internet/google-analytics-opt-out-not-really)
* [Google Analytics talk | AdBlockPlus.org](https://adblockplus.org/forum/viewtopic.php?p=27886#p27886)
* [Detecting Certificate Authority compromises and web browser collusion | TorProject Blog.org](https://blog.torproject.org/blog/detecting-certificate-authority-compromises-and-web-browser-collusion)
* [Tutorials by the hidden wiki | TheHiddenWiki](https://ev3h5yxkjz4hin75.torstorm.org/wiki/index.php/Tutorials)