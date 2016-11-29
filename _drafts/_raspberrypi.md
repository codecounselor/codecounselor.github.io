
`diskutil unmountDisk /dev/disk3`
`sudo dd bs=16m if=/Users/cc/Downloads/2016-09-23-raspbian-jessie-lite.img of=/dev/disk3`

Get pi network addresses.  I used my router to see attached devices and then assigned them fixed IPs.
If you don't have access to your router you can use [nmap](https://www.raspberrypi.org/documentation/remote-access/ip-address.md).

### Wifi Setup (Optional)

### Update Hosts
If you don't have DNS, you can put the entries into `/etc/hosts` of each device

### Setup SSH Keys

### Get the latest packages

`
ssh pi@piemaster
sudo -i
apt-get update
apt-get upgrade
apt-get install git htop vim docker-engine
`

### Install Kube-Config

[Get .deb package](https://github.com/luxas/kubernetes-on-arm#install-the-deb-package) from [Kubernetes on ARM](https://github.com/luxas/kubernetes-on-arm) project.

### Have Fun!

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



`lshw -C network` - Why isn't my wifi adapter listed

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



### apt-get update notice:

raspberrypi-sys-mods (20161018+3) jessie; urgency=medium

  * The 20161018 release has introduced a /etc/sudoers.d/010_pi-nopasswd file.
    - The file is installed even if the "pi ALL=(ALL) NOPASSWD: ALL" entry has been
      previously removed from /etc/sudoers by the user.
    - If you do not want the entry to exist, please comment out or remove 010_pi-nopasswd.
    - If upgrading to 20161018+3 from a version earlier than 20161018, the line in
      010_pi-nopasswd is automatically commented out if the entry doesn't exist in sudoers.
    - See https://github.com/RPi-Distro/raspberrypi-sys-mods/issues/6

 -- Serge Schneider <serge@raspberrypi.org>  Wed, 19 Oct 2016 10:52:07 +0100
