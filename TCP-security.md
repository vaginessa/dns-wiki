Index
-----

* [Introduction](#introduction)
* [Known attacks](known-attacks)
* [Requirements](#requirements)
* [IP Rules](#ip-rules)
* [Useful links](#useful-links)

Introduction
-----------

This guide has nothing much todo with AFWall+ itself but it can help to protect against known _problems_ and attacks (e.g. DOS/UDP flooding) and are tested with Linux kernels 2-2 up to 3.2, so that's the reason why it's written down here (security everywhere!). These _tweaks_ are based on the articles (designed for a faster broadband) that you can find on the bottom under the [useful links](https://github.com/ukanth/afwall/wiki/TCP-security#useful-links) category. 

Please make a **backup first**, and of course there is **no support** or **guarantee that it works on your system**. If you unsure, simply don't use it, ask your ROM/Kernel developer if it's useful to integrate/use it.

You can use it directly via ADB/Terminal Emulator (busybox sysctl -e -w [_your_param_] script or edit the _/proc/sys/net/ipv4/[param_script]_ directly and native under _system/etc/sysctl.conf_ (same like e.g. FreeBSD) with e.g. EsExplorer.

Known attacks
------------

* Desynchronization during connection establishment
* Desynchronization in the middle of a connection
* ICMP attacks
* Nonblind Spoofing
* Routing (RIP) attacks
* DNS attacks such DNS Poisoning and BIND attacks
* Hijacking/Injection of the connection
* Sequence Guessing
* Dsniff attacks
* Replay attacks
* LAN Sniffing (general Packet's Sniffing)
* Denial of Service (DoS)
* Unique Identifiers attacks
* TCP SYN attacks (flooding)
* IP spoofing
* Smurf attacks
* Man-in-the-Middle Attacks
* .... Authentication and encryption problems due lack of protocol itself.

A very good explanation of all the attacks listed above are on [Peters Smith fantastic article](http://linuxbox.co.uk/linux-network-security/) about common Linux security.

Requirements
------------

As described over [here](http://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml) this should be blacklisted by default for any vulnerability scanner (and only for them).

| Address | RFC | Description |
| :--- | :--: | ---: |
| 0.0.0.0/8 | [RFC1122](https://tools.ietf.org/html/rfc1122) | Host Network |
| 10.0.0.0/8 | [RFC1918](https://tools.ietf.org/html/rfc1918) | Private-Use |
| 100.64.0.0/10 | [RFC6598](https://tools.ietf.org/html/rfc6598)| Shared Address Space |
| 127.0.0.0/8 | [RFC1122](https://tools.ietf.org/html/rfc1122) | Loopback|
| 169.254.0.0/16 | [RFC3927](https://tools.ietf.org/html/rfc3927) | Link Local |
| 172.16.0.0/12 | [RFC1918](https://tools.ietf.org/html/rfc1918) | Private-Use |
| 192.0.0.0/24 | [RFC6890](https://tools.ietf.org/html/rfc6890) | IETF Protocol Assignments |
| 192.0.2.0/24 | [RFC5737](https://tools.ietf.org/html/rfc5737) | Documentation (TEST-NET-1) |
| 192.88.99.0/24 | [RFC3068](https://tools.ietf.org/html/rfc3068) | 6to4 Relay Anycast |
| 192.168.0.0/16 | [RFC1918](https://tools.ietf.org/html/rfc1918) | Private-Use |
| 198.18.0.0/15 | [RFC2544](https://tools.ietf.org/html/rfc2544) | Benchmarking (nmap) |
| 198.51.100.0/24 | [RFC5737](https://tools.ietf.org/html/rfc5737) | ? |
| 203.0.113.0/24 | [RFC5737](https://tools.ietf.org/html/rfc5737) | ? |
| 240.0.0.0/4 | [RFC1112](https://tools.ietf.org/html/rfc1112) | Reserved |
| 255.255.255.255/32 | [RFC0919](https://tools.ietf.org/html/rfc0919) | Limited Broadcast |

For [Multicast](http://www.iana.org/assignments/multicast-addresses/multicast-addresses.xhtml):

| Address | RFC | Description |
| :--- | :--: | ---: |
| 224.0.0.0/4 | [RFC5771](https://tools.ietf.org/html/rfc5771) | Multicast/Reserved |

* Time to read the articles + to backup (show current settings via _#sysctl -a_) your existent configuration (just in case something goes wrong - shit happens!) 
* ADB/Terminal Emulator/Busybox & a **rooted device**
* Kernel/ROM that support these kind of tweaks (if not it doesn't work or is useless)
* init.d support if you like to store these tweaks into a .sh script and apply them at boot (once)

| Category| Value to be set | Description |
| :--- | :--: | :---: |
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.tcp_syncookies=1 | Enable TCP SYN Cookie Protection |
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.tcp_synack_retries=2 | Tells the kernel how many times to try to retransmit the initial SYN packet for an active TCP connection attempt |
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.tcp_syn_retries=2 |  |
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.tcp_max_syn_backlog=1280 | TCP syn half-opened |
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.tcp_max_tw_buckets=16384 | Bump up tw_buckets in case we get DoS'd|
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.icmp_echo_ignore_all=1 | Ignore Directed pings |
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.icmp_echo_ignore_broadcasts=1 | Don't reply to broadcasts (prevents joining a smurf attack) |
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.icmp_ignore_bogus_error_responses=1 | Enable bad error message protection (should be enabled by default)|
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.tcp_no_metrics_save=1 | Don't cache connection metrics from previous connection|
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.tcp_fin_timeout=15 |  |
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.tcp_keepalive_intvl=30 |  |
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.tcp_keepalive_probes=5 |  |
| Hardening the TCP/IP stack to SYN attacks | net.ipv4.tcp_keepalive_time=1800|  |
| Don't pass traffic between networks or act as a router | net.ipv4.ip_forward=0 | Disable IP Packet forwarding (default disabled)|
| Don't pass traffic between networks or act as a router | net.ipv4.conf.all.send_redirects=0| No ICMP redirects |
| Don't pass traffic between networks or act as a router | net.ipv4.conf.default.send_redirects=0 |  |
| IP Spoofing protection | net.ipv4.conf.all.rp_filter=1 |  |
| IP Spoofing protection | net.ipv4.conf.default.rp_filter=1 |  |
| Enable spoofing protection (turn on reverse packet filtering) | net.ipv4.conf.all.log_martians=1 | Filter martians |
| Enable spoofing protection (turn on reverse packet filtering) | net.ipv4.conf.all.forwarding=1 | Forwarding traffic |
| Don't accept source routing | net.ipv4.conf.all.accept_source_route=0 | Source routing disable |
| Don't accept source routing | net.ipv4.conf.default.accept_source_route=0 |  |
| Don't accept redirects | net.ipv4.conf.all.accept_redirects=0 | Ignore ICMP redirects ipv4 |
| Don't accept redirects | net.ipv6.conf.all.accept_redirects=0 | Ignore ICMP redirects ipv6 |
| Don't accept redirects | net.ipv4.conf.default.accept_redirects=0 | Ignore ICMP redirects |
| Don't accept redirects | net.ipv4.conf.all.secure_redirects=0 | Ignore ICMP redirects |
| Don't accept redirects | net.ipv6.conf.all.secure_redirects=0 | Ignore ICMP redirects |
| Don't accept redirects | net.ipv4.conf.default.secure_redirects=0 | Ignore ICMP redirects |
| Don't accept redirects | net.ipv6.conf.default.accept_redirects=0 | Ignore ICMP redirects |
| Re-use sockets in time-wait state | net.ipv4.tcp_tw_recycle=1 |  |
| Re-use sockets in time-wait state | net.ipv4.tcp_tw_reuse=1 |  |
| Queue size modifications | net.core.wmem_max=1048576 |  |
| Queue size modifications | net.core.rmem_max=1048576 |  |
| Queue size modifications | net.core.rmem_default=262144 | Be careful, better leave device default! |
| Queue size modifications | net.core.wmem_default=262144 | Be careful, better leave device default! |
| Queue size modifications | net.core.optmem_max=20480 |  |
| Queue size modifications | net.unix.max_dgram_qlen=50 |  |
| Misc | net.ipv4.tcp_congestion_control=cubic | Change network congestion algorithm to CUBIC |
| Misc | net.ipv4.tcp_moderate_rcvbuf=1 |  |
| Misc | net.ipv4.route.flush=1 |  |
| Misc | net.ipv4.udp_rmem_min=6144 |  |
| Misc | net.ipv4.udp_wmem_min=6144 |  |
| Misc | net.ipv4.tcp_rfc1337=1 |  |
| Misc | net.ipv4.ip_no_pmtu_disc=0 |  |
| Misc | net.ipv4.tcp_ecn=0 |  |
| Misc | net.ipv4.tcp_rmem='6144 87380 1048576' |  |
| Misc | net.ipv4.tcp_wmem='6144 87380 1048576' |  |
| Misc | net.ipv4.tcp_timestamps=0 |  |
| Misc | net.ipv4.tcp_sack=1 |  |
| Misc | net.ipv4.tcp_fack=1 |  |
| Misc | net.ipv4.tcp_window_scaling=1 | Turn on the tcp_window_scaling |

I should point out that there are many other options/settings that are available in _/proc/sys/net_, some of  which are not there unless you compiled them into your kernel (all the ones I mentioned above “should” be in most distro’s stock kernel). I only went over, and set, the ones that have a direct impact on broadband performance–and left out some other settings that can improve security, but at the _cost of speed_.

IP Rules
------------
Most of the mentioned port's are already supported by AFWall+ and can be controlled directly with the interface (to allow/reject them) but these are normally open (and should be open to avoiding some troubles):

| Rule Name | Status | Range | Protocol | Remote Port | Local Port |
| :--- | :--: | :---: | :--: | :---: | :--: |
| Allow DNS | Allow | All IP packets | UDP | 53 | Any Port |
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

* Depending on which configuration you're use

-> AFWall+ doesn't currently supports ESP, IKE, L2TP, AH and GRE protocols via a gui switch but you can control this via custom scripts. 
-> Depending which kernel you use and which configuration it was compiled with, you will see or not see with external tools and commands traffic like IGMP, but you don't need to worry about such stuff. On some configuration it's really necessary, like on [IPv6 systems](https://github.com/ukanth/afwall/wiki/CustomScripts#important-notes-about-ipv4-and-ipv6-differences). 
-> It's not worth to touch any of the mentioned port, because it doesn't have any notable effect (also no security plus). There are rare situations, mostly only on servers which are compromised by brute force and other attacks (Port 22) but a normal Android client is mostly not affected by this and remember that the port will be automatically closed after the connection is lost/dropped, [it's also a bad idea to put such port to another place](https://www.adayinthelifeof.nl/2012/03/12/why-putting-ssh-on-another-port-than-22-is-bad-idea/). Generally in AFWall+ the whole inbound traffic is by default disabled, means you need to enable the "Allow inbound" option first.

Useful links
------------

* [TCP vs UDP by Erik Rodriguez | Skullbox.net](http://www.skullbox.net/tcpudp.php)
* [/proc file system | Wikipedia.org](http://en.wikipedia.org/wiki/Procfs)
* [Ieft.org](http://www.ietf.org/)
* [Linux tweaking | SpeedGuide.net](http://www.speedguide.net/articles/linux-tweaking-121)
* [Linux TCP Tune | PSC.edu ](http://www.psc.edu/networking/projects/tcptune/#Linux)
* [Linux TCP Tuning | Cyberciti.biz](http://www.cyberciti.biz/faq/linux-tcp-tuning)
* [IP sysctl documentation (.txt file) | Cyberciti.biz](http://www.cyberciti.biz/files/linux-kernel/Documentation/networking/ip-sysctl.txt)
* [Linux Kernel etcsysctl conf security hardening | Cyberciti.biz](http://www.cyberciti.biz/faq/linux-kernel-etcsysctl-conf-security-hardening)
* [Hardening TCP/IP Stack Syn Attacks | Symantec.com](http://www.symantec.com/connect/articles/hardening-tcpip-stack-syn-attacks)
* [Whitepaper - Android Security Hardening (33799.pdf file) | Sans.org](http://www.sans.org/reading-room/whitepapers/sysadmin/securely-deploying-android-devices-33799)
* [Ipsysctl-tutorial | Frozentux.net](https://www.frozentux.net/documents/ipsysctl-tutorial/)
* [Clock Skew Fingerprinting (fingerprinting-by-tcp-timestamps.pdf) | Anonymous-Proxy-Servers.net](https://anonymous-proxy-servers.net/paper/fingerprinting-by-tcp-timestamps.pdf)