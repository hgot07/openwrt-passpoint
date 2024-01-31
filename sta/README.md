## Clients (aka User Equipments, STA)

The modifed **/lib/netifd/hostapd.sh** allows configuring a Passpoint client.

The following options are added to the "sta" block in /etc/config/wireless.
- option iw_enable '1'  
Enable (1) or disable (0) Passpoint
- option iw_rcois 'deadbeef00'  
Enable matching by RCOIs (comma separated list of 3 or 5-octet hex)
- option iw_realm 'example. com'  
Alternatively, enable matching by an NAI realm

It is recommended to configure a WPA2/WPA3 Enterprise (client mode) network first,
and then add the iw_* parameters. Any dummy SSID may be set.

The following parameters, originally for EAP-TTLS and EAP-TLS, will also be used in the interworking.
- option identity '\<User-Name\>'
- option password '\<Password\>'
- option eap_type 'ttls' or 'tls'
- option auth 'EAP-MSCHAPV2'
- option ca_cert_usesystem '1'
- option ca_cert '\<cert_path\>'
- list domain_suffix_match '\<domain_suffix\>'

In Passpoint, **domain_suffix_match** must be used in the network selection and server authentication phases.
Other methods such as subject_match cannot prevent accidental connection to an evil-twin AP.

### EAP-TTLS configuration example

In /etc/config/wireless,
```
config wifi-iface 'wifinet7'
	option ssid '_Passpoint'
	option mode 'sta'
	option encryption 'wpa2+ccmp'
	option device 'radio0'
	option anonymous_identity 'anonymous@example.com'
	option identity 'userID@example.com'
	option password 'password'
	option eap_type 'ttls'
	option network 'wwan'
	option ca_cert_usesystem '1'
	list domain_suffix_match 'idp.example.com'
	option auth 'EAP-MSCHAPV2'
	option iw_enabled '1'
	option iw_rcois '000000'
	option ieee80211w '1'
```


### EAP-TLS configuration example

In /etc/config/wireless,
```
config wifi-iface 'wifinet8'
	option ssid '_Passpoint'
	option mode 'sta'
	option encryption 'wpa2+ccmp'
	option device 'radio0'
	option identity 'anonymous@example.com'
	option eap_type 'tls'
	option network 'wwan'
	option client_cert '/etc/luci-uploads/userID.cer'
	option priv_key '/etc/luci-uploads/userID.key'
	option priv_key_pwd 'key_password'
	option ca_cert '/etc/ssl/certs/ca-all.crt'
	list domain_suffix_match 'idp.example.com'
	option iw_enabled '1'
	option iw_rcois '000000'
	option ieee80211w '1'
```
Note: If the server certificate is issued by an intermediate CA, all intermediate certificates need to be added to the CA file (or ca_path directory). If an intermediate certificate is missing, wpad (wpa_supplicant) fails in the certificate path validation, hence does the server authentication.

