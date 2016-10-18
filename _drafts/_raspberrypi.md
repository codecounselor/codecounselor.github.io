
`diskutil unmountDisk /dev/disk3`

`sudo dd bs=16m if=/Users/cc/Downloads/2016-09-23-raspbian-jessie-lite.img of=/dev/disk3`

/etc/network/interfaces

```
auto lo
 
iface lo inet loopback
iface eth0 inet dhcp
 
allow-hotplug wlan0
auto wlan0
 
 
iface wlan0 inet dhcp
        wpa-ssid "pilab"
        wpa-psk "R@spb3ryP1"
        
```

curl -sSL https://get.docker.com | sh


var/log/daemon.log

Oct 18 09:17:57 pienode1 wpa_supplicant[1206]: Could not read interface wlan0 flags: No such device
Oct 18 09:17:57 pienode1 wpa_supplicant[1206]: nl80211: Driver does not support authentication/association or connect commands
Oct 18 09:17:57 pienode1 wpa_supplicant[1206]: Could not read interface wlan0 flags: No such device
Oct 18 09:17:57 pienode1 wpa_supplicant[1206]: Could not read interface wlan0 flags: No such device
Oct 18 09:17:57 pienode1 wpa_supplicant[1206]: WEXT: Could not set interface 'wlan0' UP
Oct 18 09:17:57 pienode1 wpa_supplicant[1206]: wlan0: Failed to initialize driver interface


raspberry pi wpasupplicant version

network={
ssid="pilab"
psk="R@spb3ryP1"
proto=RSN
key_mgmt=WPA-PSK
pairwise=CCMP
auth_alg=OPEN
}

sudo apt-get install --reinstall raspberrypi-bootloader raspberrypi-kernel