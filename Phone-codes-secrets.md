This information is intended for experienced users. It is not intended for basic users, hackers, or mobile thieves. Please do not try any of following methods if you are not familiar with mobile phones. We'll not be responsible for the use or misuse of this information, including loss of data or hardware damage. So use it at your own risk!

May not working on all devices or ROM's! Most codes are tested for Samsung phones only.


| Code | Info | Comment  |
| :--- | :----: | ----: |
| *#06# | Show IMEI number | Cell  |
| *#0*# |  Enter the service menu on newer phones like Galaxy S III | Only Samsung phones |
| *#*#4636#*#* | Phone information, usage statistics and battery | Cell  |
| *#*#34971539#*#* | Detailed camera information | Cell  |
| *#*#273282*255*663282*#*#* | Immediate backup of all media files | Only on Stock/AOSP ROM's|
| *#*#197328640#*#* | Enable test mode for service | Cell  |
| *#*#232339#*#* | Wireless LAN tests | Cell  |
| *#*#0842#*#* | Backlight/vibration test | Cell |
| *#*#2664#*#* | Test the touchscreen | Only on AOSP |
| *#*#1111#*#* | FTA software version | 1234 in the same code will give PDA and firmware version |
| *#12580*369# | Software and hardware info | Cell |
| *#9090# | Diagnostic configuration | Only OEM |
| *#872564# | USB logging control | Cell  |
| *#9900# | System dump mode | Only OEM |
| *#301279# | HSDPA/HSUPA Control Menu | Cell |
| *#*#1234#*#* | Launch SuperSU from Dialer | Only with installed SuperSU (alternative: *#*#7873778#*#*) |
| *#7465625# | View phone lock status | Cell  |
| *#*#7780#*#* | Reset the /data partition to factory state  | !!!!! Be careful !!!!! |
| *2767*3855# | Format device to factory state (will delete everything on phone) | !!!!! Be careful !!!!! |
| ##7764726 | Hidden service menu for Motorola Droid | Only on Motorola Droid phones |
| *#*#7594#*#* | Enable direct powering down of device once this code is entered | Cell  |
| *#32489# | Cipher Info | Cell  |
| *#0011# | Network Info | Right |
| *#197328640# | Cell | Cell  |
| *#745# | General Service Mode | Right |
| *#746# | RIL Dump Menu | Cell  |
| *#06#  | Debug Dump Menu | Right |
| *#1234#  | Shows IMEI (International Mobile Equipment Identity) number | Cell  |
| *#2222#  | Display Software version | Right |
| *#197328640#  | Shows Hardware Version | Cell |
| *#5282837# | Service Mode | Right |
| *#44336# | Java Version | Cell  |
| *#2263# | Software Version Info (OS) | Right |
| *#3282727336# | RF Band Selection & Network modes select | Only on Stock Lollipop (Samsung) |
| #6984125# | Shows Data Usage Status| Right |
| #9072641#  | Service-Menu: “Dial” | Cell  |
| *#0228# | Master-Key for ‘Internals’ menu | For Vodafone-branded phones try #3971258# |
| *#1575# | Realtime battery-info | Cell  |
| *#232337# | GPS Control Menu | Right |
| *#3888# | Show Bluetooth ID | Cell  |
| #232337#  | Bluetooth Test mode | Right |
| *#232338# | Bluetooth MAC Address | Cell  |
| *#232339# | WLAN/WiFi MAC Address | Right |
| *#526# or #528# | WLAN Engineering Mode | Cell  |
| #301279# | HSDPA/HSUPA Control Menu | Right |
| *##4636## | Device/Wifi/Battery/User stats | Cell  |
| *#*#273283*255*663282*#*#* | Make a quick backup of all the media files on your Android device | Cell |
| *#*#232338#*#* | Shows Wi-Fi MAC address | Right |
| *#*#1472365#*#* | Perform a quick GPS test | Cell  |
| *#*#1575#*#* | For a more advanced GPS test | Cell  |
| *#*#0283#*#* | Perform a packet loopback test | Right |
| *#*#0*#*#* | Run an LCD display test | Cell  |
| *#*#0289#*#* | Run Audio test | Cell  |
| *#*#2663#*#* | Show device’s touch-screen version | Cell  |
| *#*#0588#*# | Perform a proximity sensor test | Cell  |
| *#*#3264#*#* | Show RAM version | Cell  |
| *#*#232337#*# | Show device’s Bluetooth address | Cell  |
| *#*#7262626#*#* | Perform a field test | Cell  |
| *#*#8255#*#* | Monitor Google Talk service | Cell  |
| *#*#4986*2650468#*#* | Show Phone, Hardware, PDA, RF Call Date firmware info | Cell  |
| ##778 (+call) | Show EPST menu | Only on HTC phones |
| ##786# | Reverse Logistics Support | Only on HTC phones |
| *#*#4636#*#* | Show HTC info menu | Only on HTC phones |
| *#*#3424#*#* | Run HTC function test program | Only HTC phones |

command line via direct URI broadcast call: <br>
> am broadcast -a android.provider.Telephony.SECRET_CODE -d android_secret_code://1111


## Reference
* [[REF][INFO][R&D] "Secret Codes" and other hidden features | XDA-Forum](http://forum.xda-developers.com/showthread.php?t=1687249)
* [Hidden Android Secret Codes For Samsung, HTC, Motorola, Sony, LG And Other Devices (redmondpie.com)](http://www.redmondpie.com/hidden-android-secret-codes-for-samsung-htc-motorola-sony-lg-and-other-devices/#)

_Final version 02.01.2016_