This page should explain and give an short overview which apps are good/bad to block. It can be seen as a similar database as the [Xprivacy crowd](https://crowd.xprivacy.eu/) to get an easier decision what is okay to allow and what really don't internet access. 
 
 ## **_Common System Apps_**
 
 **(adb) - Android Debug Bridge**
 
 *   If blocked, ADB will not be able to communicate over the
 *   Rec: Block if not using ADB over a wireless connection, otherwise enable only for connection type being used
 
 **(drm) - Digital Rights Management service**
 
 *   If blocked, cannot view content that requires Android DRM
 *   Rec: Block if not viewing any content that requires Android DRM content, otherwise allow for connection type(s) being used
 
 **(gps) - GPS**
 
 *   If blocked, GPS may not be able to receive AGPS data (needs to be verified)
 *   Rec: Research if AGPS data can still be obtained if blocked; if so, block
 
 **(Kernel) - Linux Kernel**
 
 *   Cases where it needs to be enabled for all connection types (LAN, WiFi, Cell data)
 *   Rec: Allow for all connection types (including LAN)
 
 **(media) - Media server**
 
 *   Need more info
 *   Rec: Until more info provided, allow for connection types except LAN
 
 **(NTP) - Internet time servers**
 
 *   Cell network devices can still obtain current time/date from cell network even if this is blocked
 *   Rec: Block for cell devices, Allow for connection types except LAN for other devices
 
 **(shell) - Linux shell**
 
 *   Cases where it needs to be enabled for connection types except LAN (WiFi, Cell data)
 *   Rec: Allow for connection types except LAN
 
 **(Tethering) - DHCP+DNS services**
 
 *   If blocked, will not be able to tether over the connection types blocked
 *   Rec: Block if not tethering, otherwise enable only for connection types being used for tethering
 
 **(vpn) - VPN Networking**
 
 *   If blocked, will not be able to use a VPN over the connection types blocked
 *   Rec: Block if not using a VPN, otherwise enable only for connection types being used to connect to the VPN
 
 **ConfigUpdater**
 
 *   Function unclear
 *   Rec: Allow for WiFi, block for other connection types
 
 **Downloads, Media Storage, Download Manager**
 
 *   Required to for apps to download content
 *   Rec: Allow for connection types except LAN
 
 **DRM Service**
 
 *   If blocked, cannot view content that requires DRM
 *   Rec: Block if not viewing any DRM content, otherwise allow for connection type(s) being used
 
 **GoogleAccount Manager, Google Play services, et al.**
 
 *   If blocked, cannot use Google apps
 *   Rec: Block if not using Google apps, otherwise allow for all connection types except LAN
 
 **Google Partner Setup**
 
 *   No one seems to know what this does and what data it transmits/receives
 *   Rec: Block
 
 **Google Play Store**
 
 *   If blocked, cannot use the Google Play store
 *   Rec: Block if not using the Google Play store, otherwise allow for all connection types except LAN
 
 **Market Feedback Agent**
 
 *   If blocked, cannot use Google to submit feedback to app developers when apps crash
 *   Rec: Block if not using the Google Play store, otherwise allow for all connection types except LAN
 
 **PacProcessor**
 
 *   Unclear what this does and what data it transmits/receives
 *   Rec: Block
 
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

