Index
-----

* [Introduction](#Introduction)
* [Adding custom rules](#adding-custom-rules)
* [Some examples](#some-examples)
* [Loading scripts from files](#loading-scripts-from-files)
* [How do I view blocked IP address?](#how-do-I-view-blocked-IP-address?)
* [How do I block subnet?](#how-do-I-block-subnet?)
* [Block incoming request from IP](#block-incoming-request-from-ip)
* [Block outgoing request from LAN](#block-outgoing-request-from-lan)
* [Orbot transparent proxy](#orbot-transparent-proxy)
* [Useful links](#Useful-links)

Introduction
------------

Advanced AFWall+ users may wish to define a custom script to be executed by Android Firewall +.

Once a custom script is defined, it will be automatically executed every time that the AFWall+ rules are applied (inclusive on startup/shutdown if the firewall is enabled).

To define a custom script, just choose "Set custom script" from the menu (right corner).

**WARNING**: This functionality should be used only by **experienced users that know what they are doing** These examples may block your Android device if not executed with proper care. Be careful when applying these settings on remote device servers over ssh session.!

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

Loading scripts from files
--------------------------

Big scripts can be quite hard to edit in the "Set custom script" screen, so it may be a good idea to put your script in a file, then load it from there.

To do that, just use the "." (dot) shell command in the "Set custom script" dialog to load your script from an external file. e.g.:

<pre>. /path/to/script.sh  (example . /data/app-asec/myawesomescript.sh)</pre>


This will cause your script file to be loaded and executed every time the rules are applied. Make sure that this folder have the right permissions, if not AFWall+ can't read any script. 
You can even have multiple scripts executed in sequence...

<pre>. /path/to/load-modules.sh

. /path/to/myrules.sh

. /path/to/myscript.sh</pre>

However, please note that this can create a serious security breach on your device, since the script will be always executed as root! You must place your script where other applications will not be able to modify it (the sdcard is NOT a good place!).

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
3. The following lines need to be added to custom script rules in AFWall+ for each app (the app uid needs to be adjusted in each line to represent the uid of your app, in the example the uid is '10001'):

<pre>$IPTABLES -t filter -A OUTPUT -m owner --uid-owner 10001 -o lo -j ACCEPT
$IPTABLES -t nat -A OUTPUT -p tcp ! -o lo ! -d 127.0.0.1 ! -s 127.0.0.1 -m owner --uid-owner 10001 -m tcp --syn -j REDIRECT --to-ports 9040
$IPTABLES -t nat -A OUTPUT -p udp -m owner --uid-owner 10001 -m udp --dport 53 -j REDIRECT --to-ports 5400
$IPTABLES -t filter -A OUTPUT -m owner --uid-owner 10001 ! -o lo ! -d 127.0.0.1 ! -s 127.0.0.1 -j REJECT</pre>

Thanks to an0n981, original posted [here on XDA](http://forum.xda-developers.com/showpost.php?p=54134521&postcount=2126).

Useful links
------------

* [Simple Iptables Script Generator | Mista.nu](http://www.mista.nu/iptables/) (On some devices $IPT not working, try to replace with $IPTABLES or update your kernel!)
* [Simple Iptables Test | Myresolver.info](http://myresolver.info)
* [25 Most Frequently Used Linux IPTables Rules Examples | thegeekstuff.com](http://www.thegeekstuff.com/2011/06/iptables-rules-examples/)
* [Collection of basic Linux Firewall iptables rules | linuxconfig.org](http://linuxconfig.org/collection-of-basic-linux-firewall-iptables-rules)