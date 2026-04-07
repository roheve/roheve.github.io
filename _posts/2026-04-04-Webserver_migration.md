---
title: webserver migration (debian 10 to 13)

date: 2026-04-04 12:00:00 -0200
categories: [website]
tags: [debian, trixie, webserver, lxc, nginx, php, letsencrypt]
---
## debian with trixie as a webserver

My old RPi, servering my webserver stil runs on the old raspi-os based on debian 10 (oldoldstable, now eol).
Time to upgrade to the latest debian version, so it receives os updates again.

I ran my webserver on the rpi3 in 64 bit mode (I also publish some of my sites as a tor onion service, because I can).
The new webserver will run in an lxc container running debian trixie (13) also 64 bit

### Preparing the debian lxc container

Create an LXC container running debian 13 (trixie). Do rememebr the PW upu used for this containe, you need it to login. (Same is thru when configuraing a new RPi, containers are no different)

### add your user account

For the LXC container, you are logged in as root. Add a pi user (for compatibility with my old rpi setups and id=1000), then add my other userID as <user> for regular use (with id=1001) and make is a sudo user.  It will ask for a password. than pate the system and reboot.

```bash
adduser pi --disabled-password
adduser <user> --groups sudo
```

Update it with the latest and greatest patches, give it a host name, create your user login account, and enable ssh (wold be automatically enabled for an LXC container).

```bash
sudo apt update
sudo apt upgrade
#
sudo apt install vim
#
sync
sudo reboot
```

### Install the packages needed for the webserver

login as your regular user and install nginx, php and certbot for letsencrypt certificates

```bash
sudo apt install nginx
sudo apt install php-fpm
#
# for letsencrypt certificates
sudo apt install certbot
```

This will install nginx and php from the os repository.

### install tor if you also run onion services for fun

This is optional. Only if you want to run onion services. See the [instruction](https://support.torproject.org/little-t-tor/getting-started/installing/#linux "torproject site") on the tor site for debian based systems.

The short version is to add the tor project repository to your sources list and install tor.

```bash
sudo apt update
sudo apt install gnupg
#
# copy and adapt the snippet on the torproject website for debian based systems (and save the file).
sudo vim /etc/apt/sources.list.d/tor.sources
#
# get the initial signing key
wget -qO- https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc | gpg --dearmor | sudo tee /usr/share/keyrings/deb.torproject.org-keyring.gpg >/dev/null
# install tor
sudo apt update
sudo apt install tor deb.torproject.org-keyring
```

## restore your webserver configuration and site files

To restore the webserver from the backup files, copy the nginx configuration and the php configuration, as well as the web files to the new LXC container (or a new pi). Including the letsencrypt configuration and certificates.

Make sure your LCX debian installation has the same accounts with the same user IDs and group IDs as in your old machine (that makes things easier). If not you need to adjust some folder and file ownership after the restore

```bash
cd ~
scp <user>@192.168.999.999:/home/<user>/mywebserver_<date>_* .
```

make sure you are at ~
Adjust the name of your backup files in the restore command below

```bash
cd ~
sudo tar xpf mywebserver_2026-04-04_nginxconfig.tar.gz
sudo tar xpf mywebserver_2026-04-04_sitecontent.tar.gz
sudo tar xpf mywebserver_2026-04-04_certificate.tar.gz
sudo tar xpf mywebserver_2026-04-04_onion.tar.gz
sudo tar xpf mywebserver_2026-04-04_request_cer.tar.gz
```

If the php version is different (likely) maken sure to edit the /etc/nginx/nginx.conf to set the upstream php socket to the right one. I made it point to /run/php/php.sock (no version number) and the php 8 I use already created that socket.
Create the dhparam file for nginx (it is not copied in the backup, but used in the nginx configuration).

```bash
sudo openssl dhparam -out /etc/ssl/private/dh2048_pem 2048
sudo systemctl restart nginx.service
```

As my onion install used a different userID and groupID, I need to adjust the ownership of the web files and the nginx configuration files to match the new user and group IDs on the new machine.

```bash
sudo chown debian-tor:debian-tor /var/run/tor -R
sudo systemctl restart tor.service
```

You als need to make sure requests from the internet reach your new server. I needed to update the IPv4 port forwarding setting on my homerouter and something similiar for IPv6 (you might also need to change the IPv6 DNS settings for your sites unless their IPv6 IP stays the same).

Now test if everything still works.
...
