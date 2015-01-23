:warning: You normally does not need to use both products the same time and it's not recommend! 
But you're the boss, if you need it, here are some quick tips.

Index
-----

* [Requirements](#requirements)
* [First Method](#first-method-using-orbot-without-orwall)
* [Second Method](#second-method-using-orbot-together-with-afWall+)
* [Useful Links](#useful-links)


Requirements
------------

* Root access
* Kernel compiled with _CONFIG_IP_NF_MATCH_OWNE_ (to allow or reject packets on a per-command basis)
* [Orbot](https://guardianproject.info/apps/orbot/) (ensure Orbot has no Transparent Proxy **enabled** or **disabled**!)
* [OrWall](https://orwall.org/) 
* [AFWall+](https://github.com/ukanth/afwall)
* [Custom Script Code Snippet](https://github.com/ukanth/afwall/wiki)

# First method using Orbot without OrWall
------------

1) Install the custom script in AFWall+

### Our script snippet for AFWall+
> $IPTABLES -A "afwall" -d 127.0.0.1 -p tcp --dport 9050 -j ACCEPT

> $IPTABLES -A "afwall" -d 127.0.0.1 -p udp --dport 5400 -j ACCEPT


2) Select the apps that can communicate via Tor (outgoing)

* Go into Orbot and make sure _Transparent_ _Proxying_ is **enabled**!
* Select your apps e.g. Firefox,[...]


3) Now the last steps

* Now deactivate internet access for Firefox,[...] via AFWall+ (in whitelist mode)
* In Orbot Firefox,[...] needs to be selected and allowed trough tor 

Orbot will now send all app network requests trough the local Tor ports 9050 and 5400 (Transparent Proxying). The code snippet allows all apps an access to these local ports so that OrWall isn't required here. One benefit is that the apps can only pass the tor network and nothing else.


# Second method using Orbot together with AFWall+
------------

1)



2) Select the apps that can communicate via Tor (outgoing)

* Go into Orbot and make sure _Transparent_ _Proxying_ is **disabled**!
* Select your apps e.g. Firefox,[...]


3) 

... soon


Using Tor with Firefox (client/mobile)
------------

Tor acts as a Socks5 proxy on port 9050 (Tor Browser itself listens on port 9150). Recent versions of Firefox allow direction of all traffic, including DNS resolution, through a Socks5 proxy. To enable this behavior (after starting and running a previously installed version of Tor/Orbot):
    Firefox -> Tools -> Options -> Advanced -> Network -> Connection:Settings -> Manual proxy configuration (ticked) -> SOCKS Host: 127.0.0.1 (or localhost) -> Port: 9050 -> SOCKSv5 (ticked) -> No Proxy for: 127.0.0.1 (or localhost) 
    -> Remote DNS (ticked)

The last step (Remote DNS) is important so that DNS lookups are done through the proxy (with SOCKSv5), not the client computer. 

:warning: Privacy is lost if DNS lookups are done by the client computer! :warning:

    To return to using Firefox without a proxy (such as Tor), choose "No proxy" in the Firefox Network settings: 
    Firefox -> Tools -> Options -> Advanced -> Network -> Connection:Settings -> No proxy (ticked)

For mobile browsers:
Go under _Preferences_ > _Advanced_ > _Network tab_ > _Settings_ manually set Firefox to use the SOCKS proxy _localhost_ with port 9050. Then you must type _about:config_ into the address bar. Change _network.proxy.socks_remote_dns_ to _true_ and restart the browser. This channels all DNS requests through TOR's socks proxy. 


Useful Links
------------

* [TorCheck | Check.torproject.org](https://check.torproject.org/)
* [Tor FAQ | TorProject.org](https://www.torproject.org/docs/faq.html.en)