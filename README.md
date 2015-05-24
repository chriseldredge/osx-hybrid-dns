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
1. Run `brew install ./dnsmasq-regex.rb` (a patched version of dnsmasq that supports regular expressions)
1. `cp -R dns-watch/ dnsmasq.available/ dnsmasq.enabled/ dnsmasq.conf /usr/local/etc`
1. `sudo launchctl load /usr/local/etc/dns-watch/io.eldredge.dns-watch.plist`

Configure
=========

Edit these files in `/usr/local/etc` with your custom settings:

* dnsmasq.available/local.conf
* `ln -s /usr/local/etc/dnsmasq.available/local.conf /usr/local/etc/dnsmasq.enabled`
