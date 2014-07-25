Index
-----

* [Introduction](#Introduction)
* [Loading scripts from files](#loading-scripts-from-files)
* [Adding custom rules](#adding-custom-rules)
* [Some examples](#some-examples)
* [Droidwall only examples](#droidwall-only-examples)
* [How do I view blocked IP address?](#how-do-I-view-blocked-IP-address?)
* [How do I block subnet?](#how-do-I-block-subnet?)
* [Block incoming request from IP](#block-incoming-request-from-ip)
* [Block outgoing request from LAN](#block-outgoing-request-from-lan)
* [Orbot transparent proxy](#orbot-transparent-proxy)
* [Useful links](#Useful-links)

Introduction
------------

Advanced AFWall+ users may wish to define a custom script to be executed by Android Firewall+.

Once a custom script is defined, it will be automatically executed every time that the AFWall+ rules are applied (inclusive on startup/shutdown if the firewall is enabled).

To define a custom script, just choose <code>Set custom script</code> from the menu (right corner).

**WARNING**: This functionality should be used only by **experienced users that know what they are doing!** These examples may block your Android device (or block the whole internet) if not executed with proper care. Be careful when applying these settings on remote device servers over ssh session.!

Loading scripts from files
--------------------------

* Please first go into <code>Settings</code> -> <code>Developer Options</code> and enable <code>USB Debugging</code> and change Root Access to <code>Apps and ADB</code>. (Necessary if you push/test your files via adb).
* Whenever you finish using adb, always remember to disable USB Debugging and restore Root Access to Apps only. While Android 4.2+ ROMs now prompt you to authorize an RSA key fingerprint before allowing a debugging connection (thus mitigating adb exploit tools that bypass screen lock and can install root apps), you still risk additional vulnerability surface by leaving debugging enabled.

Big scripts can be quite hard to edit in the <code>Set custom script<code> screen, so it may be a good idea to put your script in a file, then load it from there.

To do that, just use the "." (dot) shell command in the <code>Set custom script</code> dialog to load your script from an external file. e.g.:

<pre>. /path/to/script.sh  (example . /data/local/myawesomescript.sh)</pre>


This will cause your script file to be loaded and executed every time the rules are applied. Make sure that this folder have the right permissions, if not AFWall+ can't read any script. 
You can even have multiple scripts executed in sequence...

<pre>. /path/to/load-modules.sh (e.g. /system/local/script.sh)

#. /path/to/myrules.sh

#. /path/to/myscript.sh</pre>


However, please note that this can create a serious security breach on your device, since the script will be always executed as root! You must place your script where other applications will not be able to modify it (the sdcard is NOT a good place!).

**Warning**
If you mistype any of those files, things may break. Because the userinit.sh script blocks all network at boot, if you make a typo in the userinit.sh script (CM-User related), you will be unable to use the Internet at all!

Adding custom rules
----------------------

If you want to add custom iptables rules, just use the **$IPTABLES** shell variable to call iptables.

The following iptables chains can be used to add custom rules:

>**afwall** - This is the main AFWall+ chain. All OUTPUT packets will pass through it. It is therefore the >perfect place if you want to add rules that apply to any interface.
>
>**afwall-3g** - This chain will only receive OUTPUT packets for the cellular network interface (no matter >if it is 2G, 3G, 4G, etc).
>
>**afwall-wifi** - This chain will only receive OUTPUT packets for the Wi-Fi interface.
>
>**afwall-reject** - This chain should be used as a **target** when you want to reject and log a >packet. >When the logging is disabled, this is exactly the same as the built-in **REJECT** target 

Please note that all those chains are guaranteed to be cleared before the custom script is executed, so you don't need to worry about rules cleanup on your script IF you are using those chains.

If you use any chain not listed above, then you need to manually purge it BEFORE adding your custom rules (otherwise the rules will be duplicated every time they are applied). _On this case, you will also need to manually purge you rules when the firewall is disabled, by defining a custom shutdown script._

**IMPORTANT: Never manually purge the OUTPUT chain - this will cause AFWall+ rules to be ignored. Use the 'afwall' chain instead.**

Some examples
-------------

Syntax to block an IP address
> $IPTABLES -A INPUT -s IP-ADDRESS -j DROP

<pre># If you just want to block access to one port from an ip 65.55.44.100 to port 25 then type command:
$IPTABLES -A INPUT -s 65.55.44.100 -p tcp --destination-port 25 -j DROP</pre>

<pre># Always allow connections to 192.168.0.1, no matter the interface
$IPTABLES -A "afwall" --destination "192.168.0.1" -j RETURN</pre>

<pre># Allow all connections to the local network (192.168.0.XXX) on Wi-fi
$IPTABLES -A "afwall-wifi" --destination "192.168.0.0/24" -j RETURN</pre>

<pre># Block all connections in the TCP port 80 (http) 
$IPTABLES -A "afwall" -p TCP --dport 80 -j "afwall-reject"</pre>

<pre># Block all connections in the TCP port 22 (ssh) 
$IPTABLES -A "afwall" -p TCP --dport 22 -j "afwall-reject"</pre>

<pre># Block HTTP connections, but only on cellular interface
$IPTABLES -A "afwall-3g" -p TCP --destination-port 80 -j "afwall-reject"</pre>

<pre># Restore policies
$IPTABLES -P INPUT ACCEPT
$IPTABLES -P FORWARD ACCEPT</pre>

<pre># Drop all but outbound
$IPTABLES -P INPUT DROP
$IPTABLES -P FORWARD DROP</pre>

If you want AFWall+to report failures on your rules, you must manually "exit" from the script on error. E.g.:

<pre># Try to apply my custom rule, but report any failure (and abort)
$IPTABLES -A "afwall" --destination "192.168.0.1" -j RETURN || exit</pre>

<pre># Try to apply another custom rule, but ignore any errors on it
$IPTABLES -A "afwall" -p TCP --destination-port 80 -j "afwall-reject"</pre>

<pre># Connect Wifi-Tethered Clients to VPN
#!/system/bin/sh 
iptables -t filter -F FORWARD
iptables -t nat -F POSTROUTING
iptables -t filter -A FORWARD -j ACCEPT
iptables -t nat -A POSTROUTING -j MASQUERADE</pre>

<pre>#Tethering + OpenVPN issues on KitKat/4.4 with OpenVPN 2.3.2 and higher
#iptables --flush
push "dhcp-option DNS 8.8.8.8"
push "dhcp-option DNS 8.8.4.4"
# push "redirect-gateway autolocal def1" (server config)
iptables -t filter -F FORWARD
iptables -t nat -F POSTROUTING
iptables -t filter -I FORWARD -j ACCEPT
iptables -t nat -I POSTROUTING -j MASQUERADE

#DHCP with OpenVPN set to tun0 and Lan is set to 192.168.43.whatever
ip rule add from 192.168.43.0/24 lookup 61
ip route add default dev tun0 scope link table 61
ip route add 192.168.43.0/24 dev wlan0 scope link table 61
ip route add broadcast 255.255.255.255 dev wlan0 scope link table 61</pre>

DroidWall only examples
-----------------------

<pre># Temporarily allow network adb access when you need it at port 5555 (for Android Studio 5005)
IP6TABLES=/system/bin/ip6tables
IPTABLES=/system/bin/iptables
ADB_UID=10004
SHELL_UID=2000
SAFE_NETWORK=10.23.69.0/24

# Allow adb (port 5555)
$IPTABLES -I INPUT-firewall -s $SAFE_NETWORK -p tcp --dport 5555 -j RETURN
$IPTABLES -I droidwall -m owner --uid-owner $ADB_UID -d $SAFE_NETWORK -m conntrack --ctstate ESTABLISHED -p tcp --sport 5555 -j RETURN
$IPTABLES -I droidwall -m owner --uid-owner $SHELL_UID -d $SAFE_NETWORK -m conntrack --ctstate ESTABLISHED -p tcp --sport 5555 -j RETURN

# Remove transproxy for adb output
$IPTABLES -t nat -I OUTPUT -d $SAFE_NETWORK -m conntrack --ctstate ESTABLISHED -p tcp --sport 5555 -j ACCEPT</pre>

<pre># Allow the UDP activity of LinPhone to bypass Tor, to allow ZRTP-encrypted Voice and Video SIP/VoIP calls. SIP account login/registration and call setup/signaling can be done over TCP, and Linphone's TCP activity is still sent through Tor with this script.
IPTABLES=/system/bin/iptables
VOIP_UID1=`dumpsys package org.linphone | grep userId | cut -d= -f2 - | cut -d' ' -f1 -`
#VOIP_UID2=`dumpsys package org.lumicall.android | grep userId | cut -d= -f2 - | cut -d' ' -f1 -`

# Allow VOIP client UDP return
$IPTABLES -I INPUT-firewall -m owner --uid-owner $VOIP_UID1 -m conntrack --ctstate RELATED,ESTABLISHED -p udp -j RETURN
#$IPTABLES -I INPUT-firewall -m owner --uid-owner $VOIP_UID2 -m conntrack --ctstate RELATED,ESTABLISHED -p udp -j RETURN

# Remove transproxy for VOIP client UDP
$IPTABLES -I droidwall -m owner --uid-owner $VOIP_UID1 -p udp -j RETURN
#$IPTABLES -t nat -I OUTPUT -m owner --uid-owner $VOIP_UID2 -p udp -j RETURN

$IPTABLES -I droidwall -m owner --uid-owner $VOIP_UID2 -p udp -j RETURN
#$IPTABLES -t nat -I OUTPUT -m owner --uid-owner $VOIP_UID2 -p udp -j RETURN</pre>

<pre># Shutdown script to run when AFWall+ is disabled. It will block everything.
# Droidwall 
#chmod 755 /data/data/com.googlecode.droidwall/app_bin/droidwall.sh

# Clear Rules
$IP6TABLES -t nat -F
$IP6TABLES -F
$IPTABLES -t nat -F
$IPTABLES -F INPUT-firewall
$IPTABLES -F OUTPUT-firewall

## Create Chains ##
$IPTABLES -N INPUT-firewall
$IPTABLES -D INPUT -j INPUT-firewall
$IPTABLES -I INPUT -j INPUT-firewall
$IPTABLES -N OUTPUT-firewall
$IPTABLES -D OUTPUT -j OUTPUT-firewall
$IPTABLES -I OUTPUT -j OUTPUT-firewall

## DROP TRAFFIC ##
$IP6TABLES -A INPUT -j LOG --log-prefix "Denied IPv6 input: "
$IP6TABLES -A INPUT -j DROP
$IP6TABLES -A OUTPUT -j LOG --log-prefix "Denied IPv6 output: "
$IP6TABLES -A OUTPUT -j DROP
$IPTABLES -A INPUT-firewall -j LOG --log-prefix "Denied IPv4 input: "
$IPTABLES -A INPUT-firewall -j DROP
$IPTABLES -A OUTPUT-firewall -j LOG --log-prefix "Denied IPv4 output: "
$IPTABLES -A OUTPUT-firewall -j DROP</pre>

<pre># Allow stock Browser (com.android.browser)
IP6TABLES=/system/bin/ip6tables
IPTABLES=/system/bin/iptables
BROWSER_UID=`dumpsys package com.android.browser | grep userId | cut -d= -f2 - | cut -d' ' -f1 -`

# Allow DNS input
$IPTABLES -I INPUT -m conntrack --ctstate RELATED,ESTABLISHED -p udp --sport 53 -j ACCEPT

# Allow already ESTABLISHED browser input
$IPTABLES -I INPUT -m conntrack --ctstate ESTABLISHED -m owner --uid-owner $BROWSER_UID -j ACCEPT

# Allow root DNS output
$IPTABLES -I droidwall-wifi -m owner --uid-owner 0 -p udp --dport 53 -j ACCEPT

# Remove transproxy for root DNS
$IPTABLES -t nat -I OUTPUT -m owner --uid-owner 0 -p udp -m multiport --port 53 -j ACCEPT

# Remove transproxy for browser
$IPTABLES -t nat -I OUTPUT -m owner --uid-owner $BROWSER_UID -j ACCEPT</pre>

<pre># Allow totify (org.torproject.android)
IP6TABLES=/system/bin/ip6tables
IPTABLES=/system/bin/iptables
ORBOT_UID=`dumpsys package org.torproject.android | grep userId | cut -d= -f2 - | cut -d' ' -f1 -`

# Fix for https://code.google.com/p/droidwall/issues/detail?id=260
chmod 755 /data/data/com.googlecode.droidwall/app_bin/droidwall.sh

## Block all IPv6 ##
$IP6TABLES -t nat -F || true
$IP6TABLES -F
$IP6TABLES -A INPUT -j LOG --log-prefix "Denied IPv6 input: "
$IP6TABLES -A INPUT -j DROP
$IP6TABLES -A OUTPUT -j LOG --log-prefix "Denied IPv6 output: "
$IP6TABLES -A OUTPUT -j DROP

## INPUT ##
# Clear previous input firewall rules
# Re-create INPUT-firewall if it doesn't exist
$IPTABLES -N INPUT-firewall || true
$IPTABLES -F INPUT-firewall || true
$IPTABLES -D INPUT -j INPUT-firewall || true
$IPTABLES -I INPUT -j INPUT-firewall || true

# Create INPUT firewall. Only allow orbot input and local orbot ports
$IPTABLES -A INPUT-firewall -m conntrack --ctstate ESTABLISHED -m tcp -p tcp -m owner --uid-owner $ORBOT_UID -j RETURN || exit
$IPTABLES -A INPUT-firewall -d 127.0.0.1 -m tcp -p tcp --dport 8118 -j DROP || exit
$IPTABLES -A INPUT-firewall -i lo -j RETURN # Transproxy output comes from lo
$IPTABLES -A INPUT-firewall -d 127.0.0.1 -m udp -p udp --dport 5400 -j RETURN || exit
$IPTABLES -A INPUT-firewall -j LOG --log-prefix "INPUT DROPPED: " --log-uid || exit
$IPTABLES -A INPUT-firewall -j DROP || exit


## OUTPUT ##
# Clear previous output firewall rules
$IPTABLES -D OUTPUT -j OUTPUT-firewall || true

## Kill droidwall lan rule. It's not currently used, but if it ever gets
# activated it allows all DNS. So kill it as future-proofing
$IPTABLES -F droidwall-wifi-lan || true
$IPTABLES -D droidwall-wifi-lan || true

## Transproxy+Tor limits ##
# Note: We deliberately omit --syn because it causes leaks if used
$IPTABLES -t nat -F
$IPTABLES -t nat -A OUTPUT ! -d 127.0.0.1 -m owner ! --uid-owner $ORBOT_UID -p tcp -j REDIRECT --to-ports 9040 || exit
$IPTABLES -t nat -A OUTPUT -p udp -m owner ! --uid-owner $ORBOT_UID -m udp --dport 53 -j REDIRECT --to-ports 5400 || exit

# Allow root's DNS proxy and kernel to do their transproxy thang
# FIXME: On newer kernels, this may require 1-9999999 instead of 0 (because
# the kernel is flagged as UID 0 instead of blank UID)
$IPTABLES -A droidwall -d 127.0.0.1/32 -p udp -m owner --uid-owner 0 -m udp --dport 5400 -j RETURN || exit

#$IPTABLES -A droidwall -s 127.0.0.1 -p tcp -m owner ! --uid-owner 0-9999999 -m tcp -m multiport --ports 9040 -j RETURN || exit
$IPTABLES -A droidwall -s 127.0.0.1 -p tcp -m owner ! --uid-owner 0-9999999 -m tcp --sport 9040 -j RETURN || exit
$IPTABLES -A droidwall -s 127.0.0.1 -p tcp -m owner ! --uid-owner 0-9999999 -m tcp --dport 9040 -j RETURN || exit

#$IPTABLES -A droidwall -d 127.0.0.1 -p tcp -m owner ! --uid-owner 0-9999999 -m tcp -m multiport --ports 9040 -j RETURN || exit
$IPTABLES -A droidwall -d 127.0.0.1 -p tcp -m owner ! --uid-owner 0-9999999 -m tcp --sport 9040 -j RETURN || exit
$IPTABLES -A droidwall -d 127.0.0.1 -p tcp -m owner ! --uid-owner 0-9999999 -m tcp --dport 9040 -j RETURN || exit

#$IPTABLES -A droidwall -s 127.0.0.1 -p udp -m owner ! --uid-owner 0-9999999 -m udp -m multiport --ports 5400 -j RETURN || exit
$IPTABLES -A droidwall -s 127.0.0.1 -p udp -m owner ! --uid-owner 0-9999999 -m udp --sport 5400 -j RETURN || exit
$IPTABLES -A droidwall -s 127.0.0.1 -p udp -m owner ! --uid-owner 0-9999999 -m udp --dport 5400 -j RETURN || exit

#$IPTABLES -A droidwall -d 127.0.0.1 -p udp -m owner ! --uid-owner 0-9999999 -m udp -m multiport --ports 5400 -j RETURN || exit
$IPTABLES -A droidwall -d 127.0.0.1 -p udp -m owner ! --uid-owner 0-9999999 -m udp --sport 5400 -j RETURN || exit
$IPTABLES -A droidwall -d 127.0.0.1 -p udp -m owner ! --uid-owner 0-9999999 -m udp --dport 5400 -j RETURN || exit

# Allow all from orbot
$IPTABLES -A droidwall -m owner --uid-owner $ORBOT_UID -j RETURN || exit

# We also want to block remaining UDP. All app UDP should be already transproxied
$IPTABLES -A droidwall -p udp ! --dport 5400 -j LOG --log-prefix "Denied UDP: " --log-uid || exit
$IPTABLES -A droidwall -p udp ! --dport 5400 -j DROP || exit

## Transproxy state leak fixes for
# https://lists.torproject.org/pipermail/tor-talk/2014-March/032503.html
$IPTABLES -A droidwall -m conntrack --ctstate INVALID -j LOG --log-prefix "Transproxy ctstate leak blocked: " --log-uid
$IPTABLES -A droidwall -m conntrack --ctstate INVALID -j DROP
$IPTABLES -A droidwall -m state --state INVALID -j LOG --log-prefix "Transproxy state leak blocked: " --log-uid
$IPTABLES -A droidwall -m state --state INVALID -j DROP

# XXX: These are probably overkill and uncessary.
# They remain for defense in depth/debugging.
$IPTABLES -A droidwall ! -o lo ! -d 127.0.0.1 ! -s 127.0.0.1 -p tcp -m tcp --tcp-flags ACK,FIN ACK,FIN -j LOG --log-prefix "Transproxy leak blocked: " --log-uid || exit
$IPTABLES -A droidwall ! -o lo ! -d 127.0.0.1 ! -s 127.0.0.1 -p tcp -m tcp --tcp-flags ACK,RST ACK,RST -j LOG --log-prefix "Transproxy leak blocked: " --log-uid || exit
$IPTABLES -A droidwall ! -o lo ! -d 127.0.0.1 ! -s 127.0.0.1 -p tcp -m tcp --tcp-flags ACK,FIN ACK,FIN -j DROP || exit
$IPTABLES -A droidwall ! -o lo ! -d 127.0.0.1 ! -s 127.0.0.1 -p tcp -m tcp --tcp-flags ACK,RST ACK,RST -j DROP || exit

## REJECT FIXUPS
# Rewrite the reject rule to drop. ICMP is just logspam and ends up going to
# the wrong ports anyways
$IPTABLES -F droidwall-reject
$IPTABLES -A droidwall-reject -j LOG --log-prefix "[DROIDWALL] " --log-uid || exit
$IPTABLES -A droidwall-reject -j DROP || exit</pre>

<pre># Try to apply my custom rule, but report any failure (and abort)
$IPTABLES -A -firewall --destination "192.168.0.1" -j RETURN || exit</pre>

<pre># CM 11 M5 boot fix (userinit.sh)
# It disables "Google Captive Portal Detection", which involves connection attempts to Google servers upon Wifi assocation (these requests are made by the Android Settings UID, which should normally be blocked from the network, unless you are first registering for Google Play).
#!/system/bin/sh

IP6TABLES=/system/bin/ip6tables
IPTABLES=/system/bin/iptables

## Block all traffic at boot ##
$IP6TABLES -F
$IP6TABLES -t nat -F
$IP6TABLES -A INPUT -j LOG --log-prefix "Denied bootup IPv6 input: "
$IP6TABLES -A INPUT -j DROP
$IP6TABLES -A OUTPUT -j LOG --log-prefix "Denied bootup IPv6 output: "
$IP6TABLES -A OUTPUT -j DROP

$IPTABLES -N INPUT-afwall
$IPTABLES -A INPUT-afwall -j LOG --log-prefix "Denied bootup IPv4 input: "
$IPTABLES -A INPUT-afwall -j DROP
$IPTABLES -I INPUT -j INPUT-afwall

$IPTABLES -N OUTPUT-afwall
$IPTABLES -A OUTPUT-afwall -j LOG --log-prefix "Denied bootup IPv4 output: "
$IPTABLES -A OUTPUT-afwall -j DROP
$IPTABLES -I OUTPUT -j OUTPUT-afwall</pre>

The Droidwalls only scripts are all downloadable via [git clone](https://gist.github.com/e0b8771cd1fd17c9b4d3.git) or as [raw files](https://gist.github.com/CHEF-KOCH/e0b8771cd1fd17c9b4d3).

Test above settings please do follow:
> adb shell su -c 'cp /PATH/userinit.sh /data/local/userinit.sh'

To stop the DNS resolve of, and HTTP request to, clients3.google.com on every connection to any Wifi Access Point:
> adb shell su -c "settings put global captive_portal_server 127.0.0.1"

> adb shell su -c "settings put global captive_portal_detection_enabled 0"

To stop the resolve/request for 2.android.pool.ntp.org on every boot (even with network time disabled!):
> su -c "settings put global ntp_server 127.0.0.1"


How do I view blocked IP address?
---------------------------------

<pre>iptables -L -v</pre>

or

<pre>iptables -L INPUT -v</pre>

or 

<pre>iptables -L INPUT -v -n</pre>

How do I block subnet?
---------------------

If you like to block a subnet like (11.00.11.00/11) use the following syntax to block 11.00.11.00/11 on eth1 public interface:
<pre>iptables -i eth1 -A INPUT -s 10.0.0.0/8 -j DROP</pre>

Block incoming request from IP
------------------------------

The following command will drop any packet coming from the IP address 1.2.3.4:
<pre>iptables -I INPUT -s {IP-HERE} -j DROP
iptables -I INPUT -s 1.2.3.4 -j DROP</pre>

You can also specify an interface such as eth1 via which a packet was received:
<pre>iptables -I INPUT -i {INTERFACE-NAME-HERE} -s {IP-HERE} -j DROP
iptables -I INPUT -i eth1 -s 1.2.3.4 -j DROP</pre>

Block outgoing request from LAN
-------------------------------

Block outgoing request from LAN IP 192.168.1.200? Here is the solution:
<pre>iptables -A OUTPUT -s 192.168.1.200 -j DROP</pre>

Orbot transparent proxy
-----------------------

1. Turn off transparent proxying in Orbot.
2. The app that should be redirected through Orbot still needs be allowed normally in AFWall+ for the desired networks.
3. Do not grant Orbot superuser access! It still opens the transproxy ports you need without root, and AFWall+ is managing installation of the transproxy rules, not Orbot.
4. The following lines need to be added to custom script rules in AFWall+ for each app (the app uid needs to be adjusted in each line to represent the uid of your app, in the example the uid is '10001'):

<pre>$IPTABLES -t filter -A OUTPUT -m owner --uid-owner 10001 -o lo -j ACCEPT
$IPTABLES -t nat -A OUTPUT -p tcp ! -o lo ! -d 127.0.0.1 ! -s 127.0.0.1 -m owner --uid-owner 10001 -m tcp --syn -j REDIRECT --to-ports 9040
$IPTABLES -t nat -A OUTPUT -p udp -m owner --uid-owner 10001 -m udp --dport 53 -j REDIRECT --to-ports 5400
$IPTABLES -t filter -A OUTPUT -m owner --uid-owner 10001 ! -o lo ! -d 127.0.0.1 ! -s 127.0.0.1 -j REJECT</pre>

Thanks to an0n981, original posted [here on XDA](http://forum.xda-developers.com/showpost.php?p=54134521&postcount=2126) and modded by [CHEF-KOCH](https://github.com/CHEF-KOCH).

Useful links
------------

* [Simple Iptables Script Generator | Mista.nu](http://www.mista.nu/iptables/) (On some devices $IPT not working, try to replace with $IPTABLES or update your kernel!)
* [Simple Iptables Test | Myresolver.info](http://myresolver.info)
* [25 Most Frequently Used Linux IPTables Rules Examples | thegeekstuff.com](http://www.thegeekstuff.com/2011/06/iptables-rules-examples/)
* [Collection of basic Linux Firewall iptables rules | linuxconfig.org](http://linuxconfig.org/collection-of-basic-linux-firewall-iptables-rules)
* [AFWall+/Droidwall permissions vulnerability | torproject.org](https://lists.torproject.org/pipermail/tor-talk/2014-March/032503.html) - [Original Issue 260 @ Droidwall](https://code.google.com/p/droidwall/issues/detail?id=260)