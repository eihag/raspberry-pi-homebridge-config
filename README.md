# Homekit server for Raspberry PI #

Use Apple Siri to ask for sensor data

Uses Node.JS project: https://github.com/nfarina/homebridge

## Installation on Raspberry Pi Zero W Raspbian Jessie ##
Updated 2017-03-13, source: https://github.com/nfarina/homebridge/wiki/Running-HomeBridge-on-a-Raspberry-Pi

Install Node.js
<pre>

cd /tmp
wget https://nodejs.org/dist/v6.10.0/node-v6.10.0-linux-armv6l.tar.xz
tar -xvf node-v6.10.0-linux-armv6l.tar.xz
sudo cp -R node-v6.10.0-linux-armv6l/* /usr/local/
</pre>

Install homebridge and dependencies
<pre>
sudo apt-get install libavahi-compat-libdnssd-dev
sudo npm install -g --unsafe-perm homebridge hap-nodejs node-gyp

</pre>

Verify installation
<pre>
homebridge
</pre>



Auto-start homebridge service with systemd
(source: https://gist.github.com/johannrichard/0ad0de1feb6adb9eb61a/)
<pre>
sudo wget https://gist.githubusercontent.com/johannrichard/0ad0de1feb6adb9eb61a/raw/1cf926e63e553c7cbfacf9970042c5ac876fadfa/homebridge -P /etc/default
sudo wget https://gist.githubusercontent.com/johannrichard/0ad0de1feb6adb9eb61a/raw/1cf926e63e553c7cbfacf9970042c5ac876fadfa/homebridge.service -P /etc/systemd/system
sudo useradd -M --system homebridge
sudo mkdir /var/lib/homebridge
sudo chown homebridge /var/lib/homebridge

sudo systemctl daemon-reload
sudo systemctl enable homebridge
sudo systemctl start homebridge
</pre>


Install accessory in homebridge to access pool temperature. Use config-sample.json for inspiration when editing config file.

<pre>
npm install -g homebridge-sensor-data-file-1.0.0.tgz
sudo vim /var/lib/homebridge/config.json
sudo chown homebridge:homebridge /var/lib/homebridge/config.json
sudo systemctl restart homebridge
</pre>

Create sample file for initial test:
<pre>
echo 20.1>/tmp/pool-temperature
</pre>
Other process (raspberry-pi-data-collector) will periodically save pool temperature to file on disk.


Now, just pair iPhone with HomeBridge and you are all set!

#### View homebridge log
<pre>
sudo journalctl -f -u homebridge
</pre>

