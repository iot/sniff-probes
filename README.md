
其原理是wifi被动侦测,我们手机wifi功能在打开的时候,会后台不停地探测手机周边是否有可以连接的wifi路由器,这个探测的动作,就会被openwrt无线路由器里执行的tcpdump脚本记录到.就算你手机当时没有连上无线路由器,仅仅是靠近该路由器你都被记录了,然后按第二个脚本触发mqtt命令

# Sniff Probes

Plug-and-play bash script for sniffing 802.11 probes requests. 

## What are Probe Requests?

Probe requests are an 802.11 WIFI packet type that function to automatically connect network devices to the wireless access points (APs) that they have previously associated with. Whenever a phone, computer, or other networked device has Wi-Fi enabled, but is not connected to a network, it is constantly "probing"; openly broadcating the network names (SSIDs) of previously connected APs. Because wireless access points have unique and often personal network names, it is easy to identify the device owner by recognizing the names of networks they frequently connect to.

For a creative application of probe request capture, see [ProbeKit](https://github.com/brannondorsey/ProbeKit). 

## Sniffing Probe Requests

```bash
# Type "ifconfig" to list available network devices.
# Wireless devices generally start with a "w"
IFACE=wlan0 ./sniff-probes.sh
```

```
00:00:19 -88dBm 00:0a:e2:1f:28:ab "cvteststation01"
00:00:19 -89dBm 00:0a:e2:1f:28:ab "cvteststation01"
00:00:22 -85dBm 5c:aa:fd:20:23:41 "Sonos_pZkIex0zatRvhdJTAifLzmatdh"
00:00:42 -86dBm f4:f5:d8:28:bc:26 "NETGEAR85-5G"
00:00:46 -89dBm f4:f5:d8:28:bc:26 "NETGEAR85-5G"
00:00:48 -84dBm f4:f5:d8:06:19:40 "Pamplona Running Club"
00:01:00 -92dBm 54:60:09:40:56:32 "seawhale"
00:01:13 -87dBm 38:63:bb:d1:6a:b7 "offline"
00:01:25 -83dBm 5c:aa:fd:20:23:41 "Sonos_pZkIex0zatRvhdJTAifLzmatdh"
```
Requires **tcpdump** and **gawk** (GNU awk). Both of these packages are installed on many *nix systems by default, but if they aren't you will have to install them manually. Your wireless device must also support monitor mode. Here is [a list of WiFi cards that support monitor mode](https://www.wirelesshack.org/best-kali-linux-compatible-usb-adapter-dongles-2016.html) (2018).

Prints `timetamp`, `signal strength`, `sender MAC address` and `SSID` to screen. Saves output as a space-delimeted "csv" to `probes.txt` by default.

Additional options:

```bash
IFACE=wlan0 OUTPUT=output.txt CHANNEL_HOP=1 ./sniff-probes.sh
```

`CHANNEL_HOP=1` enables channel hoping on `IFACE` every two seconds. This is used to increase the number of probes captured. Disabled by default.
