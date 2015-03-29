---
layout: post
title: Arch Linux on Raspberry Pi
date: '2013-01-20T12:53:00.000Z'
author: swang
tags: 
modified_time: '2013-01-22T00:55:07.601Z'
blogger_id: tag:blogger.com,1999:blog-6338160651537141164.post-2682109379472394445
blogger_orig_url: http://swang-ztransform.blogspot.com/2013/01/arch-linux-on-raspberry-pi.html
---

## Setup your SD card 

Follow this [guide](http://elinux.org/RPi_Easy_SD_Card_Setup)

I used the MacOS command line version, as the (mostly graphical interface) version forgot to mention changing disk3s1 to rdisk3

After the dd command, you should be able to read your card in Mac. Then delete all the dot folders created by Mac

```
rm -rf .Spotlight-V100 .Trashes ._.Trashes .fseventsd                                              
```

(Optional) uncomment these two lines in config.txt:

```
config_hdmi_boost=4
hdmi_force_hotplug=1
```

That's it, put the SD card in and boot it.


## Basic Steps After Boot

login as **root:root**, the default hostname is *alarmpi*.

Resize your SD card following the Manually resizing the SD card on Raspberry Pi 
part of this [guide](http://elinux.org/RPi_Resize_Flash_Partitions)

Alternatively, follow these steps:

As root:

```
fdisk /dev/mmcblk0
```

Delete the second partition */dev/mmcblk0p2*

```
d
2
```

Create a new primary partition and use default sizes prompted. This will then create a partiton that fills the disk

```
n
p
2
enter
enter
```

Save and exit fdisk:

```
w
```

Now reboot:

```
shutdown -r now
```

Once rebooted:

```
resize2fs /dev/mmcblk0p2
```

Then to create a swap file, run the following commands (taken from Arch wiki):

```
fallocate -l 512M /swapfile

chmod 600 /swapfile

mkswap /swapfile

swapon /swapfile
```

and add `/swapfile none swap defaults 0 0` to **/etc/fstab**.


If your ethernet cable is pluged in when booting, then you should have your ip address assigned and working. 
If not, try the following [guide for setting static ip](https://wiki.archlinux.org/index.php/Beginners%27_Guide)

The useful part (Wired) is copied here, plus some extra:

> First, identify the name of your ethernet interface.
`ip link`
> Usually it's eth0.
>
> You also need to know these settings:
>
> (on a Mac, you can find them in airport utility)
>
> Static IP address.
> Subnet mask.
> Gateway's IP address.
> Name servers' (DNS) IP addresses.
> Domain name (unless you're on a local LAN, in which case you can make it up). 
>
> Activate the connected Ethernet interface (e.g. eth0):
`ip link set eth0 up`
> 
> Add the address:
`ip addr add <ip address>/<subnetmask> dev <interface>`
> For example:
`ip addr add 192.168.1.101/24 dev eth0`
> Add your gateway like this, substituting your own gateway's IP address:
`ip route add default via <ip address>`
> For example:
`ip route add default via 192.168.1.254`
> Edit **resolv.conf**, substituting your name servers' IP addresses and your local domain name:
`vi /etc/resolv.conf`
> _nameserver 192.168.1.254_
>
> Change hostname in **/etc/hostname** and add the same host name to **/etc/hosts**
> _127.0.0.1 localhost.localdomain localhost yourhostname_


## Update System

### Pacman

```
pacman-key --init
```

press **Alt + F2** to another console and type some random commands
go back to **Alt + F1** and see if the key is generated.

update pacman itself:

```
pacman -Sy pacman
```

update the rest:

```
pacman -Syu
```

### Change Console Font and Keymap

Keymap is easy, according to this [guide](https://wiki.archlinux.org/index.php/KEYMAP)
just change the first line in **/etc/vconsole.conf** to `keymap=uk`, and it works for me.

Again in **/etc/vconsole.conf** for font, I used sun12x22.

### User Management

_TBA_

### Wireless Network

use **netcfg** is the easiest.

tools needed:
*netcfg, iwconfig(part of wireless_tools), wpa_supplicant*
optional: iw(does NOT work on arch linux arm, complains nl80211 not found, don't know why yet)

```
sudo cp /etc/network.d/examples/wireless-wpa /etc/network.d/swang-wireless
```

put correct ESSID (network name), KEY (wireless key)
leave the rest as is

to manually connect to a profile:

```
netcfg swang-wireless
```

or, to automatically connect to a wireless network:
edit **/etc/conf.d/netcfg**, add `swang-wireless` to `NETWORKS=()`

then

```
systemctl enable netcfg
```

to automatically to a wired network on plugin:

```
systemctl enable net-auto-wired
```

