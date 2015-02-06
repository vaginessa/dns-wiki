Index
-----

* [Introduction](#introduction)
* [Requirements](#requirements)
* [Useful links](#useful-links)

Introduction
-----------

This guide has nothing todo with AFWall+ itself but it can help to protect against known problems and attacks (e.g. DOS/UDP flood) and are tested with Linux kernels 2.2 - 2.6 (and _higher?!_), so that's the reason why it's written down here (security everywhere!). These _tweaks_ are based on the articles (designed for a faster broadband) that you can find on the bottom under the [useful links](https://github.com/ukanth/afwall/wiki/TCP-security#useful-links) category. 

Please make a **backup first**, and of course there is **no support** or **guarantee that it works on your system**. If you unsure, simply don't use it, ask your ROM/Kernel developer if it's useful to integrate/use it.

You can use it directly via ADB/Terminal Emulator (busybox sysctl -e -w [_your_param_] script or edit the _/proc/sys/net/ipv4/[param_script]_ directly and native under _system/etc/sysctl.conf_ (same like e.g. FreeBSD) with e.g. EsExplorer.

Requirements
------------
* Time to read the articles first & to backup (show current settings via _#sysctl -a_) your existent configuration
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

Then to have the settings take effect immediately, run:
<code>#sysctl -p</code>

I should point out that there are many other options/settings that are available in _/proc/sys/net_, some of  which are not there unless you compiled them into your kernel (all the ones I mentioned above “should” be in most distro’s stock kernel). I only went over, and set, the ones that have a direct impact on broadband performance–and left out some other settings that can improve security, but at the _cost of speed_.

Useful links
------------

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