# OpenWrt-Passpoint

Documents and some experimental codes to enable Passpoint (aka Hotspot 2.0) on OpenWrt (21.x or newer).

## Requirements

OpenWrt 21.x or newer as they come with Passpoint-enabled software. 22.x or newer is recommended to fully benefit from the Passpoint client feature. 2021 version of wpad (wpa_supplicant) may not join Passpoint network due to lack of some ANQP functions.

A full version of wpad (or hostapd/wpa_supplicant). If *-basic one is installed, it needs to be replaced with a full version such as wpad-openssl.

Note: Some GL.iNet routers come with firmware not based on the standard wpad. Passpoint does not work on some models.


## Access Points (AP)

Todo:
Write!

[>> Details](ap/README.md)

See also:
- [Configuring Hotspot 2.0 (Passpoint) on OpenWrt](https://hgot07.hatenablog.com/entry/2022/03/21/231715)


## Clients (aka User Equipments, STA)

Todo:
Catch up with the latest code at the OpenWrt main repository.

[>> Details](sta/README.md)

## Resources
- [OpenWrt main repository](https://git.openwrt.org/openwrt/openwrt.git)
- [OpenWrt mirror repository at GitHub](https://github.com/openwrt/openwrt)
- [hostapd configuration file](https://w1.fi/cgit/hostap/plain/hostapd/hostapd.conf)
- [wpa_supplicant configuration file](https://w1.fi/cgit/hostap/plain/wpa_supplicant/wpa_supplicant.conf)
- [IEEE GET Program - GET 802(R) Standards](https://ieeexplore.ieee.org/browse/standards/get-program/page/series?id=68)
- [Wi-Fi CERTIFIED Passpoint](https://www.wi-fi.org/discover-wi-fi/passpoint)
- [WBA OpenRoaming](https://wballiance.com/openroaming/)
