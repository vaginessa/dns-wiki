:warning: You normally does not need to use both products the same time and it's not recommend to use both apps together! But you're the boss, if you need it here are some quick tips.

Index
-----

* [Requirements](#requirements)
* First Method
* Second Method


Requirements
------------

* Root access
* [Orbot](https://guardianproject.info/apps/orbot/) (ensure Orbot has no Transparent Proxy **enabled** or **disabled**!)
* [OrWall](https://orwall.org/) 
* [AFWall+](https://github.com/ukanth/afwall)
* [Custom Script Code Snippet](https://github.com/ukanth/afwall/wiki)

# First Method using Orbot without Orbot (only AFWall)
------------

1) Install the custom script in AFWall+

### Our script snippet for afwall+
> $IPTABLES -A "afwall" -d 127.0.0.1 -p tcp --dport 9040 -j ACCEPT

> $IPTABLES -A "afwall" -d 127.0.0.1 -p udp --dport 5400 -j ACCEPT


2) Select the apps that can communicate via Tor (outgoing)

* Go into Orbot and make sure _Transparent_ _Proxying_ is **enabled**!
* Select your apps e.g. Firefox,[...]


3) Now the last steps

* Now deactivate internet access for Firefox,[...] via AFWall+ (in whitelist mode)
* In Orbot Firefox,[...] needs to be selected and allowed trough tor 

Orbot will now send all app network requests trough the local Tor ports 9040 and 5400 (Transparent Proxying). The code snippet allows all apps an access to these local ports so that OrWall isn't required here. One benefit is that the apps can only pass the tor network and nothing else.


# Second Method using Orbot with Orbot & AFWall+
------------

1)



2) Select the apps that can communicate via Tor (outgoing)

* Go into Orbot and make sure _Transparent_ _Proxying_ is **disabled**!
* Select your apps e.g. Firefox,[...]


3) 

... soon
