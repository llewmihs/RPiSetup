# RPiSetup
Instructions to use when setting up a Raspberry Pi
## Downloading and flashing
Download the latest Raspbian from here:  
https://www.raspberrypi.org/downloads/raspbian/  
Flash to a memory card using Etcher:  
https://etcher.io/
## Enable ssh and autoconnect to Wifi
In the `boot` folder `touch ssh` and `touch wpa_supplicant.conf`
Edit `wpa_supplicant.conf` as follows:  
```country=gb
update_config=1
ctrl_interface=/var/run/wpa_supplicant

network={
 scan_ssid=1
 ssid="MyNetworkSSID"
 psk="Pa55w0rd1234"
}
```  
## First boot
Throw the card in a Pi and power up!
Open up a terminal and ssh in using:
`ssh pi@<<insert pi ip here>>`
The best place to find the IP of your Pi is by logging into your router and finding it there.
### Changing the password  
Better safe than sorry, change your password using:  
`passwd`  
### Set a static IP address  
Should you wish, from the terminal open the following:  
`sudo nano /etc/dhcpcd.conf`  
And then add:  
```
interface wlan0
static ip_address=192.168.0.200/24
static routers=192.168.0.1
static domain_name_servers=192.168.0.1
```  
This line: `static ip_address=192.168.0.200/24` is your new IP, always leave the `24`, change the `200` to suit your needs/network.  
## Changing the Hostname  
`sudo nano /etc/hosts` - replace `raspberrypi` with your desired hostname  
and `sudo nano /etc/hostname` - replace `raspberrypi` with same hostname. Then reboot.
