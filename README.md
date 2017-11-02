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
### Remove some bloat
`sudo apt-get purge wolfram-engine`  
### Update and upgrade
`sudo apt-get update`  
`sudo apt-get upgrade`  
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
`sudo nano /etc/hosts` - replace `raspberrypi` with your desired hostname.  
Then `sudo nano /etc/hostname` - replace `raspberrypi` with same hostname. Then reboot.
## Setting Python3 as the default editor
edit `sudo nano ~/.bashrc file`    
add `alias python=python3`    
Save then update `source ~/.bashrc`  
Check with `python -V`  

## Installing an MQTT server
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo apt-get install mosquitto mosquitto-clients
```  
Then `pip install paho-mqtt`  
If you want to configure the server:  
Server runs on port: 1883  
You can publish via the command line:  
Publish temperature information to localhost with QoS 1:  
`mosquitto_pub -t sensors/temperature -m 32 -q 1`  
You can subscribe via:  
Subscribe to temperature information on localhost with QoS 1:  
`mosquitto_sub -t sensors/temperature -q 1`  
In either case, they will default to localhost with port 1883. This can be changed with:   
`-h (insert IP after)`  
`-p (insert port after)`  
## Installing OpenVPN (via commandline)  
`apt-get install openvpn` Then: `sudo wget -O /etc/openvpn/btguard.ca.crt http://btguard.com/btguard.ca.crt`  
Next: `sudo wget -O /etc/openvpn/btguard.conf http://btguard.com/btguard.conf`  
Add: `pass.txt` to `/etc/openvpn/btguard.conf`  
Add username and password to pass.txt in the same folder as btguard.conf  
To autostart:  
`sudo nano /etc/default/openvpn`  
Uncomment `#AUTOSTART="all"`
## Setting the screen resolution
edit the file `/booot/config.txt`
uncomment `disable overscan=1`

```
overscan_left=24
overscan_right=24
Overscan_top=10
Overscan_bottom=24

Framebuffer_width=480
Framebuffer_height=320

Sdtv_mode=2
Sdtv_aspect=2
```
## Installing VSCode
`wget https://code.headmelted.com/installers/apt.sh`

`chmod +x apt.sh`

`sudo ./apt.sh`

## Installing Node-Red
`bash <(curl -sL https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/update-nodejs-and-nodered)`  
Then to autostart:  
`sudo systemctl enable nodered.service`

## If you want to use a Zero W OTG
Add `dtoverlay=dwc2` to `config.txt`.  
And:  
Add `modules-load=dwc2,g_ether` to `cmdline.txt` after `rootwait`


