---
title: RaspberryPi bookworm webserver

date: 2024-03-30 12:00:00 -0200
categories: [rpi]
tags: [rpi, bookworm, webserver, nginx, php, letsencrypt]
---
## RPi with bookworm as a webserver

My old RPi, servering my webserver stil runs on the old raspi-os based on debian 10.
Time to upgrade to the lates os version that is still spported for some time.

As I run my webserver on a rpi3 in 64 bit mode (because of also running a tor onion service for some of my sites). Then new webserver rpi shoudl also run thjat on 64 bit.

For now i use a spare rpi2 rev 1.1 (that is the last rpi version that does not support 64-bit) to find out what i need to install. I now finally document what I did the last few times for my webserver, running ngnx, php-fmt, letsencrypt and tor.

While doing that, my webserer on the rpi3 stays operational.

### Preparing the rpi2

Prepare an SD card with the latest raspi-os 32 bit version and the lite version, as this will be a headless deployment, and update it with the latest packages, give it a host name, a login account and enable ssh.

```bash
sudo apt update
sudo apt upgrade
sudo raspi-config
sudo reboot
```

then install the packages needed for the webserver

```bash
sudo apt install nginx
sudo apt install letsencrypt
sudo apt install php-fpm
```

This will install nginx 1.22 with php version 8.2 from the os repository, good enough for now.

(to be continued...)

...
