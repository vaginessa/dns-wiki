This page should explain and give an short overview which system-apps are good or bad to block. It can be seen as a database like the [Xprivacy crowd](https://crowd.xprivacy.eu/) but only for the system-apps. This helps beginners to make an decision what is okay to block. 

Discussion/suggestion can be found [over here](https://github.com/ukanth/afwall/issues/518).
 
 ## **_Common System Apps_**
 
 **([adb](https://developer.android.com/tools/help/adb.html)) - Android Debug Bridge**
 
 *   If blocked, ADB will not be able to communicate over the
 *   Rec: Block if not using ADB over a wireless connection, otherwise enable only for connection type being used
 
 **([drm](https://source.android.com/devices/drm.html)) - Digital Rights Management service**
 
 *   If blocked, cannot view content that requires Android DRM
 *   Rec: Block if not viewing any content that requires Android DRM content, otherwise allow for connection type(s) being used
 
 **([gps](https://en.wikipedia.org/wiki/Global_Positioning_System)) - Global Position System**
 
 *   If blocked, GPS may not be able to receive AGPS data (needs to be verified)
 *   Rec: Research if AGPS data can still be obtained if blocked; if so, block
 
 **([Kernel](https://source.android.com/devices/tech/config/kernel.html)) - Linux Kernel**
 
 *   Cases where it needs to be enabled for all connection types (LAN, WiFi, Cell data)
 *   Rec: Allow for all connection types (including LAN)
 
 **([media](https://source.android.com/devices/audio/)) - Media server**
 
 *   Need more info
 *   Rec: Until more info provided, allow for connection types except LAN
 
 **([SNTP](https://android.googlesource.com/platform/frameworks/base/+/master/core/java/android/net/SntpClient.java)) - Simple Network Time Protocol**
 
 *   Cell network devices can still obtain current time/date from cell network even if this is blocked
 *   Rec: Block for cell devices, Allow for connection types except LAN for other devices
 
 **([shell](https://developer.android.com/tools/help/shell.html)) - Linux shell**
 
 *   Cases where it needs to be enabled for connection types except LAN (WiFi, Cell data)
 *   Rec: Allow for connection types except LAN
 
 **(tethering) - DHCP + DNS services**
 
 *   If blocked, will not be able to tether over the connection types blocked
 *   Rec: Block if not tethering, otherwise enable only for connection types being used for tethering
 
 **(vpn) - VPN Networking**
 
 *   If blocked, will not be able to use a VPN over the connection types blocked
 *   Rec: Block if not using a VPN, otherwise enable only for connection types being used to connect to the VPN
 
 **ConfigUpdater**
 
 *   This function exists since Android 4.2, it's designed to automatically update certificates, firewall configuration, premium SMS list [you get warning if you want to sent premium SMS], time zone information and information for SELinux configuration.
 *   Rec: Never block it.
 
 **Downloads, Media Storage, Download Manager**
 
 *   Required to for apps to download content
 *   Rec: Allow for connection types except LAN
 
 **DRM Service**
 
 *   If blocked, cannot view content that requires DRM
 *   Rec: Block if not viewing any DRM content, otherwise allow for connection type(s) being used
 
 **GoogleAccount Manager**
 
 *   If blocked, you can't login with you Google Account if an app uses e.g. Google/Facebook/.. login.
 *   Rec: Block if not using Google apps, otherwise allow for all connection types except LAN
 
 **Google Partner Setup**
 
 *   No one seems to know what this does and what data it transmits/receives
 *   Rec: Block

**[Google Play-services](https://github.com/ukanth/afwall/wiki/Google-Play-services-and-other-special-cases)**

* This service is critical and very important and should never blocked. It's for gcm (google cloud messaging).
* Rec: Never block it, if you not use any Google Apps or apps with GCM you can uninstall but only if you really not use any apps which request for any of it's API's. 
 
 **Google Play Store**
 
 *   If blocked, cannot use the Google Play store
 *   Rec: Block if not using the Google Play store, otherwise allow for all connection types except LAN
 
 **Market Feedback Agent**
 
 *   If blocked, cannot use Google to submit feedback to app developers when apps crashes.
 *   Rec: Block if not using the Google Play store, otherwise allow for all connection types except LAN
 
 **PacProcessor - Proxy Auto-Config (PAC) Processor**
 
 *   This is for auto proxy discovvering. If you switch/uninstall this .apk there will be no chance to assign to each specific app it's own specific custom proxy server for it's own way of webaccess.
 *   Rec: Never block it, if you not need it uninstall it.
 
 **ProxyHandler**
 
 *   Function not completely clear
 *   Rec: Allow for all connection types except WiFi; might be good to block if not using any proxies
 
 **Self Service**
 
 *   Unclear what this does and what data it transmits/receives
 *   Rec: Block
 
 **Setup Wizard**
 
 *   Unclear what data this transmits/receives
 *   Rec: Block
 
 **Smart Device Manager**
 
 *   Unclear exactly what this data this transmits/receives, but may be required for some tracking tools (finding lost device, for example)
 *   Rec: Block, unless determined that it is needed for any lost device finding tools

 **Android System (uid 1000)**

 *   Unclear exactly what this is needed for, but blocking it causes an exclamation mark next to the Wifi/Cellular icons (indicating no Internet) even if there is Internet.
 *   Rec: Allow for all connection types

 **Phone Services (uid 1001)**

 *   Necessary for making and receiving SIP calls, among other things
 *   Rec: Allow for all connection types




Todo:
* add UID's
* add missing system apps
* Add apk name + possible included packages (sub-processes)
* Maybe table as main view incl. symbols for a legend to get shorter/cleaner look?
* may add opengapps.org (apkmirror)? A trusted source (hosted on github, also with checksums and more) to get latest versions ^^ 