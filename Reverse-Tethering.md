```bash
# Tested working on Android 4.4.4 up to 5.x
# Original posted over here: http://forum.xda-developers.com/showthread.php?t=2287494
# All credits belongs to: class101
# This also requires the Xposed framework to be installed:
# http://repo.xposed.info/module/com.lemonsqueeze.fakewificonnection
# Download the entire package: https://mega.nz/#!QQ0zFK6I!GwnGj-zF7JnuMt9r0FEhq9NSX8ktX9Urvffv5mSfTlk
# App: https://play.google.com/store/apps/details?id=com.floriandraschbacher.reversetethering.free

# Ensure in AFWall+ that the incoming LAN option was checked together with root and LAN (for DHCP).
# Nexus 6 needs -> adb shell: settings put global tether_dun_required 0

IP=192.168.137.101			# ip of the rndis interface (if using Windows Internet Connection Sharing usually set to an ip in the 192.168.137.x range, or your home network range if using a Network Bridge like 192.168.1.x)
NETMASK=24					# netmask of the rndis interface (if you don't know this setting set it to 24, 255.255.255.255 = 32 | 255.255.255.0 = 24 | 255.255.0.0 = 16 | 255.0.0.0 = 8)
GATEWAY=192.168.137.1		# gateway of the rndis interface (main route, if using Windows Internet Connection Sharing usually set to 192.168.137.1, or your home internet box if using a Network Bridge like 192.168.1.1)
DNS1=8.8.8.8				# domain name resolution (google public dns1 = 8.8.8.8, but should be faster to your home internet box like 192.168.1.1)
DNS2=8.8.4.4				# domain name resolution (google public dns2 = 8.8.4.4)

USE_DHCP=0					# loads the DHCP server option of dnsmasq (not required, defaults to 0)
DHCP_FROM=192.168.137.10	# ignored if USE_DHCP=0
DHCP_TO=192.168.137.90		# ignored if USE_DHCP=0

# Not to need to edited after these lines  
echo -- rndis0: setting usb mode to rndis --
setprop sys.usb.config 'rndis'
echo -- rndis0: adding ip rule --
ip rule add from all lookup main
echo -- rndis0: flushing interface --
ip addr flush dev rndis0
echo -- rndis0: setting ip --
ip addr add $IP/$NETMASK dev rndis0
echo -- rndis0: starting the interface --
ip link set rndis0 up
echo -- rndis0: setting route --
#ip route add default via ${GATEWAY} dev rndis0
busybox route add -net 0.0.0.0 netmask 0.0.0.0 gw $GATEWAY dev rndis0
#echo -- rndis0: (optional) setting iptables --
#iptables -t nat -I POSTROUTING 1 -o rndis0 -j MASQUERADE
#echo -- rndis0: (optional) setting ip_forward --
#echo 1 > /proc/sys/net/ipv4/ip_forward
echo -- rndis0: setting properties --
setprop net.dns1 $DNS1
setprop net.dns2 $DNS2
setprop net.rndis0.dns1 $DNS1
setprop net.rndis0.dns2 $DNS2
setprop net.rndis0.gw $GATEWAY
setprop net.rndis0.gateway $GATEWAY
killall dnsmasq &> /dev/null
if [ $USE_DHCP = 1 ]; then
	echo -- rndis0: starting dnsmasq with dhcp --
	dnsmasq --no-poll --pid-file --interface=rndis0 --interface=wlan0 --interface=rmnet0 --bogus-priv --filterwin2k --no-resolv --server=${DNS1} --server=${DNS2} --cache-size=1000 --dhcp-range=${DHCP_FROM},${DHCP_TO} --dhcp-lease-max=253 --dhcp-authoritative --dhcp-leasefile=/cache/usb_tether_dnsmasq.leases < /dev/null
else
	echo -- rndis0: starting dnsmasq without dhcp --
	dnsmasq --no-poll --pid-file --interface=rndis0 --interface=wlan0 --interface=rmnet0 --bogus-priv --filterwin2k --no-resolv --server=${DNS1} --server=${DNS2} < /dev/null
fi
```

![Step 1](http://fs5.directupload.net/images/151011/8akgee24.png)
![Step 2](http://fs5.directupload.net/images/151011/cpjk35b5.png)
![Step 3](http://fs5.directupload.net/images/151011/sycq84ik.png)


:grey_exclamation: There is no app which proper work on all systems, so do not waste your time searching for them, the _best_ solution is that script. :grey_exclamation: