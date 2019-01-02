# Greenhouse RPi OS

## Prepare sdcard
Raspian stretch lite: https://www.raspberrypi.org/downloads/raspbian/

 * Write image to sdcard.
 * Resize second partition
 * Create `ssh` file on boot partition (the first one)
 * Edit hostname in `/etc/hostname` and `/etc/hosts`

## Setup OS

### DHCP dnsmasq

Find the MAC-address of the device in the leases file at `/var/lib/misc/dnsmasq.leases`.
Add an entry in the dhcp hosts file at `/etc/dnsmasq.dhcphosts` with the
following template: `<mac-address>,<ip-address>,<domain/host>,infinite`, eg
`b8:27:eb:05:20:3d,192.168.93.53,gh03,infinite`.

Restart dnsmasq to reload config.

### Add greenhouse user

Ssh to the device using the ip address in the leases file, default user and password is `pi:raspberry`.

```shell
# useradd -c "Greenhouse" -G gpio,sudo -m -s /bin/bash greenhouse
# passwd greenhouse
# sudo -u greenhouse mkdir /home/greenhouse/.ssh
# reboot
```

Optionally add ssh public key, from your machine:
```shell
$ scp ~/.ssh/id_rsa.pub <ip/host>:.ssh/authorized_keys
```

Ssh to device again as `greenhouse`
```
# deluser pi
```

### Install packages/services

#### wiringpi gpio utility

```
$ wget https://unicorn.drogon.net/wiringpi-2.46-1.deb
$ sudo dpkg -i wiringpi-2.46-1.deb
$ rm wiringpi-2.46-1.deb
```

#### Docker

```
curl -sSL https://get.docker.com | sh
sudo usermod -a -G docker greenhouse
```

#### K8s

See `/cluster/README.md`.
