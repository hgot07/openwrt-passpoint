## Access Point

This document provides a brief explanation of 
configuring Hotspot 2.0 (Passpoint, more officially)
on OpenWrt to make an Passpoint-capable Access Point.
Tests have been done using some GL.iNet devices
including Mango, Beryl (both int./ext. PHYs),
Beryl AX, Slate AX, etc.

![GL.iNet Beryl & NETGEAR A6210](fig/beryl.jpg "GL.iNet Beryl & NETGEAR A6210")

### Requirements

- OpenWrt-compatible device with Passpoint-capable wireless device (PHY).
- OpenWrt 21.02, or newer, including wpad (hostapd) built with hs20 option.
- 802.1x infrastructure (RADIUS server).

### Overview

hostapd (in wpad package) needs to be built with hs20 option.
To check whether the program is capable of Hotspot 2.0, please try:

> \# strings /usr/sbin/wpad | grep hs20

If nothing shows up, that hostapd isn't capable of Hotspot 2.0.

The default package installed is normally wpad-basic (-wolfssl), 
which doesn't have Hotspot 2.0 support.
**You have to remove wpad-basic and install a full version of wpad
(eg. wpad-openssl).**
Unticking checkbox "Automatically remove unused dependencies" is 
strongly recommended when you remove wpad-basic.

**Note**: GL-MT3000 users should not try changing the iw package.
Removing it would break the Wi-Fi driver. 
If you cannot see the chip name in the LuCI's wireless menu, 
the system is broken and you need to burn the firmware again.

Unlike the hostapd configuration on a Linux box,
**hostapd.conf cannot be edited manually.
UCI (Unified Configuration Interface) auto-generates 
the hostapd.conf file.**

More specifically, **a shell script "/lib/netifd/hostapd.sh" 
will generate "/var/run/hostapd-phyX.conf" 
based on the wireless configuration file "/etc/config/wireless".**


### Hotspot 2.0 configuration
We assume that an SSID has already been configured
with WPA2/WPA3 Enterprise (802.1x).
Please refer to other documents for this configuration.

Hotspot 2.0 can be enabled by adding some option/list lines
to the "config wifi-iface 'wifinetX'" section.
An example is shown below.
Some lines need to be fixed according to your own service.

```
option iw_enabled '1'
option iw_interworking '1'
option iw_access_network_type '3'
option iw_internet '1'
option iw_disable_dgaf '1'
option iw_asra '0'
option iw_esr '0'
option iw_uesa '0'
option iw_venue_group '2'
option iw_venue_type '8'
option iw_hessid '00:00:00:01:02:03'
list iw_roaming_consortium 'xxyyzz0000'
list iw_nai_realm '0,example.com,13[5:6],21[2:4][5:7]'
list iw_nai_realm '0,example.org,13[5:6],21[2:4][5:7]'
list iw_venue_name 'eng:somePublicSpace'
list iw_venue_url '1:https://www.example.com/'
option iw_network_auth_type '00'
option iw_ipaddr_type_availability '0c'
list iw_domain_name 'example.com'
option hs20 '1'
option hs20_oper_friendly_name 'eng:YourFriendPasspoint'
option hs20_operating_class '517C'
```

As you can easily guess, "option" is used to specify only one option, 
while "list" is used to list multiple options.
In the example above, two NAI realms, example. com and
example. org, are configured with EAP methods
"EAP-TLS with certificate" and "EAP-TTLS/MSCHAPv2 with username/password."

The parameter names and their contents can be found
in [the template of the hostapd configuration file](https://w1.fi/cgit/hostap/plain/hostapd/hostapd.conf).
Please look into the "/lib/netifd/hostapd.sh" script
to see which options are actually available.

You may use the MAC address associated with the SSID 
for **iw_hessid** if you are not so sure about the parameter.

Please refer to the
[IEEE 802.11 specification](https://www.ieee802.org/11/) document
about **hs20_operating_class**.


### Testing the Hotspot 2.0 functionality
To make the configuration effective,

> \# wifi

To see whether the SSID becomes available,

> \# iwinfo

And, the client device should show "Hotspot 2.0" message or 
a description embedded in the Passpoint profile.

The following command shows you whether Passpoint is supported
by the Wi-Fi device on Windows 10/11.
If "ANQP Service Information Discovery" is "Supported,"
Passpoint is supposed to work.

> \> netsh wlan show wirelesscapabilities


### Troubleshooting
If hostapd won't come up and the SSID disappears 
after setting "option iw_enabled '1'", 
there may be some wrong or missing parameters in the configuration.

Support of Hotspot 2.0 seems still in flux as of writing. 
A known problem is that UCI leaves iw_venue_name and iw_venue_url
to blank and hostapd fails to start.
Please check "/var/run/hostapd-phyX.conf" 
and see whether the parameters are passed correctly.


### Tested devices
The Passpoint AP functionality has been confirmed
 using the following devices.

- GL.iNet GL-MT3000 - [OpenWrt 23.05.2 for filogic](https://downloads.openwrt.org/releases/23.05.2/targets/mediatek/filogic/)
- GL.iNet GL-MT300N-V2 - [OpenWrt 23.05.2 for mt76x8](https://downloads.openwrt.org/releases/23.05.2/targets/ramips/mt76x8/)  
 (V4 firmware from GL.iNet is not using wpad, and Passpoint does not work on it.)
- GL.iNet GL-A1300 - [GL.iNet Firmware 4.5.0](https://dl.gl-inet.com/router/a1300/)  
 (Firmware 4.5.0 is based on OpenWrt 21.02.2)
- GL.iNet GL-AXT1800 - [GL.iNet Firmware 4.4.6](https://dl.gl-inet.com/router/axt1800/)  
 

