#### Beta article ... do not edit until it's done. THX!
Secret Codes of Android Phones


This information is intended for experienced users. It is not intended for basic users, hackers, or mobile thieves. Please do not try any of following methods if you are not familiar with mobile phones. We'll not be responsible for the use or misuse of this information, including loss of data or hardware damage. So use it at your own risk.


May not working on all devices or roms! Most codes are for Samsung phones and roms. But good to know for e.g. bugreports!

*#32489#       | Cipher Info <br>
*#0011#        | Network Info <br>
*#197328640#   | General Service Mode <br>
*#745#         | RIL Dump Menu <br>
*#746#         | Debug Dump Menu <br>
*#06#          | Shows IMEI (International Mobile Equipment Identity) number <br>
*#1234#        | Software version <br>
*#2222#        | H/W Version <br>
*#197328640#   | Service Mode <br>
*#5282837#     | Java Version <br>
*#44336#       | Software Version Info <br>
*#2263#        | RF Band Selection & Network modes select <br>
*#3282727336*# | (Data Usage Status) <br>

*#6984125*#    | Service-Menu: “Dial” <br>
*#9072641*#    | Master-Key for ‘Internals’ menu (for Vodafone-branded phones use #3971258*#) <br>
*#0228#        | Realtime battery-info <br>


*#1575#        | GPS Control Menu <br>

*#232337#      | Bluetooth ID <br>
*#*3888#       | Bluetooth Test mode <br>
*#232337#      | Bluetooth MAC Adress <br>


*#232338#      | WLAN/WiFi MAC Address <br>
*#232339#      | WLAN Test Mode <br>
*#526# /*#528# | WLAN Engineering Mode <br>


*#301279#      | HSDPA/HSUPA Control Menu


command line via direct URI broadcast call: <br>
am broadcast -a android.provider.Telephony.SECRET_CODE -d android_secret_code://1111


So how do you find new codes?<br>
Well Google it! Here are the most recommend tools:<br>
* [jad](http://www.varaneckas.com/jad/) 
* [sgs2toext4](http://forum.xda-developers.com/attachment.php?attachmentid=645192&d=1309768531)


## Some other useful links
* [[REF][INFO][R&D] "Secret Codes" and other hidden features | XDA-Forum](http://forum.xda-developers.com/showthread.php?t=1687249) 