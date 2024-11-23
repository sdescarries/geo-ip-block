# Geo-IP Block

Uses zone info from [IPdeny](https://www.ipdeny.com/ipblocks/) to populate ip sets in firewalld.

## Usage

```sh
sudo ./geo-ip-block cn ru kp ir in
```

## Installation

```sh
dnf install firewalld
systemctl enable firewalld
systemctl start firewalld
firewall-cmd --permanent --zone=public --add-service=ssh
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload
```
