# Frequently Asked Questions (FAQ)

### Youtube/Online Radio streaming is not working anymore. How can i fix this without disabling AFWall+? 
> Please whitelist "Media Server" or remove it from blacklist 
         
### What is the third column in the main page with "R"?  
> "R" indicates "Roaming". You can whitelist/blacklist applications when you are on roaming! 
         
### My Logs (logcat) are not displaying anything or always empty, why? 
> AFWall+ logs are depend on dmesg (kernel logs). Either your kernel disabled dmesg or it's getting overwritten quickly. Please check if you kernel does have an option like "Android logger Control" or something like that an enable this service.
         
### Can i block incoming SMS?
> No, you [can't block it via AFWall+](https://github.com/ukanth/afwall/issues/111), iptables can't block low level traffic.

### Can i block IPv6 traffic?
> [Sure](https://github.com/ukanth/afwall/issues/108), please use AFWall+ 1.2.4.1 or later. Some kernel have an option to disable IPv6, make sure it's enabled. Make sure you also enabled the option "Enable IPv6 support" in AFWall+ options, because this is disabled by default.

### Is there Tasker/Locale support?
> Tasker can work together with AFWall+. 

### UDP Port 53 is blocked if whitelisting are enabled, why?
> Please read [this](https://github.com/ukanth/afwall/issues/18). It's disabled by default.

### My Apps can bypass AFWall's whitelist mode before the boot are complete.
> Please read [#7](https://github.com/ukanth/afwall/issues/7), [#91](https://github.com/ukanth/afwall/issues/91). As a workaround you can try to enable the experimental functions.

### How can i show the iptables rules?
> Via  iptables -L command in apps like [Android Terminal Emulator](https://play.google.com/store/apps/developer?id=Jack+Palevich).

### How can i purge the iptables rules?
> First type su ... than iptables -F and than iptables -X. You can also do this via 'Firewall Rules' -> 'flush rules'. You need a reboot after that.

### AFWall+ does not show app xyz in my list, why?
> Only apps are listed that have internet permissions. 

### AFWall+ does not work under CM 7.x, how can i fix this?
> Please upgrade your CM version, because this is not supported anymore. As a workaround you can try to update your iptables to the latest version. 