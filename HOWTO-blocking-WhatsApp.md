Index
-----

* [Intro](#intro)
* [IP's](#ips)
* [Ports](#ports)
* [iptables](#iptables)
* [Encryption](#encryption)
* [Important](#important)
* [Useful Links](#useful-links)

Intro
--------

Since WhatsApp, Telegram, Threema, WeChat and other chat clients are very popular this article is an example how to block such messenger connections. Besides the ISP aspect - WhatsApp or other _insecure_ clients are already blocked by default in some countries (but not because of security reasons, more to block outgoing communications to others e.g. journalists). 

This is not a guarantee that it totally blocks all connections, because it's not that easy, since there are a bunch of IP's and domains that may connected to WhatsApp.Inc but it may helps to block the most important parts. All was captured via Wireshark. 


```bash
//iOS
c.whatsapp.net
c1.whatsapp.net
c2.whatsapp.net //phone number check
c3.whatsapp.net //initial login, this only used port 5222
c4.whatsapp.net
c5.whatsapp.net // Backup
c1.whatsapp.net
c2.whatsapp.net
c3.whatsapp.net //(all con. stuff are for the handshakes -> ^^ over 443 or 5222)
c4.whatsapp.net
c5.whatsapp.net
c6.whatsapp.net
c7.whatsapp.net
c8.whatsapp.net
c9.whatsapp.net
c10.whatsapp.net
//Android OS
e.whatsapp.net
e1.whatsapp.net
e2.whatsapp.net
e3.whatsapp.net //(all c. stuff are for the handshakes -> ^^ over 443 or 5222)
e4.whatsapp.net //initial
e5.whatsapp.net //initial
e6.whatsapp.net //initial
e7.whatsapp.net
e8.whatsapp.net
e9.whatsapp.net
e10.whatsapp.net
e11.whatsapp.net
e12.whatsapp.net
e13.whatsapp.net
e14.whatsapp.net
e15.whatsapp.net
e16.whatsapp.net
e17.whatsapp.net
e18.whatsapp.net
e19.whatsapp.net
e20.whatsapp.net
//Web <-> Client
s.whatsapp.net
s1.whatsapp.net
s2.whatsapp.net
s3.whatsapp.net //(all c. stuff are for the handshakes -> ^^ over 443 or 5222)
s4.whatsapp.net
s5.whatsapp.net
s6.whatsapp.net
s7.whatsapp.net
s8.whatsapp.net
s9.whatsapp.net
s10.whatsapp.net
https://sro.whatsapp.net/client/iphone/iq.php (<- for post-request -> sro.whatsapp.net)
https://sro.whatsapp.net/client/android/iq.php (^^)
static.reverse.softlayer.com (main/status/auth/..)

WhatsApp.com site only (A record):
192.155.212.203
184.173.147.38
184.173.147.39
192.155.212.202

NS records:
ns2.p13.dynect.net 204.13.250.13
ns1.p13.dynect.net 208.78.70.13
ns3.p13.dynect.net 208.78.71.13
ns4.p13.dynect.net 204.13.251.13

Webchat (web.whatsapp.com):
158.85.224.171
158.85.224.176
158.85.224.179
158.85.224.180
158.85.224.175
158.85.224.178
158.85.224.172
158.85.224.173
158.85.224.177
158.85.224.174

Crash Log
android-crashlog.whatsapp.net
```

IP's
------------

```bash
209.95.33.68 (ipv4 only but there is no ipv6 atm?)
208.43.115.202
208.43.122.131 - 208.43.122.135
208.43.244.168
208.43.244.169
208.43.244.170
208.43.244.171
208.43.244.172
208.43.244.174
208.43.244.175
208.43.96.4
208.43.96.5
208.43.96.6
208.43.96.7
208.43.97.156
192.155.212.202
192.155.212.203
184.172.19.64
184.172.19.65
184.172.19.66
184.172.19.69
184.172.19.70
184.172.19.71
184.172.19.79
184.172.19.81
184.172.19.84
184.172.19.86
184.172.19.89
184.172.19.92
184.172.19.94
184.172.19.95
184.173.136.64
184.173.136.65
184.173.136.66
184.173.136.67
184.173.136.68
184.173.136.69
184.173.136.70
184.173.136.71
184.173.136.72
184.173.136.73
184.173.136.74
184.173.136.75
184.173.136.76
184.173.136.77
184.173.136.79
184.173.136.80
184.173.136.81
184.173.136.82
184.173.136.83
184.173.136.84
184.173.136.85
184.173.136.86
184.173.136.87
184.173.136.88
184.173.136.89
184.173.136.90
184.173.136.91
184.173.136.93
184.173.136.94
184.173.136.95
184.173.147.38
184.173.147.53
184.173.147.39
174.123.168.2
174.37.240.116
108.168.174.7
75.119.222.13
64.233.162.141
50.22.198.205
50.22.199.44
50.22.203.212
50.22.225.69
50.22.210.128
50.22.210.129
50.22.210.130
50.22.210.131
50.22.210.132
50.22.210.133
50.22.210.134
50.22.210.135
50.22.210.136
50.22.210.137
50.22.210.138
50.22.210.141
50.22.210.142
50.22.210.143
50.22.210.144
50.22.210.145
50.22.210.146
50.22.210.147
50.22.210.148
50.22.210.149
50.22.210.150
50.22.210.151
50.22.210.152
50.22.210.153
50.22.210.154
50.22.210.155
50.22.210.158
50.22.210.159
50.22.210.32
50.22.210.33
50.22.213.140
50.22.213.141
50.22.213.142
50.22.213.143
50.22.227.220
50.22.227.222
50.22.227.224
50.22.227.225
50.22.227.226
50.22.227.227
50.22.235.124
50.22.235.125
50.22.235.126
50.22.235.127
50.23.142.160
50.23.142.161
50.23.142.162
50.23.142.163
50.23.142.168
50.23.142.169
50.23.142.170
50.23.142.173
50.23.142.174
50.23.142.175
50.23.142.176
50.23.142.177
50.23.142.180
50.23.142.181
50.23.142.182
50.23.142.190
50.23.142.191
50.97.57.148

IP's v6
WhatsApp does not support IPv6 only yet
```

Ports
--------

The following ports are involved: 
```bash
80
443
5222
5223
5228
5060 and alternative 5064 for SIP/Voip
Proxy to redirect from 80 to 8080 (optional)
```

IPTables
--------

```bash
iptables -I FORWARD -m string --algo bm --string "whatsapp.com" -j DROP
iptables -I FORWARD -m string --algo bm --string "whatsapp.net" -j DROP
iptables -I FORWARD -s 17.173.66.102/24 -j DROP
iptables -I FORWARD -s 17.173.66.179 -j DROP
iptables -I FORWARD -s 158.85.58.23/24 -j DROP
iptables -I FORWARD -s 158.85.58.113 -j DROP
iptables -I FORWARD -s 158.85.58.122 -j DROP
iptables -I FORWARD -s 184.173.136.85 -j DROP
iptables -I FORWARD -s 184.173.136.66 -j DROP
iptables -I FORWARD -s 184.172.19.94 -j DROP
iptables -I FORWARD -s 184.173.136.91 -j DROP
iptables -I FORWARD -s 50.22.210.150 -j DROP
iptables -I FORWARD -s 50.22.213.141 -j DROP
iptables -I FORWARD -s 50.22.210.143 -j DROP
iptables -I FORWARD -s 184.173.136.74 -j DROP
iptables -I FORWARD -s 50.22.227.222 -j DROP
iptables -I FORWARD -s 184.173.136.94 -j DROP
iptables -I FORWARD -s 50.22.210.148 -j DROP
iptables -I FORWARD -s 184.173.136.72 -j DROP
iptables -I FORWARD -s 50.23.142.175 -j DROP
iptables -I FORWARD -s 184.173.136.77 -j DROP
iptables -I FORWARD -s 50.23.142.180 -j DROP
iptables -I FORWARD -s 50.22.210.145 -j DROP
iptables -I FORWARD -s 184.173.136.65 -j DROP
iptables -I FORWARD -s 50.22.235.126 -j DROP
iptables -I FORWARD -s 208.43.96.6 -j DROP
iptables -I FORWARD -s 50.22.210.130 -j DROP
iptables -I FORWARD -s 208.43.96.4 -j DROP
iptables -I FORWARD -s 50.22.210.128 -j DROP
iptables -I FORWARD -s 208.43.244.170 -j DROP
iptables -I FORWARD -s 184.172.19.84 -j DROP
iptables -I FORWARD -s 50.23.142.190 -j DROP
iptables -I FORWARD -s 184.173.136.76 -j DROP
iptables -I FORWARD -s 50.22.210.155 -j DROP
iptables -I FORWARD -s 184.173.136.88 -j DROP
iptables -I FORWARD -s 50.22.235.124 -j DROP
iptables -I FORWARD -s 50.22.210.136 -j DROP
iptables -I FORWARD -s 184.172.19.81 -j DROP
iptables -I FORWARD -s 184.173.136.83 -j DROP
iptables -I FORWARD -s 50.23.142.173 -j DROP
iptables -I FORWARD -s 184.173.136.90 -j DROP
iptables -I FORWARD -s 184.173.136.86 -j DROP
iptables -I FORWARD -s 50.22.213.142 -j DROP
iptables -I FORWARD -s 50.22.210.154 -j DROP
iptables -I FORWARD -s 184.172.19.69 -j DROP
iptables -I FORWARD -s 50.22.210.137 -j DROP
iptables -I FORWARD -s 208.43.97.156 -j DROP
iptables -I FORWARD -s 50.22.210.151 -j DROP
iptables -I FORWARD -s 184.173.136.95 -j DROP
iptables -I FORWARD -s 50.22.210.131 -j DROP
iptables -I FORWARD -s 50.22.227.220 -j DROP
iptables -I FORWARD -s 184.173.136.89 -j DROP
iptables -I FORWARD -s 184.172.19.64 -j DROP
iptables -I FORWARD -s 184.172.19.89 -j DROP
iptables -I FORWARD -s 50.22.210.144 -j DROP
iptables -I FORWARD -s 184.173.136.73 -j DROP
iptables -I FORWARD -s 184.173.136.80 -j DROP
iptables -I FORWARD -s 184.172.19.79 -j DROP
iptables -I FORWARD -s 184.172.19.66 -j DROP
iptables -I FORWARD -s 184.172.19.95 -j DROP
iptables -I FORWARD -s 50.23.142.162 -j DROP
iptables -I FORWARD -s 50.23.142.182 -j DROP
iptables -I FORWARD -s 184.173.136.81 -j DROP
iptables -I FORWARD -s 184.172.19.71 -j DROP
iptables -I FORWARD -s 184.173.136.82 -j DROP
iptables -I FORWARD -s 184.173.136.75 -j DROP
iptables -I FORWARD -s 50.23.142.163 -j DROP
iptables -I FORWARD -s 50.22.213.140 -j DROP
iptables -I FORWARD -s 184.173.136.79 -j DROP
iptables -I FORWARD -s 50.22.210.147 -j DROP
iptables -I FORWARD -s 50.23.142.174 -j DROP
iptables -I FORWARD -s 50.22.210.152 -j DROP
iptables -I FORWARD -s 50.22.210.141 -j DROP
iptables -I FORWARD -s 50.22.227.226 -j DROP
iptables -I FORWARD -s 50.22.210.142 -j DROP
iptables -I FORWARD -s 50.22.210.146 -j DROP
iptables -I FORWARD -s 50.23.142.169 -j DROP
iptables -I FORWARD -s 50.23.142.170 -j DROP
iptables -I FORWARD -s 184.172.19.65 -j DROP
iptables -I FORWARD -s 50.22.210.33 -j DROP
iptables -I FORWARD -s 50.22.227.224 -j DROP
iptables -I FORWARD -s 184.173.136.71 -j DROP
iptables -I FORWARD -s 50.22.210.149 -j DROP
iptables -I FORWARD -s 50.23.142.160 -j DROP
iptables -I FORWARD -s 50.23.142.177 -j DROP
iptables -I FORWARD -s 50.22.210.153 -j DROP
iptables -I FORWARD -s 208.43.244.171 -j DROP
iptables -I FORWARD -s 208.43.244.174 -j DROP
iptables -I FORWARD -s 184.173.136.64 -j DROP
iptables -I FORWARD -s 50.23.142.181 -j DROP
iptables -I FORWARD -s 50.22.210.158 -j DROP
iptables -I FORWARD -s 50.22.199.44 -j DROP
iptables -I FORWARD -s 184.173.136.84 -j DROP
iptables -I FORWARD -s 208.43.244.169 -j DROP
iptables -I FORWARD -s 184.172.19.92 -j DROP
iptables -I FORWARD -s 50.22.203.212 -j DROP
iptables -I FORWARD -s 50.22.227.227 -j DROP
iptables -I FORWARD -s 50.22.210.135 -j DROP
iptables -I FORWARD -s 184.172.19.86 -j DROP
iptables -I FORWARD -s 50.22.227.225 -j DROP
iptables -I FORWARD -s 208.43.96.7 -j DROP
iptables -I FORWARD -s 208.43.244.168 -j DROP
iptables -I FORWARD -s 174.37.240.116 -j DROP
iptables -I FORWARD -s 184.173.136.69 -j DROP
iptables -I FORWARD -s 50.23.142.191 -j DROP
iptables -I FORWARD -s 184.172.19.70 -j DROP
iptables -I FORWARD -s 208.43.96.5 -j DROP
iptables -I FORWARD -s 184.173.136.70 -j DROP
iptables -I FORWARD -s 50.22.235.127 -j DROP
iptables -I FORWARD -s 50.22.198.205 -j DROP
iptables -I FORWARD -s 50.22.210.132 -j DROP
iptables -I FORWARD -s 184.173.136.93 -j DROP
iptables -I FORWARD -s 50.22.213.143 -j DROP
iptables -I FORWARD -s 184.173.136.67 -j DROP
iptables -I FORWARD -s 50.22.210.138 -j DROP
iptables -I FORWARD -s 184.173.136.68 -j DROP
iptables -I FORWARD -s 50.23.142.168 -j DROP
iptables -I FORWARD -s 208.43.244.175 -j DROP
iptables -I FORWARD -s 50.23.142.176 -j DROP
iptables -I FORWARD -s 184.173.136.87 -j DROP
iptables -I FORWARD -s 50.22.210.129 -j DROP
iptables -I FORWARD -s 50.22.210.134 -j DROP
iptables -I FORWARD -s 50.23.142.161 -j DROP
iptables -I FORWARD -s 208.43.244.172 -j DROP
iptables -I FORWARD -s 50.22.210.32 -j DROP
iptables -I FORWARD -s 50.22.210.133 -j DROP
iptables -I FORWARD -s 50.22.235.125 -j DROP
iptables -I FORWARD -s 50.22.210.159 -j DROP
iptables -I FORWARD -s 17.173.66.180 -j DROP
iptables -I FORWARD -s 17.172.232.150 -j DROP
iptables -I FORWARD -s 17.173.66.181 -j DROP
iptables -I FORWARD -s 158.85.58.9 -j DROP
iptables -I FORWARD -s 158.85.58.58 -j DROP
iptables -I FORWARD -s 17.173.66.104 -j DROP
iptables -I FORWARD -s 158.85.58.36 -j DROP
iptables -I FORWARD -s 17.143.162.225 -j DROP
iptables -I FORWARD -s 17.143.162.225 -j DROP
iptables -I FORWARD -s 158.85.58.71 -j DROP
iptables -I FORWARD -s 17.172.232.61 -j DROP
iptables -I INPUT -s 5.153.52.248/29 -j DROP
iptables -I INPUT -s 50.22.194.224/27 -j DROP
iptables -I INPUT -s 50.22.198.204/30 -j DROP
iptables -I INPUT -s 50.22.210.32/30 -j DROP
iptables -I INPUT -s 50.22.210.128/27 -j DROP
iptables -I INPUT -s 50.22.225.64/27 -j DROP
iptables -I INPUT -s 50.22.231.32/27 -j DROP
iptables -I INPUT -s 50.22.235.248/30 -j DROP
iptables -I INPUT -s 50.22.240.160/27 -j DROP
iptables -I INPUT -s 50.23.90.128/27 -j DROP
iptables -I INPUT -s 50.97.57.128/27 -j DROP
iptables -I INPUT -s 75.126.39.32/27 -j DROP
iptables -I INPUT -s 108.168.174.0/27 -j DROP
iptables -I INPUT -s 108.168.176.192/26 -j DROP
iptables -I INPUT -s 108.168.177.0/27 -j DROP
iptables -I INPUT -s 108.168.180.96/27 -j DROP
iptables -I INPUT -s 108.168.254.65/32 -j DROP
iptables -I INPUT -s 108.168.255.224/32 -j DROP
iptables -I INPUT -s 108.168.255.227/32 -j DROP
iptables -I INPUT -s 119.81.51.216/29 -j DROP
iptables -I INPUT -s 158.85.0.96/27 -j DROP
iptables -I INPUT -s 158.85.5.192/27 -j DROP
iptables -I INPUT -s 158.85.73.128/27 -j DROP
iptables -I INPUT -s 158.85.73.176/28 -j DROP
iptables -I INPUT -s 173.192.212.200/30 -j DROP
iptables -I INPUT -s 173.192.219.96/27 -j DROP
iptables -I INPUT -s 173.192.219.128/27 -j DROP
iptables -I INPUT -s 173.192.222.160/27 -j DROP
iptables -I INPUT -s 173.192.231.32/27 -j DROP
iptables -I INPUT -s 173.193.205.0/27 -j DROP
iptables -I INPUT -s 173.193.230.96/27 -j DROP
iptables -I INPUT -s 173.193.230.128/27 -j DROP
iptables -I INPUT -s 173.193.239.0/27 -j DROP
iptables -I INPUT -s 173.193.247.192/27 -j DROP
iptables -I INPUT -s 174.36.194.48/28 -j DROP
iptables -I INPUT -s 174.36.208.128/27 -j DROP
iptables -I INPUT -s 174.36.210.32/27 -j DROP
iptables -I INPUT -s 174.36.251.192/27 -j DROP
iptables -I INPUT -s 174.37.199.192/27 -j DROP
iptables -I INPUT -s 174.37.217.64/27 -j DROP
iptables -I INPUT -s 174.37.231.64/27 -j DROP
iptables -I INPUT -s 174.37.243.64/27 -j DROP
iptables -I INPUT -s 184.173.73.176/28 -j DROP
iptables -I INPUT -s 184.173.136.64/27 -j DROP
iptables -I INPUT -s 184.173.147.32/27 -j DROP
iptables -I INPUT -s 184.173.161.64/32 -j DROP
iptables -I INPUT -s 184.173.161.160/27 -j DROP
iptables -I INPUT -s 184.173.173.116/32 -j DROP
iptables -I INPUT -s 184.173.179.32/27 -j DROP
iptables -I INPUT -s 192.155.193.0/28 -j DROP
iptables -I INPUT -s 192.155.212.192/27 -j DROP
iptables -I INPUT -s 198.11.193.182/31 -j DROP
iptables -I INPUT -s 198.11.212.0/27 -j DROP
iptables -I INPUT -s 198.11.217.192/27 -j DROP
iptables -I INPUT -s 198.11.251.32/27 -j DROP
iptables -I INPUT -s 198.23.80.0/27 -j DROP
iptables -I INPUT -s 198.23.86.224/27 -j DROP
iptables -I INPUT -s 198.23.87.64/27 -j DROP
iptables -I INPUT -s 208.43.115.192/27 -j DROP
iptables -I INPUT -s 208.43.117.79/32 -j DROP
iptables -I INPUT -s 208.43.117.136/32 -j DROP
iptables -I INPUT -s 208.43.122.128/27 -j DROP
iptables -I INPUT -s 208.43.244.168/29 -j DROP
iptables -I OUTPUT -s 5.153.52.248/29 -j DROP
iptables -I OUTPUT -s 50.22.194.224/27 -j DROP
iptables -I OUTPUT -s 50.22.198.204/30 -j DROP
iptables -I OUTPUT -s 50.22.210.32/30 -j DROP
iptables -I OUTPUT -s 50.22.210.128/27 -j DROP
iptables -I OUTPUT -s 50.22.225.64/27 -j DROP
iptables -I OUTPUT -s 50.22.231.32/27 -j DROP
iptables -I OUTPUT -s 50.22.235.248/30 -j DROP
iptables -I OUTPUT -s 50.22.240.160/27 -j DROP
iptables -I OUTPUT -s 50.23.90.128/27 -j DROP
iptables -I OUTPUT -s 50.97.57.128/27 -j DROP
iptables -I OUTPUT -s 75.126.39.32/27 -j DROP
iptables -I OUTPUT -s 108.168.174.0/27 -j DROP
iptables -I OUTPUT -s 108.168.176.192/26 -j DROP
iptables -I OUTPUT -s 108.168.177.0/27 -j DROP
iptables -I OUTPUT -s 108.168.180.96/27 -j DROP
iptables -I OUTPUT -s 108.168.254.65/32 -j DROP
iptables -I OUTPUT -s 108.168.255.224/32 -j DROP
iptables -I OUTPUT -s 108.168.255.227/32 -j DROP
iptables -I OUTPUT -s 119.81.51.216/29 -j DROP
iptables -I OUTPUT -s 158.85.0.96/27 -j DROP
iptables -I OUTPUT -s 158.85.5.192/27 -j DROP
iptables -I OUTPUT -s 158.85.73.128/27 -j DROP
iptables -I OUTPUT -s 158.85.73.176/28 -j DROP
iptables -I OUTPUT -s 173.192.212.200/30 -j DROP
iptables -I OUTPUT -s 173.192.219.96/27 -j DROP
iptables -I OUTPUT -s 173.192.219.128/27 -j DROP
iptables -I OUTPUT -s 173.192.222.160/27 -j DROP
iptables -I OUTPUT -s 173.192.231.32/27 -j DROP
iptables -I OUTPUT -s 173.193.205.0/27 -j DROP
iptables -I OUTPUT -s 173.193.230.96/27 -j DROP
iptables -I OUTPUT -s 173.193.230.128/27 -j DROP
iptables -I OUTPUT -s 173.193.239.0/27 -j DROP
iptables -I OUTPUT -s 173.193.247.192/27 -j DROP
iptables -I OUTPUT -s 174.36.194.48/28 -j DROP
iptables -I OUTPUT -s 174.36.208.128/27 -j DROP
iptables -I OUTPUT -s 174.36.210.32/27 -j DROP
iptables -I OUTPUT -s 174.36.251.192/27 -j DROP
iptables -I OUTPUT -s 174.37.199.192/27 -j DROP
iptables -I OUTPUT -s 174.37.217.64/27 -j DROP
iptables -I OUTPUT -s 174.37.231.64/27 -j DROP
iptables -I OUTPUT -s 174.37.243.64/27 -j DROP
iptables -I OUTPUT -s 184.173.73.176/28 -j DROP
iptables -I OUTPUT -s 184.173.136.64/27 -j DROP
iptables -I OUTPUT -s 184.173.147.32/27 -j DROP
iptables -I OUTPUT -s 184.173.161.64/32 -j DROP
iptables -I OUTPUT -s 184.173.161.160/27 -j DROP
iptables -I OUTPUT -s 184.173.173.116/32 -j DROP
iptables -I OUTPUT -s 184.173.179.32/27 -j DROP
iptables -I OUTPUT -s 192.155.193.0/28 -j DROP
iptables -I OUTPUT -s 192.155.212.192/27 -j DROP
iptables -I OUTPUT -s 198.11.193.182/31 -j DROP
iptables -I OUTPUT -s 198.11.212.0/27 -j DROP
iptables -I OUTPUT -s 198.11.217.192/27 -j DROP
iptables -I OUTPUT -s 198.11.251.32/27 -j DROP
iptables -I OUTPUT -s 198.23.80.0/27 -j DROP
iptables -I OUTPUT -s 198.23.86.224/27 -j DROP
iptables -I OUTPUT -s 198.23.87.64/27 -j DROP
iptables -I OUTPUT -s 208.43.115.192/27 -j DROP
iptables -I OUTPUT -s 208.43.117.79/32 -j DROP
iptables -I OUTPUT -s 208.43.117.136/32 -j DROP
iptables -I OUTPUT -s 184.173.147.32/27 -j DROP
iptables -I OUTPUT -s 184.173.161.64/32 -j DROP
iptables -I OUTPUT -s 184.173.161.160/27 -j DROP
iptables -I OUTPUT -s 184.173.173.116/32 -j DROP
iptables -I OUTPUT -s 184.173.179.32/27 -j DROP
iptables -I OUTPUT -s 192.155.193.0/28 -j DROP
iptables -I OUTPUT -s 192.155.212.192/27 -j DROP
iptables -I OUTPUT -s 198.11.193.182/31 -j DROP
iptables -I OUTPUT -s 198.11.212.0/27 -j DROP
iptables -I OUTPUT -s 198.11.217.192/27 -j DROP
iptables -I OUTPUT -s 198.11.251.32/27 -j DROP
iptables -I OUTPUT -s 198.23.80.0/27 -j DROP
iptables -I OUTPUT -s 198.23.86.224/27 -j DROP
iptables -I OUTPUT -s 198.23.87.64/27 -j DROP
iptables -I OUTPUT -s 208.43.115.192/27 -j DROP
iptables -I OUTPUT -s 208.43.117.79/32 -j DROP
iptables -I OUTPUT -s 208.43.117.136/32 -j DROP
iptables -I OUTPUT -s 208.43.122.128/27 -j DROP
iptables -I OUTPUT -s 208.43.244.168/29 -j DROP
iptables -I FORWARD -s 5.153.52.248/29 -j DROP
iptables -I FORWARD -s 50.22.194.224/27 -j DROP
iptables -I FORWARD -s 50.22.198.204/30 -j DROP
iptables -I FORWARD -s 50.22.210.32/30 -j DROP
iptables -I FORWARD -s 50.22.210.128/27 -j DROP
iptables -I FORWARD -s 50.22.225.64/27 -j DROP
iptables -I FORWARD -s 50.22.231.32/27 -j DROP
iptables -I FORWARD -s 50.22.235.248/30 -j DROP
iptables -I FORWARD -s 50.22.240.160/27 -j DROP
iptables -I FORWARD -s 50.23.90.128/27 -j DROP
iptables -I FORWARD -s 50.97.57.128/27 -j DROP
iptables -I FORWARD -s 75.126.39.32/27 -j DROP
iptables -I FORWARD -s 108.168.174.0/27 -j DROP
iptables -I FORWARD -s 108.168.176.192/26 -j DROP
iptables -I FORWARD -s 108.168.177.0/27 -j DROP
iptables -I FORWARD -s 108.168.180.96/27 -j DROP
iptables -I FORWARD -s 108.168.254.65/32 -j DROP
iptables -I FORWARD -s 108.168.255.224/32 -j DROP
iptables -I FORWARD -s 108.168.255.227/32 -j DROP
iptables -I FORWARD -s 119.81.51.216/29 -j DROP
iptables -I FORWARD -s 158.85.0.96/27 -j DROP
iptables -I FORWARD -s 158.85.5.192/27 -j DROP
iptables -I FORWARD -s 158.85.73.128/27 -j DROP
iptables -I FORWARD -s 158.85.73.176/28 -j DROP
iptables -I FORWARD -s 173.192.212.200/30 -j DROP
iptables -I FORWARD -s 173.192.219.96/27 -j DROP
iptables -I FORWARD -s 173.192.219.128/27 -j DROP
iptables -I FORWARD -s 173.192.222.160/27 -j DROP
iptables -I FORWARD -s 173.192.231.32/27 -j DROP
iptables -I FORWARD -s 173.193.205.0/27 -j DROP
iptables -I FORWARD -s 173.193.230.96/27 -j DROP
iptables -I FORWARD -s 173.193.230.128/27 -j DROP
iptables -I FORWARD -s 173.193.239.0/27 -j DROP
iptables -I FORWARD -s 173.193.247.192/27 -j DROP
iptables -I FORWARD -s 174.36.194.48/28 -j DROP
iptables -I FORWARD -s 174.36.208.128/27 -j DROP
iptables -I FORWARD -s 174.36.210.32/27 -j DROP
iptables -I FORWARD -s 174.36.251.192/27 -j DROP
iptables -I FORWARD -s 174.37.199.192/27 -j DROP
iptables -I FORWARD -s 174.37.217.64/27 -j DROP
iptables -I FORWARD -s 174.37.231.64/27 -j DROP
iptables -I FORWARD -s 174.37.243.64/27 -j DROP
iptables -I FORWARD -s 184.173.73.176/28 -j DROP
iptables -I FORWARD -s 184.173.136.64/27 -j DROP
iptables -I FORWARD -s 184.173.147.32/27 -j DROP
iptables -I FORWARD -s 184.173.161.64/32 -j DROP
iptables -I FORWARD -s 184.173.161.160/27 -j DROP
iptables -I FORWARD -s 184.173.173.116/32 -j DROP
iptables -I FORWARD -s 184.173.179.32/27 -j DROP
iptables -I FORWARD -s 192.155.193.0/28 -j DROP
iptables -I FORWARD -s 192.155.212.192/27 -j DROP
iptables -I FORWARD -s 198.11.193.182/31 -j DROP
iptables -I FORWARD -s 198.11.212.0/27 -j DROP
iptables -I FORWARD -s 198.11.217.192/27 -j DROP
iptables -I FORWARD -s 198.11.251.32/27 -j DROP
iptables -I FORWARD -s 198.23.80.0/27 -j DROP
iptables -I FORWARD -s 198.23.86.224/27 -j DROP
iptables -I FORWARD -s 198.23.87.64/27 -j DROP
iptables -I FORWARD -s 208.43.115.192/27 -j DROP
iptables -I FORWARD -s 208.43.117.79/32 -j DROP
iptables -I FORWARD -s 208.43.117.136/32 -j DROP
iptables -I FORWARD -s 184.173.147.32/27 -j DROP
iptables -I FORWARD -s 184.173.161.64/32 -j DROP
iptables -I FORWARD -s 184.173.161.160/27 -j DROP
iptables -I FORWARD -s 184.173.173.116/32 -j DROP
iptables -I FORWARD -s 184.173.179.32/27 -j DROP
iptables -I FORWARD -s 192.155.193.0/28 -j DROP
iptables -I FORWARD -s 192.155.212.192/27 -j DROP
iptables -I FORWARD -s 198.11.193.182/31 -j DROP
iptables -I FORWARD -s 198.11.212.0/27 -j DROP
iptables -I FORWARD -s 198.11.217.192/27 -j DROP
iptables -I FORWARD -s 198.11.251.32/27 -j DROP
iptables -I FORWARD -s 198.23.80.0/27 -j DROP
iptables -I FORWARD -s 198.23.86.224/27 -j DROP
iptables -I FORWARD -s 198.23.87.64/27 -j DROP
iptables -I FORWARD -s 208.43.115.192/27 -j DROP
iptables -I FORWARD -s 208.43.117.79/32 -j DROP
iptables -I FORWARD -s 208.43.117.136/32 -j DROP
iptables -I FORWARD -s 208.43.122.128/27 -j DROP
iptables -I FORWARD -s 208.43.244.168/29 -j DROP

# Block Outgoing IP's
iptables -A "afwall" -d 208.43.244.168/29 -j REJECT

# Port-Range's
iptables -I FORWARD -m iprange --src-range 192.168.1.110-192.168.1.110 -j ACCEPT
iptables -I FORWARD -m iprange --dst-range 192.168.1.110-192.168.1.110 -j ACCEPT

# Per Domain (change rmnet0 to your interface wifi0 or whatever)
iptables -A INPUT -i rmnet0 -m string --algo bm --string "whatsapp.com" -j DROP
iptables -A OUTPUT -m string --algo bm --string "whatsapp.com" -j DROP
iptables -A FORWARD -i rmnet0 -m string --algo bm --string "whatsapp.com" -j DROP
```

Encryption
-----------

WhatsApp use the following encryption standard:
* Prosperity RC4 algorithm ([RFC 7465](https://tools.ietf.org/html/rfc7465))
* [Axolotl port](https://github.com/tgalal/python-axolotl) for the encryption itself like the one from TextSecure 

Pro:
* Mass surveillance isn't possible via Internet-Backbones since the key "never leave the device" (according to WhatsApp). But there is no proof.
* Needs some debugging/logging stuff to see what's going on, see [here](https://gist.github.com/fabsh/723240acce6916367865) Good for normal users, bad for advanced users or attackers.

Negative:
* Mass surveillance -> the Wa messages itself can be analyzed on the WhatsApp servers, since they can read the transport-secured messages (if no E2E!).
* RC4 is [known as vulnerable](https://community.qualys.com/blogs/securitylabs/2013/10/09/ssl-pulse-now-tracking-forward-secrecy-and-rc4) 
* Moxie Marlinspike implemented end-to-end encryption is not possible to analyze since there is no proof or source, so we must trust him if there is really a E2E encryption. It's possible that the encryption is not every time used (persistent). It's not possible to debug. 
* It's not possible to show or debug if the secret key is uploaded to any cloud like the Server.
It's not possible to see if the message is encrypted or in plain text since there is no indicator or a warning. 
* There are hidden APIs, unknown functions and _strange_ functions implemented and no one really knows why.
* Depending on the Status WhatsSpy and other _tools_ are able to track others if you know there numbers.


Some Alternatives:
* [Threema](https://threema.ch/en/) (closed source but no one found any problem atm)
* [Hoccer](http://hoccer.com/) (closed source but no one found any problem atm)
* [Surespot](https://surespot.me/) - [Source](https://github.com/surespot)
* [TextSecure](https://whispersystems.org/) - [Source](https://github.com/WhisperSystems/TextSecure)
* [Heml.is](https://heml.is/) - [Source?](https://twitter.com/HemlisMessenger/status/425250256293732352) 
* [Cryptocat](https://crypto.cat/) - [Source](https://github.com/cryptocat)
* [ChatSecure](https://chatsecure.org/) - [Source/FAQ](https://chatsecure.org/faq/)
* [Sicher](http://www.sicherapp.com/) - closed source
*  Against some rumors, [Telegram isn't secure](http://kiledjian.com/main/2014/3/17/telegram-messenger-isnt-secure) and other _mistakes_ like the [Math thing](http://unhandledexpression.com/2013/12/17/telegram-stand-back-we-know-maths/) - well it's [more secure compared to WhatsApp](https://github.com/overtake/telegram), but I would never trust someone with touch known _secure_ algorithm to "improve" the possible (theoretically) problems - this never ends up good!

Security research and tests:
* [Overview](https://missingm.co/2014/02/fighting-dishfire-the-state-of-mobile-cross-platform-encrypted-messaging/) and a scoreboard from EFF (The developer guys from HTTPS-Everywhere addon,[..]) is available over [here](https://www.eff.org/secure-messaging-scorecard).

Important
-----------

It's normally more then enough to block <code>e.whatsapp.net - e5.whatsapp.net</code>, because this is for the initial handshake which means if that fails you can't receive/send any message. You should try this first.


Useful Links
--------------

* [WhatsApp | Whatsapp.com](https://www.whatsapp.com/)
* [WHOIS WhatsApp | whois.com](http://www.whois.com/whois/whatsapp.com)
* [Open WhatsApp | openwhatsapp.org](http://www.openwhatsapp.org/)
* [Mass DMCA takedowns by WhatsApp | openwhatsapp.org](http://www.openwhatsapp.org/blog/2014/02/13/mass-dmca-takedowns-whatsapp/)
* [Unofficial WhatsApp API by perezdidac | GitHub.com](https://github.com/perezdidac/WhatsAPINet)
* [Unofficial WhatsApp API by venomous0x | GitHub.com](https://github.com/ukanth/afwall/wiki)
* [Unofficial WhatsApp Purple API by davidgfnet | GitHub.com](https://github.com/davidgfnet/whatsapp-purple)
* [Unofficial WhatsApp-Client yowsup | GitHub.com](https://github.com/tgalal/yowsup)
* [Whatsapp abused the DMCA to censor related projects from GitHub | boingboing.net](http://boingboing.net/2014/02/21/whatsapp-abused-the-dmca-to-ce.html)
* [All WhatsApp.Inc IP's only (no proof!) | ipdb.at](https://ipdb.at/org/WhatsApp_Inc)
* [All DNS Records + additional Domain Info | Robtex.com](https://www.robtex.com/q/x1?q=whatsapp.com&l=go)
* [Unofficial WhatsApp Beta Updater | Aptoide (javiersantos Store)](http://m.apps.store.aptoide.com/app/market/com.javiersantos.whatsappbetaupdater/6/9014714/Beta%20Updater%20for%20WhatsApp)
* [Is WhatsApp Down Right Now? Check | isitdownrightnow.com](http://www.isitdownrightnow.com/whatsapp.com.html)
* [WhatsApp IntoDNS | IntoDNS.com](http://www.intodns.com/whatsapp.com)
* [Extracting password from device | Chat-API by mgp25 on GitHub](https://github.com/mgp25/Chat-API/wiki/Extracting-password-from-device)
* [WhatsApp Web Wiki | wiki.dequis.org](http://wiki.dequis.org/notes/whatsapp_web/)
* [Keeping Tabs on WhatsApp's Encryption (Ger)| heise.de](http://www.heise.de/ct/artikel/Keeping-Tabs-on-WhatsApp-s-Encryption-2630361.html)
* [WhatsApp Wireshark plugin | GitHub.com](https://github.com/davidgfnet/wireshark-whatsapp)
* [Intercepting WhatsApp Messages led to Belgian Terror Arrest | arstechnica.com](http://arstechnica.com/tech-policy/2015/06/intercepted-whatsapp-messages-led-to-belgian-terror-arrests/)
* [WhatsApp exposed (2015) (.pdf) | webrtchacks.com](https://webrtchacks.com/wp-content/uploads/2015/04/WhatsappReport.pdf)
* [Iran's Judiciary Blocks WhatsApp, Other Communication Apps | ctvnews.ca](http://www.ctvnews.ca/sci-tech/iran-blocks-communication-apps-line-whatsapp-tango-1.2176978)

_Final Version 08.28.2015_