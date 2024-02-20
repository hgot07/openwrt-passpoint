## Clients (aka User Equipments, STA)

The modifed **/lib/netifd/hostapd.sh** allows configuring a Passpoint client.

The following options are added to the "sta"-mode block in /etc/config/wireless.
- option iw_enable '1'  
Enable (1) or disable (0) Passpoint.
- option iw_rcois '000000,deadbeef00'  
Enable matching by RCOIs (comma-separated list of 3 or 5-octet hex).  
FYI, the RCOI of OpenRoaming settlement-free is '5a03ba0000'.
- option iw_realm 'example. com'  
Alternatively, enable matching by an NAI realm.

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
- option client_cert '\<client_certificate_file\>'
- option priv_key '\<private_key_file\>'
- option priv_key_pwd '\<private_key_password\>'

In Passpoint, **domain_suffix_match** must be used in the network selection and server authentication phases.
Other methods such as subject_match cannot prevent accidental connection to an evil-twin AP.

### EAP-TTLS configuration example using RCOI matching

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


### EAP-TLS configuration example using NAI realm matching

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
	option priv_key '/etc/luci-uploads/userID.p12'
	option priv_key_pwd 'key_password'
	option ca_cert '/etc/ssl/certs/ca-all.crt'
	list domain_suffix_match 'idp.example.com'
	option iw_enabled '1'
	option iw_realm 'example.com'
	option ieee80211w '1'
```
Notes:  
- There is no **anonymous_identity** option. You may use anonymous identity in the **identity** option.
- You may use a pair of **client_key** and **priv_key** (key only) instead of the key file in .p12. Try .p12 if it does not work.
- If the server certificate is issued by an intermediate CA, all intermediate certificates would be needed in the CA file (or ca_path directory). If an intermediate certificate is missing, wpad (wpa_supplicant) may fail in the certificate path validation, hence does the server authentication.
