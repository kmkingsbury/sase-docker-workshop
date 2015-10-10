# sase-docker-workshop
Workshop Demo for SASE Hack-a-thon

## Supported Platforms

TODO: List your supported platforms.


# Files

<table>
  <tr>
    <th>File</th>
    <th>Description</th>
    <th>Platform</th>
  </tr>
  <tr>
    <td><tt>test-discovery</tt></td>
    <td>cans Bluetooth, reports Address, Signal, time, and alias/name if available.
    Posts the info as a JSON POST to a URL
    Script comes from Bluez git repo and is modified: http://git.kernel.org/cgit/bluetooth/bluez.git/</td>
    <td><tt>Edison</tt></td>
  </tr>
  <tr>
    <td><tt>simplewebserver.py</tt></td>
    <td>Webserver to handle the API POSTs and GETs</td>
    <td><tt>Docker</tt></td>
  </tr>
</table>



# Setup Edison
```
rfkill unblock bluetooth
hciconfig hci0 up

vi /etc/opkg/base-feeds.conf #(add these lines):
src/gz all http://repo.opkg.net/edison/repo/all
src/gz edison http://repo.opkg.net/edison/repo/edison
src/gz core2-32 http://repo.opkg.net/edison/repo/core2-32

opkg update
opkg install bluez5-dev

opkg install git

wget --no-check-certificate https://bootstrap.pypa.io/get-pip.py
python get-pip.py
pip install requests
pip install pybluez
```

# References


### Setup iBeacon Example Overview
http://blog.punchthrough.com/post/57813153898/wtf-is-an-ibeacon

### Reference for Edison and Bluetooth (used for Setup)
https://github.com/w4ilun/edison-guides/wiki/Lets-turn-Intel-Edison-into-an-iBeacon
https://github.com/w4ilun/edison-guides/wiki/Configure-Intel-Edison-for-BLE---Bluetooth-Smart-Development

This part
```
rfkill unblock bluetooth
killall bluetoothd #(or, more permanently) systemctl disable bluetooth
hciconfig hci0 up
```

### Intel Edison Bluetooth Guide
http://download.intel.com/support/edison/sb/edisonbluetooth_331704004.pdf

https://software.intel.com/en-us/articles/using-the-generic-attribute-profile-gatt-in-bluetooth-low-energy-with-your-intel-edison

### Python Examples
https://www.hackster.io/AgustinP/logging-sensor-data-using-intel-edison-amp-python

### RancherOS
http://docs.rancher.com/os/running-rancheros/workstation/docker-machine/
```
docker-machine create -d virtualbox --virtualbox-boot2docker-url https://releases.rancher.com/os/latest/machine-rancheros.iso RancherOS
```

Docker different client and server api - Docker Version Manager
https://github.com/rgbkrk/dvm
```
. ~/.dvm/dvm.sh
dvm install 1.7.0
dvm use 1.7.0
```

### Python Webserver
https://mafayyaz.wordpress.com/2013/02/08/writing-simple-http-server-in-python-with-rest-and-json/

ResinOS (Docker on Edison)
http://docs.resin.io/#/pages/installing/gettingStarted-Edison.md

## Quick Guide to Docker
Build & Tag:
```
docker build -t simplewebserver .
```
Run:
```
docker run --name simplewebserver -p 8081:80 -d -t simplewebserver
```

### Using the Webserver's API:
```
curl -X POST http://localhost:8081/api/v1/getrecord/1 -d '{\"asif1\":\"test1\"}' -H "Content-Type: application/json"
curl -X GET http://localhost:80/api/v1/getrecord/AAAAAA922489  -H "Content-Type: application/json"
```

## Author
Author:: Kevin Kingsbury (kkingsbury@gmail.com)
