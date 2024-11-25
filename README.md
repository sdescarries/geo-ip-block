# Geo-IP Block

Uses zone info from [IPdeny](https://www.ipdeny.com/ipblocks/) or [GeoLite2](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data/) if you have a license, to populate ip sets in firewalld.

## Usage

`geo-ip-block <command> [args]`

command
- `update <CC> [CC]` fetch the latest database and rebuilds zone files
- `delete` remove existing ip sets
- `create` create new ip sets
- `load` load current zone databases into ip sets
- `apply` reload firewall to apply new rules
- `full <CC> [CC]` runs the whole list of commands, suitable for a cron job

----
Full job updates the zones and applies them in firewalld. Run this job as a daily cron job for the root user to stay protected.

```cron
0 9 * * * /opt/geo-ip-block/geo-ip-block full xx yy zz
```
----
Only update the database and generate fresh lists files for IPv4 `zone4` and IPv6 `zone6`, does not need elevated privileges.

```sh
./geo-ip-block update xx yy zz
```

## Installation

```sh
dnf install firewalld curl unzip
systemctl enable firewalld
systemctl start firewalld
```

Make sure you enable the required services on your server, for example:

```sh
firewall-cmd --permanent --zone=public --add-service=ssh
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload
```

## Known Issues

- **OpenSUSE Leap 15.6**
  `firewalld` goes into an infinite loop trying to load ip sets, no workaround found.
- **Ubuntu 24.04**
  Ubuntu ships with `ufw`, you need to uninstall it and switch to `firewalld` instead.
