# liblib with pi

## Set up your raspberry pi
* [Flashing Raspian to an SD Card](http://computers.tutsplus.com/articles/how-to-flash-an-sd-card-for-raspberry-pi--mac-53600)
* [Install WAP software and configure](https://learn.adafruit.com/setting-up-a-raspberry-pi-as-a-wifi-access-point/install-software)
    * This tutorial creates a password protected network. If you want it to be open, leave off the final 5 lines that they suggest for hostapd.conf
    * You can skip the section called "Configure Network Address Translation"
* Install CouchDB `sudo apt-get install couchdb`
* Install dnsmasq `sudo apt-get install dsnmasq`
    1. Edit /etc/dnsmasq.conf
    2. Add `address=/#/192.168.42.1`
    3. Change `#interface=eth0` to `interface=wlan0`
    4. `sudo service dnsmasq restart`
* [Install NodeJs](http://weworkweplay.com/play/raspberry-pi-nodejs/)
```
wget http://node-arm.herokuapp.com/node_latest_armhf.deb 
sudo dpkg -i node_latest_armhf.deb
```
* [Route port 80 to port 3000](http://stackoverflow.com/questions/16573668/best-practices-when-running-node-js-with-port-80-ubuntu-linode)
    1. `sudo iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 80 -j REDIRECT --to-port 3000`
    2. `sudo bash -c 'iptables-save > /etc/network/iptables'`
    3. And add this to the end of /etc/network/interfaces `pre-up iptables-restore < /etc/network/iptables`
* Install liblib daemon
```
sudo npm install -g forever
npm install liblib
cd node_modules/liblib
forever start liblibd/index.js
```
