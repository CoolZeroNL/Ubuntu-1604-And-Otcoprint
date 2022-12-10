# Ubuntu-1604-And-Otcoprint

Installing Octoprint on Ubuntu 16-04 32Bit.

## Wifi

Generate psk key
```
 wpa_passphrase <ssid> [passphrase]
```

Open
```
sudo nano /etc/network/interfaces
``` 

Paste
```
auto wlp1s0
iface wlp1s0 inet dhcp
wpa-ssid <ssid>
wpa-psk <passphrase>
```

restart service
```
sudo service networking restart
``` 

## Python 3.8.4

### deps
```
sudo apt install libffi-dev build-essential zlib1g-dev
``` 

### compile

```
cd /opt
sudo wget https://www.python.org/ftp/python/3.8.4/Python-3.8.4.tgz
sudo tar -xvf Python-3.8.4.tgz
cd Python-3.8.4
sudo ./configure
sudo make && make install
```

## Octoprint

```
sudo useradd -m octo
sudo passwd octo
sudo usermod -a -G tty,dialout octo

sudo su - octo
mkkdir -p OctoPrint
cd OctoPrint
pip3.8 install https://get.octoprint.org/latest
```

### Service:

Install a small helper script to run OctoPrint at boot

Open
```
sudo nano /etc/systemd/system/octoprint.service
```

Paste
```
[Unit]
Description=The snappy web interface for your 3D printer
After=network-online.target
Wants=network-online.target

[Service]
Environment="LC_ALL=C.UTF-8"
Environment="LANG=C.UTF-8"
Type=exec
User=octo
ExecStart=/home/octo/.local/bin/octoprint

[Install]
WantedBy=multi-user.target
```

Enable service
```
sudo systemctl enable octoprint.service
```

Start:
```
service octoprint start
```

Stop:
```
service octoprint stop
```

Status:
```
service octoprint status
```
