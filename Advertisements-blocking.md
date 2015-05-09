**Please do not send any email or ask here on GitHub how to block the Android advertisements!** 


It's mentioned in our official FAQ that AFWall+ or iptables itself _wasn't designed to blocking ads_, yes iptables can block it - but there is no fine control/GUI/filter for it - to really monitor + take control over each new single HTTP(s)/IP/GET/.. request(s).


Google official decided to remove all Adblockers from the Google Play Store for [several reasons](https://adblockplus.org/blog/adblock-plus-for-android-removed-from-google-play-store) - same like any app that may need to change the SELinux policy to get working, which are against there own Play Store terms - overall that means ad-blocking on Android doesn't exists _official_.


But since blocking ads is our good right the following app(s) solutions still makes it possible to block most ads we simply does not want to see.

Most people use the following reasons if it comes to the ad blocking question:
* For security reasons, example story shown over [here](http://www.bbc.com/news/technology-26447423), [here](http://us.norton.com/yoursecurityresource/detail.jsp?aid=banner) or [here](https://news.ncsu.edu/2012/03/wms-jiang-app-ads/) + an article from theguardian if you should use it or not over [here](http://www.theguardian.com/technology/pda/2010/mar/09/adblock)
* It's _important_ in areas with limited bandwidth like 3G or models without unlimited data plans
* It's just annoying to watch videos with in-ads 
* Speed reasons (in fact if you use a HUGE list it also can be negative, because more always means more to load [at start]).
* Enabling _acceptable ads_ to help and support you're site is always possible
* Popular and small pages like [Ghacks are dying](http://www.ghacks.net/2015/02/27/ghacks-is-dying-and-needs-your-help/) because blocking everything, but just supporting by [making exclusions may help them](http://www.ghacks.net/2015/04/18/2-months-later-ghacks-is-still-dying-but-there-is-hope/) - which is a good compromise. 
* It's always up-2-you what you doing with the given information, personally I think as long you're daily visiting pages documents and list there own ads (including there sources) - so that everyone can read what they collect it should be okay - in fact most pages place there ads very well, to not disturb the user - It's a matter of communication from the Webmaster to the community.
* <other reasons>
* ... the rest is FUD (fear, uncertainty, and doubt) and possible wrong myths e.g. blocking ads doesn't prevent the government to not watch you if you already under their microscope.  


We have several ad-blocking techniques:
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


_Hosts_:
* [AdBlock Plus](https://adblockplus.org/de/android-install)
* [AdAway](https://f-droid.org/repository/browse/?fdid=org.adaway) - should be preferred by root users, since AdBlock Plus have some limitations on Android (proxy/vpn ones)


_XPosed based_:
* [UnbelovedHosts](http://unbelovedhosts.apk.defim.de/) more or less the same as AdBlock Plus and others, but is an Xposed module which updates/manage the internal HOSTS file



A uMatrix/uBlock/AdBlock/AdBlock Plus/AdGuard blocking list which includes the most important stuff is this one:
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
! EasyPrivacy by adblock.org
! easylist-downloads.adblockplus.org/easyprivacy.txt
! Fanboy’s Enhanced Tracking List by fanboy.co.nz
! http://www.fanboy.co.nz/enhancedstats.txt
! GNU privacy filter
http://gnuzilla.gnu.org/filters/blacklist.txt



! Blocks Malware domains
! 
! Malware Domain list
! http://www.malwaredomainlist.com/hostslist/hosts.txt
! Malware Domain (just Domains) [mirror from mailwaredomainlist.com]
! mirror1.malwaredomains.com/files/justdomains
! Immortal Domains (also called long-lived)
! mirror1.malwaredomains.com/files/immortal_domains.txt
! Spam404 by spam404bl.com
! spam404scamlist.txt



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


_To answer the question which is the best one_?

It's depending on user needs:
* If you want to support app developers which promoting there stuff via in-app ads - but you hate browser web ads, feel free to start with uBlock. 
* If you want to block everything, just use an HOSTS based solution. 
* If you really entirely block everything AND fine control per-site ads just use AdAway and NoScript + uBlock/uMatrix which also makes it quite easy for use to work with black-/whitelists (Useful if you like to support you favorite website - e.g. like GitHub).


Does I need Ghostery / Disconnect.me if I use an Ads blocker - or can I combine them?
It's depending which list you're use, e.g. the official easylist often gets updates (mostly around every 4/7 days). Other extensions/addons tools not so often gets updates which means you're better stick with the easylists - but generally it isn't necessary to use both together, since that always ends up with more unnecessary configurations and possible conflicts. E.g. if you not need/use any social network's you can just use the above given Fanboy's anti social list, which blocks the same as Ghostery anti-social list (except that there is no _click-2-play_ possibility. So, again it's depending on you and your needs. Another example could be that if you not have installed any browser ad blocker extension you can use Ghostery or Disconnect - but it's definitely not so effective like e.g. your lists which more often gets updates or extensions that have additional filters like the _element hiding_ or pattern based systems.

General it's smart to use as less as possible addons/extensions/tools because to the following arguments:
* Even you have 100 GB RAM on you high-end machine, that doesn't mean that you need to waste them
* With the wrong configuration it will end-up with problems 
* Duplicates are always bad since the will parsed twice or more which overall possible means it takes longer to load a site (if your blocker doesn't search for dups.)
* More extensions/addons means it's easier to track you due the risk of e.g. mime type detection, fingerprinting or possible data leaks which ends up with less privacy (e.g. Ghost Rank which _must_ be disabled).
* Systems like uBlock/uMatrix use there own pattern systems, logging systems and other stuff to monitor all browser requests, Disconnect/Ghostery doesn't have such features since it only merge there lists, for advanced users that means they simply can't beat such mechanism and you're able to see a lot of more _behind the scene_ stuff.
* Open source project's should be preferred since it's very easy to make pull requests or submit feedback (I'm not saying it's impossible for closed source projects to also do that but it's possible harder because not all company's like to communicate that often) - and of course to see the source code -> to understand what the tools/apps/addons/extensions really does in the background.