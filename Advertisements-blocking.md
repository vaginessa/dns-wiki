**Please do not send any email/ask here on GitHub how to block the Android advertisements!** 


It's mentioned in our official FAQ that AFWall+ or iptables itself _wasn't designed to blocking ads_, yes iptables can block it - but there is no fine control/GUI/filter for it - to really monitor and take control for each new single HTTP(s)/IP/GET/.. request. 


Google official decided to remove all Adblockers from the Google Play Store for [several reasons](https://adblockplus.org/blog/adblock-plus-for-android-removed-from-google-play-store) - same like any app that may need to change the SELinux policy to get working, which are against there own Play Store terms - which overall means ad-blocking on Android doesn't exists _official_.


But since blocking ads is our good right the following apps solutions still makes it possible to block most stuff we need. 


We have several ad-blocking techniques:
- via iptables (which truly works on everything VPN/Proxy,...)
- via [HOSTS](http://en.wikipedia.org/wiki/Hosts_%28file%29) (most solutions) - also [can be edited manually without any apps](http://www.techrepublic.com/article/edit-your-rooted-android-hosts-file-to-block-ad-servers/)
- a mix: Hosts + iptables (which often creates a local proxy/vpn tunnel)
- Xposed based (nothing special, HOSTS file too)
- Browser based (works only in the browser and not blocks app/in-app ads - they mostly combine a cached hosts + a filter engine e.g. pattern-based to fine control ads per site basis)



_Iptables_:
* This is only for expert users which often working with scripts and know hot to debug things. 


_Browser based Firefox (mobile) addons_:
* [uBlock](https://github.com/gorhill/uBlock/releases) also called [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/) which use a browser HOSTS + a pattern-based filtering engine (similar to NoScript / RequestsPolicy/Policeman ones)
* [uMatrix](https://github.com/gorhill/uMatrix/releases) - [the difference](https://github.com/gorhill/uMatrix/issues/32) here is that it really can control everything and should be used by advanced users which really want to control every request.
* AdBlock Edge (a fork of AdBlock Plus) are [discontinued](http://www.ghacks.net/2015/04/13/popular-adblock-plus-fork-adblock-edge-to-be-discontinued/)


_Mixed ones (which are designed to work on Android + Browser)_:
* [AdGuard](http://adguard.com/en/adguard-android/overview.html) - free version available but the important ones need a _Premium license_


_Hosts_:
* [AdBlock Plus](https://adblockplus.org/de/android-install)
* [AdAway](https://f-droid.org/repository/browse/?fdid=org.adaway) - should be preferred by root users, since AdBlock Plus have some limitations on Android (proxy/vpn ones)


_XPosed based_:
* [UnbelovedHosts](http://unbelovedhosts.apk.defim.de/) more or less the same as AdBlock Plus and others, but is an Xposed module which updates/manage the internal HOSTS file




_To answer the question which is the best one_?

It's depending on user needs:
* If you want to support app developers which promoting there stuff via in-app ads - but you hate browser web ads, feel free to start with uBlock. 
* If you want to block everything, just use an HOSTS based solution. 
* If you really entirely block everything AND fine control per-site ads just use AdAway and NoScript + uBlock/uMatrix which also makes it quite easy for use to work with black-/whitelists (Useful if you like to support you favorite website - e.g. like GitHub).