Burn the raspbian image to the SD card.  Plug it back into your computer and edit the following files on the SD card

If a `wpa_supplicant.conf` file is placed into the `/boot/` directory, this will be moved to the `/etc/wpa_supplicant/` directory the next time the system is booted, overwriting the network settings; this allows a Wifi configuration to be preloaded onto a card from a Windows or other machine that can only see the boot partition.

A typical `wpa_supplicant.conf` file is
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
    ssid="your_SSID"
    psk="your_PSK"
    key_mgmt=WPA-PSK
}
```

Place a file named `ssh` into the `/boot/` directory with no content to enable the ssh server

```
$ touch /boot/ssh
```

Boot it and get the ip from your router or a scanning tool.  The Fing app works really well for this if you can connect your phone to the same wifi.  Otherwise, SoftPerfect Network Scanner works pretty good.  In Linux, your multicast DNS will probably work so you can connect by hostname, or you can use nmap to scan.  You may now ssh in!  But you don't need to.
```
$ ssh pi@theipaddress
  Default Password: raspberry
```

Edit the hosts file within this project to point to the raspberry pi's IP address.  You may provision multiple pis by giving each one their own line in the file.  At last you may now run this script to provision the pi!  The camera mjpg stream is for the onboard raspberry pi camera module.
```
$ ansible-galaxy install -r requirements.yml -p playbooks/roles/ --force
$ ansible-playbook -i hosts playbooks/new-default.yml
$ ansible-playbook -i hosts playbooks/setup-camera-mjpg-stream.yml

```
