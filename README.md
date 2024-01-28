# OpenWrt-Passpoint

Documents and some experimental codes to enable Passpoint on OpenWrt (21.x or newer).

## Access Points (AP)

Todo:
Write!

## Clients (aka User Equipments, STA)

Todo:
Catch up with the latest code at the OpenWrt main repository.

The modifed **/lib/netifd/hostapd.sh** allows configuring a Passpoint client.

The following options are added to the "sta" block in /etc/config/wireless.
- option iw_enable '1'  
Enable (1) or disable (0) Passpoint
- option iw_rcois 'deadbeef00'  
Enable matching by RCOIs (comma separated list of 3 or 5-octet hex)
- option iw_realm 'example. com'  
Alternatively, enable matching by an NAI realm

In Passpoint, **option domain_suffix_match** must be used for the server authentication.
Other methods such as subject_match will not prevent accidental connection to an evil-twin AP.

The following parameters (e.g. EAP-TTLS) are also used in the interworking.
- option identity 'User-Name'
- option password 'Password'
- option eap_type 'ttls'
- option auth 'EAP-MSCHAPV2'
- option ca_cert_usesystem '1'

It is recommended to configure WPA2/WPA3 Enterprise (client mode) first,
and then add the iw_* parameters. Any dummy SSID may be set.
