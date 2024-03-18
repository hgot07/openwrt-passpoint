# OpenWrt-Passpoint

Documents and some experimental codes to enable Passpoint (aka Hotspot 2.0) on OpenWrt (21.x or newer). 

You can connect OpenWrt devices to OpenRoaming as well if you use OpenRoaming's RCOI (Roaming Consortium Organization Identifier).

## Prerequisites

OpenWrt 21.x or newer is required since they come with Passpoint-enabled wpad. 
Version 22.x or newer is recommended to fully benefit from the Passpoint client feature.
2021 version of wpad (wpa_supplicant) may not join Passpoint network due to the lack of some ANQP functions.

A full version of wpad (or hostapd/wpa_supplicant) is required. If *-basic one is installed, it needs to be replaced with a full version such as wpad-openssl.

Note: Some GL.iNet routers (like GL-MT3000) come with MediaTek's proprietary Wi-Fi driver whose configuration is not compatible with that of the standard wpad (combination of hostapd and wpa_supplicant). 
Passpoint does not work on those models unless you get firmware with a wpad-based Wi-Fi system. FYI, GL-MT3000 users can find an OpenWrt firmware image **[here](https://downloads.openwrt.org/releases/23.05.2/targets/mediatek/filogic/)** and use Passpoint.

## Access Points (AP)

Todo:
Write!

[>> Details](ap/README.md)

See also:
- [Configuring Hotspot 2.0 (Passpoint) on OpenWrt](https://hgot07.hatenablog.com/entry/2022/03/21/231715)


## Clients (aka User Equipments, STA)

**/lib/netifd/hostapd.sh** originates from OpenWrt.
The script reads /etc/config/wireless and produces wpa_supplicant-*.conf.

A modified script is provided here.
The functionality of Passpoint is dependent on the wpad (or wpa_supplicant). 
The modification is only about the Passpoint client feature.
It does not take effect unless you set **option iw_enabled '1'** in /etc/config/wireless.

**Pull Request is open at the link below. I'm waiting for reviews.** (Please help.)
- [wifi-scripts: Add Passpoint client support for hostapd.sh \#14684](https://github.com/openwrt/openwrt/pull/14684)

[>> Details](sta/README.md)

## Resources
- [OpenWrt main repository](https://git.openwrt.org/openwrt/openwrt.git)
- [OpenWrt mirror repository at GitHub](https://github.com/openwrt/openwrt)
- [OpenWrt 23.05.2 repository (download images and packages)](https://downloads.openwrt.org/releases/23.05.2/)
- [hostapd configuration file](https://w1.fi/cgit/hostap/plain/hostapd/hostapd.conf) (hostapd.conf)
- [wpa_supplicant configuration file](https://w1.fi/cgit/hostap/plain/wpa_supplicant/wpa_supplicant.conf) (wpa_supplicant.conf)
- [IEEE GET Program - GET 802(R) Standards](https://ieeexplore.ieee.org/browse/standards/get-program/page/series?id=68)
- [Wi-Fi CERTIFIED Passpoint](https://www.wi-fi.org/discover-wi-fi/passpoint)
- [WBA OpenRoaming](https://wballiance.com/openroaming/)
