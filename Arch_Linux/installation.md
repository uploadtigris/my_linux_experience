ip addr show --> find ethernet device

iwctl
station wlan0 get-networks\
"No station on device: 'wlan0'\
exit iwctl\
rfkill unblock wifi\
device list ( to check that your wifi card is powered on )\
iwctl\
station {wifi device} connect {SSID}
**enter password**

archinstall
