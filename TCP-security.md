Index
-----

* [Introduction](#introduction)
* [Known attacks](known-attacks)
* [Requirements](#requirements)
* [sysctl](#sysctl)
* [IP Rules](#ip-rules)
* [Security tools](#security-tools)
* [IPSec](#ipset)
* [Tcpcrypt](#tcpcrypt)
* [DNS](#dns)
* [How do I know if my applications are leaking DNS?](#how-do-i-know-if-my-applications-are-leaking-dns?)
* [Fingerprinting](#fingerprinting)
* [External Links](#external-links)

Introduction
-----------

This guide has nothing much todo with AFWall+ itself or it's own configuration, but it can help to protect against known _problems_ and _attacks_ like DNS/DOS/UDP flooding - and they're well tested with Linux Kernels 2.6 up to 4.0, so that's the reason why it's written down here (security everywhere!). These _tweaks_ are based on the articles (designed for a faster broadband) that you can find on the bottom under the ['External Links'](https://github.com/ukanth/afwall/wiki/TCP-security#useful-links) category. 

Please make a **backup first**, and of course there is **no support** or **guarantee that it works on your system**. If you unsure, simply don't use it, ask your ROM/Kernel developer if it's useful to integrate/use it.

You can use it directly via ADB/Terminal Emulator (busybox sysctl -e -w [_your_param_] script or edit the _/proc/sys/net/ipv4/[param_script]_ directly and native under _system/etc/sysctl.conf_ (same like e.g. FreeBSD) with e.g. EsExplorer.

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
* Replay attacks / Dsniff attacks
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

As described over [here](http://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml) the following stuff should be blacklisted by default for any vulnerability scanner (and only for them). These is necessary if you scan your network security against known port scanners like _Nmap_ to get an better result, since it may can display stuff which more confuse you as this shows useful stuff. 

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

Security tools
------------

The term 'security' tools are a bit misleading, the tools doesn't secure anything by itself, but they can display open ports, possible vulnerability's and other threads. Often there are called 'scanners'/'network intrusion detection'/'preventation' or penetration testing tools. 

There are mostly all **for advances users**, because it need a lot of time, knowledge and tests to really understand them, but since most of them are well documented it shouldn't be a problem to crow from a beginner to an expert. 

* [Whireshark](http://www.wireshark.org/)
* [Burp Suite](http://portswigger.net/burp/)
* [Metasploit](http://www.metasploit.com/)
* [Netcat](http://en.wikipedia.org/wiki/Netcat)
* [tcpdump](http://www.tcpdump.org/) - sometimes already shipped with Android as standalone binary
* [Kismet](http://www.kismetwireless.net/)
* [Nikto](http://www.cirt.net/nikto2)
* [Hping](http://www.hping.org/)
* [Ettercap](http://ettercap.sourceforge.net/)
* [Sysinternals](http://technet.microsoft.com/en-us/sysinternals/default.aspx)
* [w3af](http://w3af.sourceforge.net/)
* [OpenVAS](http://www.openvas.org/)
* [Scapy](http://www.secdev.org/projects/scapy/)
* Ping/telnet/dig/traceroute/whois/netstat sometimes already shipped with Android/Windows/Unix/Linux/Mac OS as standalone binaries (on some system the names are different but they do all the same)
* [Paros Proxy](http://www.parosproxy.org/)
* [Maltego](http://www.paterva.com/web4/index.php/maltego)
* [The Sleuth Kit](http://www.sleuthkit.org/sleuthkit/)
* [EnCase](http://www.guidancesoftware.com/)
* [Unicornscan](http://www.unicornscan.org/)
* [SAINT](http://www.saintcorporation.com/saint/)
* [Core Impact](http://www.coresecurity.com/content/core-impact-overview)


Some other free bootable LIVE CD's like Kali, Tails, Helix,[...] are Linux distributions that claiming to be more secure and hardened against the known attacks compared to other systems like on Windows (general there is no proof except there words since nobody really can compare Windows <-> Linux on a seriously way). They often already included a huge collection of scanning tools. It's always worth to keep on eye on this, since you don't even need to installing them. 

IPSec
------------

Internet Security (IPsec) was builded as a result of the current IP weaknesses - basically it's a own internet layer which claims to be more secure compared to e.g. IPv4 - in fact it was developed around IPv6 (from Internet Engineering Task Force [IETF]), but was engineered to provide security for both IPv4/IPv6. There are some differences in the datagram formats used for AH and ESP depending on whether IPSec is used in IPv4 and IPv6, since the two versions have different datagram formats and addressing. I highlight these differences where appropriate.

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

DNS
------------

DNS is known as  not secure anymore for several reasons. Like most "secure" communications protocols, it is susceptible to undetectable public-key substitution MITM-attacks an populate examples was the [Apple iMessages security problem](https://www.taoeffect.com/blog/2014/11/update-on-imessages-security/). 

The main problem is the protocol insecurity by [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) and [X.509](https://en.wikipedia.org/wiki/X.509). See also [Certificate Transparency](https://blog.okturtles.com/2015/03/certificate-transparency-on-blockchains/).

The core problems are always:
* [MITM](http://en.wikipedia.org/wiki/Man-in-the-middle_attack) (Man-In-The-Middle)
* DNS-based censorship circumvention
* [Domain theft's](https://www.techdirt.com/articles/20141006/02561228743/5000-domains-seized-based-sealed-court-filing-confused-domain-owners-have-no-idea-why.shtml) ("seizures")
* Certificate revocation
* DOS (Denial-Of-Service) attacks

Blocking generally DNS isn't possible since this is needed on Android/Windows/Linux/Mac OS or any other OS, but we simply can use secure and proofed alternatives. - Which is more or less less/more complicated and depending on your knowledge about how to change that. 

There are alternatives like:
* DNSSEC
* OpenDNS (or other providers which claiming to not log/censorship anything)
* DNSCrypt / _TCPCrypt_
* [TACK](https://lwn.net/Articles/499134/) / [HPKP](https://developer.mozilla.org/en-US/docs/Web/Security/Public_Key_Pinning)
* [Perspective](http://perspectives-project.org/) + available as [Firefox addon](http://perspectives-project.org/firefox/) - Please read the _Known Issues_ on the page before installing it!

But even with such popular alternatives there are several problems, e.g. DNSSEC suffering from miscellaneous leaks mentioned over [here](http://ianix.com/pub/dnssec-outages.html) or [here](http://sockpuppet.org/blog/2015/01/15/against-dnssec/) + it [doesn't prevent MITM attacks](http://www.thoughtcrime.org/blog/ssl-and-the-future-of-authenticity/).

ToDo:
- Find an easier solution especially on Android since no app or config respect both Firefox mobile AND the OS. Sad but true, to install just another app + addon shouldn't be always the goal since that always ends up with possible conflicts or security problems (such as mime type detection on some security scanning sites like panopticklick,...). 


How do I know if my applications are leaking DNS?
------------

A very detailed answer what DNS (Domain Name System) is can be found over [here](https://trac.torproject.org/projects/tor/wiki/doc/DnsResolver).

There are several ways, the most easiest way is to visit some webpages that automatically detect what is your current DNS, like:
* [DNS Leak Test | dnsleaktest.com](https://www.dnsleaktest.com/)
* [IP Leak Test | ipleak.net](http://ipleak.net/)

If you Browser shows a wrong DNS according to what your own settings telling you, this usually means something is wrong or maybe compromised.

Per-Browser this must be set to get a correct behavior, because they using there own DNS internal settings:
* On Firefox / Firefox Mobile (about:config): _network.dns.disablePrefetch_ needs to be set to _true_.


On the OS level you can:
* Use 3rd Party Local DNS Servers/Resolvers, [here](https://trac.torproject.org/projects/tor/wiki/doc/DnsResolver#Local_DNS_Resolvers).
* Apply Windows Tweak and Registry Hacks, [here](https://trac.torproject.org/projects/tor/wiki/doc/DnsResolver#Tweak_Windows) - on non servers 4 hours is enouth.
* Apply MacOS Tweaks, [here](https://trac.torproject.org/projects/tor/wiki/doc/DnsResolver#Tweak_MacOS).
* Configure Firewall as Fail-safe To Prevent Leaks, [here](https://trac.torproject.org/projects/tor/wiki/doc/DnsResolver#Tweak_Firewalls)
* Generally use [secure alternatives](https://github.com/okTurtles/dnschain)

Sometimes these messages may be false alarms. To find out, you should run a packet sniffer on your network interface. The basic command to do this is <code>tcpdump -pni eth0 'port domain'</code>.

If you are using an VPN this also can "fix" the DNS problem, but sometimes even this isn't enouth, especially on Android and OpenVPN, some older versions and provider still suffering from this issue, a workaround can be found over [here](https://gist.github.com/CHEF-KOCH/52fe5cd9a5aa7721fe74).

Another possible problem is that you [ISP](http://en.wikipedia.org/wiki/Internet_service_provider) [mitm](http://en.wikipedia.org/wiki/Man-in-the-middle_attack) and manipulate the DNS traffic (mostly due censorship or to spoof)! There are only a few methods to bypass this:
* Use [TOR](https://www.torproject.org/) + setup it (simply use the setup wizard if you don't know how)
* Use a SSH tunnel
* Choose an VPN which doesn't censorship
* Use [JonDo](https://anonymous-proxy-servers.net/en/jondo.html) (the proxy) [but browser is also good]
* Use [DNSCrypt](https://www.opendns.com/about/innovations/dnscrypt/) or httpsdnsd (HTTPSDNS daemon is already running if you use JonDo)
* For general implementation info about DNS Transport over TCP take a look at [here](https://www.ietf.org/rfc/rfc5966.txt)

There are also several tips, tricks and guides directly with a lot of examples over the official Tor Wiki page, see [here](https://trac.torproject.org/projects/tor/wiki/doc/DnsResolver) & [here](https://trac.torproject.org/projects/tor/wiki/doc/Preventing_Tor_DNS_Leaks). Remember that the given tricks on this pages are optimized for TOR/I2P, so you may need to adjust some example configuration given from there.

Fingerprinting
------------

AFWall+ or any other Firewall does not protect against the following fingerprint detection mechanism:
* JavaScript link rewriting  / JavaScript Performance Fingerprinting 
* Window Name detection (DCOM identifier storage)
* Headers (referrer, HTTP, agent,... + Timestamps + Timezone leaks)
* LXC-specific leaks
* Operating System Type Fingerprinting
* Keystroke Fingerprinting
* Super Cookies (Master/Ever Cookies/HSTS super-cookies/Flash) [this is a result because of third-party cookies blocking]
* WebGL / WebRTC
* Display Media information
* Monitor, Widget, and OS Desktop Resolution + Fonts 
* USB Device ID Enumeration
* Invasive Authentication Mechanisms
* Open TCP Port Fingerprinting 
* HTML5 Canvas Image Extraction
* Flash (Since Android 4.x removed official in Android -> HTML5)
* Plugins intended to be good, but leaking sensitive data (meta,big/small,...) 
* SSL/TLS session resumption, HTTP Keep-Alive and SPDY
* DOM Storage and Auth
* Several bypass xyz settings (like Proxy/VPN/physically access/records,...)

A full overview can be found over [here](https://www.torproject.org/projects/torbrowser/design/#fingerprinting-linkability). A fingerprint test can be found over [here](https://panopticlick.eff.org/about.php).

It's highly recommend to use Tor/Orbot + NoScript (NSA NoScript) + Ref Control and Random Agent Spoofer.
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