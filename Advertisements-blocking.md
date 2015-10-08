:exclamation: **Please do not send any eMail or ask here on GitHub how to block the Android advertisements!** :exclamation: 

Index
---------

* [Overview](#overview)
* [Reasons to block ads](#reasons-to-block-ads)
* [Ad-blocking techniques and tools](#ad--blocking-techniques-and-tools)
* [Example lists](#example-lists)
* [The best solution?](#the-best-solution-?)
* [Does I need Ghostery?](#does-i-need-ghostery-?)
* [Final words](#final-words)

Overview
---------

It's mentioned in our official FAQ that AFWall+ or iptables itself _wasn't explicit designed to block every ad_, yes iptables itself could handle and block it - but there is no fine control/GUI/filter for it - to really monitor + take control over each new single HTTP(s)/IP/GET/.. request(s). AFWall+ will not (or ever? soon?) get any interface for it since that will confuse beginners, would need more manpower we currently don't have and could never beat any existent solutions since they're all designed to only do block ads. AFWall+ is a firewall and not a all-in-one solution and was never designed to be one. 


Google official decided to remove all Ad blocking apps from there own Google Play Store due [several reasons](https://adblockplus.org/blog/adblock-plus-for-android-removed-from-google-play-store) - same like any app that may need to change the SELinux policy to get property working, which are against there own [Play Store terms](https://play.google.com/about/developer-content-policy.html) [TOS](https://play.google.com/about/android-developer-policies.html) - overall that means ad-blocking on Android doesn't exists - _official_.


Reasons to block ads
---------

Since _blocking ads is our good right_, the following app(s) solution(s) still makes it possible to block most annoying ads we simply does not want to see.

Most people use the following reasons if it comes to the ad blocking question:
* For [security reasons](https://www.fireeye.com/blog/threat-research/2015/10/kemoge_another_mobi.html), example story shown over [here](http://www.bbc.com/news/technology-26447423), [here](http://us.norton.com/yoursecurityresource/detail.jsp?aid=banner) or [here](https://news.ncsu.edu/2012/03/wms-jiang-app-ads/) + an article from theguardian if you should use it or not over [here](http://www.theguardian.com/technology/pda/2010/mar/09/adblock)
* It's _important_ in areas with limited <code>bandwidth</code> like 3G or models without unlimited data plans
* It's just <code>annoying</code> to watch videos with in-ads (it never can block hard-coded watermarks!)
* <code>Speed reasons</code> (in fact if you use a HUGE list it also can be negative, because more always means more to load [_at start_]).
* Enabling <code>acceptable ads</code> to <code>help and support you're favorite site</code> is always possible. Popular and small pages like [Ghacks are dying](http://www.ghacks.net/2015/02/27/ghacks-is-dying-and-needs-your-help/) because blocking everything, but just supporting by [making exclusions may help them](http://www.ghacks.net/2015/04/18/2-months-later-ghacks-is-still-dying-but-there-is-hope/) - which is a good compromise. 
* <code>It's always up2you</code> what you doing with the given information. _Personally I think as long you're daily visiting pages documents and list there own ads (including there sources) - so that everyone can read what they collect it should be okay - in fact most pages place there ads very well, to not disturb the user - It's a matter of communication from the Webmaster to the community_
* For reasons to hide from _illegal_ sites (like warez, drugs, ...), see example over [here](https://torrentfreak.com/uk-police-hijack-ads-on-251-pirate-sites-150825/).
* <code>Other reasons</code>?!
* ... <code>the rest is FUD</code> (fear, uncertainty, and doubt) and possible wrong myths e.g. blocking ads doesn't prevent the government to not watching you're activities you if you already under their microscope.  

Ad-blocking techniques and tools
---------

- via iptables (which truly works on everything VPN/Proxy,...)
- via [HOSTS](http://en.wikipedia.org/wiki/Hosts_%28file%29) (most solutions) - also [can be edited manually without any apps](http://www.techrepublic.com/article/edit-your-rooted-android-hosts-file-to-block-ad-servers/)
- a mix: Hosts + iptables (apps often creates [or using Android's own] local proxy/vpn tunnel)
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


_Hosts_ (_/system/etc/hosts_):
* [AdBlock Plus](https://adblockplus.org/de/android-install)
* [AdAway](https://f-droid.org/repository/browse/?fdid=org.adaway) - should be preferred by root users, since AdBlock Plus have some limitations on Android (proxy/vpn ones). Please note that AdAway isn't anymore under active development (only pull requests and pre-compiled builds are supported).
* AdFree - [See AdAway vs. AdFree](http://forum.xda-developers.com/showthread.php?t=1247853)


_XPosed based_:
* [UnbelovedHosts](http://unbelovedhosts.apk.defim.de/) more or less the same as AdBlock Plus and others, but is an Xposed module which updates/manage the internal HOSTS file


Example lists
---------

A uMatrix/uBlock/AdBlock/AdBlock Plus/AdGuard blocking list which includes the [most important lists](https://github.com/gorhill/uBlock/wiki/Filter-lists-from-around-the-web) is this one:
```bash
! Ads related
!
! Adblock Warning Removal List
! https://easylist-downloads.adblockplus.org/antiadblockfilters.txt
! Anti-Adblock Killer | Reek
! raw.githubusercontent.com/reek/anti-adblock-killer/master/anti-adblock-killer-filters.txt
! EasyList
! easylist-downloads.adblockplus.org/easylist.txt
! EasyList w/o element hiding rules
! easylist-downloads.adblockplus.org/easylist_noelemhide.txt
! Acceptable Ads
! https://adblockplus.org/en/acceptable-ads



! Privacy related
!
! Basic tracking list by Disconnect‎
https://s3.amazonaws.com/lists.disconnect.me/simple_tracking.txt
! EasyPrivacy by adblock.org
! easylist-downloads.adblockplus.org/easyprivacy.txt
! Fanboy’s Enhanced Tracking List by fanboy.co.nz
! http://www.fanboy.co.nz/enhancedstats.txt
! GNU privacy filter
! http://gnuzilla.gnu.org/filters/blacklist.txt
! CHEF-KOCH's NSA blocklist
! https://github.com/CHEF-KOCH/NSABlocklist/blob/master/HOSTS.txt



! Blocks Malware domains
!
! Malvertising filter list by Disconnect‎
https://s3.amazonaws.com/lists.disconnect.me/simple_malvertising.txt
! Malware filter list by Disconnect‎
https://s3.amazonaws.com/lists.disconnect.me/simple_malware.txt
! Malware Domain list
! http://www.malwaredomainlist.com/hostslist/hosts.txt
! Malware Domain (just Domains) [mirror from mailwaredomainlist.com]
! mirror1.malwaredomains.com/files/justdomains
! Immortal Domains (also called long-lived)
! mirror1.malwaredomains.com/files/immortal_domains.txt
! Spam404 by spam404bl.com
! http://spam404bl.com/blacklist.txt



! Anti social networks and buttons
! 
! Fanboy's anti-social list which blocks buttons, sites, ...
! http://www.fanboy.co.nz/fanboy-antifacebook.txt
! Fanboy’s Annoyance List by adblockplus.org
! easylist-downloads.adblockplus.org/fanboy-annoyance.txt
! Fanboy's third-party Annoyance list
! easylist-downloads.adblockplus.org/fanboy-annoyance.txt
! Fanboy's Social Blocking List 
! easylist-downloads.adblockplus.org/fanboy-social.txt



! Multipurpose and Misc
!
! Dan Pollock’s hosts file by someonewhocares.org [also avb. as IPv6 and nulled)
! someonewhocares.org/hosts/hosts
! Fanboy's ultimate list (combines all)
! http://www.fanboy.co.nz/fanboy-ultimate.txt
! hpHosts’s Ad and tracking servers by hosts-file.net [sometimes offline]
! hosts-file.net/ad-servers
! MVPS HOSTS
! winhelp2002.mvps.org/hosts.txt



! YouTube related
! 
! YouTube all suggestions and comments
! https://easylist-downloads.adblockplus.org/yt_annoyances_full.txt
! YouTube suggestions 
https://easylist-downloads.adblockplus.org/yt_annoyances_suggestions.txt
! YouTube annoyances
! https://easylist-downloads.adblockplus.org/yt_annoyances_other.txt
! YouTube Remove Comments
https://easylist-downloads.adblockplus.org/yt_annoyances_comments.txt



! Cookie related
!
! EU: Prebake - Filter Obtrusive Cookie Notices
! https://raw.githubusercontent.com/liamja/Prebake/master/obtrusive.txt
```

Remember that a bigger list always means that it takes a bit longer to parse all the visiting list which maybe reduce the loading speed (if it's not cached). And it isn't necessary to really use all lists due the fact that it mostly ends up with troubles, duplicates or other problems.
Since _uBlock 0.9.8.2_ Disconnect.me Malware and Tracking list(s) was merged, this means you can remove (if installed) the Disconnect.me addon + disable Firefox safe-browsing feature, this is now redundant and definitely safes bandwidth. 


The best solution?
---------

It's depending on your needs:
* If you want to support app developers which promoting there stuff via e.g. in-app/video ads - but you hate browser web ads, feel free to start with uBlock.
* If you want to block everything without much controlling options, just use an HOSTS based solution. Every OS does support HOSTS (only the file format and possible entries differ). 
* If you really entirely block everything AND want fine control per-site ads just start with AdAway and NoScript (for JavaScript, XSS,[..]) + uBlock or uMatrix which also makes it quite easy for you to work with black-/whitelists that are useful if we want like to support our favorite website like e.g. GitHub.com (www.GitHub.com).


Does I need Ghostery / Disconnect.me if I use an Ads blocker - or can I combine them?
---------

Some say this and some say this but the truth somehow lies in the middle and is depending what background knowledge you might have. 

General it's depending which filter list(s) you're use or combine, e.g. the official easylist often gets updates (mostly around every 4/7 days). Other extensions/addons tools not so often gets updates regularly which means you're better stick with the easylist ones - but generally **it isn't necessary to use both together**, since that always ends up with more unnecessary configurations and possible conflicts without any notable effect e.g. if you not need/use any social network's you can just use the above given Fanboy's anti social list, which blocks the same as Ghostery anti-social list (except that there is no _click-2-play_ possibility. So, again it's depending on you and your needs. Another example could be that if you not have installed any browser ad blocker extension you could use Ghostery or Disconnect, that also not shows xyz - but it's definitely not so effective like additional filters like the _element hiding_ or pattern based systems.

General it's smart to use as less as possible addons/extensions/tools because to the following reasons:
* Even you have 100 GB RAM installed on you high-end machine, that doesn't mean automatically that you need to waste them.
* With the wrong configuration(s) it definitely will end-up with troubles. 
* Duplicates are always bad since that will parsed twice (or more) which overall possible means it takes longer to load a site (if your blocker doesn't search for dups.) - or requires more CPU/RAM power (more battery drain -> especially on bigger HOST file during the Android cold start).
* More extensions/addons means it's easier to track you due the risk of e.g. [mime type detection](https://developer.mozilla.org/en-US/docs/How_Mozilla_determines_MIME_Types), fingerprinting or possible data leaks which ends up with less privacy (e.g. [Ghost Rank](http://lifehacker.com/ad-blocking-extension-ghostery-actually-sells-data-to-a-514417864) which must be disabled [FAQ](https://www.ghostery.com/en/faq#q5-general)).
* Browser addons like uBlock/uMatrix use there own pattern/filter/logging systems (and other stuff) to monitor all browser/ads requests, Disconnect/Ghostery doesn't have such filter/monitor features since it only merge there own lists. You're able to see a lot of more _behind the scene_ stuff with such addons. 
* _Open source project's should be preferred_ since it's very easy to making pull requests or submit feedback (I'm not saying it's impossible for closed source projects to also do that but it's possible harder because not all company's like to communicate with you) - and of course to see the source code -> to understand what the tools/apps/addons/extensions really does in the background.

Final words
---------

Don't believe in myths that more tools and blocking always ends up with more privacy or less problems, the hole grail should be to use as less as possible addons + as less filtering as possible which does block stuff you hate or possible compromise you're security. In fact more from everything always means that you need more time to manage it to stay secure and up-2-date. If you really want to hear my _recommendation_ (I normally not often give one) is that for a normal root user AdAway (to not manually update Android's hosts) + uBlock (for Browsers) should be enough to handle all above given reasons. 


_ Final version 08.24.2015_