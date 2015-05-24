OS X Hybrid DNS
===============

This is a collection of custom scripts I use with dnsmasq to use
VPN DNS servers for a white list of search domains and use
local network DNS configuration for all other lookups.

This allows DNS lookups on the local network to continue working
and makes sure edge-cache DNS records use appropriate addresses
for the location of the VPN client, instead of using ones that
would be relevant to the VPN server.

Install
=======

1. Clone this repository somewhere in your home directory
1. Install [Homebrew](http://brew.sh/)
1. Run `brew install ./Formula/dnsmasq-regex.rb` (a patched version of dnsmasq that supports regular expressions)
1. `cp -R dns-watch/ dnsmasq.available/ dnsmasq.enabled/ dnsmasq.conf /usr/local/etc`
1. `sudo launchctl load /usr/local/etc/dns-watch/io.eldredge.dns-watch.plist`

Configure
=========

* Edit `/usr/local/etc/dnsmasq.available/local.conf` with DNS records for localhost
* Edit `/usr/local/etc/dns-watch/settings` to choose mDNSResponder or discoveryd for Mavericks / Yosemite
* `ln -s /usr/local/etc/dnsmasq.available/local.conf /usr/local/etc/dnsmasq.enabled`

If you are using a `ppp` based VPN, do:

    sudo mkdir /etc/ppp
    sudo ln -s /usr/local/etc/dns-watch/local-dns /etc/ppp/ip-up
    sudo ln -s /usr/local/etc/dns-watch/local-dns /etc/ppp/ip-down

Watch /var/log/dns-watch and look for lines like:

    ActiveServices: A8227183-B047-4D4E-9BCE-490997F5030F 9D299D25-805E-449A-BC39-EC863A6E0C57

Make note of service IDs as you connect/disconnect your VPN.

For each service ID that correlates to a VPN that you want custom DNS settings for,
create `/usr/local/etc/dnsmasq.available/${SERVICE_ID}.profile`

Example profile:

    server=/localnet/10.0.0.1

This will instruct dnsmasq to send queries for the localnet domain to 10.0.0.1.
