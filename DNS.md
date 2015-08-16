Index
-----

* [Description](#description)
* [DNS under Android](#dns-under-android)
* [resolv.conf](#resolv.conf)
* [dnsmasq](#dnsmasq)
* [DNSCrypt](#dnscrypt)
* [DNS problems](#dns-problems)
* [How can I gather DNS (A/AAA/...) requests?](#how-can-i-gather-dns--(-a-/-aaa-/-...)-requests-?)
* [How do I know if my applications are leaking DNS?](#how-do-i-know-if-my-applications-are-leaking-dns-?)
* [Resolver commands](#resolver-commands)
* [Changing the default DNS](#changing-the-default-dns)
* [Commands to check if DNS is working](#commands-to-check-if-dns-is-working)
* [Browser](#browser)
* [Apps to change the default DNS](#apps-to-change-the-default-dns)
* [Useful links](#useful links)

Description
-----------

[DNS](https://en.wikipedia.org/wiki/Domain_Name_System) primarily uses User Datagram Protocol (UDP) on port number 53 to serve the requests, TCP (Transmission Control Protocol) is only in usage if data size exceeds >512 bytes or for special tasks, some resolvers implementing an TCP for all queries option (mostly optional by default).

DNS is known as not to be secure anymore for several reasons. Like most "secure" communications protocols, it is susceptible to undetectable public-key substitution MITM-attacks an populate examples was the [Apple iMessages security problem](https://www.taoeffect.com/blog/2014/11/update-on-imessages-security/). 

The main problem is the protocol insecurity by [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) and [X.509](https://en.wikipedia.org/wiki/X.509). See also [Certificate Transparency](https://blog.okturtles.com/2015/03/certificate-transparency-on-blockchains/).

The core problems are:
* [MITM](http://en.wikipedia.org/wiki/Man-in-the-middle_attack) (Man-In-The-Middle)
* DNS-based censorship circumvention
* [Domain theft's](https://www.techdirt.com/articles/20141006/02561228743/5000-domains-seized-based-sealed-court-filing-confused-domain-owners-have-no-idea-why.shtml) ("seizures")
* Certificate revocation
* DOS (Denial-Of-Service) attacks, even DNSSEC doesn't protect against this
* Logging on exit notes
* DNS cache poisoning
* Other exploits that are used for e.g. phishing
* DNS hijacking
* ...

Blocking the DNS (port 53) isn't possible (without problems) since this is necessary on Android/Windows/Linux/Mac OS or any other OS, but we simply can use secure and proofed alternatives. - Which is more or less complicated and depending on your knowledge about how to change that. But don't worry we are here to explain all stuff in easy steps!

There are alternatives like:
* Choosing a logging free DNS resolver e.g [OpenNIC](https://www.opennicproject.org/) 
* [DNSSEC](https://en.wikipedia.org/wiki/Domain_Name_System_Security_Extensions) / DNS-based Authentication of Named Entities ([DANE](https://en.wikipedia.org/wiki/DNS-based_Authentication_of_Named_Entities))
* OpenDNS (or other providers which claiming to not log/censorship anything)
* [DNSCrypt](http://dnscrypt.org/) / _TCPCrypt_ (DNSCrypt will be a part of the final [CM 12.1](http://review.cyanogenmod.org/#/q/status:open+project:CyanogenMod/android_external_dnscrypt_dnscrypt-proxy+branch:cm-12.1+topic:dnscrypt))
* [TACK](https://lwn.net/Articles/499134/) / [HPKP](https://developer.mozilla.org/en-US/docs/Web/Security/Public_Key_Pinning) implementations within the apps 

But even with such popular alternatives there are several problems, e.g. DNSSEC suffering from miscellaneous leaks mentioned over [here](http://ianix.com/pub/dnssec-outages.html) or [here](http://sockpuppet.org/blog/2015/01/15/against-dnssec/) + it [doesn't prevent MITM attacks](http://www.thoughtcrime.org/blog/ssl-and-the-future-of-authenticity/).


DNS under Android
-----------

Android use a a dynamic DNS ([DDNS](https://en.wikipedia.org/wiki/Dynamic_DNS)) by default, which updates the DNS every time when the IP address changes ISP <-> wifi.

By default the Google DNS server (since 2009) is set (8.8.8.8/8.8.4.4), currently the DNS servers gets overridden after each reboot even with [setprop](http://android.git.kernel.org/?p=platform/frameworks/base.git;a=object;h=6d626d41e9db62a0eadb61ccb2aa4081a8b9f6d0). Googles public DNS supports DNSSEC validation since 2013 by default unless you do not want this (via opt-out), which means that this server is secured and nothing speaks against using it (except the anti-Google paranoia of course).

// IP change or reconnection (connectivity changes -> RIL via e.g. ndc resolver setifdns rmnet0 x.x.x.x y.y.y.y). 

How can I gather DNS (A/AAA/...) requests?
-----------

<s>All AOSP based ROMs coming with TCPdump as binary included, so we can just use this to show what's going on behind, there are several [Tutorials](http://www.kandroid.org/online-pdk/guide/tcpdump.html) and [documents](http://inst.eecs.berkeley.edu/~ee122/fa06/projects/tcpdump-2up.pdf) available to handling TCPdump. If this is to complicated for you, you can just grab AdAway (needs root) and use there own TCPDump/dnsmasq/libpcap interface to list all requests - it also provides an interface to add them to your hosts or to an separate white-/blacklist.</s>

// Android use a kind of BIND (which includes "dig"). <code>dig github.com | grep "Query time"</code>
// See, [#37668](https://code.google.com/p/android/issues/detail?id=37668)
// Workaround, just use external apps like [DNS Lookup](https://play.google.com/store/apps/details?id=com.kodholken.dnslookup)


resolv.conf
-----------

The configuration file for DNS resolvers is <code>/etc/resolv.conf</code> take a look at the [man page](http://www.kernel.org/doc/man-pages/online/pages/man5/resolv.conf.5.html). 

Allows a maximum of three nameserver lines. In order to overcome this limitation, you can use a locally caching nameserver like dnsmasq (see below). Changes made to /etc/resolv.conf take effect immediately (needs confirmation?). 


dnsmasq
-----------

Dnsmasq provides services as a DNS forwarder cacher and a DHCP server. As a Domain Name Server (DNS) it can cache DNS queries to improve connection speeds to previously visited sites, and as a DHCP server dnsmasq can be used to provide internal IP addresses and routes to computers on a LAN. 

:exclamation: dnsmasq only allows three nameservers as a workaround we can create <code>resolv.dnsmasq.conf</code> and add this list into our <code>/etc/dnsmasq.conf</code> -> _resolv-file=/etc/resolv.dnsmasq.conf_ 


// listen-address=127.0.0.1 needs to be set for local DNS cache ... because DNSCrypt isn't an DNS Cache (so we use dnsmasq)
// Empty example [dnsmasq](https://github.com/Android-Apps/dnsmasq/blob/master/dnsmasq.conf.example)
// start it via service dhcpsrv  /system/bin/logwrapper  /system/bin/dnsmasq -k -C /system/etc/dnsmasq.conf


DNSCrypt
-----------

[DNSCrypt](http://dnscrypt.org/) encrypts and authenticates DNS traffic between user and DNS resolver, the latest flashable .zip for Android is available over [here](https://download.dnscrypt.org/dnscrypt-proxy/). When DNSCrypt is enabled, _dnscrypt-proxy_ accepts incoming requests on <code>127.0.0.1:53</code> to one chosen OpenDNS resolver. Compatible resolver names are visible under <code>/etc/dnscrypt-proxy/dnscrypt-resolvers.csv</code> (always latest [dnscrypt-resolvers.csv](https://download.dnscrypt.org/dnscrypt-proxy/dnscrypt-resolvers.csv)).

// Unbound, dnsmasq (Android default) or pdnsd are working with DNSCrypt but may needs configuration changes to work proper together 

```
// Ensure that DNSCrypt itself is running
ps w | grep dnscrypt
dnscrypt enable  #Enable dnscrypt
dnscrypt disable #Disable dnscrypt
```

```
// An example could be looks like this, the .zip package normally contains an init.d but this is just in case
# DNSCrypt copy & paste example config for init.d
dnscrypt-proxy \
  --resolver-name=<INSERT-YOUR-DNS-RESOLVER-NAME-HER> \
  --resolvers-list=/system/etc/dnscrypt-proxy/dnscrypt-resolvers.csv \
  --provider-key=<OPTIONAL-ONLY-NESSARY-IF YOU-ARE-NOT-USING-ONE-FROM-the-resolvers.csv-list!> \
  --provider-name=<OPTIONAL-ONLY-NESSARY-IF YOU-ARE-NOT-USING-ONE-FROM-the-resolvers.csv-list!>  \
  --resolver-address=<OPTIONAL-IF-THIS-is-different!> \
  --max-active-requests=150 \
  --ephemeral-keys \ 
  --edns-payload-size=4096 \
  --test=3600 \
  --daemonize \
  --loglevel=3

// All other command-line switches can be found on the official homepage and of course within the documents. 
```

How do I know if my applications are leaking DNS?
-----------

There are several ways, the most easiest way is to visit some web pages that automatically detect what is your current DNS, like:
* [DNS Leak Test | dnsleaktest.com](https://www.dnsleaktest.com/)
* [IP Leak Test | ipleak.net](http://ipleak.net/)

If you Browser shows a wrong DNS according to what your own settings telling you, this usually means something is wrong or maybe compromised.

Per-Browser this must be set to get a correct behavior, because they using there own DNS internal settings:
* On Firefox Mobile (about:config): _network.dns.disablePrefetch_ needs to be set to _true_.


On the OS level:
* Use 3rd Party Local DNS Servers/Resolvers, [here](https://trac.torproject.org/projects/tor/wiki/doc/DnsResolver#Local_DNS_Resolvers).
* Apply Windows Tweak and Registry Hacks, [here](https://trac.torproject.org/projects/tor/wiki/doc/DnsResolver#Tweak_Windows) - on non servers max 4 hours is enough.
* Apply MacOS Tweaks, [here](https://trac.torproject.org/projects/tor/wiki/doc/DnsResolver#Tweak_MacOS).
* Configure Firewall as Fail-safe To Prevent Leaks, [here](https://trac.torproject.org/projects/tor/wiki/doc/DnsResolver#Tweak_Firewalls)
* Generally use [secure alternatives](https://github.com/okTurtles/dnschain) + use a [online browser check](https://www.botfrei.de/en/index.html)
* Use always the latest software to ensure possible bugs and security holes are fixed tools like sumo
* Use a secure, user/privacy friendly search engine like DuckDuck, Disconnect or Ixquick. Even better would be an decentralized search like YaCy, FAROO or any other based on P2P/...
* Verify no external addons/software/app leaks something (speaking about external metadata and protocol headers from going in .plaintxt out!)

On Tor:
* Read the _Advanced Tor usage_ FAQ section, the two important ones are [this](https://www.torproject.org/docs/faq.html.en#WarningsAboutSOCKSandDNSInformationLeaks) and [this](https://www.torproject.org/docs/faq.html.en#SocksAndDNS)

Sometimes these messages may be false alarms. To find out, you should run a packet sniffer on your network interface. The basic command to do this is <code>tcpdump -pni eth0 'port domain'</code>.

If you are using an VPN this also can "fix" the DNS problem, but sometimes even this isn't enough, especially on Android and OpenVPN, some older versions and provider still suffering from this issue, a workaround can be found over [here](https://gist.github.com/CHEF-KOCH/52fe5cd9a5aa7721fe74).

Another possible problem is that you [ISP](http://en.wikipedia.org/wiki/Internet_service_provider) [mitm](http://en.wikipedia.org/wiki/Man-in-the-middle_attack) and manipulate the DNS traffic (mostly due censorship or to spoof)! There are only a few methods to bypass this:
* Use [TOR](https://www.torproject.org/) + setup it (simply use the setup wizard if you don't know how)
* Use a SSH tunnel
* Choose an VPN which doesn't censorship
* Use [JonDo](https://anonymous-proxy-servers.net/en/jondo.html) (the proxy) [but browser is also good]
* Use [DNSCrypt](https://www.opendns.com/about/innovations/dnscrypt/) or httpsdnsd (HTTPSDNS daemon is already running if you use JonDo)
* For general implementation info about DNS Transport over TCP take a look at [here](https://www.ietf.org/rfc/rfc5966.txt)

There are also several tips, tricks and guides directly with a lot of examples over the official Tor Wiki page, see [here](https://trac.torproject.org/projects/tor/wiki/doc/DnsResolver) & [here](https://trac.torproject.org/projects/tor/wiki/doc/Preventing_Tor_DNS_Leaks). Remember that the given tricks on this pages are optimized for TOR/I2P, so you may need to adjust some example configuration given from there.

DNS problems 
-----------

An easy method to look at opened or closed threads is to search via <code>is:issue is:open dns</code> / <code>is:issue is:closed dns</code> which shows the important threads (if the topic/thread content was correct labaled), alternative just click on the follow links (or copy/paste the issue number in the search e.g. <code>https://github.com/ukanth/afwall/issues/<insert-issue-number-here>.


Already reported DNS related topics:
* #377 [DNS (port 53) is blocked for Wifi tethering](https://github.com/ukanth/afwall/issues/377)
* #344 [Wi-Fi-Hotspot not working while AFWall+ is enabled](https://github.com/ukanth/afwall/issues/344)
* #326 [Add mDNS support for local networks](https://github.com/ukanth/afwall/issues/326)
* #318 [Show host names in log view](https://github.com/ukanth/afwall/issues/318)
* #257 [USB/Bluetooth Tethering fails on CM11 mako](https://github.com/ukanth/afwall/issues/257)
* #209 [Bluetooth tethering borked as well](https://github.com/ukanth/afwall/issues/209)
* #206 [DNS Requests fail](https://github.com/ukanth/afwall/issues/206)
* #178 [Tethering](https://github.com/ukanth/afwall/issues/178)
* #18 [UDP 53 bypass because logging & whitelisting are enabled](https://github.com/ukanth/afwall/issues/18)
* #511506 [Investigate horrible DNS performance on Android when running dualstack](http://code.google.com/p/chromium/issues/detail?id=511506)
* #79504 [DNS (local) resolution on Android Lollipop](https://code.google.com/p/android/issues/detail?id=79504)
* On AFWAll+ in whitelisting mode root/DNS+DHCP options needs to be checked to allow DNS traffic (netd runs as root that why you also need root checked too)

**Important**: Please always use the search function here on AFWall's/[AOSP issue tracker](https://code.google.com/p/android/issues/list?can=2&q=DNS&colspec=ID+Type+Status+Owner+Summary+Stars&cells=tiles) to search already known existent problems to avoid duplicate threads. 


Resolver commands
-----------

## Android 4.0.4
http://androidxref.com/4.0.4/xref/system/netd/CommandListener.cpp#778

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`

## Android 4.1.1
http://androidxref.com/4.1.1/xref/system/netd/CommandListener.cpp#803

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`

## Android 4.2_r1
http://androidxref.com/4.2_r1/xref/system/netd/CommandListener.cpp#873

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`

## Android 4.3_r2.1
http://androidxref.com/4.3_r2.1/xref/system/netd/CommandListener.cpp#770

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <domains> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`
* `ndc resolver setifaceforpid <iface> <pid>`
* `ndc resolver clearifaceforpid <pid>`

## Android 4.4_r1
http://androidxref.com/4.4_r1/xref/system/netd/CommandListener.cpp#941

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <domains> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`
* `ndc resolver setifaceforpid <iface> <pid>`
* `ndc resolver clearifaceforpid <pid>`
* `ndc resolver setifaceforuid <iface> <l> <h>`
* `ndc resolver clearifaceforuid <l> <h>`
* `ndc resolver clearifacemapping`

## Android 4.4.2_r1
http://androidxref.com/4.4.2_r1/xref/system/netd/CommandListener.cpp#942

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <domains> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`
* `ndc resolver setifaceforpid <iface> <pid>`
* `ndc resolver clearifaceforpid <pid>`
* `ndc resolver setifaceforuidrange <iface> <l> <h>`
* `ndc resolver clearifaceforuidrange <l> <h>`
* `ndc resolver clearifacemapping`

## Android 4.4.3_r1.1
http://androidxref.com/4.4.3_r1.1/xref/system/netd/CommandListener.cpp#940

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <domains> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`
* `ndc resolver setifaceforpid <iface> <pid>`
* `ndc resolver clearifaceforpid <pid>`
* `ndc resolver setifaceforuidrange <iface> <l> <h>`
* `ndc resolver clearifaceforuidrange <if> <l> <h>`
* `ndc resolver clearifacemapping`

## Android 4.4.4_r1
http://androidxref.com/4.4.4_r1/xref/system/netd/CommandListener.cpp#940

* `ndc resolver setdefaultif <iface>`
* `ndc resolver setifdns <iface> <domains> <dns1> <dns2> ...`
* `ndc resolver flushdefaultif`
* `ndc resolver flushif <iface>`
* `ndc resolver setifaceforpid <iface> <pid>`
* `ndc resolver clearifaceforpid <pid>`
* `ndc resolver setifaceforuid <iface> <l> <h>`
* `ndc resolver clearifaceforuid <if> <l> <h>`
* `ndc resolver clearifacemapping`

## Android 5.0.0_r2
http://androidxref.com/5.0.0_r2/xref/system/netd/server/CommandListener.cpp#776

* `ndc resolver setnetdns <netId> <domains> <dns1> <dns2> ...`
* `ndc resolver flushnet <netId>`

## Android 5.1.0_r1
http://androidxref.com/5.1.0_r1/xref/system/netd/server/CommandListener.cpp#791

* `ndc resolver setnetdns <netId> <domains> <dns1> <dns2> ...`
* `ndc resolver clearnetdns <netId>`
* `ndc resolver flushnet <netId>`

## Master tree (latest versions [08.07.2015](https://android.googlesource.com/platform/system/netd/+/master/server/CommandListener.cpp#805) checked)
* `ndc resolver setnetdns <netId> <domains> <dns1> <dns2> ...` 
* `ndc resolver clearnetdns <netId>`
* `ndc resolver flushnet <netId>`


Changing the default DNS
---------

On Android < 4.3 we can use the command <code>getprop | grep dns</code> to know all the DNS properties being used. This command requires BusyBox! 


'rmnet0’ is the interface name for the 3G connection. net.rmnet0.dns1 and net.rmnet0.dns2 are the properties to be changed to point to OpenDNS server (the settings are still present in CM/AOSP code). Since, these properties are changed after the connection is established, net.dns1 and net.dns2 also have to be changed.
Execute these commands as root user: <code>setprop net.rmnet0.dns1 208.67.222.222.</code> <code>setprop net.rmnet0.dns2 208.67.220.220</code>. <code>setprop net.dns1 208.67.222.222</code>. <code>setprop net.dns2 208.67.220.220</code>.


Remember, the settings will be applicable only for the current session! You will have to repeat it when you are re-connecting to the network.

Android system chooses the DNS servers using the script located at _/system/etc/dhcpcd/dhcpcd-hooks/20-dns.conf_

<code>20-dns.conf</code>

To change the DNS servers, use the command _setprop property name_
<pre>
setprop net.dns1=208.67.222.222
setprop net.dns2=208.67.220.220
setprop net.eth0.dns1=208.67.222.222
setprop net.eth0.dns2=208.67.220.220 
setprop net.rmnet0.dns1=208.67.222.222
setprop net.rmnet0.dns2=208.67.220.220
setprop dhcp.tiwlan0.dns1=208.67.222.222
setprop dhcp.tiwlan0.dns2=208.67.220.220
setprop net.ppp0.dns1=208.67.222.222
setprop net.ppp0.dns2=208.67.220.220
setprop net.pdpbr1.dns1=208.67.222.222
setprop net.pdpbr1.dns2=208.67.220.220</pre>

Or via init.d script (won't reapply after connectivity change):
<pre>
#!/system/bin/sh
setprop net.dns1 208.67.222.222
setprop net.dns2 208.67.220.220

# Optional
setprop dhcp.tiwlan0.dns1 208.67.222.222
setprop dhcp.tiwlan0.dns2 208.67.220.220
setprop net.ppp0.dns1 208.67.222.222
setprop net.ppp0.dns2 208.67.220.220
setprop net.rmnet0.dns1 208.67.222.222
setprop net.rmnet0.dns2 208.67.220.220
setprop net.pdpbr1.dns1 208.67.222.222
setprop net.pdpbr1.dns2 208.67.220.220</pre>

To check against it (on e.g. wlan) we use:
<pre>
tcpdump -ns0 -i wlan0 'port 53'</pre>

[DNS check tool](http://dnscheck.pingdom.com/) is a secure proof if DNS is working or not, alternative you can use nslookup via command line. Please remember that there are some problems generally with the DNS security protocol, there are several known attacks, like DOS, Cache poisoning, ghost domain names & [others](http://ianix.com/pub/dnssec-outages.html). For more information take a look over [here](http://www.theregister.co.uk/2015/03/18/is_the_dns_security_protocol_a_waste_of_everyones_time_and_money/#)


If there is no setprop you can write the values before the <code>unset_dns_props()</code> begins. Here is an [example 20-dns.conf file](https://gist.github.com/CHEF-KOCH/b054c88d8ba7975a1517). You can get the dns information by using the _getprop | grep dns_ command but this will only work for Android <4.3 devices. 


The <code>getprop</code> or <code>setprop</code> method **does not work on Android versions >4.4+** anymore. Those values, when changed, get simply ignored by the _netd_ daemon. It's necessary to communicate directly to the daemon via the <code>/dev/socket/netd socket</code>. In Android it's now present a tool called <code>ndc</code> which does exactly this job.

On 4.3 or 4.4 KitKat (#su):
<pre>
ndc resolver setifdns eth0 "" 208.67.222.222 208.67.220.220 192.168.1.1
ndc resolver setdefaultif eth0</pre>

Or via AFWall+ custom script:
<pre>
$IPTABLES -t nat -D OUTPUT -p tcp --dport 53 -j DNAT --to-destination 208.67.222.222:53 || true
$IPTABLES -t nat -D OUTPUT -p udp --dport 53 -j DNAT --to-destination 208.67.222.222:53 || true
</pre>

Or via init.d:
<pre>
#!/system/bin/sh
# File without file extension

IP6TABLES=/system/bin/ip6tables
IPTABLES=/system/bin/iptables

# Maybe need to change $IPTABLES to iptables (if there are troubles applying them)
$IPTABLES -t nat -A OUTPUT -p tcp --dport 53 -j DNAT --to-destination 208.67.222.222:53
$IPTABLES -t nat -A OUTPUT -p udp --dport 53 -j DNAT --to-destination 208.67.222.222:53</pre>

If you still like external apps, you should take a look at Override DNS [tested, working on 4.4.4/5.x] which does more or less the same. That may solve some problems on Android 4.4/Lollipop/M but there is no guarantee, some ROMs may handle it different.

Commands to check if DNS is working
---------

The following may are necessary to indicate if all is working (dhcp/nameserver/dnsmasq,...), may needs to be changed for your interfaces you want to check: cellular, tethered, ...

* Grep the current DNS resolver/settings, reads them via: <code>adb shell getprop | grep dns</code>
* The actual DNS servers used are the ones listed in the output of: <code>adb shell dumpsys connectivity</code> or <code>adb shell dumpsys connectivity | grep DnsAddresses</code>
* Via nslookup <code>nslookup google.com</code>
* See the current dhcp info <code>cat /system/etc/dhcpcd/dhcpcd.conf</code>
* List the tethered dns configuration <code>adb logcat | egrep '(TetherController|dnsmasq)'</code>
* Check routing via <code>ip ru</code> + <code>ip route ls</code>
* On Bluetooth tethering <code>ip addr show bt-pan<code> (optional)
* TCPDump check state and export them: <code>tcpdump -ni any -s0 -U -w /sdcard/icmp4.pcap icmp4</code> or <code>adb shell /data/tcpdump -ni wlan0 "icmp6 or port 67 or port 68"

Browser
---------

The normal mobile browser (even stock yes) working out-of-the-box without need any manual adjustment to work with the default DNS.

But some browsers are hard-coding them (so that it isn't possible to override this) and on some systems it may needs configurations changes to get theme proper working e.g. if you changed the DNS system widely. 

Working without any manual adjustments:
* Dolphin 
* Naked Browser
* Firefox (supports DANE via [addon](http://www.dnssec-validator.cz/))
* TOR (Orweb), see [this article](https://trac.torproject.org/projects/tor/wiki/doc/DnsResolver) ~deprecated~
* ....

Needs changes in the settings:
* Chrome -> Clean DNS cache .... (doesn't support DANE (official), [addon](https://chrome.google.com/webstore/detail/dnssec-validator/feijekkdahhnjbhpiffgejphmokchdbo?hl=en) is available)
* Firefox -> for Tor/Orbot .... (disable internal via network.dnsCacheExpiration;0)
* ...

Apps to change the default DNS
-----------

The following apps are success tested on all systems to work:
* OverrideDNS (paid)
* ... add more, remember they must work on all systems (even 5.1.1/M)


Useful links
-----------
* [Latest AOSP netd version (source code) | Android.GoogleSource.com](https://android.googlesource.com/platform/system/netd/+/master/)
* [List common DNS issues (read-only) | Google Issue Tracker](https://code.google.com/p/android/issues/list?can=2&q=DNS&colspec=ID+Type+Status+Owner+Summary+Stars&cells=tiles)
* [DNS (local) resolution on Android Lollipop #79504 | Android Open Source Project - Issue Tracker](https://code.google.com/p/android/issues/detail?id=79504)
* [Android 5 broke tethering (DNS REFUSED) #82545 | Android Open Source Project - Issue Tracker](https://code.google.com/p/android/issues/detail?id=82545)
* [NsdManager | Android Developers.com](http://developer.android.com/reference/android/net/nsd/NsdManager.html)
* [Microsoft KB99686 – Enabling IP Routing | Support.Microsoft.com](https://support.microsoft.com/en-us/kb/99686)
* [Net_DNS2 v1.2.5 DANE TLSA Support | netdns2.com](http://netdns2.com/2012/12/net_dns2-version-1-2-5-released/)
* [DNSSEC-Tools | dnssec-tools.org](http://www.dnssec-tools.org/)
* [Unbound | unbound.net](http://www.unbound.net/) (name server)
* [OpenNIC nameserver list](http://wiki.opennicproject.org/Tier2) + [nearest nameserver](http://www.opennicproject.org/nearest-servers/)
* [Extension Mechanisms for DNS (EDNS0) | Wikipedia](http://en.wikipedia.org/wiki/Extension_mechanisms_for_DNS)
* [DNS Reply Size Test Server | DNS-OARC.net](https://www.dns-oarc.net/oarc/services/replysizetest)


Todo:
* <s>Complete the missing parts </s>
* <s>Link all dns related stuff in this thread (e.g. from the FAQ)</s>
* <s>Add several workarounds since newer systems ignoring the etc/resolver.conf or dhcpcd/dhcpcd-hooks/20-dns.conf files</s>, explained with given links
* <s>Add AFWall+ workarounds via custom scripts or separate tips</s>
* Possible explain why Android 4.4.+/5+ wants to call the RIL with RIL_REQUEST_SETUP_DATA_CALL <netcfg dhcp iface>
* Explain the [broken MTU](https://code.google.com/p/android/issues/detail?id=152819) (saw this on so many roms) and list this bug (see ConnectivityService.cpp), since Android 5.1.x always seems to use 1500 regardless of what value has dhcp daemon set in option 26 (call interface_mtu).
* How can I gather DNS (A/AAA/...) requests? -> must be re-written 
* Add example output how it should looks like, really? (low-prio)
* Add traceroute, whois, and dig commands to work with (low-prio)
* On Android 5.x DNS problems try to [disable IPv6](https://en.m.wikipedia.org/wiki/Comparison_of_IPv6_support_in_operating_systems), since Android 5.0.1/5.x doesn't like DHCPv6
* Is there a decentralized search for Android as app available (similar to yacy)? Mail me if there is one. (low-prio) + add them in the article
* Android auto config compatibility (not yet), e.g. [test network fails](https://code.google.com/p/android/issues/detail?id=93753)
* DNSCrypt, dnsmasq & resolv.conf should be mentioned + example configuration should be given (I need confirmation if that works or not) 